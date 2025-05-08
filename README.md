This file is a merged representation of the entire codebase, combined into a single document by Repomix.
The content has been processed where security check has been disabled.

<file_summary>
This section contains a summary of this file.

<purpose>
This file contains a packed representation of the entire repository's contents.
It is designed to be easily consumable by AI systems for analysis, code review,
or other automated processes.
</purpose>

<file_format>
The content is organized as follows:
1. This summary section
2. Repository information
3. Directory structure
4. Repository files, each consisting of:
  - File path as an attribute
  - Full contents of the file
</file_format>

<usage_guidelines>
- This file should be treated as read-only. Any changes should be made to the
  original repository files, not this packed version.
- When processing this file, use the file path to distinguish
  between different files in the repository.
- Be aware that this file may contain sensitive information. Handle it with
  the same level of security as you would the original repository.
</usage_guidelines>

<notes>
- Some files may have been excluded based on .gitignore rules and Repomix's configuration
- Binary files are not included in this packed representation. Please refer to the Repository Structure section for a complete list of file paths, including binary files
- Files matching patterns in .gitignore are excluded
- Files matching default ignore patterns are excluded
- Security check has been disabled - content may contain sensitive information
- Files are sorted by Git change count (files with more changes are at the bottom)
</notes>

<additional_info>

</additional_info>

</file_summary>

<directory_structure>
.actor/
  actor.json
  ACTOR.md
  Dockerfile
  input_schema.json
.github/
  scripts/
    before-beta-release.cjs
  workflows/
    check.yaml
    pre_release.yaml
    release.yaml
src/
  actor/
    const.ts
    README.md
    server.ts
    types.ts
    utils.ts
  examples/
    client_sse.py
    clientSse.ts
    clientStdio.ts
    clientStdioChat.ts
    clientStreamableHttp.ts
  mcp/
    actors.ts
    client.ts
    const.ts
    proxy.ts
    server.ts
    utils.ts
  tools/
    actor.ts
    build.ts
    helpers.ts
    index.ts
    store_collection.ts
    utils.ts
  apify-client.ts
  const.ts
  index.ts
  input.ts
  main.ts
  stdio.ts
  tsconfig.json
  types.ts
tests/
  integration/
    actor.server-sse.test.ts
    actor.server-streamable.test.ts
    stdio.test.ts
    suite.ts
  unit/
    input.test.ts
    mcp.utils.test.ts
    tools.actor.test.ts
    tools.utils.test.ts
  helpers.ts
  README.md
.dockerignore
.editorconfig
.env.example
.gitignore
.npmignore
.nvmrc
CHANGELOG.md
Dockerfile
eslint.config.mjs
glama.json
LICENSE.md
package.json
README.md
smithery.yaml
tsconfig.eslint.json
tsconfig.json
vitest.config.ts
</directory_structure>

<files>
This section contains the contents of the repository's files.

<file path=".actor/actor.json">
{
  "actorSpecification": 1,
  "name": "apify-mcp-server",
  "title": "Model Context Protocol Server for Apify Actors",
  "description": "Implementation of a Model Context Protocol (MCP) Server for Apify Actors that enables AI applications (and AI agents) to interact with Apify Actors",
  "version": "0.1",
  "input": "./input_schema.json",
  "readme": "./ACTOR.md",
  "dockerfile": "./Dockerfile",
  "webServerMcpPath": "/sse"
}
</file>

<file path=".actor/ACTOR.md">
# Apify Model Context Protocol (MCP) Server

[![Actors MCP Server](https://apify.com/actor-badge?actor=apify/actors-mcp-server)](https://apify.com/apify/actors-mcp-server)

Implementation of an MCP server for all [Apify Actors](https://apify.com/store).
This server enables interaction with one or more Apify Actors that can be defined in the MCP Server configuration.

The server can be used in two ways:
- **🇦 [MCP Server Actor](https://apify.com/apify/actors-mcp-server)** – HTTP server accessible via Server-Sent Events (SSE), see [guide](#-mcp-server-actor)
- **⾕ MCP Server Stdio** – Local server available via standard input/output (stdio), see [guide](#-mcp-server-at-a-local-host)

You can also interact with the MCP server using a chat-like UI with 💬 [Tester MCP Client](https://apify.com/jiri.spilka/tester-mcp-client)

# 🎯 What does Apify MCP server do?

The MCP Server Actor allows an AI assistant to use any [Apify Actor](https://apify.com/store) as a tool to perform a specific task.
For example, it can:
- Use [Facebook Posts Scraper](https://apify.com/apify/facebook-posts-scraper) to extract data from Facebook posts from multiple pages/profiles
- Use [Google Maps Email Extractor](https://apify.com/lukaskrivka/google-maps-with-contact-details) to extract Google Maps contact details
- Use [Google Search Results Scraper](https://apify.com/apify/google-search-scraper) to scrape Google Search Engine Results Pages (SERPs)
- Use [Instagram Scraper](https://apify.com/apify/instagram-scraper) to scrape Instagram posts, profiles, places, photos, and comments
- Use [RAG Web Browser](https://apify.com/apify/web-scraper) to search the web, scrape the top N URLs, and return their content

# MCP Clients

To interact with the Apify MCP server, you can use MCP clients such as:
- [Claude Desktop](https://claude.ai/download) (only Stdio support)
- [Visual Studio Code](https://code.visualstudio.com/) (Stdio and SSE support)
- [LibreChat](https://www.librechat.ai/) (Stdio and SSE support, yet without Authorization header)
- [Apify Tester MCP Client](https://apify.com/jiri.spilka/tester-mcp-client) (SSE support with Authorization headers)
- Other clients at [https://modelcontextprotocol.io/clients](https://modelcontextprotocol.io/clients)
- More clients at [https://glama.ai/mcp/clients](https://glama.ai/mcp/clients)

When you have Actors integrated with the MCP server, you can ask:
- "Search the web and summarize recent trends about AI Agents"
- "Find the top 10 best Italian restaurants in San Francisco"
- "Find and analyze the Instagram profile of The Rock"
- "Provide a step-by-step guide on using the Model Context Protocol with source URLs"
- "What Apify Actors can I use?"

The following image shows how the Apify MCP server interacts with the Apify platform and AI clients:

![Actors-MCP-server](https://raw.githubusercontent.com/apify/actors-mcp-server/refs/heads/master/docs/actors-mcp-server.png)

With the MCP Tester client you can load Actors dynamically but this is not yet supported by other MCP clients.
We also plan to add more features, see [Roadmap](#-roadmap-march-2025) for more details.

# 🔄 What is the Model Context Protocol?

The Model Context Protocol (MCP) allows AI applications (and AI agents), such as Claude Desktop, to connect to external tools and data sources.
MCP is an open protocol that enables secure, controlled interactions between AI applications, AI Agents, and local or remote resources.

For more information, see the [Model Context Protocol](https://modelcontextprotocol.org/) website or the blog post [What is MCP and why does it matter?](https://blog.apify.com/what-is-model-context-protocol/).

# 🤖 How is MCP Server related to AI Agents?

The Apify MCP Server exposes Apify's Actors through the MCP protocol, allowing AI Agents or frameworks that implement the MCP protocol to access all Apify Actors as tools for data extraction, web searching, and other tasks.

To learn more about AI Agents, explore our blog post: [What are AI Agents?](https://blog.apify.com/what-are-ai-agents/) and browse Apify's curated [AI Agent collection](https://apify.com/store/collections/ai_agents).
Interested in building and monetizing your own AI agent on Apify? Check out our [step-by-step guide](https://blog.apify.com/how-to-build-an-ai-agent/) for creating, publishing, and monetizing AI agents on the Apify platform.

# 🧱 Components

## Tools

### Actors

Any [Apify Actor](https://apify.com/store) can be used as a tool.
By default, the server is pre-configured with the Actors specified below, but this can be overridden by providing Actor input.

```text
'apify/instagram-scraper'
'apify/rag-web-browser'
'lukaskrivka/google-maps-with-contact-details'
```
The MCP server loads the Actor input schema and creates MCP tools corresponding to the Actors.
See this example of input schema for the [RAG Web Browser](https://apify.com/apify/rag-web-browser/input-schema).

The tool name must always be the full Actor name, such as `apify/rag-web-browser`.
The arguments for an MCP tool represent the input parameters of the Actor.
For example, for the `apify/rag-web-browser` Actor, the arguments are:

```json
{
  "query": "restaurants in San Francisco",
  "maxResults": 3
}
```
You don't need to specify the input parameters or which Actor to call; everything is managed by an LLM.
When a tool is called, the arguments are automatically passed to the Actor by the LLM.
You can refer to the specific Actor's documentation for a list of available arguments.

### Helper tools

The server provides a set of helper tools to discover available Actors and retrieve their details:
- `get-actor-details`: Retrieves documentation, input schema, and details about a specific Actor.
- `discover-actors`: Searches for relevant Actors using keywords and returns their details.

There are also tools to manage the available tools list. However, dynamically adding and removing tools requires the MCP client to have the capability to update the tools list (handle `ToolListChangedNotificationSchema`), which is typically not supported.

You can try this functionality using the [Apify Tester MCP Client](https://apify.com/jiri.spilka/tester-mcp-client) Actor.
To enable it, set the `enableActorAutoLoading` parameter.

- `add-actor-as-tool`: Adds an Actor by name to the available tools list without executing it, requiring user consent to run later.
- `remove-actor-from-tool`: Removes an Actor by name from the available tools list when it's no longer needed.

## Prompt & Resources

The server does not provide any resources and prompts.
We plan to add [Apify's dataset](https://docs.apify.com/platform/storage/dataset) and [key-value store](https://docs.apify.com/platform/storage/key-value-store) as resources in the future.

# ⚙️ Usage

The Apify MCP Server can be used in two ways: **as an Apify Actor** running on the Apify platform
or as a **local server** running on your machine.

## 🇦 MCP Server Actor

### Standby web server

The Actor runs in [**Standby mode**](https://docs.apify.com/platform/actors/running/standby) with an HTTP web server that receives and processes requests.

To start the server with default Actors, send an HTTP GET request with your [Apify API token](https://console.apify.com/settings/integrations) to the following URL:
```
https://actors-mcp-server.apify.actor?token=<APIFY_TOKEN>
```
It is also possible to start the MCP server with a different set of Actors.
To do this, create a [task](https://docs.apify.com/platform/actors/running/tasks) and specify the list of Actors you want to use.

Then, run the task in Standby mode with the selected Actors:
```shell
https://USERNAME--actors-mcp-server-task.apify.actor?token=<APIFY_TOKEN>
```

You can find a list of all available Actors in the [Apify Store](https://apify.com/store).

#### 💬 Interact with the MCP Server over SSE

Once the server is running, you can interact with Server-Sent Events (SSE) to send messages to the server and receive responses.
The easiest way is to use [Tester MCP Client](https://apify.com/jiri.spilka/tester-mcp-client) on Apify.

[Claude Desktop](https://claude.ai/download) currently lacks SSE support, but you can use it with Stdio transport; see [MCP Server at a local host](#-mcp-server-at-a-local-host) for more details.
Note: The free version of Claude Desktop may experience intermittent connection issues with the server.

In the client settings, you need to provide server configuration:
```json
{
    "mcpServers": {
        "apify": {
            "type": "sse",
            "url": "https://actors-mcp-server.apify.actor/sse",
            "env": {
                "APIFY_TOKEN": "your-apify-token"
            }
        }
    }
}
```
Alternatively, you can use [clientSse.ts](https://github.com/apify/actor-mcp-server/tree/main/src/examples/clientSse.ts) script or test the server using `curl` </> commands.

1. Initiate Server-Sent-Events (SSE) by sending a GET request to the following URL:
    ```
    curl https://actors-mcp-server.apify.actor/sse?token=<APIFY_TOKEN>
    ```
    The server will respond with a `sessionId`, which you can use to send messages to the server:
    ```shell
    event: endpoint
    data: /message?sessionId=a1b
    ```

2. Send a message to the server by making a POST request with the `sessionId`:
    ```shell
    curl -X POST "https://actors-mcp-server.apify.actor/message?token=<APIFY_TOKEN>&session_id=a1b" -H "Content-Type: application/json" -d '{
      "jsonrpc": "2.0",
      "id": 1,
      "method": "tools/call",
      "params": {
        "arguments": { "searchStringsArray": ["restaurants in San Francisco"], "maxCrawledPlacesPerSearch": 3 },
        "name": "lukaskrivka/google-maps-with-contact-details"
      }
    }'
    ```
    The MCP server will start the Actor `lukaskrivka/google-maps-with-contact-details` with the provided arguments as input parameters.
    For this POST request, the server will respond with:

    ```text
    Accepted
    ```

3. Receive the response. The server will invoke the specified Actor as a tool using the provided query parameters and stream the response back to the client via SSE.
    The response will be returned as JSON text.

    ```text
    event: message
    data: {"result":{"content":[{"type":"text","text":"{\"searchString\":\"restaurants in San Francisco\",\"rank\":1,\"title\":\"Gary Danko\",\"description\":\"Renowned chef Gary Danko's fixed-price menus of American cuisine ... \",\"price\":\"$100+\"...}}]}}
    ```

## ⾕ MCP Server at a local host

You can run the Apify MCP Server on your local machine by configuring it with Claude Desktop or any other [MCP client](https://modelcontextprotocol.io/clients).
You can also use [Smithery](https://smithery.ai/server/@apify/actors-mcp-server) to install the server automatically.

### Prerequisites

- MacOS or Windows
- The latest version of Claude Desktop must be installed (or another MCP client)
- [Node.js](https://nodejs.org/en) (v18 or higher)
- [Apify API Token](https://docs.apify.com/platform/integrations/api#api-token) (`APIFY_TOKEN`)

Make sure you have the `node` and `npx` installed properly:
```bash
node -v
npx -v
```
If not, follow this guide to install Node.js: [Downloading and installing Node.js and npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm).

#### Claude Desktop

To configure Claude Desktop to work with the MCP server, follow these steps. For a detailed guide, refer to the [Claude Desktop Users Guide](https://modelcontextprotocol.io/quickstart/user).

1. Download Claude for desktop
   - Available for Windows and macOS.
   - For Linux users, you can build a Debian package using this [unofficial build script](https://github.com/aaddrick/claude-desktop-debian).
2. Open the Claude Desktop app and enable **Developer Mode** from the top-left menu bar.
3. Once enabled, open **Settings** (also from the top-left menu bar) and navigate to the **Developer Option**, where you'll find the **Edit Config** button.
4. Open the configuration file and edit the following file:

    - On macOS: `~/Library/Application\ Support/Claude/claude_desktop_config.json`
    - On Windows: `%APPDATA%/Claude/claude_desktop_config.json`
    - On Linux: `~/.config/Claude/claude_desktop_config.json`

    ```json
    {
     "mcpServers": {
       "actors-mcp-server": {
         "command": "npx",
         "args": ["-y", "@apify/actors-mcp-server"],
         "env": {
            "APIFY_TOKEN": "your-apify-token"
         }
       }
     }
    }
    ```
    Alternatively, you can use the `actors` argument to select one or more Apify Actors:
    ```json
   {
    "mcpServers": {
      "actors-mcp-server": {
        "command": "npx",
        "args": [
          "-y", "@apify/actors-mcp-server",
          "--actors", "lukaskrivka/google-maps-with-contact-details,apify/instagram-scraper"
        ],
        "env": {
           "APIFY_TOKEN": "your-apify-token"
        }
      }
    }
   }
    ```
5. Restart Claude Desktop

    - Fully quit Claude Desktop (ensure it's not just minimized or closed).
    - Restart Claude Desktop.
    - Look for the 🔌 icon to confirm that the Actors MCP server is connected.

6. Open the Claude Desktop chat and ask "What Apify Actors can I use?"

   ![Claude-desktop-with-Actors-MCP-server](https://raw.githubusercontent.com/apify/actors-mcp-server/refs/heads/master/docs/claude-desktop.png)

7. Examples

   You can ask Claude to perform tasks, such as:
    ```text
    Find and analyze recent research papers about LLMs.
    Find the top 10 best Italian restaurants in San Francisco.
    Find and analyze the Instagram profile of The Rock.
    ```

#### VS Code

For one-click installation, click one of the install buttons below:

[![Install with NPX in VS Code](https://img.shields.io/badge/VS_Code-NPM-0098FF?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=actors-mcp-server&config=%7B%22command%22%3A%22npx%22%2C%22args%22%3A%5B%22-y%22%2C%22%40apify%2Factors-mcp-server%22%5D%2C%22env%22%3A%7B%22APIFY_TOKEN%22%3A%22%24%7Binput%3Aapify_token%7D%22%7D%7D&inputs=%5B%7B%22type%22%3A%22promptString%22%2C%22id%22%3A%22apify_token%22%2C%22description%22%3A%22Apify+API+Token%22%2C%22password%22%3Atrue%7D%5D) [![Install with NPX in VS Code Insiders](https://img.shields.io/badge/VS_Code_Insiders-NPM-24bfa5?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=actors-mcp-server&config=%7B%22command%22%3A%22npx%22%2C%22args%22%3A%5B%22-y%22%2C%22%40apify%2Factors-mcp-server%22%5D%2C%22env%22%3A%7B%22APIFY_TOKEN%22%3A%22%24%7Binput%3Aapify_token%7D%22%7D%7D&inputs=%5B%7B%22type%22%3A%22promptString%22%2C%22id%22%3A%22apify_token%22%2C%22description%22%3A%22Apify+API+Token%22%2C%22password%22%3Atrue%7D%5D&quality=insiders)

##### Manual installation

You can manually install the Apify MCP Server in VS Code. First, click one of the install buttons at the top of this section for a one-click installation.

Alternatively, add the following JSON block to your User Settings (JSON) file in VS Code. You can do this by pressing `Ctrl + Shift + P` and typing `Preferences: Open User Settings (JSON)`.

```json
{
  "mcp": {
    "inputs": [
      {
        "type": "promptString",
        "id": "apify_token",
        "description": "Apify API Token",
        "password": true
      }
    ],
    "servers": {
      "actors-mcp-server": {
        "command": "npx",
        "args": ["-y", "@apify/actors-mcp-server"],
        "env": {
          "APIFY_TOKEN": "${input:apify_token}"
        }
      }
    }
  }
}
```

Optionally, you can add it to a file called `.vscode/mcp.json` in your workspace - just omit the top-level `mcp {}` key. This will allow you to share the configuration with others.

If you want to specify which Actors to load, you can add the `--actors` argument:

```json
{
  "servers": {
    "actors-mcp-server": {
      "command": "npx",
      "args": [
        "-y", "@apify/actors-mcp-server",
        "--actors", "lukaskrivka/google-maps-with-contact-details,apify/instagram-scraper"
      ],
      "env": {
        "APIFY_TOKEN": "${input:apify_token}"
      }
    }
  }
}
```

#### VS Code

For one-click installation, click one of the install buttons below:

[![Install with NPX in VS Code](https://img.shields.io/badge/VS_Code-NPM-0098FF?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=actors-mcp-server&config=%7B%22command%22%3A%22npx%22%2C%22args%22%3A%5B%22-y%22%2C%22%40apify%2Factors-mcp-server%22%5D%2C%22env%22%3A%7B%22APIFY_TOKEN%22%3A%22%24%7Binput%3Aapify_token%7D%22%7D%7D&inputs=%5B%7B%22type%22%3A%22promptString%22%2C%22id%22%3A%22apify_token%22%2C%22description%22%3A%22Apify+API+Token%22%2C%22password%22%3Atrue%7D%5D) [![Install with NPX in VS Code Insiders](https://img.shields.io/badge/VS_Code_Insiders-NPM-24bfa5?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=actors-mcp-server&config=%7B%22command%22%3A%22npx%22%2C%22args%22%3A%5B%22-y%22%2C%22%40apify%2Factors-mcp-server%22%5D%2C%22env%22%3A%7B%22APIFY_TOKEN%22%3A%22%24%7Binput%3Aapify_token%7D%22%7D%7D&inputs=%5B%7B%22type%22%3A%22promptString%22%2C%22id%22%3A%22apify_token%22%2C%22description%22%3A%22Apify+API+Token%22%2C%22password%22%3Atrue%7D%5D&quality=insiders)

##### Manual installation

You can manually install the Apify MCP Server in VS Code. First, click one of the install buttons at the top of this section for a one-click installation.

Alternatively, add the following JSON block to your User Settings (JSON) file in VS Code. You can do this by pressing `Ctrl + Shift + P` and typing `Preferences: Open User Settings (JSON)`.

```json
{
  "mcp": {
    "inputs": [
      {
        "type": "promptString",
        "id": "apify_token",
        "description": "Apify API Token",
        "password": true
      }
    ],
    "servers": {
      "actors-mcp-server": {
        "command": "npx",
        "args": ["-y", "@apify/actors-mcp-server"],
        "env": {
          "APIFY_TOKEN": "${input:apify_token}"
        }
      }
    }
  }
}
```

Optionally, you can add it to a file called `.vscode/mcp.json` in your workspace - just omit the top-level `mcp {}` key. This will allow you to share the configuration with others.

If you want to specify which Actors to load, you can add the `--actors` argument:

```json
{
  "servers": {
    "actors-mcp-server": {
      "command": "npx",
      "args": [
        "-y", "@apify/actors-mcp-server",
        "--actors", "lukaskrivka/google-maps-with-contact-details,apify/instagram-scraper"
      ],
      "env": {
        "APIFY_TOKEN": "${input:apify_token}"
      }
    }
  }
}
```

#### Debugging NPM package @apify/actors-mcp-server with @modelcontextprotocol/inspector

To debug the server, use the [MCP Inspector](https://github.com/modelcontextprotocol/inspector) tool:

```shell
export APIFY_TOKEN=your-apify-token
npx @modelcontextprotocol/inspector npx -y @apify/actors-mcp-server
```

### Installing via Smithery

To install Apify Actors MCP Server for Claude Desktop automatically via [Smithery](https://smithery.ai/server/@apify/actors-mcp-server):

```bash
npx -y @smithery/cli install @apify/actors-mcp-server --client claude
```

#### Stdio clients

Create an environment file `.env` with the following content:
```text
APIFY_TOKEN=your-apify-token
```
In the `examples` directory, you can find an example client to interact with the server via
standard input/output (stdio):

- [`clientStdio.ts`](https://github.com/apify/actor-mcp-server/tree/main/src/examples/clientStdio.ts)
    This client script starts the MCP server with two specified Actors.
    It then calls the `apify/rag-web-browser` tool with a query and prints the result.
    It demonstrates how to connect to the MCP server, list available tools, and call a specific tool using stdio transport.
    ```bash
    node dist/examples/clientStdio.js
    ```

# 👷🏼 Development

## Prerequisites

- [Node.js](https://nodejs.org/en) (v18 or higher)
- Python 3.9 or higher

Create an environment file `.env` with the following content:
```text
APIFY_TOKEN=your-apify-token
```

Build the actor-mcp-server package:

```bash
npm run build
```

## Local client (SSE)

To test the server with the SSE transport, you can use the script `examples/clientSse.ts`:
Currently, the Node.js client does not support establishing a connection to a remote server with custom headers.
You need to change the URL to your local server URL in the script.

```bash
node dist/examples/clientSse.js
```

## Debugging

Since MCP servers operate over standard input/output (stdio), debugging can be challenging.
For the best debugging experience, use the [MCP Inspector](https://github.com/modelcontextprotocol/inspector).

You can launch the MCP Inspector via [`npm`](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm) with this command:

```bash
export APIFY_TOKEN=your-apify-token
npx @modelcontextprotocol/inspector node ./dist/stdio.js
```

Upon launching, the Inspector will display a URL that you can access in your browser to begin debugging.

## ⓘ Limitations and feedback

The Actor input schema is processed to be compatible with most MCP clients while adhering to [JSON Schema](https://json-schema.org/) standards. The processing includes:
- **Descriptions** are truncated to 500 characters (as defined in `MAX_DESCRIPTION_LENGTH`).
- **Enum fields** are truncated to a maximum combined length of 200 characters for all elements (as defined in `ACTOR_ENUM_MAX_LENGTH`).
- **Required fields** are explicitly marked with a "REQUIRED" prefix in their descriptions for compatibility with frameworks that may not handle JSON schema properly.
- **Nested properties** are built for special cases like proxy configuration and request list sources to ensure correct input structure.
- **Array item types** are inferred when not explicitly defined in the schema, using a priority order: explicit type in items > prefill type > default value type > editor type.
- **Enum values and examples** are added to property descriptions to ensure visibility even if the client doesn't fully support JSON schema.

Memory for each Actor is limited to 4GB.
Free users have an 8GB limit, 128MB needs to be allocated for running `Actors-MCP-Server`.

If you need other features or have any feedback, [submit an issue](https://console.apify.com/actors/1lSvMAaRcadrM1Vgv/issues) in Apify Console to let us know.

# 🚀 Roadmap (March 2025)

- Add Apify's dataset and key-value store as resources.
- Add tools such as Actor logs and Actor runs for debugging.

# 🐛 Troubleshooting

- Make sure you have the `node` installed by running `node -v`
- Make sure you have the `APIFY_TOKEN` environment variable set
- Always use the latest version of the MCP server by setting `@apify/actors-mcp-server@latest`

# 📚 Learn more

- [Model Context Protocol](https://modelcontextprotocol.org/)
- [What are AI Agents?](https://blog.apify.com/what-are-ai-agents/)
- [What is MCP and why does it matter?](https://blog.apify.com/what-is-model-context-protocol/)
- [Tester MCP Client](https://apify.com/jiri.spilka/tester-mcp-client)
- [AI agent workflow: building an agent to query Apify datasets](https://blog.apify.com/ai-agent-workflow/)
- [MCP Client development guide](https://github.com/cyanheads/model-context-protocol-resources/blob/main/guides/mcp-client-development-guide.md)
- [How to build and monetize an AI agent on Apify](https://blog.apify.com/how-to-build-an-ai-agent/)
</file>

<file path=".actor/Dockerfile">
# Specify the base Docker image. You can read more about
# the available images at https://docs.apify.com/sdk/js/docs/guides/docker-images
# You can also use any other image from Docker Hub.
FROM apify/actor-node:20 AS builder

# Check preinstalled packages
RUN npm ls crawlee apify puppeteer playwright

# Copy just package.json and package-lock.json
# to speed up the build using Docker layer cache.
COPY package*.json ./

# Install all dependencies. Don't audit to speed up the installation.
RUN npm install --include=dev --audit=false

# Next, copy the source files using the user set
# in the base image.
COPY . ./

# Install all dependencies and build the project.
# Don't audit to speed up the installation.
RUN npm run build

# Create final image
FROM apify/actor-node:20

# Check preinstalled packages
RUN npm ls crawlee apify puppeteer playwright

# Copy just package.json and package-lock.json
# to speed up the build using Docker layer cache.
COPY package*.json ./

# Install NPM packages, skip optional and development dependencies to
# keep the image small. Avoid logging too much and print the dependency
# tree for debugging
RUN npm --quiet set progress=false \
    && npm install --omit=dev --omit=optional \
    && echo "Installed NPM packages:" \
    && (npm list --omit=dev --all || true) \
    && echo "Node.js version:" \
    && node --version \
    && echo "NPM version:" \
    && npm --version \
    && rm -r ~/.npm

# Copy built JS files from builder image
COPY --from=builder /usr/src/app/dist ./dist

# Next, copy the remaining files and directories with the source code.
# Since we do this after NPM install, quick build will be really fast
# for most source file changes.
COPY . ./


# Run the image.
CMD npm run start:prod --silent
</file>

<file path=".actor/input_schema.json">
{
    "title": "Apify MCP Server",
    "type": "object",
    "schemaVersion": 1,
    "properties": {
        "actors": {
            "title": "Actors to be exposed for an AI application (AI agent)",
            "type": "array",
            "description": "List Actors to be exposed to an AI application (AI agent) for communication via the MCP protocol. \n\n Ensure the Actor definitions fit within the LLM context by limiting the number of used Actors.",
            "editor": "stringList",
            "prefill": [
                "apify/instagram-scraper",
                "apify/rag-web-browser",
                "lukaskrivka/google-maps-with-contact-details"
            ]
        },
        "enableActorAutoLoading": {
            "title": "Enable automatic loading of Actors based on context and use-case (experimental, check if it supported by your client) (deprecated, use enableAddingActors instead)",
            "type": "boolean",
            "description": "When enabled, the server can dynamically add Actors as tools based on user requests and context. \n\nNote: MCP client must support notification on tool updates. To try it, you can use the [Tester MCP Client](https://apify.com/jiri.spilka/tester-mcp-client). This is an experimental feature and may require client-specific support.",
            "default": false,
            "editor": "hidden"
        },
        "enableAddingActors": {
            "title": "Enable adding Actors based on context and use-case (experimental, check if it supported by your client)",
            "type": "boolean",
            "description": "When enabled, the server can dynamically add Actors as tools based on user requests and context. \n\nNote: MCP client must support notification on tool updates. To try it, you can use the [Tester MCP Client](https://apify.com/jiri.spilka/tester-mcp-client). This is an experimental feature and may require client-specific support.",
            "default": false
        },
        "maxActorMemoryBytes": {
            "title": "Limit the maximum memory used by an Actor",
            "type": "integer",
            "description": "Limit the maximum memory used by an Actor in bytes. This is important setting for Free plan users to avoid exceeding the memory limit.",
            "prefill": 4096,
            "default": 4096
        },
        "debugActor": {
            "title": "Debug Actor",
            "type": "string",
            "description": "Specify the name of the Actor that will be used for debugging in normal mode",
            "editor": "textfield",
            "prefill": "apify/rag-web-browser",
            "sectionCaption": "Debugging settings (normal mode)"
        },
        "debugActorInput": {
            "title": "Debug Actor input",
            "type": "object",
            "description": "Specify the input for the Actor that will be used for debugging in normal mode",
            "editor": "json",
            "prefill": {
            "query": "hello world"
            }
        }
    }
}
</file>

<file path=".github/scripts/before-beta-release.cjs">
const { execSync } = require('node:child_process');
const fs = require('node:fs');
const path = require('node:path');

const PKG_JSON_PATH = path.join(__dirname, '..', '..', 'package.json');

const pkgJson = require(PKG_JSON_PATH); // eslint-disable-line import/no-dynamic-require

const PACKAGE_NAME = pkgJson.name;
const VERSION = pkgJson.version;

const nextVersion = getNextVersion(VERSION);
console.log(`before-deploy: Setting version to ${nextVersion}`); // eslint-disable-line no-console
pkgJson.version = nextVersion;

fs.writeFileSync(PKG_JSON_PATH, `${JSON.stringify(pkgJson, null, 2)}\n`);

function getNextVersion(version) {
    const versionString = execSync(`npm show ${PACKAGE_NAME} versions --json`, { encoding: 'utf8' });
    const versions = JSON.parse(versionString);

    if (versions.some((v) => v === VERSION)) {
        console.error(`before-deploy: A release with version ${VERSION} already exists. Please increment version accordingly.`); // eslint-disable-line no-console
        process.exit(1);
    }

    const prereleaseNumbers = versions
        .filter((v) => (v.startsWith(VERSION) && v.includes('-')))
        .map((v) => Number(v.match(/\.(\d+)$/)[1]));
    const lastPrereleaseNumber = Math.max(-1, ...prereleaseNumbers);
    return `${version}-beta.${lastPrereleaseNumber + 1}`;
}
</file>

<file path=".github/workflows/check.yaml">
# This workflow runs for every pull request to lint and test the proposed changes.

name: Check

on:
    pull_request:

    # Push to master will trigger code checks
    push:
        branches:
            - master
        tags-ignore:
            - "**" # Ignore all tags to prevent duplicate builds when tags are pushed.

jobs:
    lint_and_test:
        name: Code checks
        runs-on: ubuntu-latest

        steps:
            -   uses: actions/checkout@v4
            -   name: Use Node.js 22
                uses: actions/setup-node@v4
                with:
                    node-version: 22
                    cache: 'npm'
                    cache-dependency-path: 'package-lock.json'
            -   name: Install Dependencies
                run: npm ci

            -   name: Lint
                run: npm run lint

            -   name: Build
                run: npm run build

            -   name: Test
                run: npm run test

            -   name: Type checks
                run: npm run type-check
</file>

<file path=".github/workflows/pre_release.yaml">
name: Create a pre-release

on:
    workflow_dispatch:
    # Push to master will deploy a beta version
    push:
        branches:
            - master
        tags-ignore:
            - "**" # Ignore all tags to prevent duplicate builds when tags are pushed.

concurrency:
    group: release
    cancel-in-progress: false

jobs:
    release_metadata:
        if: "!startsWith(github.event.head_commit.message, 'docs') && !startsWith(github.event.head_commit.message, 'ci') && startsWith(github.repository, 'apify/')"
        name: Prepare release metadata
        runs-on: ubuntu-latest
        outputs:
            version_number: ${{ steps.release_metadata.outputs.version_number }}
            changelog: ${{ steps.release_metadata.outputs.changelog }}
        steps:
            -   uses: apify/workflows/git-cliff-release@main
                name: Prepare release metadata
                id: release_metadata
                with:
                    release_type: prerelease
                    existing_changelog_path: CHANGELOG.md

    wait_for_checks:
        name: Wait for code checks to pass
        runs-on: ubuntu-latest
        steps:
            -   uses: lewagon/wait-on-check-action@v1.3.4
                with:
                    ref: ${{ github.ref }}
                    repo-token: ${{ secrets.GITHUB_TOKEN }}
                    check-regexp: (Code checks)
                    wait-interval: 5

    update_changelog:
        needs: [ release_metadata, wait_for_checks ]
        name: Update changelog
        runs-on: ubuntu-latest
        outputs:
            changelog_commitish: ${{ steps.commit.outputs.commit_long_sha || github.sha }}

        steps:
            -   name: Checkout repository
                uses: actions/checkout@v4
                with:
                    token: ${{ secrets.APIFY_SERVICE_ACCOUNT_GITHUB_TOKEN }}

            -   name: Use Node.js 22
                uses: actions/setup-node@v4
                with:
                    node-version: 22

            -   name: Update package version in package.json
                run: npm version --no-git-tag-version --allow-same-version ${{ needs.release_metadata.outputs.version_number }}

            -   name: Update CHANGELOG.md
                uses: DamianReeves/write-file-action@master
                with:
                    path: CHANGELOG.md
                    write-mode: overwrite
                    contents: ${{ needs.release_metadata.outputs.changelog }}

            -   name: Commit changes
                id: commit
                uses: EndBug/add-and-commit@v9
                with:
                    author_name: Apify Release Bot
                    author_email: noreply@apify.com
                    message: "chore(release): Update changelog and package version [skip ci]"

    publish_to_npm:
        name: Publish to NPM
        needs: [ release_metadata, wait_for_checks ]
        runs-on: ubuntu-latest
        steps:
            -   uses: actions/checkout@v4
                with:
                    ref: ${{ needs.update_changelog.changelog_commitish }}
            -   name: Use Node.js 22
                uses: actions/setup-node@v4
                with:
                    node-version: 22
                    cache: 'npm'
                    cache-dependency-path: 'package-lock.json'
            -   name: Install dependencies
                run: |
                    echo "access=public" >> .npmrc
                    echo "//registry.npmjs.org/:_authToken=${NPM_TOKEN}" >> .npmrc
                    npm ci
            - # Check version consistency and increment pre-release version number for beta only.
                name: Bump pre-release version
                run: node ./.github/scripts/before-beta-release.cjs
            -   name: Build module
                run: npm run build
            -   name: Publish to NPM
                run: npm publish --tag beta

env:
    NODE_AUTH_TOKEN: ${{ secrets.APIFY_SERVICE_ACCOUNT_NPM_TOKEN }}
    NPM_TOKEN: ${{ secrets.APIFY_SERVICE_ACCOUNT_NPM_TOKEN }}
</file>

<file path=".github/workflows/release.yaml">
name: Create a release

on:
    # Trigger a stable version release via GitHub's UI, with the ability to specify the type of release.
    workflow_dispatch:
        inputs:
            release_type:
                description: Release type
                required: true
                type: choice
                default: auto
                options:
                    - auto
                    - custom
                    - patch
                    - minor
                    - major
            custom_version:
                description: The custom version to bump to (only for "custom" type)
                required: false
                type: string
                default: ""

concurrency:
    group: release
    cancel-in-progress: false

jobs:
    release_metadata:
        name: Prepare release metadata
        runs-on: ubuntu-latest
        outputs:
            version_number: ${{ steps.release_metadata.outputs.version_number }}
            tag_name: ${{ steps.release_metadata.outputs.tag_name }}
            changelog: ${{ steps.release_metadata.outputs.changelog }}
            release_notes: ${{ steps.release_metadata.outputs.release_notes }}
        steps:
            -   uses: apify/workflows/git-cliff-release@main
                name: Prepare release metadata
                id: release_metadata
                with:
                    release_type: ${{ inputs.release_type }}
                    custom_version: ${{ inputs.custom_version }}
                    existing_changelog_path: CHANGELOG.md

    update_changelog:
        needs: [ release_metadata ]
        name: Update changelog
        runs-on: ubuntu-latest
        outputs:
            changelog_commitish: ${{ steps.commit.outputs.commit_long_sha || github.sha }}

        steps:
            -   name: Checkout repository
                uses: actions/checkout@v4
                with:
                    token: ${{ secrets.APIFY_SERVICE_ACCOUNT_GITHUB_TOKEN }}

            -   name: Use Node.js 22
                uses: actions/setup-node@v4
                with:
                    node-version: 22

            -   name: Update package version in package.json
                run: npm version --no-git-tag-version --allow-same-version ${{ needs.release_metadata.outputs.version_number }}

            -   name: Update CHANGELOG.md
                uses: DamianReeves/write-file-action@master
                with:
                    path: CHANGELOG.md
                    write-mode: overwrite
                    contents: ${{ needs.release_metadata.outputs.changelog }}

            -   name: Commit changes
                id: commit
                uses: EndBug/add-and-commit@v9
                with:
                    author_name: Apify Release Bot
                    author_email: noreply@apify.com
                    message: "chore(release): Update changelog and package version [skip ci]"

    create_github_release:
        name: Create github release
        needs: [release_metadata, update_changelog]
        runs-on: ubuntu-latest
        env:
            GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        steps:
            -   name: Create release
                uses: softprops/action-gh-release@v2
                with:
                    tag_name: ${{ needs.release_metadata.outputs.tag_name }}
                    name: ${{ needs.release_metadata.outputs.version_number }}
                    target_commitish: ${{ needs.update_changelog.outputs.changelog_commitish }}
                    body: ${{ needs.release_metadata.outputs.release_notes }}

    publish_to_npm:
        name: Publish to NPM
        needs: [ update_changelog ]
        runs-on: ubuntu-latest
        steps:
            -   uses: actions/checkout@v4
                with:
                    ref: ${{ needs.update_changelog.changelog_commitish }}
            -   name: Use Node.js 22
                uses: actions/setup-node@v4
                with:
                    node-version: 22
                    cache: 'npm'
                    cache-dependency-path: 'package-lock.json'
            -   name: Install dependencies
                run: |
                    echo "access=public" >> .npmrc
                    echo "//registry.npmjs.org/:_authToken=${NPM_TOKEN}" >> .npmrc
                    npm ci
            -   name: Build module
                run: npm run build
            -   name: Publish to NPM
                run: npm publish --tag latest

env:
    NODE_AUTH_TOKEN: ${{ secrets.APIFY_SERVICE_ACCOUNT_NPM_TOKEN }}
    NPM_TOKEN: ${{ secrets.APIFY_SERVICE_ACCOUNT_NPM_TOKEN }}
</file>

<file path="src/actor/const.ts">
/**
 * Constants for the Actor.
 */
export const HEADER_READINESS_PROBE = 'x-apify-container-server-readiness-probe';

export enum Routes {
    ROOT = '/',
    MCP = '/mcp',
    SSE = '/sse',
    MESSAGE = '/message',
}

export const getHelpMessage = (host: string) => `To interact with the server you can either:
- send request to ${host}${Routes.MCP}?token=YOUR-APIFY-TOKEN and receive a response
or
- connect for Server-Sent Events (SSE) via GET request to: ${host}${Routes.SSE}?token=YOUR-APIFY-TOKEN
- send messages via POST request to: ${host}${Routes.MESSAGE}?token=YOUR-APIFY-TOKEN
  (Include your message content in the request body.)`;
</file>

<file path="src/actor/README.md">
# Actor

Code related to Apify Actor called Actors-MCP-Server.
This Actor will be deprecated in favor of Apify MCP Server, therefore we are keeping it separate from the main codebase.

The only exception is the `src/main.ts` file that also belongs to the Actor.
</file>

<file path="src/actor/server.ts">
/*
 * Express server implementation used for standby Actor mode.
 */

import { randomUUID } from 'node:crypto';

import { SSEServerTransport } from '@modelcontextprotocol/sdk/server/sse.js';
import { StreamableHTTPServerTransport } from '@modelcontextprotocol/sdk/server/streamableHttp.js';
import type { Request, Response } from 'express';
import express from 'express';

import log from '@apify/log';

import { type ActorsMcpServer } from '../mcp/server.js';
import { parseInputParamsFromUrl, processParamsGetTools } from '../mcp/utils.js';
import { getHelpMessage, HEADER_READINESS_PROBE, Routes } from './const.js';
import { getActorRunData } from './utils.js';

export function createExpressApp(
    host: string,
    mcpServer: ActorsMcpServer,
): express.Express {
    const app = express();
    let transportSSE: SSEServerTransport;
    const transports: { [sessionId: string]: StreamableHTTPServerTransport } = {};

    function respondWithError(res: Response, error: unknown, logMessage: string, statusCode = 500) {
        log.error(`${logMessage}: ${error}`);
        if (!res.headersSent) {
            res.status(statusCode).json({
                jsonrpc: '2.0',
                error: {
                    code: statusCode === 500 ? -32603 : -32000,
                    message: statusCode === 500 ? 'Internal server error' : 'Bad Request',
                },
                id: null,
            });
        }
    }

    app.get(Routes.ROOT, async (req: Request, res: Response) => {
        if (req.headers && req.get(HEADER_READINESS_PROBE) !== undefined) {
            log.debug('Received readiness probe');
            res.status(200).json({ message: 'Server is ready' }).end();
            return;
        }
        try {
            log.info(`Received GET message at: ${Routes.ROOT}`);
            // TODO: I think we should remove this logic, root should return only help message
            const tools = await processParamsGetTools(req.url, process.env.APIFY_TOKEN as string);
            if (tools) {
                mcpServer.updateTools(tools);
            }
            res.setHeader('Content-Type', 'text/event-stream');
            res.setHeader('Cache-Control', 'no-cache');
            res.setHeader('Connection', 'keep-alive');
            res.status(200).json({ message: `Actor is using Model Context Protocol. ${getHelpMessage(host)}`, data: getActorRunData() }).end();
        } catch (error) {
            respondWithError(res, error, `Error in GET ${Routes.ROOT}`);
        }
    });

    app.head(Routes.ROOT, (_req: Request, res: Response) => {
        res.status(200).end();
    });

    app.get(Routes.SSE, async (req: Request, res: Response) => {
        try {
            log.info(`Received GET message at: ${Routes.SSE}`);
            const input = parseInputParamsFromUrl(req.url);
            if (input.actors || input.enableAddingActors) {
                await mcpServer.loadToolsFromUrl(req.url, process.env.APIFY_TOKEN as string);
            }
            // Load default tools if no actors are specified
            if (!input.actors) {
                await mcpServer.loadDefaultTools(process.env.APIFY_TOKEN as string);
            }
            transportSSE = new SSEServerTransport(Routes.MESSAGE, res);
            await mcpServer.connect(transportSSE);
        } catch (error) {
            respondWithError(res, error, `Error in GET ${Routes.SSE}`);
        }
    });

    app.post(Routes.MESSAGE, async (req: Request, res: Response) => {
        try {
            log.info(`Received POST message at: ${Routes.MESSAGE}`);
            if (transportSSE) {
                await transportSSE.handlePostMessage(req, res);
            } else {
                log.error('Server is not connected to the client.');
                res.status(400).json({
                    jsonrpc: '2.0',
                    error: {
                        code: -32000,
                        message: 'Bad Request: Server is not connected to the client. '
                        + 'Connect to the server with GET request to /sse endpoint',
                    },
                    id: null,
                });
            }
        } catch (error) {
            respondWithError(res, error, `Error in POST ${Routes.MESSAGE}`);
        }
    });

    // express.json() middleware to parse JSON bodies.
    // It must be used before the POST /mcp route but after the GET /sse route :shrug:
    app.use(express.json());
    app.post(Routes.MCP, async (req: Request, res: Response) => {
        log.info('Received MCP request:', req.body);
        try {
            // Check for existing session ID
            const sessionId = req.headers['mcp-session-id'] as string | undefined;
            let transport: StreamableHTTPServerTransport;

            if (sessionId && transports[sessionId]) {
            // Reuse existing transport
                transport = transports[sessionId];
            } else if (!sessionId && isInitializeRequest(req.body)) {
            // New initialization request - use JSON response mode
                transport = new StreamableHTTPServerTransport({
                    sessionIdGenerator: () => randomUUID(),
                    enableJsonResponse: true, // Enable JSON response mode
                });
                // Load MCP server tools
                // TODO using query parameters in POST request is not standard
                const input = parseInputParamsFromUrl(req.url);
                if (input.actors || input.enableAddingActors) {
                    await mcpServer.loadToolsFromUrl(req.url, process.env.APIFY_TOKEN as string);
                }
                // Load default tools if no actors are specified
                if (!input.actors) {
                    await mcpServer.loadDefaultTools(process.env.APIFY_TOKEN as string);
                }

                // Connect the transport to the MCP server BEFORE handling the request
                await mcpServer.connect(transport);

                // After handling the request, if we get a session ID back, store the transport
                await transport.handleRequest(req, res, req.body);

                // Store the transport by session ID for future requests
                if (transport.sessionId) {
                    transports[transport.sessionId] = transport;
                }
                return; // Already handled
            } else {
            // Invalid request - no session ID or not initialization request
                res.status(400).json({
                    jsonrpc: '2.0',
                    error: {
                        code: -32000,
                        message: 'Bad Request: No valid session ID provided or not initialization request',
                    },
                    id: null,
                });
                return;
            }

            // Handle the request with existing transport - no need to reconnect
            await transport.handleRequest(req, res, req.body);
        } catch (error) {
            respondWithError(res, error, 'Error handling MCP request');
        }
    });

    // Handle GET requests for SSE streams according to spec
    app.get(Routes.MCP, async (_req: Request, res: Response) => {
        // We don't support GET requests for this server
        // The spec requires returning 405 Method Not Allowed in this case
        res.status(405).set('Allow', 'POST').send('Method Not Allowed');
    });

    // Catch-all for undefined routes
    app.use((req: Request, res: Response) => {
        res.status(404).json({ message: `There is nothing at route ${req.method} ${req.originalUrl}. ${getHelpMessage(host)}` }).end();
    });

    return app;
}

// Helper function to detect initialize requests
function isInitializeRequest(body: unknown): boolean {
    if (Array.isArray(body)) {
        return body.some((msg) => typeof msg === 'object' && msg !== null && 'method' in msg && msg.method === 'initialize');
    }
    return typeof body === 'object' && body !== null && 'method' in body && body.method === 'initialize';
}
</file>

<file path="src/actor/types.ts">
export interface ActorRunData {
    id?: string;
    actId?: string;
    userId?: string;
    startedAt?: string;
    finishedAt: null;
    status: 'RUNNING';
    meta: {
        origin?: string;
    };
    options: {
        build?: string;
        memoryMbytes?: string;
    };
    buildId?: string;
    defaultKeyValueStoreId?: string;
    defaultDatasetId?: string;
    defaultRequestQueueId?: string;
    buildNumber?: string;
    containerUrl?: string;
    standbyUrl?: string;
}
</file>

<file path="src/actor/utils.ts">
import { Actor } from 'apify';

import type { ActorRunData } from './types.js';

export function getActorRunData(): ActorRunData | null {
    return Actor.isAtHome() ? {
        id: process.env.ACTOR_RUN_ID,
        actId: process.env.ACTOR_ID,
        userId: process.env.APIFY_USER_ID,
        startedAt: process.env.ACTOR_STARTED_AT,
        finishedAt: null,
        status: 'RUNNING',
        meta: {
            origin: process.env.APIFY_META_ORIGIN,
        },
        options: {
            build: process.env.ACTOR_BUILD_NUMBER,
            memoryMbytes: process.env.ACTOR_MEMORY_MBYTES,
        },
        buildId: process.env.ACTOR_BUILD_ID,
        defaultKeyValueStoreId: process.env.ACTOR_DEFAULT_KEY_VALUE_STORE_ID,
        defaultDatasetId: process.env.ACTOR_DEFAULT_DATASET_ID,
        defaultRequestQueueId: process.env.ACTOR_DEFAULT_REQUEST_QUEUE_ID,
        buildNumber: process.env.ACTOR_BUILD_NUMBER,
        containerUrl: process.env.ACTOR_WEB_SERVER_URL,
        standbyUrl: process.env.ACTOR_STANDBY_URL,
    } : null;
}
</file>

<file path="src/examples/client_sse.py">
"""
Test Apify MCP Server using SSE client

It is using python client as the typescript one does not support custom headers when connecting to the SSE server.

Install python dependencies (assumes you have python installed):
> pip install requests python-dotenv mcp
"""

import asyncio
import os
from pathlib import Path

import requests
from dotenv import load_dotenv
from mcp.client.session import ClientSession
from mcp.client.sse import sse_client

load_dotenv(Path(__file__).resolve().parent.parent.parent / ".env")

MCP_SERVER_URL = "https://actors-mcp-server.apify.actor"

HEADERS = {"Authorization": f"Bearer {os.getenv('APIFY_TOKEN')}"}

async def run() -> None:

    print("Start MCP Server with Actors")
    r = requests.get(MCP_SERVER_URL, headers=HEADERS)
    print("MCP Server Response:", r.json(), end="\n\n")

    async with sse_client(url=f"{MCP_SERVER_URL}/sse", timeout=60, headers=HEADERS) as (read, write):
        async with ClientSession(read, write) as session:
            await session.initialize()

            tools = await session.list_tools()
            print("Available Tools:", tools, end="\n\n")
            for tool in tools.tools:
                print(f"\n### Tool name ###: {tool.name}")
                print(f"\tdescription: {tool.description}")
                print(f"\tinputSchema: {tool.inputSchema}")

            if hasattr(tools, "tools") and not tools.tools:
                print("No tools available!")
                return

            print("\n\nCall tool")
            result = await session.call_tool("apify/rag-web-browser", { "query": "example.com", "maxResults": 3 })
            print("Tools call result:")

            for content in result.content:
                print(content)

asyncio.run(run())
</file>

<file path="src/examples/clientSse.ts">
/* eslint-disable no-console */
/**
 * Connect to the MCP server using SSE transport and call a tool.
 * The Actors MCP Server will load default Actors.
 *
 * It requires the `APIFY_TOKEN` in the `.env` file.
 */

import path from 'node:path';
import { fileURLToPath } from 'node:url';

import { Client } from '@modelcontextprotocol/sdk/client/index.js';
import { SSEClientTransport } from '@modelcontextprotocol/sdk/client/sse.js';
import { CallToolResultSchema } from '@modelcontextprotocol/sdk/types.js';
import dotenv from 'dotenv'; // eslint-disable-line import/no-extraneous-dependencies
import type { EventSourceInit } from 'eventsource';
import { EventSource } from 'eventsource'; // eslint-disable-line import/no-extraneous-dependencies

import { actorNameToToolName } from '../tools/utils.js';

const REQUEST_TIMEOUT = 120_000; // 2 minutes
const filename = fileURLToPath(import.meta.url);
const dirname = path.dirname(filename);

dotenv.config({ path: path.resolve(dirname, '../../.env') });

const SERVER_URL = process.env.MCP_SERVER_URL_BASE || 'https://actors-mcp-server.apify.actor/sse';
// We need to change forward slash / to underscore -- in the tool name as Anthropic does not allow forward slashes in the tool name
const SELECTED_TOOL = actorNameToToolName('apify/rag-web-browser');
// const QUERY = 'web browser for Anthropic';
const QUERY = 'apify';

if (!process.env.APIFY_TOKEN) {
    console.error('APIFY_TOKEN is required but not set in the environment variables.');
    process.exit(1);
}

// Declare EventSource on globalThis if not available (needed for Node.js environment)
declare global {

    // eslint-disable-next-line no-var, vars-on-top
    var EventSource: {
        new(url: string, eventSourceInitDict?: EventSourceInit): EventSource;
        prototype: EventSource;
        CONNECTING: 0;
        OPEN: 1;
        CLOSED: 2;
    };
}

if (typeof globalThis.EventSource === 'undefined') {
    globalThis.EventSource = EventSource;
}

async function main(): Promise<void> {
    const transport = new SSEClientTransport(
        new URL(SERVER_URL),
        {
            requestInit: {
                headers: {
                    authorization: `Bearer ${process.env.APIFY_TOKEN}`,
                },
            },
            eventSourceInit: {
                // The EventSource package augments EventSourceInit with a "fetch" parameter.
                // You can use this to set additional headers on the outgoing request.
                // Based on this example: https://github.com/modelcontextprotocol/typescript-sdk/issues/118
                async fetch(input: Request | URL | string, init?: RequestInit) {
                    const headers = new Headers(init?.headers || {});
                    headers.set('authorization', `Bearer ${process.env.APIFY_TOKEN}`);
                    return fetch(input, { ...init, headers });
                },
            // We have to cast to "any" to use it, since it's non-standard
            } as any, // eslint-disable-line @typescript-eslint/no-explicit-any
        },
    );
    const client = new Client(
        { name: 'example-client', version: '1.0.0' },
        { capabilities: {} },
    );

    try {
        // Connect to the MCP server
        await client.connect(transport);

        // List available tools
        const tools = await client.listTools();
        console.log('Available tools:', tools);

        if (tools.tools.length === 0) {
            console.log('No tools available');
            return;
        }

        const selectedTool = tools.tools.find((tool) => tool.name === SELECTED_TOOL);
        if (!selectedTool) {
            console.error(`The specified tool: ${selectedTool} is not available. Exiting.`);
            return;
        }

        // Call a tool
        console.log(`Calling actor ... ${SELECTED_TOOL}`);
        const result = await client.callTool(
            { name: SELECTED_TOOL, arguments: { query: QUERY } },
            CallToolResultSchema,
            { timeout: REQUEST_TIMEOUT },
        );
        console.log('Tool result:', JSON.stringify(result, null, 2));
    } catch (error: unknown) {
        if (error instanceof Error) {
            console.error('Error:', error.message);
        } else {
            console.error('An unknown error occurred:', error);
        }
    } finally {
        await client.close();
    }
}

await main();
</file>

<file path="src/examples/clientStdio.ts">
/* eslint-disable no-console */
/**
 * Connect to the MCP server using stdio transport and call a tool.
 * This script uses a selected tool without LLM involvement.
 * You need to provide the path to the MCP server and `APIFY_TOKEN` in the `.env` file.
 * You can choose actors to run in the server, for example: `apify/rag-web-browser`.
 */

import { execSync } from 'node:child_process';
import path from 'node:path';
import { fileURLToPath } from 'node:url';

import { Client } from '@modelcontextprotocol/sdk/client/index.js';
import { StdioClientTransport } from '@modelcontextprotocol/sdk/client/stdio.js';
import { CallToolResultSchema } from '@modelcontextprotocol/sdk/types.js';
import dotenv from 'dotenv'; // eslint-disable-line import/no-extraneous-dependencies

import { actorNameToToolName } from '../tools/utils.js';

// Resolve dirname equivalent in ES module
const filename = fileURLToPath(import.meta.url);
const dirname = path.dirname(filename);

dotenv.config({ path: path.resolve(dirname, '../../.env') });
const SERVER_PATH = path.resolve(dirname, '../../dist/stdio.js');
const NODE_PATH = execSync(process.platform === 'win32' ? 'where node' : 'which node').toString().trim();

const TOOLS = 'apify/rag-web-browser,lukaskrivka/google-maps-with-contact-details';
const SELECTED_TOOL = actorNameToToolName('apify/rag-web-browser');

if (!process.env.APIFY_TOKEN) {
    console.error('APIFY_TOKEN is required but not set in the environment variables.');
    process.exit(1);
}

// Create server parameters for stdio connection
const transport = new StdioClientTransport({
    command: NODE_PATH,
    args: [SERVER_PATH, '--actors', TOOLS],
    env: { APIFY_TOKEN: process.env.APIFY_TOKEN || '' },
});

// Create a new client instance
const client = new Client(
    { name: 'example-client', version: '0.1.0' },
    { capabilities: {} },
);

// Main function to run the example client
async function run() {
    try {
        // Connect to the MCP server
        await client.connect(transport);

        // List available tools
        const tools = await client.listTools();
        console.log('Available tools:', tools);

        if (tools.tools.length === 0) {
            console.log('No tools available');
            return;
        }

        // Example: Call the first available tool
        const selectedTool = tools.tools.find((tool) => tool.name === SELECTED_TOOL);

        if (!selectedTool) {
            console.error(`The specified tool: ${selectedTool} is not available. Exiting.`);
            return;
        }

        // Call a tool
        console.log('Calling actor ...');
        const result = await client.callTool(
            { name: SELECTED_TOOL, arguments: { query: 'web browser for Anthropic' } },
            CallToolResultSchema,
        );
        console.log('Tool result:', JSON.stringify(result));

        await client.close();
    } catch (error) {
        console.error('Error:', error);
    }
}

run().catch((error) => {
    console.error(`Error running MCP client: ${error as Error}`);
    process.exit(1);
});
</file>

<file path="src/examples/clientStdioChat.ts">
/* eslint-disable no-console */
/**
 * Create a simple chat client that connects to the Model Context Protocol server using the stdio transport.
 * Based on the user input, the client sends a query to the MCP server, retrieves results and processes them.
 *
 * You can expect the following output:
 *
 * MCP Client Started!
 * Type your queries or 'quit|q|exit' to exit.
 * You: Find to articles about AI agent and return URLs
 * [internal] Received response from Claude: [{"type":"text","text":"I'll search for information about AI agents
 *   and provide you with a summary."},{"type":"tool_use","id":"tool_01He9TkzQfh2979bbeuxWVqM","name":"search",
 *   "input":{"query":"what are AI agents definition capabilities applications","maxResults":2}}]
 * [internal] Calling tool: {"name":"search","arguments":{"query":"what are AI agents definition ...
 * I can help analyze the provided content about AI agents.
 * This appears to be crawled content from AWS and IBM websites explaining what AI agents are.
 * Let me summarize the key points:
 */

import { execSync } from 'node:child_process';
import path from 'node:path';
import * as readline from 'node:readline';
import { fileURLToPath } from 'node:url';

import { Anthropic } from '@anthropic-ai/sdk'; // eslint-disable-line import/no-extraneous-dependencies
import type { Message, MessageParam, ToolUseBlock } from '@anthropic-ai/sdk/resources/messages';
import { Client } from '@modelcontextprotocol/sdk/client/index.js';
import { StdioClientTransport } from '@modelcontextprotocol/sdk/client/stdio.js';
import { CallToolResultSchema } from '@modelcontextprotocol/sdk/types.js';
import dotenv from 'dotenv'; // eslint-disable-line import/no-extraneous-dependencies

const filename = fileURLToPath(import.meta.url);
const dirname = path.dirname(filename);

dotenv.config({ path: path.resolve(dirname, '../../.env') });

const REQUEST_TIMEOUT = 120_000; // 2 minutes
const MAX_TOKENS = 2048; // Maximum tokens for Claude response

// const CLAUDE_MODEL = 'claude-3-5-sonnet-20241022'; // the most intelligent model
// const CLAUDE_MODEL = 'claude-3-5-haiku-20241022'; // a fastest model
const CLAUDE_MODEL = 'claude-3-haiku-20240307'; // a fastest and most compact model for near-instant responsiveness
const DEBUG = true;
const DEBUG_SERVER_PATH = path.resolve(dirname, '../../dist/stdio.js');

const NODE_PATH = execSync('which node').toString().trim();

dotenv.config(); // Load environment variables from .env

export type Tool = {
    name: string;
    description: string | undefined;
    input_schema: unknown;
}

class MCPClient {
    private anthropic: Anthropic;
    private client = new Client(
        {
            name: 'example-client',
            version: '0.1.0',
        },
        {
            capabilities: {}, // Optional capabilities
        },
    );

    private tools: Tool[] = [];

    constructor() {
        this.anthropic = new Anthropic({ apiKey: process.env.ANTHROPIC_API_KEY });
    }

    /**
     * Start the server using node and provided server script path.
     * Connect to the server using stdio transport and list available tools.
     */
    async connectToServer(serverArgs: string[]) {
        const transport = new StdioClientTransport({
            command: NODE_PATH,
            args: serverArgs,
            env: { APIFY_TOKEN: process.env.APIFY_TOKEN || '' },
        });

        await this.client.connect(transport);
        const response = await this.client.listTools();

        this.tools = response.tools.map((x) => ({
            name: x.name,
            description: x.description,
            input_schema: x.inputSchema,
        }));
        console.log('Connected to server with tools:', this.tools.map((x) => x.name));
    }

    /**
     * Process LLM response and check whether it contains any tool calls.
     * If a tool call is found, call the tool and return the response and save the results to messages with type: user.
     * If the tools response is too large, truncate it to the limit.
     */
    async processMsg(response: Message, messages: MessageParam[]): Promise<MessageParam[]> {
        for (const content of response.content) {
            if (content.type === 'text') {
                messages.push({ role: 'assistant', content: content.text });
            } else if (content.type === 'tool_use') {
                await this.handleToolCall(content, messages);
            }
        }
        return messages;
    }

    /**
     * Call the tool and return the response.
     */
    private async handleToolCall(content: ToolUseBlock, messages: MessageParam[], toolCallCount = 0): Promise<MessageParam[]> {
        // eslint-disable-next-line @typescript-eslint/no-explicit-any
        const params = { name: content.name, arguments: content.input as any };
        console.log(`[internal] Calling tool (count: ${toolCallCount}): ${JSON.stringify(params)}`);
        let results;
        try {
            results = await this.client.callTool(params, CallToolResultSchema, { timeout: REQUEST_TIMEOUT });
            if (results.content instanceof Array && results.content.length !== 0) {
                const text = results.content.map((x) => x.text);
                messages.push({ role: 'user', content: `Tool result: ${text.join('\n\n')}` });
            } else {
                messages.push({ role: 'user', content: `No results retrieved from ${params.name}` });
            }
        } catch (error) {
            messages.push({ role: 'user', content: `Error calling tool: ${params.name}, error: ${error}` });
        }
        // Get next response from Claude
        const nextResponse: Message = await this.anthropic.messages.create({
            model: CLAUDE_MODEL,
            max_tokens: MAX_TOKENS,
            messages,
            tools: this.tools as any[], // eslint-disable-line @typescript-eslint/no-explicit-any
        });

        for (const c of nextResponse.content) {
            if (c.type === 'text') {
                messages.push({ role: 'assistant', content: c.text });
            } else if (c.type === 'tool_use' && toolCallCount < 3) {
                return await this.handleToolCall(c, messages, toolCallCount + 1);
            }
        }

        return messages;
    }

    /**
     * Process user query by sending it to the server and returning the response.
     * Also, process any tool calls.
     */
    async processQuery(query: string, messages: MessageParam[]): Promise<MessageParam[]> {
        messages.push({ role: 'user', content: query });
        const response: Message = await this.anthropic.messages.create({
            model: CLAUDE_MODEL,
            max_tokens: MAX_TOKENS,
            messages,
            tools: this.tools as any[], // eslint-disable-line @typescript-eslint/no-explicit-any
        });
        console.log('[internal] Received response from Claude:', JSON.stringify(response.content));
        return await this.processMsg(response, messages);
    }

    /**
     * Create a chat loop that reads user input from the console and sends it to the server for processing.
     */
    async chatLoop() {
        const rl = readline.createInterface({
            input: process.stdin,
            output: process.stdout,
            prompt: 'You: ',
        });

        console.log("MCP Client Started!\nType your queries or 'quit|q|exit' to exit.");
        rl.prompt();

        let lastPrintMessage = 0;
        const messages: MessageParam[] = [];
        rl.on('line', async (input) => {
            const v = input.trim().toLowerCase();
            if (v === 'quit' || v === 'q' || v === 'exit') {
                rl.close();
                return;
            }
            try {
                await this.processQuery(input, messages);
                for (let i = lastPrintMessage + 1; i < messages.length; i++) {
                    if (messages[i].role === 'assistant') {
                        console.log('CLAUDE:', messages[i].content);
                    } else if (messages[i].role === 'user') {
                        console.log('USER:', messages[i].content.slice(0, 500), '...');
                    } else {
                        console.log('CLAUDE[thinking]:', messages[i].content);
                    }
                }
                lastPrintMessage += messages.length;
            } catch (error) {
                console.error('Error processing query:', error);
            }
            rl.prompt();
        });
    }
}

async function main() {
    const client = new MCPClient();

    if (process.argv.length < 3) {
        if (DEBUG) {
            process.argv.push(DEBUG_SERVER_PATH);
        } else {
            console.error('Usage: node <path_to_server_script>');
            process.exit(1);
        }
    }

    try {
        await client.connectToServer(process.argv.slice(2));
        await client.chatLoop();
    } catch (error) {
        console.error('Error:', error);
    }
}

main().catch(console.error);
</file>

<file path="src/examples/clientStreamableHttp.ts">
import { Client } from '@modelcontextprotocol/sdk/client/index.js';
import { StreamableHTTPClientTransport } from '@modelcontextprotocol/sdk/client/streamableHttp.js';
import type { CallToolRequest, ListToolsRequest } from '@modelcontextprotocol/sdk/types.js';
import {
    CallToolResultSchema,
    ListToolsResultSchema,
    LoggingMessageNotificationSchema,
} from '@modelcontextprotocol/sdk/types.js';

import log from '@apify/log';

import { HelperTools } from '../const.js';

log.setLevel(log.LEVELS.DEBUG);

async function main(): Promise<void> {
    // Create a new client with streamable HTTP transport
    const client = new Client({
        name: 'example-client',
        version: '1.0.0',
    });

    const transport = new StreamableHTTPClientTransport(
        new URL('http://localhost:3000/mcp'),
    );

    // Connect the client using the transport and initialize the server
    await client.connect(transport);
    log.debug('Connected to MCP server');

    // Set up notification handlers for server-initiated messages
    client.setNotificationHandler(LoggingMessageNotificationSchema, (notification) => {
        log.debug(`Notification received: ${notification.params.level} - ${notification.params.data}`);
    });

    // List and call tools
    await listTools(client);

    await callSearchTool(client);
    await callActor(client);

    // Keep the connection open to receive notifications
    log.debug('\nKeeping connection open to receive notifications. Press Ctrl+C to exit.');
}

async function listTools(client: Client): Promise<void> {
    try {
        const toolsRequest: ListToolsRequest = {
            method: 'tools/list',
            params: {},
        };
        const toolsResult = await client.request(toolsRequest, ListToolsResultSchema);
        log.debug(`Tools available, count: ${toolsResult.tools.length}`);
        for (const tool of toolsResult.tools) {
            log.debug(`Tool: ${tool.name}, Description: ${tool.description}`);
        }
        if (toolsResult.tools.length === 0) {
            log.debug('No tools available from the server');
        }
    } catch (error) {
        log.error(`Tools not supported by this server (${error})`);
    }
}

async function callSearchTool(client: Client): Promise<void> {
    try {
        const searchRequest: CallToolRequest = {
            method: 'tools/call',
            params: {
                name: HelperTools.SEARCH_ACTORS,
                arguments: { search: 'rag web browser', limit: 1 },
            },
        };
        const searchResult = await client.request(searchRequest, CallToolResultSchema);
        log.debug('Search result:');
        searchResult.content.forEach((item) => {
            if (item.type === 'text') {
                log.debug(`\t${item.text}`);
            }
        });
    } catch (error) {
        log.error(`Error calling greet tool: ${error}`);
    }
}

async function callActor(client: Client): Promise<void> {
    try {
        log.debug('\nCalling Actor...');
        const actorRequest: CallToolRequest = {
            method: 'tools/call',
            params: {
                name: 'apify/rag-web-browser',
                arguments: { query: 'apify mcp server' },
            },
        };
        const actorResult = await client.request(actorRequest, CallToolResultSchema);
        log.debug('Actor results:');
        actorResult.content.forEach((item) => {
            if (item.type === 'text') {
                log.debug(`- ${item.text}`);
            }
        });
    } catch (error) {
        log.error(`Error calling Actor: ${error}`);
    }
}

main().catch((error: unknown) => {
    log.error(`Error running MCP client: ${error as Error}`);
    process.exit(1);
});
</file>

<file path="src/mcp/actors.ts">
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
</file>

<file path="src/mcp/client.ts">
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
</file>

<file path="src/mcp/const.ts">
export const MAX_TOOL_NAME_LENGTH = 64;
export const SERVER_ID_LENGTH = 8;
export const EXTERNAL_TOOL_CALL_TIMEOUT_MSEC = 120_000; // 2 minutes
</file>

<file path="src/mcp/proxy.ts">
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
</file>

<file path="src/mcp/server.ts">
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
</file>

<file path="src/mcp/utils.ts">
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
</file>

<file path="src/tools/actor.ts">
import type { Client } from '@modelcontextprotocol/sdk/client/index.js';
import { Ajv } from 'ajv';
import type { ActorCallOptions } from 'apify-client';

import log from '@apify/log';

import { ApifyClient } from '../apify-client.js';
import { ACTOR_ADDITIONAL_INSTRUCTIONS, ACTOR_MAX_MEMORY_MBYTES } from '../const.js';
import { getActorsMCPServerURL, isActorMCPServer } from '../mcp/actors.js';
import { createMCPClient } from '../mcp/client.js';
import { getMCPServerTools } from '../mcp/proxy.js';
import type { ToolWrap } from '../types.js';
import { getActorDefinition } from './build.js';
import {
    actorNameToToolName,
    addEnumsToDescriptionsWithExamples,
    buildNestedProperties,
    filterSchemaProperties,
    markInputPropertiesAsRequired,
    shortenProperties,
} from './utils.js';

/**
 * Calls an Apify actor and retrieves the dataset items.
 *
 *
 * It requires the `APIFY_TOKEN` environment variable to be set.
 * If the `APIFY_IS_AT_HOME` the dataset items are pushed to the Apify dataset.
 *
 * @param {string} actorName - The name of the actor to call.
 * @param {ActorCallOptions} callOptions - The options to pass to the actor.
 * @param {unknown} input - The input to pass to the actor.
 * @param {string} apifyToken - The Apify token to use for authentication.
 * @returns {Promise<object[]>} - A promise that resolves to an array of dataset items.
 * @throws {Error} - Throws an error if the `APIFY_TOKEN` is not set
 */
export async function callActorGetDataset(
    actorName: string,
    input: unknown,
    apifyToken: string,
    callOptions: ActorCallOptions | undefined = undefined,
): Promise<object[]> {
    const name = actorName;
    try {
        log.info(`Calling Actor ${name} with input: ${JSON.stringify(input)}`);

        const client = new ApifyClient({ token: apifyToken });
        const actorClient = client.actor(name);

        const results = await actorClient.call(input, callOptions);
        const dataset = await client.dataset(results.defaultDatasetId).listItems();
        log.info(`Actor ${name} finished with ${dataset.items.length} items`);

        return dataset.items;
    } catch (error) {
        log.error(`Error calling actor: ${error}. Actor: ${name}, input: ${JSON.stringify(input)}`);
        throw new Error(`Error calling Actor: ${error}`);
    }
}

/**
 * This function is used to fetch normal non-MCP server Actors as a tool.
 *
 * Fetches actor input schemas by Actor IDs or Actor full names and creates MCP tools.
 *
 * This function retrieves the input schemas for the specified actors and compiles them into MCP tools.
 * It uses the AJV library to validate the input schemas.
 *
 * Tool name can't contain /, so it is replaced with _
 *
 * The input schema processing workflow:
 * 1. Properties are marked as required using markInputPropertiesAsRequired() to add "REQUIRED" prefix to descriptions
 * 2. Nested properties are built by analyzing editor type (proxy, requestListSources) using buildNestedProperties()
 * 3. Properties are filtered using filterSchemaProperties()
 * 4. Properties are shortened using shortenProperties()
 * 5. Enums are added to descriptions with examples using addEnumsToDescriptionsWithExamples()
 *
 * @param {string[]} actors - An array of actor IDs or Actor full names.
 * @returns {Promise<Tool[]>} - A promise that resolves to an array of MCP tools.
 */
export async function getNormalActorsAsTools(
    actors: string[],
    apifyToken: string,
): Promise<ToolWrap[]> {
    const ajv = new Ajv({ coerceTypes: 'array', strict: false });
    const getActorDefinitionWithToken = async (actorId: string) => {
        const actor = await getActorDefinition(actorId, apifyToken);
        return actor;
    };
    const results = await Promise.all(actors.map(getActorDefinitionWithToken));
    const tools: ToolWrap[] = [];
    for (const result of results) {
        if (result) {
            if (result.input && 'properties' in result.input && result.input) {
                result.input.properties = markInputPropertiesAsRequired(result.input);
                result.input.properties = buildNestedProperties(result.input.properties);
                result.input.properties = filterSchemaProperties(result.input.properties);
                result.input.properties = shortenProperties(result.input.properties);
                result.input.properties = addEnumsToDescriptionsWithExamples(result.input.properties);
            }
            try {
                const memoryMbytes = result.defaultRunOptions?.memoryMbytes || ACTOR_MAX_MEMORY_MBYTES;
                tools.push({
                    type: 'actor',
                    tool: {
                        name: actorNameToToolName(result.actorFullName),
                        actorFullName: result.actorFullName,
                        description: `${result.description} Instructions: ${ACTOR_ADDITIONAL_INSTRUCTIONS}`,
                        inputSchema: result.input || {},
                        ajvValidate: ajv.compile(result.input || {}),
                        memoryMbytes: memoryMbytes > ACTOR_MAX_MEMORY_MBYTES ? ACTOR_MAX_MEMORY_MBYTES : memoryMbytes,
                    },
                });
            } catch (validationError) {
                log.error(`Failed to compile AJV schema for Actor: ${result.actorFullName}. Error: ${validationError}`);
            }
        }
    }
    return tools;
}

async function getMCPServersAsTools(
    actors: string[],
    apifyToken: string,
): Promise<ToolWrap[]> {
    const actorsMCPServerTools: ToolWrap[] = [];
    for (const actorID of actors) {
        const serverUrl = await getActorsMCPServerURL(actorID, apifyToken);
        log.info(`ActorID: ${actorID} MCP server URL: ${serverUrl}`);

        let client: Client | undefined;
        try {
            client = await createMCPClient(serverUrl, apifyToken);
            const serverTools = await getMCPServerTools(actorID, client, serverUrl);
            actorsMCPServerTools.push(...serverTools);
        } finally {
            if (client) await client.close();
        }
    }

    return actorsMCPServerTools;
}

export async function getActorsAsTools(
    actors: string[],
    apifyToken: string,
): Promise<ToolWrap[]> {
    log.debug(`Fetching actors as tools...`);
    log.debug(`Actors: ${actors}`);
    // Actorized MCP servers
    const actorsMCPServers: string[] = [];
    for (const actorID of actors) {
        // TODO: rework, we are fetching actor definition from API twice - in the getMCPServerTools
        if (await isActorMCPServer(actorID, apifyToken)) {
            actorsMCPServers.push(actorID);
        }
    }
    // Normal Actors as a tool
    const toolActors = actors.filter((actorID) => !actorsMCPServers.includes(actorID));
    log.debug(`actorsMCPserver: ${actorsMCPServers}`);
    log.debug(`toolActors: ${toolActors}`);

    // Normal Actors as a tool
    const normalTools = await getNormalActorsAsTools(toolActors, apifyToken);

    // Tools from Actorized MCP servers
    const mcpServerTools = await getMCPServersAsTools(actorsMCPServers, apifyToken);

    return [...normalTools, ...mcpServerTools];
}
</file>

<file path="src/tools/build.ts">
import { Ajv } from 'ajv';
import { z } from 'zod';
import zodToJsonSchema from 'zod-to-json-schema';

import log from '@apify/log';

import { ApifyClient } from '../apify-client.js';
import { ACTOR_README_MAX_LENGTH, HelperTools } from '../const.js';
import type {
    ActorDefinitionPruned,
    ActorDefinitionWithDesc,
    InternalTool,
    ISchemaProperties,
    ToolWrap,
} from '../types.js';
import { filterSchemaProperties, shortenProperties } from './utils.js';

const ajv = new Ajv({ coerceTypes: 'array', strict: false });

/**
 * Get Actor input schema by Actor name.
 * First, fetch the Actor details to get the default build tag and buildId.
 * Then, fetch the build details and return actorName, description, and input schema.
 * @param {string} actorIdOrName - Actor ID or Actor full name.
 * @param {number} limit - Truncate the README to this limit.
 * @param {string} apifyToken
 * @returns {Promise<ActorDefinitionWithDesc | null>} - The actor definition with description or null if not found.
 */
export async function getActorDefinition(
    actorIdOrName: string,
    apifyToken: string,
    limit: number = ACTOR_README_MAX_LENGTH,
): Promise<ActorDefinitionPruned | null> {
    const client = new ApifyClient({ token: apifyToken });
    const actorClient = client.actor(actorIdOrName);
    try {
        // Fetch actor details
        const actor = await actorClient.get();
        if (!actor) {
            log.error(`Failed to fetch input schema for Actor: ${actorIdOrName}. Actor not found.`);
            return null;
        }

        // fnesveda: The default build is not necessarily tagged, you can specify any build number as default build.
        // There will be a new API endpoint to fetch a default build.
        // For now, we'll use the tagged build, it will work for 90% of Actors. Later, we can update this.
        const tag = actor.defaultRunOptions?.build || '';
        const buildId = actor.taggedBuilds?.[tag]?.buildId || '';

        if (!buildId) {
            log.error(`Failed to fetch input schema for Actor: ${actorIdOrName}. Build ID not found.`);
            return null;
        }
        // Fetch build details and return the input schema
        const buildDetails = await client.build(buildId).get();
        if (buildDetails?.actorDefinition) {
            const actorDefinitions = buildDetails?.actorDefinition as ActorDefinitionWithDesc;
            actorDefinitions.id = actor.id;
            actorDefinitions.readme = truncateActorReadme(actorDefinitions.readme || '', limit);
            actorDefinitions.description = actor.description || '';
            actorDefinitions.actorFullName = `${actor.username}/${actor.name}`;
            actorDefinitions.defaultRunOptions = actor.defaultRunOptions;
            return pruneActorDefinition(actorDefinitions);
        }
        return null;
    } catch (error) {
        const errorMessage = `Failed to fetch input schema for Actor: ${actorIdOrName} with error ${error}.`;
        log.error(errorMessage);
        throw new Error(errorMessage);
    }
}
function pruneActorDefinition(response: ActorDefinitionWithDesc): ActorDefinitionPruned {
    return {
        id: response.id,
        actorFullName: response.actorFullName || '',
        buildTag: response?.buildTag || '',
        readme: response?.readme || '',
        input: response?.input && 'type' in response.input && 'properties' in response.input
            ? {
                ...response.input,
                type: response.input.type as string,
                properties: response.input.properties as Record<string, ISchemaProperties>,
            }
            : undefined,
        description: response.description,
        defaultRunOptions: response.defaultRunOptions,
    };
}
/** Prune Actor README if it is too long
 * If the README is too long
 * - We keep the README as it is up to the limit.
 * - After the limit, we keep heading only
 * - We add a note that the README was truncated because it was too long.
 */
function truncateActorReadme(readme: string, limit = ACTOR_README_MAX_LENGTH): string {
    if (readme.length <= limit) {
        return readme;
    }
    const readmeFirst = readme.slice(0, limit);
    const readmeRest = readme.slice(limit);
    const lines = readmeRest.split('\n');
    const prunedReadme = lines.filter((line) => line.startsWith('#'));
    return `${readmeFirst}\n\nREADME was truncated because it was too long. Remaining headers:\n${prunedReadme.join(', ')}`;
}

const GetActorDefinitionArgsSchema = z.object({
    actorName: z.string()
        .describe('Retrieve input, readme, and other details for Actor ID or Actor full name. '
            + 'Actor name is always composed from `username/name`'),
    limit: z.number()
        .int()
        .default(ACTOR_README_MAX_LENGTH)
        .describe(`Truncate the README to this limit. Default value is ${ACTOR_README_MAX_LENGTH}.`),
});

export const actorDefinitionTool: ToolWrap = {
    type: 'internal',
    tool: {
        name: HelperTools.GET_ACTOR_DETAILS,
        // TODO: remove actorFullName from internal tools
        actorFullName: HelperTools.GET_ACTOR_DETAILS,
        description: 'Get documentation, readme, input schema and other details about an Actor. '
            + 'For example, when user says, I need to know more about web crawler Actor.'
            + 'Get details for an Actor with with Actor ID or Actor full name, i.e. username/name.'
            + `Limit the length of the README if needed.`,
        inputSchema: zodToJsonSchema(GetActorDefinitionArgsSchema),
        ajvValidate: ajv.compile(zodToJsonSchema(GetActorDefinitionArgsSchema)),
        call: async (toolArgs) => {
            const { args, apifyToken } = toolArgs;

            const parsed = GetActorDefinitionArgsSchema.parse(args);
            const v = await getActorDefinition(parsed.actorName, apifyToken, parsed.limit);
            if (v && v.input && 'properties' in v.input && v.input) {
                const properties = filterSchemaProperties(v.input.properties as { [key: string]: ISchemaProperties });
                v.input.properties = shortenProperties(properties);
            }
            return { content: [{ type: 'text', text: JSON.stringify(v) }] };
        },
    } as InternalTool,
};
</file>

<file path="src/tools/helpers.ts">
import { Ajv } from 'ajv';
import { z } from 'zod';
import zodToJsonSchema from 'zod-to-json-schema';

import { HelperTools } from '../const.js';
import type { ActorTool, InternalTool, ToolWrap } from '../types';
import { getActorsAsTools } from './actor.js';
import { actorNameToToolName } from './utils.js';

const ajv = new Ajv({ coerceTypes: 'array', strict: false });

const HELP_TOOL_TEXT = `Apify MCP server help:

Note: "MCP" stands for "Model Context Protocol". The user can use the "RAG Web Browser" tool to get the content of the links mentioned in this help and present it to the user.

This MCP server can be used in the following ways:
- Locally over "STDIO".
- Remotely over "SSE" or streamable "HTTP" transport with the "Actors MCP Server Apify Actor".
- Remotely over "SSE" or streamable "HTTP" transport with "https://mcp.apify.com".

# Usage
## Locally over "STDIO"
1. The user should install the "@apify/actors-mcp-server" NPM package.
2. The user should configure the MCP client to use the MCP server. Refer to "https://github.com/apify/actors-mcp-server" or the MCP client documentation for more details (the user can specify which MCP client is being used).
The user needs to set the following environment variables:
- "APIFY_TOKEN": Apify token to authenticate with the MCP server.
If the user wants to load an Actor outside the default ones, the user needs to pass it as a CLI argument:
- "--actors <actor1,actor2,...>" // comma-separated list of Actor names, for example, "apify/rag-web-browser,apify/instagram-scraper".
If the user wants to enable the dynamic addition of Actors to the MCP server, the user needs to pass the following CLI argument:
- "--enable-adding-actors".

## Remotely over "SSE" or streamable "HTTP" transport with "Actors MCP Server Apify Actor"
1. The user should configure the MCP client to use the "Actors MCP Server Apify Actor" with:
   - "SSE" transport URL: "https://actors-mcp-server.apify.actor/sse".
   - Streamable "HTTP" transport URL: "https://actors-mcp-server.apify.actor/mcp".
2. The user needs to pass an "APIFY_TOKEN" as a URL query parameter "?token=<APIFY_TOKEN>" or set the following headers: "Authorization: Bearer <APIFY_TOKEN>".
If the user wants to load an Actor outside the default ones, the user needs to pass it as a URL query parameter:
- "?actors=<actor1,actor2,...>" // comma-separated list of Actor names, for example, "apify/rag-web-browser,apify/instagram-scraper".
If the user wants to enable the addition of Actors to the MCP server dynamically, the user needs to pass the following URL query parameter:
- "?enable-adding-actors=true".

## Remotely over "SSE" or streamable "HTTP" transport with "https://mcp.apify.com"
1. The user should configure the MCP client to use "https://mcp.apify.com" with:
   - "SSE" transport URL: "https://mcp.apify.com/sse".
   - Streamable "HTTP" transport URL: "https://mcp.apify.com/".
2. The user needs to pass an "APIFY_TOKEN" as a URL query parameter "?token=<APIFY_TOKEN>" or set the following headers: "Authorization: Bearer <APIFY_TOKEN>".
If the user wants to load an Actor outside the default ones, the user needs to pass it as a URL query parameter:
- "?actors=<actor1,actor2,...>" // comma-separated list of Actor names, for example, "apify/rag-web-browser,apify/instagram-scraper".
If the user wants to enable the addition of Actors to the MCP server dynamically, the user needs to pass the following URL query parameter:
- "?enable-adding-actors=true".

# Features
## Dynamic adding of Actors
THIS FEATURE MAY NOT BE SUPPORTED BY ALL MCP CLIENTS. THE USER MUST ENSURE THAT THE CLIENT SUPPORTS IT!
To enable this feature, see the usage section. Once dynamic adding is enabled, tools will be added that allow the user to add or remove Actors from the MCP server.
Tools related:
- "add-actor".
- "remove-actor".
If the user is using these tools and it seems like the tools have been added but cannot be called, the issue may be that the client does not support dynamic adding of Actors.
In that case, the user should check the MCP client documentation to see if the client supports this feature.
`;

export const AddToolArgsSchema = z.object({
    actorName: z.string()
        .describe('Add a tool, Actor or MCP-Server to available tools by Actor ID or tool full name.'
            + 'Tool name is always composed from `username/name`'),
});
export const addTool: ToolWrap = {
    type: 'internal',
    tool: {
        name: HelperTools.ADD_ACTOR,
        description: 'Add a tool, Actor or MCP-Server to available tools by Actor ID or Actor name. '
            + 'A tool is an Actor or MCP-Server that can be called by the user'
            + 'Do not execute the tool, only add it and list it in available tools. '
            + 'For example, add a tool with username/name when user wants to scrape data from a website.',
        inputSchema: zodToJsonSchema(AddToolArgsSchema),
        ajvValidate: ajv.compile(zodToJsonSchema(AddToolArgsSchema)),
        // TODO: I don't like that we are passing apifyMcpServer and mcpServer to the tool
        call: async (toolArgs) => {
            const { apifyMcpServer, mcpServer, apifyToken, args } = toolArgs;
            const parsed = AddToolArgsSchema.parse(args);
            const tools = await getActorsAsTools([parsed.actorName], apifyToken);
            const toolsAdded = apifyMcpServer.updateTools(tools);
            await mcpServer.notification({ method: 'notifications/tools/list_changed' });

            return {
                content: [{
                    type: 'text',
                    text: `Actor added: ${toolsAdded.map((t) => `${(t.tool as ActorTool).actorFullName} (tool name: ${t.tool.name})`).join(', ')}`,
                }],
            };
        },
    } as InternalTool,
};
export const RemoveToolArgsSchema = z.object({
    toolName: z.string()
        .describe('Tool name to remove from available tools.')
        .transform((val) => actorNameToToolName(val)),
});
export const removeTool: ToolWrap = {
    type: 'internal',
    tool: {
        name: HelperTools.REMOVE_ACTOR,
        description: 'Remove a tool, an Actor or MCP-Server by name from available tools. '
            + 'For example, when user says, I do not need a tool username/name anymore',
        inputSchema: zodToJsonSchema(RemoveToolArgsSchema),
        ajvValidate: ajv.compile(zodToJsonSchema(RemoveToolArgsSchema)),
        // TODO: I don't like that we are passing apifyMcpServer and mcpServer to the tool
        call: async (toolArgs) => {
            const { apifyMcpServer, mcpServer, args } = toolArgs;

            const parsed = RemoveToolArgsSchema.parse(args);
            apifyMcpServer.tools.delete(parsed.toolName);
            await mcpServer.notification({ method: 'notifications/tools/list_changed' });
            return { content: [{ type: 'text', text: `Tool ${parsed.toolName} was removed` }] };
        },
    } as InternalTool,
};

// Tool takes no arguments
export const HelpToolArgsSchema = z.object({});
export const helpTool: ToolWrap = {
    type: 'internal',
    tool: {
        name: HelperTools.HELP_TOOL,
        description: 'Helper tool to get information on how to use and troubleshoot the Apify MCP server. '
            + 'This tool always returns the same help message with information about the server and how to use it. '
            + 'Call this tool in case of any problems or uncertainties with the server. ',
        inputSchema: zodToJsonSchema(HelpToolArgsSchema),
        ajvValidate: ajv.compile(zodToJsonSchema(HelpToolArgsSchema)),
        call: async () => {
            return { content: [{ type: 'text', text: HELP_TOOL_TEXT }] };
        },
    } as InternalTool,
};
</file>

<file path="src/tools/index.ts">
// Import specific tools that are being used
import { callActorGetDataset, getActorsAsTools } from './actor.js';
import { actorDefinitionTool } from './build.js';
import { addTool, removeTool } from './helpers.js';
import { searchActorTool } from './store_collection.js';

// Export only the tools that are being used
export { addTool, removeTool, actorDefinitionTool, searchActorTool as searchTool, getActorsAsTools, callActorGetDataset };
</file>

<file path="src/tools/store_collection.ts">
import { Ajv } from 'ajv';
import type { ActorStoreList } from 'apify-client';
import { z } from 'zod';
import zodToJsonSchema from 'zod-to-json-schema';

import { ApifyClient } from '../apify-client.js';
import { HelperTools } from '../const.js';
import type { ActorStorePruned, HelperTool, PricingInfo, ToolWrap } from '../types.js';

function pruneActorStoreInfo(response: ActorStoreList): ActorStorePruned {
    const stats = response.stats || {};
    const pricingInfo = (response.currentPricingInfo || {}) as PricingInfo;
    return {
        id: response.id,
        name: response.name?.toString() || '',
        username: response.username?.toString() || '',
        actorFullName: `${response.username}/${response.name}`,
        title: response.title?.toString() || '',
        description: response.description?.toString() || '',
        stats: {
            totalRuns: stats.totalRuns,
            totalUsers30Days: stats.totalUsers30Days,
            publicActorRunStats30Days: 'publicActorRunStats30Days' in stats
                ? stats.publicActorRunStats30Days : {},
        },
        currentPricingInfo: {
            pricingModel: pricingInfo.pricingModel?.toString() || '',
            pricePerUnitUsd: pricingInfo?.pricePerUnitUsd ?? 0,
            trialMinutes: pricingInfo?.trialMinutes ?? 0,
        },
        url: response.url?.toString() || '',
        totalStars: 'totalStars' in response ? (response.totalStars as number) : null,
    };
}

export async function searchActorsByKeywords(
    search: string,
    apifyToken: string,
    limit: number | undefined = undefined,
    offset: number | undefined = undefined,
): Promise<ActorStorePruned[] | null> {
    const client = new ApifyClient({ token: apifyToken });
    const results = await client.store().list({ search, limit, offset });
    return results.items.map((x) => pruneActorStoreInfo(x));
}

const ajv = new Ajv({ coerceTypes: 'array', strict: false });
export const SearchToolArgsSchema = z.object({
    limit: z.number()
        .int()
        .min(1)
        .max(100)
        .default(10)
        .describe('The maximum number of Actors to return. Default value is 10.'),
    offset: z.number()
        .int()
        .min(0)
        .default(0)
        .describe('The number of elements that should be skipped at the start. Default value is 0.'),
    search: z.string()
        .default('')
        .describe('String of key words to search Actors by. '
            + 'Searches the title, name, description, username, and readme of an Actor.'
            + 'Only key word search is supported, no advanced search.'
            + 'Always prefer simple keywords over complex queries.'),
    category: z.string()
        .default('')
        .describe('Filters the results by the specified category.'),
});
export const searchActorTool: ToolWrap = {
    type: 'internal',
    tool: {
        name: HelperTools.SEARCH_ACTORS,
        actorFullName: HelperTools.SEARCH_ACTORS,
        description: `Discover available Actors or MCP-Servers in Apify Store using full text search using keywords.`
            + `Users try to discover Actors using free form query in this case search query must be converted to full text search. `
            + `Returns a list of Actors with name, description, run statistics, pricing, starts, and URL. `
            + `You perhaps need to use this tool several times to find the right Actor. `
            + `You should prefer simple keywords over complex queries. `
            + `Limit number of results returned but ensure that relevant results are returned. `
            + `This is not a general search tool, it is designed to search for Actors in Apify Store. `,
        inputSchema: zodToJsonSchema(SearchToolArgsSchema),
        ajvValidate: ajv.compile(zodToJsonSchema(SearchToolArgsSchema)),
        call: async (toolArgs) => {
            const { args, apifyToken } = toolArgs;
            const parsed = SearchToolArgsSchema.parse(args);
            const actors = await searchActorsByKeywords(
                parsed.search,
                apifyToken,
                parsed.limit,
                parsed.offset,
            );
            return { content: actors?.map((item) => ({ type: 'text', text: JSON.stringify(item) })) };
        },
    } as HelperTool,
};
</file>

<file path="src/tools/utils.ts">
import { ACTOR_ENUM_MAX_LENGTH, ACTOR_MAX_DESCRIPTION_LENGTH } from '../const.js';
import type { IActorInputSchema, ISchemaProperties } from '../types.js';

export function actorNameToToolName(actorName: string): string {
    return actorName
        .replace(/\//g, '-slash-')
        .replace(/\./g, '-dot-')
        .slice(0, 64);
}

/**
 * Builds nested properties for object types in the schema.
 *
 * Specifically handles special cases like proxy configuration and request list sources
 * by adding predefined nested properties to these object types.
 * This is necessary for the agent to correctly infer how to structure object inputs
 * when passing arguments to the Actor.
 *
 * For proxy objects (type='object', editor='proxy'), adds 'useApifyProxy' property.
 * For request list sources (type='array', editor='requestListSources'), adds URL structure to items.
 *
 * @param {Record<string, ISchemaProperties>} properties - The input schema properties
 * @returns {Record<string, ISchemaProperties>} Modified properties with nested properties
 */
export function buildNestedProperties(properties: Record<string, ISchemaProperties>): Record<string, ISchemaProperties> {
    const clonedProperties = { ...properties };

    for (const [propertyName, property] of Object.entries(clonedProperties)) {
        if (property.type === 'object' && property.editor === 'proxy') {
            clonedProperties[propertyName] = {
                ...property,
                properties: {
                    ...property.properties,
                    useApifyProxy: {
                        title: 'Use Apify Proxy',
                        type: 'boolean',
                        description: 'Whether to use Apify Proxy - ALWAYS SET TO TRUE.',
                        default: true,
                        examples: [true],
                    },
                },
                required: ['useApifyProxy'],
            };
        } else if (property.type === 'array' && property.editor === 'requestListSources') {
            clonedProperties[propertyName] = {
                ...property,
                items: {
                    ...property.items,
                    type: 'object',
                    title: 'Request list source',
                    description: 'Request list source',
                    properties: {
                        url: {
                            title: 'URL',
                            type: 'string',
                            description: 'URL of the request list source',
                        },
                    },
                },
            };
        }
    }

    return clonedProperties;
}

/**
 * Filters schema properties to include only the necessary fields.
 *
 * This is done to reduce the size of the input schema and to make it more readable.
 *
 * @param properties
 */
export function filterSchemaProperties(properties: { [key: string]: ISchemaProperties }): {
    [key: string]: ISchemaProperties
} {
    const filteredProperties: { [key: string]: ISchemaProperties } = {};
    for (const [key, property] of Object.entries(properties)) {
        filteredProperties[key] = {
            title: property.title,
            description: property.description,
            enum: property.enum,
            type: property.type,
            default: property.default,
            prefill: property.prefill,
            properties: property.properties,
            items: property.items,
            required: property.required,
        };
        if (property.type === 'array' && !property.items?.type) {
            const itemsType = inferArrayItemType(property);
            if (itemsType) {
                filteredProperties[key].items = {
                    ...filteredProperties[key].items,
                    title: filteredProperties[key].title ?? 'Item',
                    description: filteredProperties[key].description ?? 'Item',
                    type: itemsType,
                };
            }
        }
    }
    return filteredProperties;
}

/**
 * Marks input properties as required by adding a "REQUIRED" prefix to their descriptions.
 * Takes an IActorInput object and returns a modified Record of SchemaProperties.
 *
 * This is done for maximum compatibility in case where library or agent framework does not consider
 * required fields and does not handle the JSON schema properly: we are prepending this to the description
 * as a preventive measure.
 * @param {IActorInputSchema} input - Actor input object containing properties and required fields
 * @returns {Record<string, ISchemaProperties>} - Modified properties with required fields marked
 */
export function markInputPropertiesAsRequired(input: IActorInputSchema): Record<string, ISchemaProperties> {
    const { required = [], properties } = input;

    for (const property of Object.keys(properties)) {
        if (required.includes(property)) {
            properties[property] = {
                ...properties[property],
                description: `**REQUIRED** ${properties[property].description}`,
            };
        }
    }

    return properties;
}

/**
 * Helps determine the type of items in an array schema property.
 * Priority order: explicit type in items > prefill type > default value type > editor type.
 *
 * Based on JSON schema, the array needs a type, and most of the time Actor input schema does not have this, so we need to infer that.
 *
 */
export function inferArrayItemType(property: ISchemaProperties): string | null {
    return property.items?.type
        || (Array.isArray(property.prefill) && property.prefill.length > 0 && typeof property.prefill[0])
        || (Array.isArray(property.default) && property.default.length > 0 && typeof property.default[0])
        || (property.editor && getEditorItemType(property.editor))
        || null;

    function getEditorItemType(editor: string): string | null {
        const editorTypeMap: Record<string, string> = {
            requestListSources: 'object',
            stringList: 'string',
            json: 'object',
            globs: 'object',
        };
        return editorTypeMap[editor] || null;
    }
}

/**
 * Add enum values as string to property descriptions.
 *
 * This is done as a preventive measure to prevent cases where library or agent framework
 * does not handle enums or examples based on JSON schema definition.
 *
 * https://json-schema.org/understanding-json-schema/reference/enum
 * https://json-schema.org/understanding-json-schema/reference/annotations
 *
 * @param properties
 */
export function addEnumsToDescriptionsWithExamples(properties: Record<string, ISchemaProperties>): Record<string, ISchemaProperties> {
    for (const property of Object.values(properties)) {
        if (property.enum && property.enum.length > 0) {
            property.description = `${property.description}\nPossible values: ${property.enum.slice(0, 20).join(',')}`;
        }
        const value = property.prefill ?? property.default;
        if (value && !(Array.isArray(value) && value.length === 0)) {
            property.examples = Array.isArray(value) ? value : [value];
            property.description = `${property.description}\nExample values: ${JSON.stringify(value)}`;
        }
    }
    return properties;
}

/**
 * Helper function to shorten the enum list if it is too long.
 *
 * @param {string[]} enumList - The list of enum values to be shortened.
 * @returns {string[] | undefined} - The shortened enum list or undefined if the list is too long.
 */
export function shortenEnum(enumList: string[]): string[] | undefined {
    let charCount = 0;
    const resultEnumList = enumList.filter((enumValue) => {
        charCount += enumValue.length;
        return charCount <= ACTOR_ENUM_MAX_LENGTH;
    });

    return resultEnumList.length > 0 ? resultEnumList : undefined;
}

/**
 * Shortens the description, enum, and items.enum properties of the schema properties.
 * @param properties
 */
export function shortenProperties(properties: { [key: string]: ISchemaProperties }): {
    [key: string]: ISchemaProperties
} {
    for (const property of Object.values(properties)) {
        if (property.description.length > ACTOR_MAX_DESCRIPTION_LENGTH) {
            property.description = `${property.description.slice(0, ACTOR_MAX_DESCRIPTION_LENGTH)}...`;
        }

        if (property.enum && property.enum?.length > 0) {
            property.enum = shortenEnum(property.enum);
        }

        if (property.items?.enum && property.items.enum.length > 0) {
            property.items.enum = shortenEnum(property.items.enum);
        }
    }

    return properties;
}
</file>

<file path="src/apify-client.ts">
import type { ApifyClientOptions } from 'apify';
import { ApifyClient as _ApifyClient } from 'apify-client';
import type { AxiosRequestConfig } from 'axios';

import { USER_AGENT_ORIGIN } from './const.js';

/**
 * Adds a User-Agent header to the request config.
 * @param config
 * @private
 */
function addUserAgent(config: AxiosRequestConfig): AxiosRequestConfig {
    const updatedConfig = { ...config };
    updatedConfig.headers = updatedConfig.headers ?? {};
    updatedConfig.headers['User-Agent'] = `${updatedConfig.headers['User-Agent'] ?? ''}; ${USER_AGENT_ORIGIN}`;
    return updatedConfig;
}

export function getApifyAPIBaseUrl(): string {
    // Workaround for Actor server where the platform APIFY_API_BASE_URL did not work with getActorDefinition from actors.ts
    if (process.env.APIFY_IS_AT_HOME) return 'https://api.apify.com';
    return process.env.APIFY_API_BASE_URL || 'https://api.apify.com';
}

export class ApifyClient extends _ApifyClient {
    constructor(options: ApifyClientOptions) {
        super({
            ...options,
            baseUrl: getApifyAPIBaseUrl(),
            requestInterceptors: [addUserAgent],
        });
    }
}
</file>

<file path="src/const.ts">
// Actor input const
export const ACTOR_README_MAX_LENGTH = 5_000;
export const ACTOR_ENUM_MAX_LENGTH = 200;
export const ACTOR_MAX_DESCRIPTION_LENGTH = 500;

// Actor output const
export const ACTOR_OUTPUT_MAX_CHARS_PER_ITEM = 5_000;
export const ACTOR_OUTPUT_TRUNCATED_MESSAGE = `Output was truncated because it will not fit into context.`
    + `There is no reason to call this tool again!`;

export const ACTOR_ADDITIONAL_INSTRUCTIONS = 'Never call/execute tool/Actor unless confirmed by the user. '
    + 'Always limit the number of results in the call arguments.';

// Actor run const
export const ACTOR_MAX_MEMORY_MBYTES = 4_096; // If the Actor requires 8GB of memory, free users can't run actors-mcp-server and requested Actor

// MCP Server
export const SERVER_NAME = 'apify-mcp-server';
export const SERVER_VERSION = '1.0.0';

// User agent headers
export const USER_AGENT_ORIGIN = 'Origin/mcp-server';

export enum HelperTools {
    SEARCH_ACTORS = 'search-actors',
    ADD_ACTOR = 'add-actor',
    REMOVE_ACTOR = 'remove-actor',
    GET_ACTOR_DETAILS = 'get-actor-details',
    HELP_TOOL = 'help-tool',
}

export const defaults = {
    actors: [
        'apify/rag-web-browser',
    ],
    helperTools: [
        HelperTools.SEARCH_ACTORS,
        HelperTools.GET_ACTOR_DETAILS,
        HelperTools.HELP_TOOL,
    ],
    actorAddingTools: [
        HelperTools.ADD_ACTOR,
        HelperTools.REMOVE_ACTOR,
    ],
};

export const APIFY_USERNAME = 'apify';
</file>

<file path="src/index.ts">
/*
 This file provides essential functions and tools for MCP servers, serving as a library.
 The ActorsMcpServer should be the only class exported from the package
*/

import { ActorsMcpServer } from './mcp/server.js';

export { ActorsMcpServer };
</file>

<file path="src/input.ts">
/*
 * Actor input processing.
 */
import log from '@apify/log';

import type { Input } from './types.js';

/**
 * Process input parameters, split Actors string into an array
 * @param originalInput
 * @returns input
 */
export function processInput(originalInput: Partial<Input>): Input {
    const input = originalInput as Input;

    // actors can be a string or an array of strings
    if (input.actors && typeof input.actors === 'string') {
        input.actors = input.actors.split(',').map((format: string) => format.trim()) as string[];
    }

    // enableAddingActors is deprecated, use enableActorAutoLoading instead
    if (input.enableAddingActors === undefined) {
        if (input.enableActorAutoLoading !== undefined) {
            log.warning('enableActorAutoLoading is deprecated, use enableAddingActors instead');
            input.enableAddingActors = input.enableActorAutoLoading === true || input.enableActorAutoLoading === 'true';
        } else {
            input.enableAddingActors = false;
        }
    } else {
        input.enableAddingActors = input.enableAddingActors === true || input.enableAddingActors === 'true';
    }
    return input;
}
</file>

<file path="src/main.ts">
/**
 * Serves as an Actor MCP SSE server entry point.
 * This file needs to be named `main.ts` to be recognized by the Apify platform.
 */

import { Actor } from 'apify';
import type { ActorCallOptions } from 'apify-client';

import log from '@apify/log';

import { createExpressApp } from './actor/server.js';
import { processInput } from './input.js';
import { ActorsMcpServer } from './mcp/server.js';
import { callActorGetDataset, getActorsAsTools } from './tools/index.js';
import type { Input } from './types.js';

const STANDBY_MODE = Actor.getEnv().metaOrigin === 'STANDBY';

await Actor.init();

const HOST = Actor.isAtHome() ? process.env.ACTOR_STANDBY_URL as string : 'http://localhost';
const PORT = Actor.isAtHome() ? Number(process.env.ACTOR_STANDBY_PORT) : 3001;

if (!process.env.APIFY_TOKEN) {
    log.error('APIFY_TOKEN is required but not set in the environment variables.');
    process.exit(1);
}

const input = processInput((await Actor.getInput<Partial<Input>>()) ?? ({} as Input));
log.info(`Loaded input: ${JSON.stringify(input)} `);

if (STANDBY_MODE) {
    const mcpServer = new ActorsMcpServer({
        enableAddingActors: Boolean(input.enableAddingActors),
        enableDefaultActors: false,
    });

    const app = createExpressApp(HOST, mcpServer);
    log.info('Actor is running in the STANDBY mode.');

    // Load only Actors specified in the input
    // If you wish to start without any Actor, create a task and leave the input empty
    if (input.actors && input.actors.length > 0) {
        const { actors } = input;
        const actorsToLoad = Array.isArray(actors) ? actors : actors.split(',');
        const tools = await getActorsAsTools(actorsToLoad, process.env.APIFY_TOKEN as string);
        mcpServer.updateTools(tools);
    }
    app.listen(PORT, () => {
        log.info(`The Actor web server is listening for user requests at ${HOST}`);
    });
} else {
    log.info('Actor is not designed to run in the NORMAL model (use this mode only for debugging purposes)');

    if (input && !input.debugActor && !input.debugActorInput) {
        await Actor.fail('If you need to debug a specific Actor, please provide the debugActor and debugActorInput fields in the input');
    }
    const options = { memory: input.maxActorMemoryBytes } as ActorCallOptions;
    const items = await callActorGetDataset(input.debugActor!, input.debugActorInput!, process.env.APIFY_TOKEN, options);

    await Actor.pushData(items);
    log.info(`Pushed ${items.length} items to the dataset`);
    await Actor.exit();
}
</file>

<file path="src/stdio.ts">
#!/usr/bin/env node
/**
 * This script initializes and starts the Apify MCP server using the Stdio transport.
 *
 * Usage:
 *   node <script_name> --actors=<actor1,actor2,...>
 *
 * Command-line arguments:
 *   --actors - A comma-separated list of Actor full names to add to the server.
 *
 * Example:
 *   node stdio.js --actors=apify/google-search-scraper,apify/instagram-scraper
 */

import { StdioServerTransport } from '@modelcontextprotocol/sdk/server/stdio.js';
import minimist from 'minimist';

import log from '@apify/log';

import { defaults } from './const.js';
import { ActorsMcpServer } from './mcp/server.js';
import { getActorsAsTools } from './tools/index.js';

// Configure logging, set to ERROR
log.setLevel(log.LEVELS.ERROR);

// Parse command line arguments
const parser = minimist;
const argv = parser(process.argv.slice(2), {
    boolean: [
        'enable-adding-actors',
        'enableActorAutoLoading', // deprecated
    ],
    string: ['actors'],
    default: {
        'enable-adding-actors': false,
    },
});
const enableAddingActors = argv['enable-adding-actors'] || argv.enableActorAutoLoading || false;
const { actors = '' } = argv;
const actorList = actors ? actors.split(',').map((a: string) => a.trim()) : [];

// Validate environment
if (!process.env.APIFY_TOKEN) {
    log.error('APIFY_TOKEN is required but not set in the environment variables.');
    process.exit(1);
}

async function main() {
    const mcpServer = new ActorsMcpServer({ enableAddingActors, enableDefaultActors: false });
    const tools = await getActorsAsTools(actorList.length ? actorList : defaults.actors, process.env.APIFY_TOKEN as string);
    mcpServer.updateTools(tools);

    // Start server
    const transport = new StdioServerTransport();
    await mcpServer.connect(transport);
}

main().catch((error) => {
    log.error(`Server error: ${error}`);
    process.exit(1);
});
</file>

<file path="src/tsconfig.json">
{
    "extends": "../tsconfig.json",
    "compilerOptions": {
        "rootDir": "./",
        "outDir": "../dist",
    }
}
</file>

<file path="src/types.ts">
import type { Server } from '@modelcontextprotocol/sdk/server/index.js';
import type { ValidateFunction } from 'ajv';
import type { ActorDefaultRunOptions, ActorDefinition } from 'apify-client';

import type { ActorsMcpServer } from './mcp/server.js';

export interface ISchemaProperties {
    type: string;

    title: string;
    description: string;

    enum?: string[]; // Array of string options for the enum
    enumTitles?: string[]; // Array of string titles for the enum
    default?: unknown;
    prefill?: unknown;

    items?: ISchemaProperties;
    editor?: string;
    examples?: unknown[];

    properties?: Record<string, ISchemaProperties>;
    required?: string[];
}

export interface IActorInputSchema {
    title?: string;
    description?: string;

    type: string;

    properties: Record<string, ISchemaProperties>;

    required?: string[];
    schemaVersion?: number;
}

export type ActorDefinitionWithDesc = Omit<ActorDefinition, 'input'> & {
    id: string;
    actorFullName: string;
    description: string;
    defaultRunOptions: ActorDefaultRunOptions;
    input?: IActorInputSchema;
}

export type ActorDefinitionPruned = Pick<ActorDefinitionWithDesc,
    'id' | 'actorFullName' | 'buildTag' | 'readme' | 'input' | 'description' | 'defaultRunOptions'>

/**
 * Base interface for all tools in the MCP server.
 * Contains common properties shared by all tool types.
 */
export interface ToolBase {
    /** Unique name/identifier for the tool */
    name: string;
    /** Description of what the tool does */
    description: string;
    /** JSON schema defining the tool's input parameters */
    inputSchema: object;
    /** AJV validation function for the input schema */
    ajvValidate: ValidateFunction;
}

/**
 * Interface for Actor-based tools - tools that wrap Apify Actors.
 * Extends ToolBase with Actor-specific properties.
 */
export interface ActorTool extends ToolBase {
    /** Full name of the Apify Actor (username/name) */
    actorFullName: string;
    /** Optional memory limit in MB for the Actor execution */
    memoryMbytes?: number;
}

/**
 * Arguments passed to internal tool calls.
 * Contains both the tool arguments and server references.
 */
export type InternalToolArgs = {
    /** Arguments passed to the tool */
    args: Record<string, unknown>;
    /** Reference to the Apify MCP server instance */
    apifyMcpServer: ActorsMcpServer;
    /** Reference to the MCP server instance */
    mcpServer: Server;
    /** Apify API token */
    apifyToken: string;
}

/**
 * Interface for internal tools - tools implemented directly in the MCP server.
 * Extends ToolBase with a call function implementation.
 */
export interface HelperTool extends ToolBase {
    /**
     * Executes the tool with the given arguments
     * @param toolArgs - Arguments and server references
     * @returns Promise resolving to the tool's output
     */
    call: (toolArgs: InternalToolArgs) => Promise<object>;
}

/**
* Actorized MCP server tool where this MCP server acts as a proxy.
* Extends ToolBase with tool associated MCP server.
*/
export interface ActorMCPTool extends ToolBase {
    // Origin MCP server tool name, is needed for the tool call
    originToolName: string;
    // ID of the Actorized MCP server
    actorID: string;
    /**
     * ID of the Actorized MCP server the tool is associated with.
     * See getMCPServerID()
     */
    serverId: string;
    // Connection URL of the Actorized MCP server
    serverUrl: string;
}

/**
 * Type discriminator for tools - indicates whether a tool is internal or Actor-based.
 */
export type ToolType = 'internal' | 'actor' | 'actor-mcp';

/**
 * Wrapper interface that combines a tool with its type discriminator.
 * Used to store and manage tools of different types uniformly.
 */
export interface ToolWrap {
    /** Type of the tool (internal or actor) */
    type: ToolType;
    /** The tool instance */
    tool: ActorTool | HelperTool | ActorMCPTool;
}

//  ActorStoreList for actor-search tool
export interface ActorStats {
    totalRuns: number;
    totalUsers30Days: number;
    publicActorRunStats30Days: unknown;
}

export interface PricingInfo {
    pricingModel?: string;
    pricePerUnitUsd?: number;
    trialMinutes?: number
}

export interface ActorStorePruned {
    id: string;
    name: string;
    username: string;
    actorFullName?: string;
    title?: string;
    description?: string;
    stats: ActorStats;
    currentPricingInfo: PricingInfo;
    url: string;
    totalStars?: number | null;
}

/**
 * Interface for internal tools - tools implemented directly in the MCP server.
 * Extends ToolBase with a call function implementation.
 */
export interface InternalTool extends ToolBase {
    /**
     * Executes the tool with the given arguments
     * @param toolArgs - Arguments and server references
     * @returns Promise resolving to the tool's output
     */
    call: (toolArgs: InternalToolArgs) => Promise<object>;
}

export type Input = {
    actors: string[] | string;
    /**
    * @deprecated Use `enableAddingActors` instead.
    */
    enableActorAutoLoading?: boolean | string;
    enableAddingActors?: boolean | string;
    maxActorMemoryBytes?: number;
    debugActor?: string;
    debugActorInput?: unknown;
};
</file>

<file path="tests/integration/actor.server-sse.test.ts">
import type { Server as HttpServer } from 'node:http';

import type { Express } from 'express';

import log from '@apify/log';

import { createExpressApp } from '../../src/actor/server.js';
import { ActorsMcpServer } from '../../src/mcp/server.js';
import { createMCPSSEClient } from '../helpers.js';
import { createIntegrationTestsSuite } from './suite.js';

let app: Express;
let mcpServer: ActorsMcpServer;
let httpServer: HttpServer;
const httpServerPort = 50000;
const httpServerHost = `http://localhost:${httpServerPort}`;
const mcpUrl = `${httpServerHost}/sse`;

createIntegrationTestsSuite({
    suiteName: 'Actors MCP Server SSE',
    createClientFn: async (options) => await createMCPSSEClient(mcpUrl, options),
    beforeAllFn: async () => {
        mcpServer = new ActorsMcpServer({
            enableDefaultActors: false,
        });
        log.setLevel(log.LEVELS.OFF);

        // Create express app using the proper server setup
        app = createExpressApp(httpServerHost, mcpServer);

        // Start test server
        await new Promise<void>((resolve) => {
            httpServer = app.listen(httpServerPort, () => resolve());
        });
    },
    beforeEachFn: async () => {
        await mcpServer.reset();
    },
    afterAllFn: async () => {
        await mcpServer.close();
        await new Promise<void>((resolve) => {
            httpServer.close(() => resolve());
        });
    },
});
</file>

<file path="tests/integration/actor.server-streamable.test.ts">
import type { Server as HttpServer } from 'node:http';

import type { Express } from 'express';

import log from '@apify/log';

import { createExpressApp } from '../../src/actor/server.js';
import { ActorsMcpServer } from '../../src/mcp/server.js';
import { createMCPStreamableClient } from '../helpers.js';
import { createIntegrationTestsSuite } from './suite.js';

let app: Express;
let mcpServer: ActorsMcpServer;
let httpServer: HttpServer;
const httpServerPort = 50001;
const httpServerHost = `http://localhost:${httpServerPort}`;
const mcpUrl = `${httpServerHost}/mcp`;

createIntegrationTestsSuite({
    suiteName: 'Actors MCP Server Streamable HTTP',
    createClientFn: async (options) => await createMCPStreamableClient(mcpUrl, options),
    beforeAllFn: async () => {
        mcpServer = new ActorsMcpServer({
            enableDefaultActors: false,
        });
        log.setLevel(log.LEVELS.OFF);

        // Create express app using the proper server setup
        app = createExpressApp(httpServerHost, mcpServer);

        // Start test server
        await new Promise<void>((resolve) => {
            httpServer = app.listen(httpServerPort, () => resolve());
        });
    },
    beforeEachFn: async () => {
        await mcpServer.reset();
    },
    afterAllFn: async () => {
        await mcpServer.close();
        await new Promise<void>((resolve) => {
            httpServer.close(() => resolve());
        });
    },
});
</file>

<file path="tests/integration/stdio.test.ts">
import { createMCPStdioClient } from '../helpers.js';
import { createIntegrationTestsSuite } from './suite.js';

createIntegrationTestsSuite({
    suiteName: 'MCP STDIO',
    createClientFn: createMCPStdioClient,
});
</file>

<file path="tests/integration/suite.ts">
import type { Client } from '@modelcontextprotocol/sdk/client/index.js';
import { afterAll, afterEach, beforeAll, beforeEach, describe, expect, it } from 'vitest';

import { defaults, HelperTools } from '../../src/const.js';
import { actorNameToToolName } from '../../src/tools/utils.js';
import type { MCPClientOptions } from '../helpers';

interface IntegrationTestsSuiteOptions {
    suiteName: string;
    createClientFn: (options?: MCPClientOptions) => Promise<Client>;
    beforeAllFn?: () => Promise<void>;
    afterAllFn?: () => Promise<void>;
    beforeEachFn?: () => Promise<void>;
    afterEachFn?: () => Promise<void>;
}

export function createIntegrationTestsSuite(
    options: IntegrationTestsSuiteOptions,
) {
    const {
        suiteName,
        createClientFn,
        beforeAllFn,
        afterAllFn,
        beforeEachFn,
        afterEachFn,
    } = options;

    // Hooks
    if (beforeAllFn) {
        beforeAll(beforeAllFn);
    }
    if (afterAllFn) {
        afterAll(afterAllFn);
    }
    if (beforeEachFn) {
        beforeEach(beforeEachFn);
    }
    if (afterEachFn) {
        afterEach(afterEachFn);
    }

    describe(suiteName, () => {
        it('list default tools', async () => {
            const client = await createClientFn();
            const tools = await client.listTools();
            const names = tools.tools.map((tool) => tool.name);

            expect(names.length).toEqual(defaults.actors.length + defaults.helperTools.length);
            for (const tool of defaults.helperTools) {
                expect(names).toContain(tool);
            }
            for (const actor of defaults.actors) {
                expect(names).toContain(actorNameToToolName(actor));
            }
            await client.close();
        });

        it('use only apify/python-example Actor and call it', async () => {
            const actorName = 'apify/python-example';
            const selectedToolName = actorNameToToolName(actorName);
            const client = await createClientFn({
                actors: [actorName],
                enableAddingActors: false,
            });
            const tools = await client.listTools();
            const names = tools.tools.map((tool) => tool.name);
            expect(names.length).toEqual(defaults.helperTools.length + 1);
            for (const tool of defaults.helperTools) {
                expect(names).toContain(tool);
            }
            expect(names).toContain(selectedToolName);

            const result = await client.callTool({
                name: selectedToolName,
                arguments: {
                    first_number: 1,
                    second_number: 2,
                },
            });

            expect(result).toEqual({
                content: [{
                    text: JSON.stringify({
                        first_number: 1,
                        second_number: 2,
                        sum: 3,
                    }),
                    type: 'text',
                }],
            });

            await client.close();
        });

        it('load Actors from parameters', async () => {
            const actors = ['apify/rag-web-browser', 'apify/instagram-scraper'];
            const client = await createClientFn({
                actors,
                enableAddingActors: false,
            });
            const tools = await client.listTools();
            const names = tools.tools.map((tool) => tool.name);
            expect(names.length).toEqual(defaults.helperTools.length + actors.length);
            for (const tool of defaults.helperTools) {
                expect(names).toContain(tool);
            }
            for (const actor of actors) {
                expect(names).toContain(actorNameToToolName(actor));
            }

            await client.close();
        });

        it('load Actor dynamically and call it', async () => {
            const actor = 'apify/python-example';
            const selectedToolName = actorNameToToolName(actor);
            const client = await createClientFn({
                enableAddingActors: true,
            });
            const tools = await client.listTools();
            const names = tools.tools.map((tool) => tool.name);
            expect(names.length).toEqual(defaults.helperTools.length + defaults.actorAddingTools.length + defaults.actors.length);
            for (const tool of defaults.helperTools) {
                expect(names).toContain(tool);
            }
            for (const tool of defaults.actorAddingTools) {
                expect(names).toContain(tool);
            }
            for (const actorTool of defaults.actors) {
                expect(names).toContain(actorNameToToolName(actorTool));
            }

            // Add Actor dynamically
            await client.callTool({
                name: HelperTools.ADD_ACTOR,
                arguments: {
                    actorName: actor,
                },
            });

            // Check if tools was added
            const toolsAfterAdd = await client.listTools();
            const namesAfterAdd = toolsAfterAdd.tools.map((tool) => tool.name);
            expect(namesAfterAdd.length).toEqual(defaults.helperTools.length + defaults.actorAddingTools.length + defaults.actors.length + 1);
            expect(namesAfterAdd).toContain(selectedToolName);

            const result = await client.callTool({
                name: selectedToolName,
                arguments: {
                    first_number: 1,
                    second_number: 2,
                },
            });

            expect(result).toEqual({
                content: [{
                    text: JSON.stringify({
                        first_number: 1,
                        second_number: 2,
                        sum: 3,
                    }),
                    type: 'text',
                }],
            });

            await client.close();
        });

        it('should remove Actor from tools list', async () => {
            const actor = 'apify/python-example';
            const selectedToolName = actorNameToToolName(actor);
            const client = await createClientFn({
                actors: [actor],
                enableAddingActors: true,
            });

            // Verify actor is in the tools list
            const toolsBefore = await client.listTools();
            const namesBefore = toolsBefore.tools.map((tool) => tool.name);
            expect(namesBefore).toContain(selectedToolName);

            // Remove the actor
            await client.callTool({
                name: HelperTools.REMOVE_ACTOR,
                arguments: {
                    toolName: selectedToolName,
                },
            });

            // Verify actor is removed
            const toolsAfter = await client.listTools();
            const namesAfter = toolsAfter.tools.map((tool) => tool.name);
            expect(namesAfter).not.toContain(selectedToolName);

            await client.close();
        });
    });
}
</file>

<file path="tests/unit/input.test.ts">
import { describe, expect, it } from 'vitest';

import { processInput } from '../../src/input.js';
import type { Input } from '../../src/types.js';

describe('processInput', () => {
    it('should handle string actors input and convert to array', async () => {
        const input: Partial<Input> = {
            actors: 'actor1, actor2,actor3',
        };
        const processed = processInput(input);
        expect(processed.actors).toEqual(['actor1', 'actor2', 'actor3']);
    });

    it('should keep array actors input unchanged', async () => {
        const input: Partial<Input> = {
            actors: ['actor1', 'actor2', 'actor3'],
        };
        const processed = processInput(input);
        expect(processed.actors).toEqual(['actor1', 'actor2', 'actor3']);
    });

    it('should handle enableActorAutoLoading to set enableAddingActors', async () => {
        const input: Partial<Input> = {
            actors: ['actor1'],
            enableActorAutoLoading: true,
        };
        const processed = processInput(input);
        expect(processed.enableAddingActors).toBe(true);
    });

    it('should not override existing enableAddingActors with enableActorAutoLoading', async () => {
        const input: Partial<Input> = {
            actors: ['actor1'],
            enableActorAutoLoading: true,
            enableAddingActors: false,
        };
        const processed = processInput(input);
        expect(processed.enableAddingActors).toBe(false);
    });

    it('should default enableAddingActors to false when not provided', async () => {
        const input: Partial<Input> = {
            actors: ['actor1'],
        };
        const processed = processInput(input);
        expect(processed.enableAddingActors).toBe(false);
    });
});
</file>

<file path="tests/unit/mcp.utils.test.ts">
import { describe, expect, it } from 'vitest';

import { parseInputParamsFromUrl } from '../../src/mcp/utils.js';

describe('parseInputParamsFromUrl', () => {
    it('should parse Actors from URL query params', () => {
        const url = 'https://actors-mcp-server.apify.actor?token=123&actors=apify/web-scraper';
        const result = parseInputParamsFromUrl(url);
        expect(result.actors).toEqual(['apify/web-scraper']);
    });

    it('should parse multiple Actors from URL', () => {
        const url = 'https://actors-mcp-server.apify.actor?actors=apify/instagram-scraper,lukaskrivka/google-maps';
        const result = parseInputParamsFromUrl(url);
        expect(result.actors).toEqual(['apify/instagram-scraper', 'lukaskrivka/google-maps']);
    });

    it('should handle URL without query params', () => {
        const url = 'https://actors-mcp-server.apify.actor';
        const result = parseInputParamsFromUrl(url);
        expect(result.actors).toBeUndefined();
    });

    it('should parse enableActorAutoLoading flag', () => {
        const url = 'https://actors-mcp-server.apify.actor?enableActorAutoLoading=true';
        const result = parseInputParamsFromUrl(url);
        expect(result.enableAddingActors).toBe(true);
    });

    it('should parse enableAddingActors flag', () => {
        const url = 'https://actors-mcp-server.apify.actor?enableAddingActors=true';
        const result = parseInputParamsFromUrl(url);
        expect(result.enableAddingActors).toBe(true);
    });

    it('should parse enableAddingActors flag', () => {
        const url = 'https://actors-mcp-server.apify.actor?enableAddingActors=false';
        const result = parseInputParamsFromUrl(url);
        expect(result.enableAddingActors).toBe(false);
    });

    it('should handle Actors as string parameter', () => {
        const url = 'https://actors-mcp-server.apify.actor?actors=apify/rag-web-browser';
        const result = parseInputParamsFromUrl(url);
        expect(result.actors).toEqual(['apify/rag-web-browser']);
    });
});
</file>

<file path="tests/unit/tools.actor.test.ts">
import { describe, expect, it } from 'vitest';

import { ACTOR_ENUM_MAX_LENGTH } from '../../src/const.js';
import { actorNameToToolName, inferArrayItemType, shortenEnum } from '../../src/tools/utils.js';

describe('actors', () => {
    describe('actorNameToToolName', () => {
        it('should replace slashes and dots with dash notation', () => {
            expect(actorNameToToolName('apify/web-scraper')).toBe('apify-slash-web-scraper');
            expect(actorNameToToolName('my.actor.name')).toBe('my-dot-actor-dot-name');
        });

        it('should handle empty strings', () => {
            expect(actorNameToToolName('')).toBe('');
        });

        it('should handle strings without slashes or dots', () => {
            expect(actorNameToToolName('actorname')).toBe('actorname');
        });

        it('should handle strings with multiple slashes and dots', () => {
            expect(actorNameToToolName('actor/name.with/multiple.parts')).toBe('actor-slash-name-dot-with-slash-multiple-dot-parts');
        });

        it('should handle tool names longer than 64 characters', () => {
            const longName = 'a'.repeat(70);
            const expected = 'a'.repeat(64);
            expect(actorNameToToolName(longName)).toBe(expected);
        });

        it('infers array item type from editor', () => {
            const property = {
                type: 'array',
                editor: 'stringList',
                title: '',
                description: '',
                enum: [],
                default: '',
                prefill: '',
            };
            expect(inferArrayItemType(property)).toBe('string');
        });

        it('shorten enum list', () => {
            const enumList: string[] = [];
            const wordLength = 10;
            const wordCount = 30;

            for (let i = 0; i < wordCount; i++) {
                enumList.push('a'.repeat(wordLength));
            }

            const shortenedList = shortenEnum(enumList);

            expect(shortenedList?.length || 0).toBe(ACTOR_ENUM_MAX_LENGTH / wordLength);
        });
    });
});
</file>

<file path="tests/unit/tools.utils.test.ts">
import { describe, expect, it } from 'vitest';

import { ACTOR_ENUM_MAX_LENGTH, ACTOR_MAX_DESCRIPTION_LENGTH } from '../../src/const.js';
import { buildNestedProperties, markInputPropertiesAsRequired, shortenProperties } from '../../src/tools/utils.js';
import type { IActorInputSchema, ISchemaProperties } from '../../src/types.js';

describe('buildNestedProperties', () => {
    it('should add useApifyProxy property to proxy objects', () => {
        const properties: Record<string, ISchemaProperties> = {
            proxy: {
                type: 'object',
                editor: 'proxy',
                title: 'Proxy configuration',
                description: 'Proxy settings',
                properties: {},
            },
            otherProp: {
                type: 'string',
                title: 'Other property',
                description: 'Some other property',
            },
        };

        const result = buildNestedProperties(properties);

        // Check that proxy object has useApifyProxy property
        expect(result.proxy.properties).toBeDefined();
        expect(result.proxy.properties?.useApifyProxy).toBeDefined();
        expect(result.proxy.properties?.useApifyProxy.type).toBe('boolean');
        expect(result.proxy.properties?.useApifyProxy.default).toBe(true);
        expect(result.proxy.required).toContain('useApifyProxy');

        // Check that other properties remain unchanged
        expect(result.otherProp).toEqual(properties.otherProp);
    });

    it('should add URL structure to requestListSources array items', () => {
        const properties: Record<string, ISchemaProperties> = {
            sources: {
                type: 'array',
                editor: 'requestListSources',
                title: 'Request list sources',
                description: 'Sources to scrape',
            },
            otherProp: {
                type: 'string',
                title: 'Other property',
                description: 'Some other property',
            },
        };

        const result = buildNestedProperties(properties);

        // Check that requestListSources array has proper item structure
        expect(result.sources.items).toBeDefined();
        expect(result.sources.items?.type).toBe('object');
        expect(result.sources.items?.properties?.url).toBeDefined();
        expect(result.sources.items?.properties?.url.type).toBe('string');

        // Check that other properties remain unchanged
        expect(result.otherProp).toEqual(properties.otherProp);
    });

    it('should not modify properties that don\'t match special cases', () => {
        const properties: Record<string, ISchemaProperties> = {
            regularObject: {
                type: 'object',
                title: 'Regular object',
                description: 'A regular object without special editor',
                properties: {
                    subProp: {
                        type: 'string',
                        title: 'Sub property',
                        description: 'Sub property description',
                    },
                },
            },
            regularArray: {
                type: 'array',
                title: 'Regular array',
                description: 'A regular array without special editor',
                items: {
                    type: 'string',
                    title: 'Item',
                    description: 'Item description',
                },
            },
        };

        const result = buildNestedProperties(properties);

        // Check that regular properties remain unchanged
        expect(result).toEqual(properties);
    });

    it('should handle empty properties object', () => {
        const properties: Record<string, ISchemaProperties> = {};
        const result = buildNestedProperties(properties);
        expect(result).toEqual({});
    });
});

describe('markInputPropertiesAsRequired', () => {
    it('should add REQUIRED prefix to required properties', () => {
        const input: IActorInputSchema = {
            title: 'Test Schema',
            type: 'object',
            required: ['requiredProp1', 'requiredProp2'],
            properties: {
                requiredProp1: {
                    type: 'string',
                    title: 'Required Property 1',
                    description: 'This is required',
                },
                requiredProp2: {
                    type: 'number',
                    title: 'Required Property 2',
                    description: 'This is also required',
                },
                optionalProp: {
                    type: 'boolean',
                    title: 'Optional Property',
                    description: 'This is optional',
                },
            },
        };

        const result = markInputPropertiesAsRequired(input);

        // Check that required properties have REQUIRED prefix
        expect(result.requiredProp1.description).toContain('**REQUIRED**');
        expect(result.requiredProp2.description).toContain('**REQUIRED**');

        // Check that optional properties remain unchanged
        expect(result.optionalProp.description).toBe('This is optional');
    });

    it('should handle input without required fields', () => {
        const input: IActorInputSchema = {
            title: 'Test Schema',
            type: 'object',
            properties: {
                prop1: {
                    type: 'string',
                    title: 'Property 1',
                    description: 'Description 1',
                },
                prop2: {
                    type: 'number',
                    title: 'Property 2',
                    description: 'Description 2',
                },
            },
        };

        const result = markInputPropertiesAsRequired(input);

        // Check that no properties were modified
        expect(result).toEqual(input.properties);
    });

    it('should handle empty required array', () => {
        const input: IActorInputSchema = {
            title: 'Test Schema',
            type: 'object',
            required: [],
            properties: {
                prop1: {
                    type: 'string',
                    title: 'Property 1',
                    description: 'Description 1',
                },
            },
        };

        const result = markInputPropertiesAsRequired(input);

        // Check that no properties were modified
        expect(result).toEqual(input.properties);
    });
});

describe('shortenProperties', () => {
    it('should truncate long descriptions', () => {
        const longDescription = 'a'.repeat(ACTOR_MAX_DESCRIPTION_LENGTH + 100);
        const properties: Record<string, ISchemaProperties> = {
            prop1: {
                type: 'string',
                title: 'Property 1',
                description: longDescription,
            },
        };

        const result = shortenProperties(properties);

        // Check that description was truncated
        expect(result.prop1.description.length).toBeLessThanOrEqual(ACTOR_MAX_DESCRIPTION_LENGTH + 3); // +3 for "..."
        expect(result.prop1.description.endsWith('...')).toBe(true);
    });

    it('should not modify descriptions that are within limits', () => {
        const description = 'This is a normal description';
        const properties: Record<string, ISchemaProperties> = {
            prop1: {
                type: 'string',
                title: 'Property 1',
                description,
            },
        };

        const result = shortenProperties(properties);

        // Check that description was not modified
        expect(result.prop1.description).toBe(description);
    });

    it('should shorten enum values if they exceed the limit', () => {
        // Create an enum with many values to exceed the character limit
        const enumValues = Array.from({ length: 50 }, (_, i) => `enum-value-${i}`);
        const properties: Record<string, ISchemaProperties> = {
            prop1: {
                type: 'string',
                title: 'Property 1',
                description: 'Property with enum',
                enum: enumValues,
            },
        };

        const result = shortenProperties(properties);

        // Check that enum was shortened
        expect(result.prop1.enum).toBeDefined();
        expect(result.prop1.enum!.length).toBeLessThan(enumValues.length);

        // Calculate total character length of enum values
        const totalLength = result.prop1.enum!.reduce((sum, val) => sum + val.length, 0);
        expect(totalLength).toBeLessThanOrEqual(ACTOR_ENUM_MAX_LENGTH);
    });

    it('should shorten items.enum values if they exceed the limit', () => {
        // Create an enum with many values to exceed the character limit
        const enumValues = Array.from({ length: 50 }, (_, i) => `enum-value-${i}`);
        const properties: Record<string, ISchemaProperties> = {
            prop1: {
                type: 'array',
                title: 'Property 1',
                description: 'Property with items.enum',
                items: {
                    type: 'string',
                    title: 'Item',
                    description: 'Item description',
                    enum: enumValues,
                },
            },
        };

        const result = shortenProperties(properties);

        // Check that items.enum was shortened
        expect(result.prop1.items?.enum).toBeDefined();
        expect(result.prop1.items!.enum!.length).toBeLessThan(enumValues.length);

        // Calculate total character length of enum values
        const totalLength = result.prop1.items!.enum!.reduce((sum, val) => sum + val.length, 0);
        expect(totalLength).toBeLessThanOrEqual(ACTOR_ENUM_MAX_LENGTH);
    });

    it('should handle properties without enum or items.enum', () => {
        const properties: Record<string, ISchemaProperties> = {
            prop1: {
                type: 'string',
                title: 'Property 1',
                description: 'Regular property',
            },
            prop2: {
                type: 'array',
                title: 'Property 2',
                description: 'Array property',
                items: {
                    type: 'string',
                    title: 'Item',
                    description: 'Item description',
                },
            },
        };

        const result = shortenProperties(properties);

        // Check that properties were not modified
        expect(result).toEqual(properties);
    });

    it('should handle empty enum arrays', () => {
        const properties: Record<string, ISchemaProperties> = {
            prop1: {
                type: 'string',
                title: 'Property 1',
                description: 'Property with empty enum',
                enum: [],
            },
            prop2: {
                type: 'array',
                title: 'Property 2',
                description: 'Array with empty items.enum',
                items: {
                    type: 'string',
                    title: 'Item',
                    description: 'Item description',
                    enum: [],
                },
            },
        };

        const result = shortenProperties(properties);

        // Check that properties were not modified
        expect(result).toEqual(properties);
    });
});
</file>

<file path="tests/helpers.ts">
import { Client } from '@modelcontextprotocol/sdk/client/index.js';
import { SSEClientTransport } from '@modelcontextprotocol/sdk/client/sse.js';
import { StdioClientTransport } from '@modelcontextprotocol/sdk/client/stdio.js';
import { StreamableHTTPClientTransport } from '@modelcontextprotocol/sdk/client/streamableHttp.js';

export interface MCPClientOptions {
    actors?: string[];
    enableAddingActors?: boolean;
}

export async function createMCPSSEClient(
    serverUrl: string,
    options?: MCPClientOptions,
): Promise<Client> {
    if (!process.env.APIFY_TOKEN) {
        throw new Error('APIFY_TOKEN environment variable is not set.');
    }
    const url = new URL(serverUrl);
    const { actors, enableAddingActors } = options || {};
    if (actors) {
        url.searchParams.append('actors', actors.join(','));
    }
    if (enableAddingActors) {
        url.searchParams.append('enableAddingActors', 'true');
    }

    const transport = new SSEClientTransport(
        url,
        {
            requestInit: {
                headers: {
                    authorization: `Bearer ${process.env.APIFY_TOKEN}`,
                },
            },
        },
    );

    const client = new Client({
        name: 'sse-client',
        version: '1.0.0',
    });
    await client.connect(transport);

    return client;
}

export async function createMCPStreamableClient(
    serverUrl: string,
    options?: MCPClientOptions,
): Promise<Client> {
    if (!process.env.APIFY_TOKEN) {
        throw new Error('APIFY_TOKEN environment variable is not set.');
    }
    const url = new URL(serverUrl);
    const { actors, enableAddingActors } = options || {};
    if (actors) {
        url.searchParams.append('actors', actors.join(','));
    }
    if (enableAddingActors) {
        url.searchParams.append('enableAddingActors', 'true');
    }

    const transport = new StreamableHTTPClientTransport(
        url,
        {
            requestInit: {
                headers: {
                    authorization: `Bearer ${process.env.APIFY_TOKEN}`,
                },
            },
        },
    );

    const client = new Client({
        name: 'streamable-http-client',
        version: '1.0.0',
    });
    await client.connect(transport);

    return client;
}

export async function createMCPStdioClient(
    options?: MCPClientOptions,
): Promise<Client> {
    if (!process.env.APIFY_TOKEN) {
        throw new Error('APIFY_TOKEN environment variable is not set.');
    }
    const { actors, enableAddingActors } = options || {};
    const args = ['dist/stdio.js'];
    if (actors) {
        args.push('--actors', actors.join(','));
    }
    if (enableAddingActors) {
        args.push('--enable-adding-actors');
    }
    const transport = new StdioClientTransport({
        command: 'node',
        args,
        env: {
            APIFY_TOKEN: process.env.APIFY_TOKEN as string,
        },
    });
    const client = new Client({
        name: 'stdio-client',
        version: '1.0.0',
    });
    await client.connect(transport);

    return client;
}
</file>

<file path="tests/README.md">
# Tests

This directory contains **unit** and **integration** tests for the `actors-mcp-server` project.

# Unit Tests

Unit tests are located in the `tests/unit` directory.

To run the unit tests, you can use the following command:
```bash
npm run test:unit
```

# Integration Tests

Integration tests are located in the `tests/integration` directory.
In order to run the integration tests, you need to have the `APIFY_TOKEN` environment variable set.
Also following Actors need to exist on the target execution Apify platform:
```
ALL DEFAULT ONES DEFINED IN consts.ts AND ALSO EXPLICITLY:
apify/rag-web-browser
apify/instagram-scraper
apify/python-example
```

To run the integration tests, you can use the following command:
```bash
APIFY_TOKEN=your_token npm run test:integration
```
</file>

<file path=".dockerignore">
# configurations
.idea

# crawlee and apify storage folders
apify_storage
crawlee_storage
storage

# installed files
node_modules

# git folder
.git

# data
data
src/storage
dist
.env
</file>

<file path=".editorconfig">
root = true

[*]
indent_style = space
indent_size = 4
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true
end_of_line = lf
</file>

<file path=".env.example">
APIFY_TOKEN=
# ANTHROPIC_API_KEY is only required when you want to run examples/clientStdioChat.js
ANTHROPIC_API_KEY=
</file>

<file path=".gitignore">
# This file tells Git which files shouldn't be added to source control

.idea
.vscode
storage
apify_storage
crawlee_storage
node_modules
dist
tsconfig.tsbuildinfo
storage/*
!storage/key_value_stores
storage/key_value_stores/*
!storage/key_value_stores/default
storage/key_value_stores/default/*
!storage/key_value_stores/default/INPUT.json

# Added by Apify CLI
.venv
.env
.aider*
</file>

<file path=".npmignore">
# .npmignore
# Exclude everything by default
*

# Include specific files and folders
!dist/
!README.md
!LICENSE
</file>

<file path=".nvmrc">
22.13.1
</file>

<file path="CHANGELOG.md">
# Changelog

All notable changes to this project will be documented in this file.

## [0.1.30](https://github.com/apify/actors-mcp-server/releases/tag/v0.1.30) (2025-05-07)

### 🐛 Bug Fixes

- Stdio print error ([#101](https://github.com/apify/actors-mcp-server/pull/101)) ([1964e69](https://github.com/apify/actors-mcp-server/commit/1964e69f5bcf64e47892b4d09044ac40ccd97ac9)) by [@MQ37](https://github.com/MQ37)
- Actor server default tool loading ([#104](https://github.com/apify/actors-mcp-server/pull/104)) ([f4eca84](https://github.com/apify/actors-mcp-server/commit/f4eca8471a933e48ceecf70edf8a8657a27c7978)) by [@MQ37](https://github.com/MQ37)
- Stdio and streamable http client examples ([#106](https://github.com/apify/actors-mcp-server/pull/106)) ([8d58bfa](https://github.com/apify/actors-mcp-server/commit/8d58bfaee377877968c96aa465f0125159b84e8b)) by [@MQ37](https://github.com/MQ37), closes [#105](https://github.com/apify/actors-mcp-server/issues/105)
- Update README with a link to a blog post ([#112](https://github.com/apify/actors-mcp-server/pull/112)) ([bfe9286](https://github.com/apify/actors-mcp-server/commit/bfe92861c8f85c4af50d77c94173e4abedca8f57)) by [@jirispilka](https://github.com/jirispilka), closes [#65](https://github.com/apify/actors-mcp-server/issues/65)


## [0.1.29](https://github.com/apify/actors-mcp-server/releases/tag/v0.1.29) (2025-04-29)

### 🐛 Bug Fixes

- Add tool call timeout ([#93](https://github.com/apify/actors-mcp-server/pull/93)) ([409ad50](https://github.com/apify/actors-mcp-server/commit/409ad50569e5bd2e3dff950c11523a8440a98034)) by [@MQ37](https://github.com/MQ37)
- Adds glama.json file to allow claim the server on Glama ([#95](https://github.com/apify/actors-mcp-server/pull/95)) ([b57fe24](https://github.com/apify/actors-mcp-server/commit/b57fe243e9c12126ed2aaf7b830d7fa45f7a7d1c)) by [@jirispilka](https://github.com/jirispilka), closes [#85](https://github.com/apify/actors-mcp-server/issues/85)
- Code improvements ([#91](https://github.com/apify/actors-mcp-server/pull/91)) ([b43361a](https://github.com/apify/actors-mcp-server/commit/b43361ab63402dc1f64487e01e32024d517c91b5)) by [@MQ37](https://github.com/MQ37)
- Rename tools ([#99](https://github.com/apify/actors-mcp-server/pull/99)) ([45ffae6](https://github.com/apify/actors-mcp-server/commit/45ffae60e8209b5949a3ad04f65faad73faa50d8)) by [@MQ37](https://github.com/MQ37), closes [#98](https://github.com/apify/actors-mcp-server/issues/98)


## [0.1.28](https://github.com/apify/actors-mcp-server/releases/tag/v0.1.28) (2025-04-22)

### 🐛 Bug Fixes

- Default actors not loaded ([#94](https://github.com/apify/actors-mcp-server/pull/94)) ([fde4c3b](https://github.com/apify/actors-mcp-server/commit/fde4c3b0d66195439d2677d0ac33a08bc77b84cd)) by [@jirispilka](https://github.com/jirispilka)


## [0.1.27](https://github.com/apify/actors-mcp-server/releases/tag/v0.1.27) (2025-04-22)

### 🐛 Bug Fixes

- Move logic to enableAddingActors and enableDefaultActors to constructor ([#90](https://github.com/apify/actors-mcp-server/pull/90)) ([0f44740](https://github.com/apify/actors-mcp-server/commit/0f44740ed3c34a15d938133ac30254afe5d81c57)) by [@jirispilka](https://github.com/jirispilka)


## [0.1.26](https://github.com/apify/actors-mcp-server/releases/tag/v0.1.26) (2025-04-22)

### 🐛 Bug Fixes

- Readme smithery ([#92](https://github.com/apify/actors-mcp-server/pull/92)) ([e585cf3](https://github.com/apify/actors-mcp-server/commit/e585cf394a16aa9891428106d91c443ce9791001)) by [@jirispilka](https://github.com/jirispilka)


## [0.1.25](https://github.com/apify/actors-mcp-server/releases/tag/v0.1.25) (2025-04-21)

### 🐛 Bug Fixes

- Load actors as tools dynamically based on input ([#87](https://github.com/apify/actors-mcp-server/pull/87)) ([5238225](https://github.com/apify/actors-mcp-server/commit/5238225a08094e7959a21c842c4c56cfaae1e8f8)) by [@jirispilka](https://github.com/jirispilka), closes [#88](https://github.com/apify/actors-mcp-server/issues/88)


## [0.1.24](https://github.com/apify/actors-mcp-server/releases/tag/v0.1.24) (2025-04-21)

### 🚀 Features

- Decouple Actor from mcp-server, add ability to call Actorized MCP and load tools ([#59](https://github.com/apify/actors-mcp-server/pull/59)) ([fe8d9c2](https://github.com/apify/actors-mcp-server/commit/fe8d9c22c404eb8a22cdce70feb81ca166eb4f7f)) by [@MQ37](https://github.com/MQ37), closes [#55](https://github.com/apify/actors-mcp-server/issues/55), [#56](https://github.com/apify/actors-mcp-server/issues/56)

### 🐛 Bug Fixes

- Update search tool description ([#82](https://github.com/apify/actors-mcp-server/pull/82)) ([43e6dab](https://github.com/apify/actors-mcp-server/commit/43e6dab1883b5dd4e915f475e2d7f71e892ed0bf)) by [@jirispilka](https://github.com/jirispilka), closes [#78](https://github.com/apify/actors-mcp-server/issues/78)
- Load default Actors for the &#x2F;mcp route ([#86](https://github.com/apify/actors-mcp-server/pull/86)) ([b01561f](https://github.com/apify/actors-mcp-server/commit/b01561fd7dbd8061606b226ee6977403969e7b48)) by [@jirispilka](https://github.com/jirispilka)


## [0.1.23](https://github.com/apify/actors-mcp-server/releases/tag/v0.1.23) (2025-04-16)

### 🐛 Bug Fixes

- Add default Actors in standby mode ([#77](https://github.com/apify/actors-mcp-server/pull/77)) ([4b44e78](https://github.com/apify/actors-mcp-server/commit/4b44e7869549697ff2256a7794e61e3cfec3dd4e)) by [@jirispilka](https://github.com/jirispilka)


## [0.1.22](https://github.com/apify/actors-mcp-server/releases/tag/v0.1.22) (2025-04-16)

### 🐛 Bug Fixes

- Deprecate enableActorAutoLoading in favor of enable-adding-actors, and load tools only if not provided in query parameter ([#63](https://github.com/apify/actors-mcp-server/pull/63)) ([8add54c](https://github.com/apify/actors-mcp-server/commit/8add54ce94952bc23653b1f5c6c568e51589ffa5)) by [@jirispilka](https://github.com/jirispilka), closes [#54](https://github.com/apify/actors-mcp-server/issues/54)
- CI ([#75](https://github.com/apify/actors-mcp-server/pull/75)) ([3433a39](https://github.com/apify/actors-mcp-server/commit/3433a39305f59c7964401a3d68db06cb47bb243a)) by [@jirispilka](https://github.com/jirispilka)
- Add tools with query parameter ([#76](https://github.com/apify/actors-mcp-server/pull/76)) ([dc9a07a](https://github.com/apify/actors-mcp-server/commit/dc9a07a37db076eb9fe064e726cae6e7bdb2bf0f)) by [@jirispilka](https://github.com/jirispilka)


## [0.1.21](https://github.com/apify/actors-mcp-server/releases/tag/v0.1.21) (2025-03-27)

### 🐛 Bug Fixes

- Update README for a localhost configuration ([#52](https://github.com/apify/actors-mcp-server/pull/52)) ([82e8f6c](https://github.com/apify/actors-mcp-server/commit/82e8f6c2c7d1b3284f1c6f6f583caac5eb2973a1)) by [@jirispilka](https://github.com/jirispilka), closes [#51](https://github.com/apify/actors-mcp-server/issues/51)
- Update README.md guide link ([#53](https://github.com/apify/actors-mcp-server/pull/53)) ([cd30df2](https://github.com/apify/actors-mcp-server/commit/cd30df2eed1f87396d3f5a143fdd1bb69a8e00ba)) by [@jirispilka](https://github.com/jirispilka)


## [0.1.20](https://github.com/apify/actors-mcp-server/releases/tag/v0.1.20) (2025-03-21)

### 🚀 Features

- Return run information when MCP server is started in standby mode ([#48](https://github.com/apify/actors-mcp-server/pull/48)) ([880dccb](https://github.com/apify/actors-mcp-server/commit/880dccb812312cfecbe5e5fe55d12b98822d7a05)) by [@jirispilka](https://github.com/jirispilka)


## [0.1.19](https://github.com/apify/actors-mcp-server/releases/tag/v0.1.19) (2025-03-21)

### 🐛 Bug Fixes

- Update readme with correct links ([#47](https://github.com/apify/actors-mcp-server/pull/47)) ([2fe8cde](https://github.com/apify/actors-mcp-server/commit/2fe8cdeb6f50cee88b81b8f8e7b41a97b029c803)) by [@jirispilka](https://github.com/jirispilka)


## [0.1.18](https://github.com/apify/actors-mcp-server/releases/tag/v0.1.18) (2025-03-20)

### 🐛 Bug Fixes

- Truncate properties ([#46](https://github.com/apify/actors-mcp-server/pull/46)) ([3ee4543](https://github.com/apify/actors-mcp-server/commit/3ee4543fd8dde49b72d3323c67c3e25a27ba00ff)) by [@jirispilka](https://github.com/jirispilka)


## [0.1.17](https://github.com/apify/actors-mcp-server/releases/tag/v0.1.17) (2025-03-18)

### 🐛 Bug Fixes

- Tool schema array type infer and nested props ([#45](https://github.com/apify/actors-mcp-server/pull/45)) ([25fd5ad](https://github.com/apify/actors-mcp-server/commit/25fd5ad4cddb31470ff40937b080565442707070)) by [@MQ37](https://github.com/MQ37)


## [0.1.16](https://github.com/apify/actors-mcp-server/releases/tag/v0.1.16) (2025-03-14)

### 🐛 Bug Fixes

- Add enum values and examples to schema property descriptions ([#42](https://github.com/apify/actors-mcp-server/pull/42)) ([e4e5a9e](https://github.com/apify/actors-mcp-server/commit/e4e5a9e0828c3adeb8e6ebbb9a7d1a0987d972b7)) by [@MQ37](https://github.com/MQ37)


## [0.1.15](https://github.com/apify/actors-mcp-server/releases/tag/v0.1.15) (2025-03-13)

### 🐛 Bug Fixes

- InferArrayItemType ([#41](https://github.com/apify/actors-mcp-server/pull/41)) ([64e0955](https://github.com/apify/actors-mcp-server/commit/64e09551a2383bf304e24e96dddff29fa3c50b2f)) by [@MQ37](https://github.com/MQ37)


## [0.1.14](https://github.com/apify/actors-mcp-server/releases/tag/v0.1.14) (2025-03-13)

### 🚀 Features

- Tool-items-type ([#39](https://github.com/apify/actors-mcp-server/pull/39)) ([12344c8](https://github.com/apify/actors-mcp-server/commit/12344c8c68d397caa937684f7082485d6cbf41ad)) by [@MQ37](https://github.com/MQ37)


## [0.1.13](https://github.com/apify/actors-mcp-server/releases/tag/v0.1.13) (2025-03-12)

### 🐛 Bug Fixes

- Update inspector command in readme ([#38](https://github.com/apify/actors-mcp-server/pull/38)) ([4c2323e](https://github.com/apify/actors-mcp-server/commit/4c2323ea3d40fa6742cf59673643d0a9aa8e12ce)) by [@jirispilka](https://github.com/jirispilka)


## [0.1.12](https://github.com/apify/actors-mcp-server/releases/tag/v0.1.12) (2025-03-12)

### 🐛 Bug Fixes

- Rename tool name, sent `notifications&#x2F;tools&#x2F;list_changed` ([#37](https://github.com/apify/actors-mcp-server/pull/37)) ([8a00881](https://github.com/apify/actors-mcp-server/commit/8a00881bd64a13eb5d0bd4cfcbf270bc19570f6b)) by [@jirispilka](https://github.com/jirispilka), closes [#11](https://github.com/apify/actors-mcp-server/issues/11)


## [0.1.11](https://github.com/apify/actors-mcp-server/releases/tag/v0.1.11) (2025-03-06)

### 🐛 Bug Fixes

- Correct readme ([#35](https://github.com/apify/actors-mcp-server/pull/35)) ([9443d86](https://github.com/apify/actors-mcp-server/commit/9443d86aac4db5a1851b664bb2cacd80c38ba429)) by [@jirispilka](https://github.com/jirispilka)


## [0.1.10](https://github.com/apify/actors-mcp-server/releases/tag/v0.1.10) (2025-02-28)

### 🚀 Features

- Update README with a link to relevant blogposts ([#34](https://github.com/apify/actors-mcp-server/pull/34)) ([a7c8ea2](https://github.com/apify/actors-mcp-server/commit/a7c8ea24da243283195822d16b56f135786866f4)) by [@jirispilka](https://github.com/jirispilka)

### 🐛 Bug Fixes

- Update README.md ([#33](https://github.com/apify/actors-mcp-server/pull/33)) ([d053c63](https://github.com/apify/actors-mcp-server/commit/d053c6381939e46da7edce409a529fd1581a8143)) by [@RVCA212](https://github.com/RVCA212)

### Deployment

- Dockerfile and Smithery config ([#29](https://github.com/apify/actors-mcp-server/pull/29)) ([dcd1a91](https://github.com/apify/actors-mcp-server/commit/dcd1a91b83521c58e6dd479054687cb717bf88f2)) by [@calclavia](https://github.com/calclavia)


## [0.1.9](https://github.com/apify/actors-mcp-server/releases/tag/v0.1.9) (2025-02-07)

### 🐛 Bug Fixes

- Stdio and SSE example, improve logging ([#32](https://github.com/apify/actors-mcp-server/pull/32)) ([1b1852c](https://github.com/apify/actors-mcp-server/commit/1b1852cdb49c5de3f8dd48a1d9abc5fd28c58b3a)) by [@jirispilka](https://github.com/jirispilka)


## [0.1.8](https://github.com/apify/actors-mcp-server/releases/tag/v0.1.8) (2025-01-31)

### 🐛 Bug Fixes

- Actor auto loading (corret tool-&gt;Actor name conversion) ([#31](https://github.com/apify/actors-mcp-server/pull/31)) ([45073ea](https://github.com/apify/actors-mcp-server/commit/45073ea49f56784cc4e11bed84c01bcb136b2d8e)) by [@jirispilka](https://github.com/jirispilka)


## [0.1.7](https://github.com/apify/actors-mcp-server/releases/tag/v0.1.7) (2025-01-30)

### 🐛 Bug Fixes

- Add internal tools for Actor discovery ([#28](https://github.com/apify/actors-mcp-server/pull/28)) ([193f098](https://github.com/apify/actors-mcp-server/commit/193f0983255d8170c90109d162589e62ec10e340)) by [@jirispilka](https://github.com/jirispilka)
- Update README.md ([#30](https://github.com/apify/actors-mcp-server/pull/30)) ([23bb32e](https://github.com/apify/actors-mcp-server/commit/23bb32e1f2d5b10d3d557de87cb2d97b5e81921b)) by [@jirispilka](https://github.com/jirispilka)


## [0.1.6](https://github.com/apify/actors-mcp-server/releases/tag/v0.1.6) (2025-01-23)

### 🐛 Bug Fixes

- ClientSse example, update README.md ([#27](https://github.com/apify/actors-mcp-server/pull/27)) ([0449700](https://github.com/apify/actors-mcp-server/commit/0449700a55a8d024e2e1260efa68bb9d0dddec75)) by [@jirispilka](https://github.com/jirispilka)


## [0.1.5](https://github.com/apify/actors-mcp-server/releases/tag/v0.1.5) (2025-01-22)

### 🐛 Bug Fixes

- Add log to stdio ([#25](https://github.com/apify/actors-mcp-server/pull/25)) ([b6e58cd](https://github.com/apify/actors-mcp-server/commit/b6e58cd79f36cfcca1f51b843b5af7ae8e519935)) by [@jirispilka](https://github.com/jirispilka)
- Claude desktop img link ([#26](https://github.com/apify/actors-mcp-server/pull/26)) ([6bd3b75](https://github.com/apify/actors-mcp-server/commit/6bd3b75fb8036e57f6e420392f54030345f0f42d)) by [@jirispilka](https://github.com/jirispilka)


## [0.1.4](https://github.com/apify/actors-mcp-server/releases/tag/v0.1.4) (2025-01-22)

### 🐛 Bug Fixes

- Update README.md ([#22](https://github.com/apify/actors-mcp-server/pull/22)) ([094abc9](https://github.com/apify/actors-mcp-server/commit/094abc95e670c338bd7e90b86f256f4153f92c4d)) by [@jirispilka](https://github.com/jirispilka)
- Remove code check from Release ([#23](https://github.com/apify/actors-mcp-server/pull/23)) ([90cafe6](https://github.com/apify/actors-mcp-server/commit/90cafe6e9b84a237d21ea6d33bfd27a0f81ac915)) by [@jirispilka](https://github.com/jirispilka)


## [0.1.3](https://github.com/apify/actors-mcp-server/releases/tag/v0.1.3) (2025-01-21)

### 🚀 Features

- Update README.md with missing image, and a section on how is MCP related to AI Agents ([#11](https://github.com/apify/actors-mcp-server/pull/11)) ([e922033](https://github.com/apify/actors-mcp-server/commit/e9220332d9ccfbd2efdfb95f07f7c7a52fffc92b)) by [@jirispilka](https://github.com/jirispilka)


## [0.1.2](https://github.com/apify/actors-mcp-server/releases/tag/v0.1.2) (2025-01-21)

### 🚀 Features

- Truncate input schema, limit description to 200 characters ([#10](https://github.com/apify/actors-mcp-server/pull/10)) ([a194765](https://github.com/apify/actors-mcp-server/commit/a1947657fd6f7cf557e5ce24a6bbccb97e875733)) by [@jirispilka](https://github.com/jirispilka)


## [0.1.1](https://github.com/apify/actors-mcp-server/releases/tag/v0.1.1) (2025-01-17)

### 🚀 Features

- MCP server implementation ([#1](https://github.com/apify/actors-mcp-server/pull/1)) ([5e2c9f0](https://github.com/apify/actors-mcp-server/commit/5e2c9f06008304257c887dc3c67eb9ddfd32d6cd)) by [@jirispilka](https://github.com/jirispilka)

### 🐛 Bug Fixes

- Update express routes to correctly handle GET and HEAD requests, fix CI ([#5](https://github.com/apify/actors-mcp-server/pull/5)) ([ec6e9b0](https://github.com/apify/actors-mcp-server/commit/ec6e9b0a4657f673b3650a5906fe00e66411d7f1)) by [@jirispilka](https://github.com/jirispilka)
- Correct publishing of npm module ([#6](https://github.com/apify/actors-mcp-server/pull/6)) ([4c953e9](https://github.com/apify/actors-mcp-server/commit/4c953e9fe0c735f1690c8356884dd78d8608979f)) by [@jirispilka](https://github.com/jirispilka)
</file>

<file path="Dockerfile">
# Generated by https://smithery.ai. See: https://smithery.ai/docs/config#dockerfile
# Stage 1: Build the TypeScript project
FROM node:18-alpine AS builder

# Set working directory
WORKDIR /app

# Copy package files and install dependencies
COPY package.json package-lock.json ./
RUN npm install

# Copy source files
COPY src ./src
COPY tsconfig.json ./

# Build the project
RUN npm run build

# Stage 2: Set up the runtime environment
FROM node:18-alpine

# Set working directory
WORKDIR /app

# Copy only the necessary files from the build stage
COPY --from=builder /app/dist ./dist
COPY package.json package-lock.json ./

# Install production dependencies only
RUN npm ci --omit=dev

# Expose any necessary ports (example: 3000)
EXPOSE 3000

# Set the environment variable for the Apify token
ENV APIFY_TOKEN=<your-apify-token>

# Set the entry point for the container
ENTRYPOINT ["node", "dist/main.js"]
</file>

<file path="eslint.config.mjs">
import apifyTypeScriptConfig from '@apify/eslint-config/ts.js';

// eslint-disable-next-line import/no-default-export
export default [
    { ignores: ['**/dist'] }, // Ignores need to happen first
    ...apifyTypeScriptConfig,
    {
        languageOptions: {
            sourceType: 'module',

            parserOptions: {
                project: 'tsconfig.eslint.json',
            },
        },
    },
];
</file>

<file path="glama.json">
{
    "$schema": "https://glama.ai/mcp/schemas/server.json",
    "maintainers": [ "jirispilka", "mq37" ]
}
</file>

<file path="LICENSE.md">
Apache License
                           Version 2.0, January 2004
                        http://www.apache.org/licenses/

TERMS AND CONDITIONS FOR USE, REPRODUCTION, AND DISTRIBUTION

1. Definitions.

   "License" shall mean the terms and conditions for use, reproduction,
   and distribution as defined by Sections 1 through 9 of this document.

   "Licensor" shall mean the copyright owner or entity authorized by
   the copyright owner that is granting the License.

   "Legal Entity" shall mean the union of the acting entity and all
   other entities that control, are controlled by, or are under common
   control with that entity. For the purposes of this definition,
   "control" means (i) the power, direct or indirect, to cause the
   direction or management of such entity, whether by contract or
   otherwise, or (ii) ownership of fifty percent (50%) or more of the
   outstanding shares, or (iii) beneficial ownership of such entity.

   "You" (or "Your") shall mean an individual or Legal Entity
   exercising permissions granted by this License.

   "Source" form shall mean the preferred form for making modifications,
   including but not limited to software source code, documentation
   source, and configuration files.

   "Object" form shall mean any form resulting from mechanical
   transformation or translation of a Source form, including but
   not limited to compiled object code, generated documentation,
   and conversions to other media types.

   "Work" shall mean the work of authorship, whether in Source or
   Object form, made available under the License, as indicated by a
   copyright notice that is included in or attached to the work
   (an example is provided in the Appendix below).

   "Derivative Works" shall mean any work, whether in Source or Object
   form, that is based on (or derived from) the Work and for which the
   editorial revisions, annotations, elaborations, or other modifications
   represent, as a whole, an original work of authorship. For the purposes
   of this License, Derivative Works shall not include works that remain
   separable from, or merely link (or bind by name) to the interfaces of,
   the Work and Derivative Works thereof.

   "Contribution" shall mean any work of authorship, including
   the original version of the Work and any modifications or additions
   to that Work or Derivative Works thereof, that is intentionally
   submitted to Licensor for inclusion in the Work by the copyright owner
   or by an individual or Legal Entity authorized to submit on behalf of
   the copyright owner. For the purposes of this definition, "submitted"
   means any form of electronic, verbal, or written communication sent
   to the Licensor or its representatives, including but not limited to
   communication on electronic mailing lists, source code control systems,
   and issue tracking systems that are managed by, or on behalf of, the
   Licensor for the purpose of discussing and improving the Work, but
   excluding communication that is conspicuously marked or otherwise
   designated in writing by the copyright owner as "Not a Contribution."

   "Contributor" shall mean Licensor and any individual or Legal Entity
   on behalf of whom a Contribution has been received by Licensor and
   subsequently incorporated within the Work.

2. Grant of Copyright License. Subject to the terms and conditions of
   this License, each Contributor hereby grants to You a perpetual,
   worldwide, non-exclusive, no-charge, royalty-free, irrevocable
   copyright license to reproduce, prepare Derivative Works of,
   publicly display, publicly perform, sublicense, and distribute the
   Work and such Derivative Works in Source or Object form.

3. Grant of Patent License. Subject to the terms and conditions of
   this License, each Contributor hereby grants to You a perpetual,
   worldwide, non-exclusive, no-charge, royalty-free, irrevocable
   (except as stated in this section) patent license to make, have made,
   use, offer to sell, sell, import, and otherwise transfer the Work,
   where such license applies only to those patent claims licensable
   by such Contributor that are necessarily infringed by their
   Contribution(s) alone or by combination of their Contribution(s)
   with the Work to which such Contribution(s) was submitted. If You
   institute patent litigation against any entity (including a
   cross-claim or counterclaim in a lawsuit) alleging that the Work
   or a Contribution incorporated within the Work constitutes direct
   or contributory patent infringement, then any patent licenses
   granted to You under this License for that Work shall terminate
   as of the date such litigation is filed.

4. Redistribution. You may reproduce and distribute copies of the
   Work or Derivative Works thereof in any medium, with or without
   modifications, and in Source or Object form, provided that You
   meet the following conditions:

   (a) You must give any other recipients of the Work or
   Derivative Works a copy of this License; and

   (b) You must cause any modified files to carry prominent notices
   stating that You changed the files; and

   (c) You must retain, in the Source form of any Derivative Works
   that You distribute, all copyright, patent, trademark, and
   attribution notices from the Source form of the Work,
   excluding those notices that do not pertain to any part of
   the Derivative Works; and

   (d) If the Work includes a "NOTICE" text file as part of its
   distribution, then any Derivative Works that You distribute must
   include a readable copy of the attribution notices contained
   within such NOTICE file, excluding those notices that do not
   pertain to any part of the Derivative Works, in at least one
   of the following places: within a NOTICE text file distributed
   as part of the Derivative Works; within the Source form or
   documentation, if provided along with the Derivative Works; or,
   within a display generated by the Derivative Works, if and
   wherever such third-party notices normally appear. The contents
   of the NOTICE file are for informational purposes only and
   do not modify the License. You may add Your own attribution
   notices within Derivative Works that You distribute, alongside
   or as an addendum to the NOTICE text from the Work, provided
   that such additional attribution notices cannot be construed
   as modifying the License.

   You may add Your own copyright statement to Your modifications and
   may provide additional or different license terms and conditions
   for use, reproduction, or distribution of Your modifications, or
   for any such Derivative Works as a whole, provided Your use,
   reproduction, and distribution of the Work otherwise complies with
   the conditions stated in this License.

5. Submission of Contributions. Unless You explicitly state otherwise,
   any Contribution intentionally submitted for inclusion in the Work
   by You to the Licensor shall be under the terms and conditions of
   this License, without any additional terms or conditions.
   Notwithstanding the above, nothing herein shall supersede or modify
   the terms of any separate license agreement you may have executed
   with Licensor regarding such Contributions.

6. Trademarks. This License does not grant permission to use the trade
   names, trademarks, service marks, or product names of the Licensor,
   except as required for reasonable and customary use in describing the
   origin of the Work and reproducing the content of the NOTICE file.

7. Disclaimer of Warranty. Unless required by applicable law or
   agreed to in writing, Licensor provides the Work (and each
   Contributor provides its Contributions) on an "AS IS" BASIS,
   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or
   implied, including, without limitation, any warranties or conditions
   of TITLE, NON-INFRINGEMENT, MERCHANTABILITY, or FITNESS FOR A
   PARTICULAR PURPOSE. You are solely responsible for determining the
   appropriateness of using or redistributing the Work and assume any
   risks associated with Your exercise of permissions under this License.

8. Limitation of Liability. In no event and under no legal theory,
   whether in tort (including negligence), contract, or otherwise,
   unless required by applicable law (such as deliberate and grossly
   negligent acts) or agreed to in writing, shall any Contributor be
   liable to You for damages, including any direct, indirect, special,
   incidental, or consequential damages of any character arising as a
   result of this License or out of the use or inability to use the
   Work (including but not limited to damages for loss of goodwill,
   work stoppage, computer failure or malfunction, or any and all
   other commercial damages or losses), even if such Contributor
   has been advised of the possibility of such damages.

9. Accepting Warranty or Additional Liability. While redistributing
   the Work or Derivative Works thereof, You may choose to offer,
   and charge a fee for, acceptance of support, warranty, indemnity,
   or other liability obligations and/or rights consistent with this
   License. However, in accepting such obligations, You may act only
   on Your own behalf and on Your sole responsibility, not on behalf
   of any other Contributor, and only if You agree to indemnify,
   defend, and hold each Contributor harmless for any liability
   incurred by, or claims asserted against, such Contributor by reason
   of your accepting any such warranty or additional liability.

END OF TERMS AND CONDITIONS

APPENDIX: How to apply the Apache License to your work.

      To apply the Apache License to your work, attach the following
      boilerplate notice, with the fields enclosed by brackets "{}"
      replaced with your own identifying information. (Don't include
      the brackets!)  The text should be enclosed in the appropriate
      comment syntax for the file format. We also recommend that a
      file or class name and description of purpose be included on the
      same "printed page" as the copyright notice for easier
      identification within third-party archives.

Copyright 2024 Apify Technologies s.r.o.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
</file>

<file path="package.json">
{
  "name": "@apify/actors-mcp-server",
  "version": "0.1.30",
  "type": "module",
  "description": "Model Context Protocol Server for Apify",
  "engines": {
    "node": ">=18.0.0"
  },
  "main": "dist/index.js",
  "bin": {
    "actors-mcp-server": "./dist/stdio.js"
  },
  "files": [
    "dist",
    "LICENSE"
  ],
  "repository": {
    "type": "git",
    "url": "https://github.com/apify/actors-mcp-server.git"
  },
  "bugs": {
    "url": "https://github.com/apify/actors-mcp-server/issues"
  },
  "homepage": "https://apify.com/apify/actors-mcp-server",
  "keywords": [
    "apify",
    "mcp",
    "server",
    "actors",
    "model context protocol"
  ],
  "dependencies": {
    "@apify/log": "^2.5.16",
    "@modelcontextprotocol/sdk": "^1.10.1",
    "ajv": "^8.17.1",
    "apify": "^3.4.0",
    "apify-client": "^2.12.1",
    "express": "^4.21.2",
    "minimist": "^1.2.8",
    "zod": "^3.24.1",
    "zod-to-json-schema": "^3.24.1"
  },
  "devDependencies": {
    "@anthropic-ai/sdk": "^0.33.1",
    "@anthropic-ai/tokenizer": "^0.0.4",
    "@apify/eslint-config": "^1.0.0",
    "@apify/tsconfig": "^0.1.0",
    "@types/express": "^4.0.0",
    "@types/minimist": "^1.2.5",
    "@types/yargs-parser": "^21.0.3",
    "dotenv": "^16.4.7",
    "eslint": "^9.19.0",
    "eventsource": "^3.0.2",
    "tsx": "^4.6.2",
    "typescript": "^5.3.3",
    "typescript-eslint": "^8.23.0",
    "vitest": "^3.0.8"
  },
  "scripts": {
    "start": "npm run start:dev",
    "start:prod": "node dist/main.js",
    "start:dev": "tsx src/main.ts",
    "lint": "eslint .",
    "lint:fix": "eslint . --fix",
    "build": "tsc -b src",
    "build:watch": "tsc -b src -w",
    "type-check": "tsc --noEmit",
    "inspector": "npm run build && npx @modelcontextprotocol/inspector dist/stdio.js",
    "test": "npm run test:unit",
    "test:unit": "vitest run tests/unit",
    "test:integration": "npm run build && vitest run tests/integration",
    "clean": "tsc -b src --clean"
  },
  "author": "Apify",
  "license": "MIT"
}
</file>

<file path="README.md">
# Apify Model Context Protocol (MCP) Server

[![Actors MCP Server](https://apify.com/actor-badge?actor=apify/actors-mcp-server)](https://apify.com/apify/actors-mcp-server)
[![smithery badge](https://smithery.ai/badge/@apify/actors-mcp-server)](https://smithery.ai/server/@apify/actors-mcp-server)

Implementation of an MCP server for all [Apify Actors](https://apify.com/store).
This server enables interaction with one or more Apify Actors that can be defined in the MCP Server configuration.

The server can be used in two ways:
- **🇦 [MCP Server Actor](https://apify.com/apify/actors-mcp-server)** – HTTP server accessible via Server-Sent Events (SSE), see [guide](#-mcp-server-actor)
- **⾕ MCP Server Stdio** – Local server available via standard input/output (stdio), see [guide](#-mcp-server-at-a-local-host)

You can also interact with the MCP server using a chat-like UI with 💬 [Tester MCP Client](https://apify.com/jiri.spilka/tester-mcp-client)

# 🎯 What does Apify MCP server do?

The MCP Server Actor allows an AI assistant to use any [Apify Actor](https://apify.com/store) as a tool to perform a specific task.
For example, it can:
- Use [Facebook Posts Scraper](https://apify.com/apify/facebook-posts-scraper) to extract data from Facebook posts from multiple pages/profiles
- Use [Google Maps Email Extractor](https://apify.com/lukaskrivka/google-maps-with-contact-details) to extract Google Maps contact details
- Use [Google Search Results Scraper](https://apify.com/apify/google-search-scraper) to scrape Google Search Engine Results Pages (SERPs)
- Use [Instagram Scraper](https://apify.com/apify/instagram-scraper) to scrape Instagram posts, profiles, places, photos, and comments
- Use [RAG Web Browser](https://apify.com/apify/web-scraper) to search the web, scrape the top N URLs, and return their content

# MCP Clients

To interact with the Apify MCP server, you can use MCP clients such as:
- [Claude Desktop](https://claude.ai/download) (only Stdio support)
- [Visual Studio Code](https://code.visualstudio.com/) (Stdio and SSE support)
- [LibreChat](https://www.librechat.ai/) (Stdio and SSE support, yet without Authorization header)
- [Apify Tester MCP Client](https://apify.com/jiri.spilka/tester-mcp-client) (SSE support with Authorization headers)
- Other clients at [https://modelcontextprotocol.io/clients](https://modelcontextprotocol.io/clients)
- More clients at [https://glama.ai/mcp/clients](https://glama.ai/mcp/clients)

When you have Actors integrated with the MCP server, you can ask:
- "Search the web and summarize recent trends about AI Agents"
- "Find the top 10 best Italian restaurants in San Francisco"
- "Find and analyze the Instagram profile of The Rock"
- "Provide a step-by-step guide on using the Model Context Protocol with source URLs"
- "What Apify Actors can I use?"

The following image shows how the Apify MCP server interacts with the Apify platform and AI clients:

![Actors-MCP-server](https://raw.githubusercontent.com/apify/actors-mcp-server/refs/heads/master/docs/actors-mcp-server.png)

With the MCP Tester client you can load Actors dynamically but this is not yet supported by other MCP clients.
We also plan to add more features, see [Roadmap](#-roadmap-march-2025) for more details.

# 🔄 What is the Model Context Protocol?

The Model Context Protocol (MCP) allows AI applications (and AI agents), such as Claude Desktop, to connect to external tools and data sources.
MCP is an open protocol that enables secure, controlled interactions between AI applications, AI Agents, and local or remote resources.

For more information, see the [Model Context Protocol](https://modelcontextprotocol.org/) website or the blog post [What is MCP and why does it matter?](https://blog.apify.com/what-is-model-context-protocol/).

# 🤖 How is MCP Server related to AI Agents?

The Apify MCP Server exposes Apify's Actors through the MCP protocol, allowing AI Agents or frameworks that implement the MCP protocol to access all Apify Actors as tools for data extraction, web searching, and other tasks.

To learn more about AI Agents, explore our blog post: [What are AI Agents?](https://blog.apify.com/what-are-ai-agents/) and browse Apify's curated [AI Agent collection](https://apify.com/store/collections/ai_agents).
Interested in building and monetizing your own AI agent on Apify? Check out our [step-by-step guide](https://blog.apify.com/how-to-build-an-ai-agent/) for creating, publishing, and monetizing AI agents on the Apify platform.

# 🧱 Components

## Tools

### Actors

Any [Apify Actor](https://apify.com/store) can be used as a tool.
By default, the server is pre-configured with the Actors specified below, but this can be overridden by providing Actor input.

```text
'apify/instagram-scraper'
'apify/rag-web-browser'
'lukaskrivka/google-maps-with-contact-details'
```
The MCP server loads the Actor input schema and creates MCP tools corresponding to the Actors.
See this example of input schema for the [RAG Web Browser](https://apify.com/apify/rag-web-browser/input-schema).

The tool name must always be the full Actor name, such as `apify/rag-web-browser`.
The arguments for an MCP tool represent the input parameters of the Actor.
For example, for the `apify/rag-web-browser` Actor, the arguments are:

```json
{
  "query": "restaurants in San Francisco",
  "maxResults": 3
}
```
You don't need to specify the input parameters or which Actor to call; everything is managed by an LLM.
When a tool is called, the arguments are automatically passed to the Actor by the LLM.
You can refer to the specific Actor's documentation for a list of available arguments.

### Helper tools

The server provides a set of helper tools to discover available Actors and retrieve their details:
- `get-actor-details`: Retrieves documentation, input schema, and details about a specific Actor.
- `discover-actors`: Searches for relevant Actors using keywords and returns their details.

There are also tools to manage the available tools list. However, dynamically adding and removing tools requires the MCP client to have the capability to update the tools list (handle `ToolListChangedNotificationSchema`), which is typically not supported.

You can try this functionality using the [Apify Tester MCP Client](https://apify.com/jiri.spilka/tester-mcp-client) Actor.
To enable it, set the `enableActorAutoLoading` parameter.

- `add-actor-as-tool`: Adds an Actor by name to the available tools list without executing it, requiring user consent to run later.
- `remove-actor-from-tool`: Removes an Actor by name from the available tools list when it's no longer needed.

## Prompt & Resources

The server does not provide any resources and prompts.
We plan to add [Apify's dataset](https://docs.apify.com/platform/storage/dataset) and [key-value store](https://docs.apify.com/platform/storage/key-value-store) as resources in the future.

# ⚙️ Usage

The Apify MCP Server can be used in two ways: **as an Apify Actor** running on the Apify platform
or as a **local server** running on your machine.

## 🇦 MCP Server Actor

### Standby web server

The Actor runs in [**Standby mode**](https://docs.apify.com/platform/actors/running/standby) with an HTTP web server that receives and processes requests.

To start the server with default Actors, send an HTTP GET request with your [Apify API token](https://console.apify.com/settings/integrations) to the following URL:
```
https://actors-mcp-server.apify.actor?token=<APIFY_TOKEN>
```
It is also possible to start the MCP server with a different set of Actors.
To do this, create a [task](https://docs.apify.com/platform/actors/running/tasks) and specify the list of Actors you want to use.

Then, run the task in Standby mode with the selected Actors:
```shell
https://USERNAME--actors-mcp-server-task.apify.actor?token=<APIFY_TOKEN>
```

You can find a list of all available Actors in the [Apify Store](https://apify.com/store).

#### 💬 Interact with the MCP Server over SSE

Once the server is running, you can interact with Server-Sent Events (SSE) to send messages to the server and receive responses.
The easiest way is to use [Tester MCP Client](https://apify.com/jiri.spilka/tester-mcp-client) on Apify.

[Claude Desktop](https://claude.ai/download) currently lacks SSE support, but you can use it with Stdio transport; see [MCP Server at a local host](#-mcp-server-at-a-local-host) for more details.
Note: The free version of Claude Desktop may experience intermittent connection issues with the server.

In the client settings, you need to provide server configuration:
```json
{
    "mcpServers": {
        "apify": {
            "type": "sse",
            "url": "https://actors-mcp-server.apify.actor/sse",
            "env": {
                "APIFY_TOKEN": "your-apify-token"
            }
        }
    }
}
```
Alternatively, you can use [clientSse.ts](https://github.com/apify/actor-mcp-server/tree/main/src/examples/clientSse.ts) script or test the server using `curl` </> commands.

1. Initiate Server-Sent-Events (SSE) by sending a GET request to the following URL:
    ```
    curl https://actors-mcp-server.apify.actor/sse?token=<APIFY_TOKEN>
    ```
    The server will respond with a `sessionId`, which you can use to send messages to the server:
    ```shell
    event: endpoint
    data: /message?sessionId=a1b
    ```

2. Send a message to the server by making a POST request with the `sessionId`:
    ```shell
    curl -X POST "https://actors-mcp-server.apify.actor/message?token=<APIFY_TOKEN>&session_id=a1b" -H "Content-Type: application/json" -d '{
      "jsonrpc": "2.0",
      "id": 1,
      "method": "tools/call",
      "params": {
        "arguments": { "searchStringsArray": ["restaurants in San Francisco"], "maxCrawledPlacesPerSearch": 3 },
        "name": "lukaskrivka/google-maps-with-contact-details"
      }
    }'
    ```
    The MCP server will start the Actor `lukaskrivka/google-maps-with-contact-details` with the provided arguments as input parameters.
    For this POST request, the server will respond with:

    ```text
    Accepted
    ```

3. Receive the response. The server will invoke the specified Actor as a tool using the provided query parameters and stream the response back to the client via SSE.
    The response will be returned as JSON text.

    ```text
    event: message
    data: {"result":{"content":[{"type":"text","text":"{\"searchString\":\"restaurants in San Francisco\",\"rank\":1,\"title\":\"Gary Danko\",\"description\":\"Renowned chef Gary Danko's fixed-price menus of American cuisine ... \",\"price\":\"$100+\"...}}]}}
    ```

## ⾕ MCP Server at a local host

You can run the Apify MCP Server on your local machine by configuring it with Claude Desktop or any other [MCP client](https://modelcontextprotocol.io/clients).
You can also use [Smithery](https://smithery.ai/server/@apify/actors-mcp-server) to install the server automatically.

### Prerequisites

- MacOS or Windows
- The latest version of Claude Desktop must be installed (or another MCP client)
- [Node.js](https://nodejs.org/en) (v18 or higher)
- [Apify API Token](https://docs.apify.com/platform/integrations/api#api-token) (`APIFY_TOKEN`)

Make sure you have the `node` and `npx` installed properly:
```bash
node -v
npx -v
```
If not, follow this guide to install Node.js: [Downloading and installing Node.js and npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm).

#### Claude Desktop

To configure Claude Desktop to work with the MCP server, follow these steps. For a detailed guide, refer to the [Claude Desktop Users Guide](https://modelcontextprotocol.io/quickstart/user) or watch the [video tutorial](https://youtu.be/gf5WXeqydUU?t=440).

1. Download Claude for desktop
   - Available for Windows and macOS.
   - For Linux users, you can build a Debian package using this [unofficial build script](https://github.com/aaddrick/claude-desktop-debian).
2. Open the Claude Desktop app and enable **Developer Mode** from the top-left menu bar.
3. Once enabled, open **Settings** (also from the top-left menu bar) and navigate to the **Developer Option**, where you'll find the **Edit Config** button.
4. Open the configuration file and edit the following file:

    - On macOS: `~/Library/Application\ Support/Claude/claude_desktop_config.json`
    - On Windows: `%APPDATA%/Claude/claude_desktop_config.json`
    - On Linux: `~/.config/Claude/claude_desktop_config.json`

    ```json
    {
     "mcpServers": {
       "actors-mcp-server": {
         "command": "npx",
         "args": ["-y", "@apify/actors-mcp-server"],
         "env": {
            "APIFY_TOKEN": "your-apify-token"
         }
       }
     }
    }
    ```
    Alternatively, you can use the `actors` argument to select one or more Apify Actors:
    ```json
   {
    "mcpServers": {
      "actors-mcp-server": {
        "command": "npx",
        "args": [
          "-y", "@apify/actors-mcp-server",
          "--actors", "lukaskrivka/google-maps-with-contact-details,apify/instagram-scraper"
        ],
        "env": {
           "APIFY_TOKEN": "your-apify-token"
        }
      }
    }
   }
    ```
5. Restart Claude Desktop

    - Fully quit Claude Desktop (ensure it's not just minimized or closed).
    - Restart Claude Desktop.
    - Look for the 🔌 icon to confirm that the Actors MCP server is connected.

6. Open the Claude Desktop chat and ask "What Apify Actors can I use?"

   ![Claude-desktop-with-Actors-MCP-server](https://raw.githubusercontent.com/apify/actors-mcp-server/refs/heads/master/docs/claude-desktop.png)

7. Examples

   You can ask Claude to perform tasks, such as:
    ```text
    Find and analyze recent research papers about LLMs.
    Find the top 10 best Italian restaurants in San Francisco.
    Find and analyze the Instagram profile of The Rock.
    ```

#### VS Code

For one-click installation, click one of the install buttons below:

[![Install with NPX in VS Code](https://img.shields.io/badge/VS_Code-NPM-0098FF?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=actors-mcp-server&config=%7B%22command%22%3A%22npx%22%2C%22args%22%3A%5B%22-y%22%2C%22%40apify%2Factors-mcp-server%22%5D%2C%22env%22%3A%7B%22APIFY_TOKEN%22%3A%22%24%7Binput%3Aapify_token%7D%22%7D%7D&inputs=%5B%7B%22type%22%3A%22promptString%22%2C%22id%22%3A%22apify_token%22%2C%22description%22%3A%22Apify+API+Token%22%2C%22password%22%3Atrue%7D%5D) [![Install with NPX in VS Code Insiders](https://img.shields.io/badge/VS_Code_Insiders-NPM-24bfa5?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=actors-mcp-server&config=%7B%22command%22%3A%22npx%22%2C%22args%22%3A%5B%22-y%22%2C%22%40apify%2Factors-mcp-server%22%5D%2C%22env%22%3A%7B%22APIFY_TOKEN%22%3A%22%24%7Binput%3Aapify_token%7D%22%7D%7D&inputs=%5B%7B%22type%22%3A%22promptString%22%2C%22id%22%3A%22apify_token%22%2C%22description%22%3A%22Apify+API+Token%22%2C%22password%22%3Atrue%7D%5D&quality=insiders)

##### Manual installation

You can manually install the Apify MCP Server in VS Code. First, click one of the install buttons at the top of this section for a one-click installation.

Alternatively, add the following JSON block to your User Settings (JSON) file in VS Code. You can do this by pressing `Ctrl + Shift + P` and typing `Preferences: Open User Settings (JSON)`.

```json
{
  "mcp": {
    "inputs": [
      {
        "type": "promptString",
        "id": "apify_token",
        "description": "Apify API Token",
        "password": true
      }
    ],
    "servers": {
      "actors-mcp-server": {
        "command": "npx",
        "args": ["-y", "@apify/actors-mcp-server"],
        "env": {
          "APIFY_TOKEN": "${input:apify_token}"
        }
      }
    }
  }
}
```

Optionally, you can add it to a file called `.vscode/mcp.json` in your workspace - just omit the top-level `mcp {}` key. This will allow you to share the configuration with others.

If you want to specify which Actors to load, you can add the `--actors` argument:

```json
{
  "servers": {
    "actors-mcp-server": {
      "command": "npx",
      "args": [
        "-y", "@apify/actors-mcp-server",
        "--actors", "lukaskrivka/google-maps-with-contact-details,apify/instagram-scraper"
      ],
      "env": {
        "APIFY_TOKEN": "${input:apify_token}"
      }
    }
  }
}
```

#### VS Code

For one-click installation, click one of the install buttons below:

[![Install with NPX in VS Code](https://img.shields.io/badge/VS_Code-NPM-0098FF?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=actors-mcp-server&config=%7B%22command%22%3A%22npx%22%2C%22args%22%3A%5B%22-y%22%2C%22%40apify%2Factors-mcp-server%22%5D%2C%22env%22%3A%7B%22APIFY_TOKEN%22%3A%22%24%7Binput%3Aapify_token%7D%22%7D%7D&inputs=%5B%7B%22type%22%3A%22promptString%22%2C%22id%22%3A%22apify_token%22%2C%22description%22%3A%22Apify+API+Token%22%2C%22password%22%3Atrue%7D%5D) [![Install with NPX in VS Code Insiders](https://img.shields.io/badge/VS_Code_Insiders-NPM-24bfa5?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=actors-mcp-server&config=%7B%22command%22%3A%22npx%22%2C%22args%22%3A%5B%22-y%22%2C%22%40apify%2Factors-mcp-server%22%5D%2C%22env%22%3A%7B%22APIFY_TOKEN%22%3A%22%24%7Binput%3Aapify_token%7D%22%7D%7D&inputs=%5B%7B%22type%22%3A%22promptString%22%2C%22id%22%3A%22apify_token%22%2C%22description%22%3A%22Apify+API+Token%22%2C%22password%22%3Atrue%7D%5D&quality=insiders)

##### Manual installation

You can manually install the Apify MCP Server in VS Code. First, click one of the install buttons at the top of this section for a one-click installation.

Alternatively, add the following JSON block to your User Settings (JSON) file in VS Code. You can do this by pressing `Ctrl + Shift + P` and typing `Preferences: Open User Settings (JSON)`.

```json
{
  "mcp": {
    "inputs": [
      {
        "type": "promptString",
        "id": "apify_token",
        "description": "Apify API Token",
        "password": true
      }
    ],
    "servers": {
      "actors-mcp-server": {
        "command": "npx",
        "args": ["-y", "@apify/actors-mcp-server"],
        "env": {
          "APIFY_TOKEN": "${input:apify_token}"
        }
      }
    }
  }
}
```

Optionally, you can add it to a file called `.vscode/mcp.json` in your workspace - just omit the top-level `mcp {}` key. This will allow you to share the configuration with others.

If you want to specify which Actors to load, you can add the `--actors` argument:

```json
{
  "servers": {
    "actors-mcp-server": {
      "command": "npx",
      "args": [
        "-y", "@apify/actors-mcp-server",
        "--actors", "lukaskrivka/google-maps-with-contact-details,apify/instagram-scraper"
      ],
      "env": {
        "APIFY_TOKEN": "${input:apify_token}"
      }
    }
  }
}
```

#### Debugging NPM package @apify/actors-mcp-server with @modelcontextprotocol/inspector

To debug the server, use the [MCP Inspector](https://github.com/modelcontextprotocol/inspector) tool:

```shell
export APIFY_TOKEN=your-apify-token
npx @modelcontextprotocol/inspector npx -y @apify/actors-mcp-server
```

### Installing via Smithery

To install Apify Actors MCP Server for Claude Desktop automatically via [Smithery](https://smithery.ai/server/@apify/actors-mcp-server):

```bash
npx -y @smithery/cli install @apify/actors-mcp-server --client claude
```

#### Stdio clients

Create an environment file `.env` with the following content:
```text
APIFY_TOKEN=your-apify-token
```
In the `examples` directory, you can find an example client to interact with the server via
standard input/output (stdio):

- [`clientStdio.ts`](https://github.com/apify/actor-mcp-server/tree/main/src/examples/clientStdio.ts)
    This client script starts the MCP server with two specified Actors.
    It then calls the `apify/rag-web-browser` tool with a query and prints the result.
    It demonstrates how to connect to the MCP server, list available tools, and call a specific tool using stdio transport.
    ```bash
    node dist/examples/clientStdio.js
    ```

# 👷🏼 Development

## Prerequisites

- [Node.js](https://nodejs.org/en) (v18 or higher)
- Python 3.9 or higher

Create an environment file `.env` with the following content:
```text
APIFY_TOKEN=your-apify-token
```

Build the actor-mcp-server package:

```bash
npm run build
```

## Local client (SSE)

To test the server with the SSE transport, you can use the script `examples/clientSse.ts`:
Currently, the Node.js client does not support establishing a connection to a remote server with custom headers.
You need to change the URL to your local server URL in the script.

```bash
node dist/examples/clientSse.js
```

## Debugging

Since MCP servers operate over standard input/output (stdio), debugging can be challenging.
For the best debugging experience, use the [MCP Inspector](https://github.com/modelcontextprotocol/inspector).

You can launch the MCP Inspector via [`npm`](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm) with this command:

```bash
export APIFY_TOKEN=your-apify-token
npx @modelcontextprotocol/inspector node ./dist/stdio.js
```

Upon launching, the Inspector will display a URL that you can access in your browser to begin debugging.

## ⓘ Limitations and feedback

The Actor input schema is processed to be compatible with most MCP clients while adhering to [JSON Schema](https://json-schema.org/) standards. The processing includes:
- **Descriptions** are truncated to 500 characters (as defined in `MAX_DESCRIPTION_LENGTH`).
- **Enum fields** are truncated to a maximum combined length of 200 characters for all elements (as defined in `ACTOR_ENUM_MAX_LENGTH`).
- **Required fields** are explicitly marked with a "REQUIRED" prefix in their descriptions for compatibility with frameworks that may not handle JSON schema properly.
- **Nested properties** are built for special cases like proxy configuration and request list sources to ensure correct input structure.
- **Array item types** are inferred when not explicitly defined in the schema, using a priority order: explicit type in items > prefill type > default value type > editor type.
- **Enum values and examples** are added to property descriptions to ensure visibility even if the client doesn't fully support JSON schema.

Memory for each Actor is limited to 4GB.
Free users have an 8GB limit, 128MB needs to be allocated for running `Actors-MCP-Server`.

If you need other features or have any feedback, [submit an issue](https://console.apify.com/actors/1lSvMAaRcadrM1Vgv/issues) in Apify Console to let us know.

# 🚀 Roadmap (March 2025)

- Add Apify's dataset and key-value store as resources.
- Add tools such as Actor logs and Actor runs for debugging.

# 🐛 Troubleshooting

- Make sure you have the `node` installed by running `node -v`
- Make sure you have the `APIFY_TOKEN` environment variable set
- Always use the latest version of the MCP server by setting `@apify/actors-mcp-server@latest`

# 📚 Learn more

- [Model Context Protocol](https://modelcontextprotocol.org/)
- [What are AI Agents?](https://blog.apify.com/what-are-ai-agents/)
- [What is MCP and why does it matter?](https://blog.apify.com/what-is-model-context-protocol/)
- [How to use MCP with Apify Actors](https://blog.apify.com/how-to-use-mcp/)
- [Tester MCP Client](https://apify.com/jiri.spilka/tester-mcp-client)
- [AI agent workflow: building an agent to query Apify datasets](https://blog.apify.com/ai-agent-workflow/)
- [MCP Client development guide](https://github.com/cyanheads/model-context-protocol-resources/blob/main/guides/mcp-client-development-guide.md)
- [How to build and monetize an AI agent on Apify](https://blog.apify.com/how-to-build-an-ai-agent/)
</file>

<file path="smithery.yaml">
# Smithery configuration file: https://smithery.ai/docs/config#smitheryyaml

startCommand:
  type: stdio
  configSchema:
    # JSON Schema defining the configuration options for the MCP.
    type: object
    required:
      - apifyToken
    properties:
      apifyToken:
        type: string
        description: The API token for accessing Apify's services.
  commandFunction:
    # A function that produces the CLI command to start the MCP on stdio.
    |-
    (config) => ({ command: 'node', args: ['dist/main.js'], env: { APIFY_TOKEN: config.apifyToken } })
</file>

<file path="tsconfig.eslint.json">
{
    "extends": "./tsconfig.json",
    "include": [
        "src",
        "test",
        "tests",
        "vitest.config.ts"
    ],
}
</file>

<file path="tsconfig.json">
{
    "extends": "@apify/tsconfig",
    "compilerOptions": {
        "module": "ES2022",
        "skipLibCheck": true,
    },
}
</file>

<file path="vitest.config.ts">
// eslint-disable-next-line import/extensions
import { defineConfig } from 'vitest/config';

// eslint-disable-next-line import/no-default-export
export default defineConfig({
    test: {
        globals: true,
        environment: 'node',
        include: ['tests/**/*.test.ts'],
        testTimeout: 60_000, // 1 minute
    },
});
</file>

</files>
