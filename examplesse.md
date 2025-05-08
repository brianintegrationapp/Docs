Here is an example of the mcp folder of a working SSE MCP server:

actor.ts:


import type { ActorDefinition } from 'apify-client';

import { ApifyClient, getApifyAPIBaseUrl } from '../apify-client.js';

export async function isActorMCPServer(actorID: string, apifyToken: string): Promise<boolean> {
    const mcpPath = await getActorsMCPServerPath(actorID, apifyToken);
    return (mcpPath?.length || 0) > 0;
}

export async function getActorsMCPServerPath(actorID: string, apifyToken: string): Promise<string | undefined> {
    const actorDefinition = await getActorDefinition(actorID, apifyToken);

    if ('webServerMcpPath' in actorDefinition && typeof actorDefinition.webServerMcpPath === 'string') {
        return actorDefinition.webServerMcpPath;
    }

    return undefined;
}

export async function getActorsMCPServerURL(actorID: string, apifyToken: string): Promise<string> {
    // TODO: get from API instead
    const standbyBaseUrl = process.env.HOSTNAME === 'mcp-securitybyobscurity.apify.com'
        ? 'securitybyobscurity.apify.actor' : 'apify.actor';
    const standbyUrl = await getActorStandbyURL(actorID, apifyToken, standbyBaseUrl);
    const mcpPath = await getActorsMCPServerPath(actorID, apifyToken);
    return `${standbyUrl}${mcpPath}`;
}

/**
* Gets Actor ID from the Actor object.
*
* @param actorID
* @param apifyToken
*/
export async function getRealActorID(actorID: string, apifyToken: string): Promise<string> {
    const apifyClient = new ApifyClient({ token: apifyToken });

    const actor = apifyClient.actor(actorID);
    const info = await actor.get();
    if (!info) {
        throw new Error(`Actor ${actorID} not found`);
    }
    return info.id;
}

/**
* Returns standby URL for given Actor ID.
*
* @param actorID
* @param standbyBaseUrl
* @param apifyToken
* @returns
*/
export async function getActorStandbyURL(actorID: string, apifyToken: string, standbyBaseUrl = 'apify.actor'): Promise<string> {
    const actorRealID = await getRealActorID(actorID, apifyToken);
    return `https://${actorRealID}.${standbyBaseUrl}`;
}

export async function getActorDefinition(actorID: string, apifyToken: string): Promise<ActorDefinition> {
    const apifyClient = new ApifyClient({ token: apifyToken });
    const actor = apifyClient.actor(actorID);
    const info = await actor.get();
    if (!info) {
        throw new Error(`Actor ${actorID} not found`);
    }

    const actorObjID = info.id;
    const res = await fetch(`${getApifyAPIBaseUrl()}/v2/acts/${actorObjID}/builds/default`, {
        headers: {
            // This is done so tests can pass with public Actors without token
            ...(apifyToken ? { Authorization: `Bearer ${apifyToken}` } : {}),
        },
    });
    if (!res.ok) {
        throw new Error(`Failed to fetch default build for actor ${actorID}: ${res.statusText}`);
    }
    const json = await res.json() as any; // eslint-disable-line @typescript-eslint/no-explicit-any
    const buildInfo = json.data;
    if (!buildInfo) {
        throw new Error(`Default build for Actor ${actorID} not found`);
    }
    const { actorDefinition } = buildInfo;
    if (!actorDefinition) {
        throw new Error(`Actor default build ${actorID} does not have Actor definition`);
    }

    return actorDefinition;
}


client.ts:

import { Client } from '@modelcontextprotocol/sdk/client/index.js';
import { SSEClientTransport } from '@modelcontextprotocol/sdk/client/sse.js';

import { getMCPServerID } from './utils.js';

/**
 * Creates and connects a ModelContextProtocol client.
 */
export async function createMCPClient(
    url: string, token: string,
): Promise<Client> {
    const transport = new SSEClientTransport(
        new URL(url),
        {
            requestInit: {
                headers: {
                    authorization: `Bearer ${token}`,
                },
            },
            eventSourceInit: {
                // The EventSource package augments EventSourceInit with a "fetch" parameter.
                // You can use this to set additional headers on the outgoing request.
                // Based on this example: https://github.com/modelcontextprotocol/typescript-sdk/issues/118
                async fetch(input: Request | URL | string, init?: RequestInit) {
                    const headers = new Headers(init?.headers || {});
                    headers.set('authorization', `Bearer ${token}`);
                    return fetch(input, { ...init, headers });
                },
            // We have to cast to "any" to use it, since it's non-standard
            } as any, // eslint-disable-line @typescript-eslint/no-explicit-any
        });

    const client = new Client({
        name: getMCPServerID(url),
        version: '1.0.0',
    });

    await client.connect(transport);

    return client;
}

const.ts:

export const MAX_TOOL_NAME_LENGTH = 64;
export const SERVER_ID_LENGTH = 8;
export const EXTERNAL_TOOL_CALL_TIMEOUT_MSEC = 120_000; // 2 minutes

proxy.ts:


import type { Client } from '@modelcontextprotocol/sdk/client/index.js';
import Ajv from 'ajv';

import type { ActorMCPTool, ToolWrap } from '../types.js';
import { getMCPServerID, getProxyMCPServerToolName } from './utils.js';

export async function getMCPServerTools(
    actorID: string,
    client: Client,
    // Name of the MCP server
    serverUrl: string,
): Promise<ToolWrap[]> {
    const res = await client.listTools();
    const { tools } = res;

    const ajv = new Ajv({ coerceTypes: 'array', strict: false });

    const compiledTools: ToolWrap[] = [];
    for (const tool of tools) {
        const mcpTool: ActorMCPTool = {
            actorID,
            serverId: getMCPServerID(serverUrl),
            serverUrl,
            originToolName: tool.name,

            name: getProxyMCPServerToolName(serverUrl, tool.name),
            description: tool.description || '',
            inputSchema: tool.inputSchema,
            ajvValidate: ajv.compile(tool.inputSchema),
        };

        const wrap: ToolWrap = {
            type: 'actor-mcp',
            tool: mcpTool,
        };

        compiledTools.push(wrap);
    }

    return compiledTools;
}


server.ts:

/**
 * Model Context Protocol (MCP) server for Apify Actors
 */

import type { Client } from '@modelcontextprotocol/sdk/client/index.js';
import { Server } from '@modelcontextprotocol/sdk/server/index.js';
import type { Transport } from '@modelcontextprotocol/sdk/shared/transport.js';
import { CallToolRequestSchema, CallToolResultSchema, ErrorCode, ListToolsRequestSchema, McpError } from '@modelcontextprotocol/sdk/types.js';
import type { ActorCallOptions } from 'apify-client';

import log from '@apify/log';

import {
    ACTOR_OUTPUT_MAX_CHARS_PER_ITEM,
    ACTOR_OUTPUT_TRUNCATED_MESSAGE,
    defaults,
    SERVER_NAME,
    SERVER_VERSION,
} from '../const.js';
import { helpTool } from '../tools/helpers.js';
import {
    actorDefinitionTool,
    addTool,
    callActorGetDataset,
    getActorsAsTools,
    removeTool,
    searchTool,
} from '../tools/index.js';
import { actorNameToToolName } from '../tools/utils.js';
import type { ActorMCPTool, ActorTool, HelperTool, ToolWrap } from '../types.js';
import { createMCPClient } from './client.js';
import { EXTERNAL_TOOL_CALL_TIMEOUT_MSEC } from './const.js';
import { processParamsGetTools } from './utils.js';

type ActorsMcpServerOptions = {
    enableAddingActors?: boolean;
    enableDefaultActors?: boolean;
};

/**
 * Create Apify MCP server
 */
export class ActorsMcpServer {
    public readonly server: Server;
    public readonly tools: Map<string, ToolWrap>;
    private readonly options: ActorsMcpServerOptions;

    constructor(options: ActorsMcpServerOptions = {}) {
        this.options = {
            enableAddingActors: options.enableAddingActors ?? false,
            enableDefaultActors: options.enableDefaultActors ?? true, // Default to true for backward compatibility
        };
        this.server = new Server(
            {
                name: SERVER_NAME,
                version: SERVER_VERSION,
            },
            {
                capabilities: {
                    tools: { listChanged: true },
                    logging: {},
                },
            },
        );
        this.tools = new Map();
        this.setupErrorHandling();
        this.setupToolHandlers();

        // Add default tools
        this.updateTools([searchTool, actorDefinitionTool, helpTool]);

        // Add tools to dynamically load Actors
        if (this.options.enableAddingActors) {
            this.loadToolsToAddActors();
        }

        // Initialize automatically for backward compatibility
        this.initialize().catch((error) => {
            log.error('Failed to initialize server:', error);
        });
    }

    /**
    * Resets the server to the default state.
    * This method clears all tools and loads the default tools.
    * Used primarily for testing purposes.
    */
    public async reset(): Promise<void> {
        this.tools.clear();
        this.updateTools([searchTool, actorDefinitionTool, helpTool]);
        if (this.options.enableAddingActors) {
            this.loadToolsToAddActors();
        }

        // Initialize automatically for backward compatibility
        await this.initialize();
    }

    /**
     * Initialize the server with default tools if enabled
     */
    public async initialize(): Promise<void> {
        if (this.options.enableDefaultActors) {
            await this.loadDefaultTools(process.env.APIFY_TOKEN as string);
        }
    }

    /**
     * Loads default tools if not already loaded.
     */
    public async loadDefaultTools(apifyToken: string) {
        const missingDefaultTools = defaults.actors.filter((name) => !this.tools.has(actorNameToToolName(name)));
        const tools = await getActorsAsTools(missingDefaultTools, apifyToken);
        if (tools.length > 0) {
            log.info('Loading default tools...');
            this.updateTools(tools);
        }
    }

    /**
     * Loads tools from URL params.
     *
     * This method also handles enabling of Actor autoloading via the processParamsGetTools.
     *
     * Used primarily for SSE.
     */
    public async loadToolsFromUrl(url: string, apifyToken: string) {
        const tools = await processParamsGetTools(url, apifyToken);
        if (tools.length > 0) {
            log.info('Loading tools from query parameters...');
            this.updateTools(tools);
        }
    }

    /**
     * Add Actors to server dynamically
     */
    public loadToolsToAddActors() {
        this.updateTools([addTool, removeTool]);
    }

    /**
     * Upsert new tools.
     * @param tools - Array of tool wrappers.
     * @returns Array of tool wrappers.
     */
    public updateTools(tools: ToolWrap[]) {
        for (const wrap of tools) {
            this.tools.set(wrap.tool.name, wrap);
            log.info(`Added/updated tool: ${wrap.tool.name}`);
        }
        return tools;
    }

    /**
     * Returns an array of tool names.
     * @returns {string[]} - An array of tool names.
     */
    public getToolNames(): string[] {
        return Array.from(this.tools.keys());
    }

    private setupErrorHandling(): void {
        this.server.onerror = (error) => {
            console.error('[MCP Error]', error); // eslint-disable-line no-console
        };
        process.on('SIGINT', async () => {
            await this.server.close();
            process.exit(0);
        });
    }

    private setupToolHandlers(): void {
        /**
         * Handles the request to list tools.
         * @param {object} request - The request object.
         * @returns {object} - The response object containing the tools.
         */
        this.server.setRequestHandler(ListToolsRequestSchema, async () => {
            const tools = Array.from(this.tools.values()).map((tool) => (tool.tool));
            return { tools };
        });

        /**
         * Handles the request to call a tool.
         * @param {object} request - The request object containing tool name and arguments.
         * @throws {McpError} - based on the McpServer class code from the typescript MCP SDK
         */
        this.server.setRequestHandler(CallToolRequestSchema, async (request) => {
            const { name, arguments: args } = request.params;
            const apifyToken = (request.params.apifyToken || process.env.APIFY_TOKEN) as string;

            // Remove apifyToken from request.params just in case
            delete request.params.apifyToken;

            // Validate token
            if (!apifyToken) {
                const msg = 'APIFY_TOKEN is required. It must be set in the environment variables or passed as a parameter in the body.';
                log.error(msg);
                await this.server.sendLoggingMessage({ level: 'error', data: msg });
                throw new McpError(
                    ErrorCode.InvalidParams,
                    msg,
                );
            }

            // TODO - if connection is /mcp client will not receive notification on tool change

            // Find tool by name or actor full name
            const tool = Array.from(this.tools.values())
                .find((t) => t.tool.name === name || (t.type === 'actor' && (t.tool as ActorTool).actorFullName === name));
            if (!tool) {
                const msg = `Tool ${name} not found. Available tools: ${this.getToolNames().join(', ')}`;
                log.error(msg);
                await this.server.sendLoggingMessage({ level: 'error', data: msg });
                throw new McpError(
                    ErrorCode.InvalidParams,
                    msg,
                );
            }
            if (!args) {
                const msg = `Missing arguments for tool ${name}`;
                log.error(msg);
                await this.server.sendLoggingMessage({ level: 'error', data: msg });
                throw new McpError(
                    ErrorCode.InvalidParams,
                    msg,
                );
            }
            log.info(`Validate arguments for tool: ${tool.tool.name} with arguments: ${JSON.stringify(args)}`);
            if (!tool.tool.ajvValidate(args)) {
                const msg = `Invalid arguments for tool ${tool.tool.name}: args: ${JSON.stringify(args)} error: ${JSON.stringify(tool?.tool.ajvValidate.errors)}`;
                log.error(msg);
                await this.server.sendLoggingMessage({ level: 'error', data: msg });
                throw new McpError(
                    ErrorCode.InvalidParams,
                    msg,
                );
            }

            try {
                // Handle internal tool
                if (tool.type === 'internal') {
                    const internalTool = tool.tool as HelperTool;
                    const res = await internalTool.call({
                        args,
                        apifyMcpServer: this,
                        mcpServer: this.server,
                        apifyToken,
                    }) as object;

                    return { ...res };
                }

                if (tool.type === 'actor-mcp') {
                    const serverTool = tool.tool as ActorMCPTool;
                    let client: Client | undefined;
                    try {
                        client = await createMCPClient(serverTool.serverUrl, apifyToken);
                        const res = await client.callTool({
                            name: serverTool.originToolName,
                            arguments: args,
                        }, CallToolResultSchema, {
                            timeout: EXTERNAL_TOOL_CALL_TIMEOUT_MSEC,
                        });

                        return { ...res };
                    } finally {
                        if (client) await client.close();
                    }
                }

                // Handle actor tool
                if (tool.type === 'actor') {
                    const actorTool = tool.tool as ActorTool;

                    const callOptions: ActorCallOptions = {
                        memory: actorTool.memoryMbytes,
                    };

                    const items = await callActorGetDataset(actorTool.actorFullName, args, apifyToken as string, callOptions);

                    const content = items.map((item) => {
                        const text = JSON.stringify(item).slice(0, ACTOR_OUTPUT_MAX_CHARS_PER_ITEM);
                        return text.length === ACTOR_OUTPUT_MAX_CHARS_PER_ITEM
                            ? { type: 'text', text: `${text} ... ${ACTOR_OUTPUT_TRUNCATED_MESSAGE}` }
                            : { type: 'text', text };
                    });
                    return { content };
                }
            } catch (error) {
                log.error(`Error calling tool ${name}: ${error}`);
                throw new McpError(
                    ErrorCode.InternalError,
                    `An error occurred while calling the tool.`,
                );
            }

            const msg = `Unknown tool: ${name}`;
            log.error(msg);
            await this.server.sendLoggingMessage({
                level: 'error',
                data: msg,
            });
            throw new McpError(
                ErrorCode.InvalidParams,
                msg,
            );
        });
    }

    async connect(transport: Transport): Promise<void> {
        await this.server.connect(transport);
    }

    async close(): Promise<void> {
        await this.server.close();
    }
}

utils.ts:


import { createHash } from 'node:crypto';
import { parse } from 'node:querystring';

import { processInput } from '../input.js';
import { addTool, getActorsAsTools, removeTool } from '../tools/index.js';
import type { Input, ToolWrap } from '../types.js';
import { MAX_TOOL_NAME_LENGTH, SERVER_ID_LENGTH } from './const.js';

/**
 * Generates a unique server ID based on the provided URL.
 *
 * URL is used instead of Actor ID becase one Actor may expose multiple servers - legacy SSE / streamable HTTP.
 *
 * @param url The URL to generate the server ID from.
 * @returns A unique server ID.
 */
export function getMCPServerID(url: string): string {
    const serverHashDigest = createHash('sha256').update(url).digest('hex');

    return serverHashDigest.slice(0, SERVER_ID_LENGTH);
}

/**
 * Generates a unique tool name based on the provided URL and tool name.
 * @param url The URL to generate the tool name from.
 * @param toolName The tool name to generate the tool name from.
 * @returns A unique tool name.
 */
export function getProxyMCPServerToolName(url: string, toolName: string): string {
    const prefix = getMCPServerID(url);

    const fullName = `${prefix}-${toolName}`;
    return fullName.slice(0, MAX_TOOL_NAME_LENGTH);
}

/**
 * Process input parameters and get tools
 * If URL contains query parameter `actors`, return tools from Actors otherwise return null.
 * @param url
 * @param apifyToken
 */
export async function processParamsGetTools(url: string, apifyToken: string) {
    const input = parseInputParamsFromUrl(url);
    let tools: ToolWrap[] = [];
    if (input.actors) {
        const actors = input.actors as string[];
        // Normal Actors as a tool
        tools = await getActorsAsTools(actors, apifyToken);
    }
    if (input.enableAddingActors) {
        tools.push(addTool, removeTool);
    }
    return tools;
}

export function parseInputParamsFromUrl(url: string): Input {
    const query = url.split('?')[1] || '';
    const params = parse(query) as unknown as Input;
    return processInput(params);
}
