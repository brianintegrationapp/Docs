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
src/
  index.ts
.editorconfig
.eslintrc.js
.gitignore
.prettierignore
.prettierrc
package.json
README.md
tsconfig.json
</directory_structure>

<files>
This section contains the contents of the repository's files.

<file path="src/index.ts">
#!/usr/bin/env node

import { Server } from '@modelcontextprotocol/sdk/server/index.js'
import { StdioServerTransport } from '@modelcontextprotocol/sdk/server/stdio.js'
import {
  CallToolRequestSchema,
  ListToolsRequestSchema,
} from '@modelcontextprotocol/sdk/types.js'
import { Tool } from '@modelcontextprotocol/sdk/types.js'
import { IntegrationAppClient } from '@integration-app/sdk'


const client = new IntegrationAppClient({
  token: process.env.INTEGRATION_APP_TOKEN,
})

export async function startServer() {
  if (!process.env.INTEGRATION_KEY) {
    throw new Error('INTEGRATION_KEY is not set')
  }

  if (!process.env.INTEGRATION_APP_TOKEN) {
    throw new Error('INTEGRATION_APP_TOKEN is not set')
  }

  const integrationKey = process.env.INTEGRATION_KEY
  const integration = await client.integration(integrationKey).get()

  const server = new Server(
    {
      name: `integration-app-${integrationKey}`,
      version: '1.0.0',
      description: `Working with ${integration.name}.`,
    },
    {
      capabilities: {
        tools: {},
      },
    },
  )

  const tools = await getToolsFromActions(integrationKey)

  server.setRequestHandler(ListToolsRequestSchema, async () => ({
    tools,
  }))

  server.setRequestHandler(CallToolRequestSchema, async (request) => {
    try {
      const { name, arguments: args } = request.params

      const result = await callToolFromAction(integrationKey, name, args)

      return {
        content: [{ type: 'text', text: JSON.stringify(result) }],
        isError: false,
      }
    } catch (error) {
      return {
        content: [
          {
            type: 'text',
            text: `Error: ${error instanceof Error ? error.message : String(error)}`,
          },
        ],
        isError: true,
      }
    }
  })

  const transport = new StdioServerTransport()
  await server.connect(transport)
}

export async function getToolsFromActions(integrationKey: string): Promise<Tool[]> {
  const integration = await client
    .integration(integrationKey)
    .get()

  const actions = await client.actions.find({
    integrationId: integration.id,
  })

  const tools: Tool[] = []

  for (const action of actions.items) {
    tools.push({
      name: action.key,
      description: action.name,
      inputSchema: (action.inputSchema as Tool['inputSchema']) || {
        type: 'object',
        properties: undefined,
      },
    })
  }

  return tools
}

export async function callToolFromAction(integrationKey: string, name: string, args: any) {
  const result = await client
    .actionInstance({
      autoCreate: true,
      integrationKey,
      parentKey: name,
    })
    .run(args)
  return result.output
}

void startServer()
</file>

<file path=".editorconfig">
root = true

[*]
charset = utf-8
end_of_line = lf
trim_trailing_whitespace = true
insert_final_newline = true

[*.md]
insert_final_newline = false
trim_trailing_whitespace = false

[*.{css,js,jsx,json,ts,tsx,yml}]
indent_size = 2
indent_style = space

[*.py]
indent_size = 4
indent_style = space
</file>

<file path=".eslintrc.js">
/**
 * @type {import("eslint").Linter.Config}
 */
const config = {
  parserOptions: {
    ecmaVersion: 'latest',
    sourceType: 'module',
    project: 'tsconfig.json',
    tsconfigRootDir: __dirname,
  },
  ignorePatterns: [
    '*.d.ts',
    '*.js',
    '*.css',
    'dist/**',
    '.github/**/*',
    '*.example.jsx',
  ],
  extends: [
    'plugin:prettier/recommended',
  ],
  root: true,
  env: {
    node: true,
    jest: true,
  },
  rules: {
    'prettier/prettier': ['error', {
      "endOfLine": "auto"
    }],
    'no-console': ['error', { allow: ['error', 'debug', 'warn'] }],
  },
  overrides: [
    {
      files: ['*.ts', '*.tsx'],
      parser: '@typescript-eslint/parser',
      plugins: [
        '@typescript-eslint/eslint-plugin', 'unused-imports'
      ],
      extends: ['plugin:@typescript-eslint/recommended',],
      rules: {
        '@typescript-eslint/no-empty-interface': 'off',
        '@typescript-eslint/no-empty-function': 'off',
        '@typescript-eslint/interface-name-prefix': 'off',
        '@typescript-eslint/explicit-function-return-type': 'off',
        '@typescript-eslint/explicit-module-boundary-types': 'off',
        '@typescript-eslint/no-explicit-any': ['off', {
          ignoreRestArgs: true,
        }],
        '@typescript-eslint/ban-ts-comment': 'off',
        '@typescript-eslint/no-var-requires': 'off',
        '@typescript-eslint/no-floating-promises': 'error',
        "@typescript-eslint/no-unused-vars": "off", // handled by no-unused-imports
        "unused-imports/no-unused-imports": "error",
        "unused-imports/no-unused-vars": [
          "warn",
          {
            "vars": "all",
            "varsIgnorePattern": "^_",
            "args": "after-used",
            "argsIgnorePattern": "^_",
            "caughtErrors": "all",
            "caughtErrorsIgnorePattern": "^_",
            "destructuredArrayIgnorePattern": "^_"
          }
        ],
      },
    },
  ]
}

module.exports = config
</file>

<file path=".gitignore">
node_modules
dist
</file>

<file path=".prettierignore">
**/*/.github
**/*/.next
**/*/node_modules
**/*.js
**/*.d.ts
**/*.yml
**/*.json
**/*.md
**/*.mdx
**/*.example.jsx
**/*.nvmrc
**/*.prettierignore
</file>

<file path=".prettierrc">
{
  "trailingComma": "all",
  "semi": false,
  "jsxSingleQuote": true,
  "singleQuote": true,
  "tabWidth": 2,
  "useTabs": false,
  "bracketSameLine": false,
  "arrowParens": "always"
}
</file>

<file path="package.json">
{
  "name": "@integration-app/mcp-server",
  "version": "1.0.3",
  "description": "Model Context Protocol server for Integration.app",
  "main": "dist/index.js",
  "types": "dist/index.d.ts",
  "bin": {
    "@integration-app/mcp-server": "./dist/index.js"
  },
  "scripts": {
    "start": "tsx src/index.ts",
    "build": "tsc",
    "postbuild": "chmod +x dist/index.js",
    "prepublishOnly": "npm run build"
  },
  "type": "module",
  "files": [
    "dist",
    "README.md"
  ],
  "dependencies": {
    "@integration-app/sdk": "^1.5.1",
    "@modelcontextprotocol/sdk": "^1.1.1"
  },
  "devDependencies": {
    "@types/eslint": "^8.56.12",
    "@types/node": "^22.10.1",
    "@typescript-eslint/eslint-plugin": "^7.18.0",
    "@typescript-eslint/parser": "^7.18.0",
    "eslint": "^8.57.1",
    "eslint-config-prettier": "^9.1.0",
    "eslint-plugin-prettier": "^5.2.1",
    "eslint-plugin-unused-imports": "^3.0.0",
    "prettier": "^3.3.3",
    "tsx": "^4.19.2",
    "typescript": "^5.7.2"
  },
  "publishConfig": {
    "access": "public"
  }
}
</file>

<file path="README.md">
# Integration App MCP Server 

## Overview 

This is an implementation of the [Model Context Protocol (MCP) server](https://modelcontextprotocol.org/) that exposes tools powered by [Integration App](https://integration.app).

## Managing Tools

This server uses Actions defined in an Integration App workspace as tools. 
To understand how this works and how to effectively manage tools for each application, please refer to the [Using Tools](https://console.integration.app/docs/use-cases/ai/use-tools) guide.

## Running the server

1. Install [node.js](https://nodejs.org)
2. Configure some actions in your Integration App workspace
3. Get Integration App token from your [Workspace Settings](https://console.integration.app/w/0/settings/testing) page or generate using your Workspace Key and Secret ([Authentication Guide](https://console.integration.app/w/625eb136b4af031bffb2e9eb/docs/getting-started/authentication)).

You need to provide two environment variables to the server:
* `INTEGRATION_APP_TOKEN` - token for accessing Integration App API
* `INTEGRATION_KEY` - key of the integration you want to use tools for

This server exposes tools from one integration at a time. If you want to expose tools from multiple integrations, you can create multiple servers or modify the code to iterate over multiple integrations.

Here is an example of claude_desktop_config.json file with the server configured: 

```json
{
  "mcpServers": {
    "integration-app-hubspot": {
      "command": "npx",
      "args": ["-y", "@integration-app/mcp-server"],
      "env": {
         "INTEGRATION_APP_TOKEN": "<your-integration-app-token>",
         "INTEGRATION_KEY": "hubspot"
      }
    }
  }
}
```

## Testing 

To understand if everything works as expected, you can ask Claude what tools are available: 

![Claude Test](https://github.com/user-attachments/assets/693aba6f-d7ee-47ad-9fcd-a966e1935214)
</file>

<file path="tsconfig.json">
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "ES2020",
    "moduleResolution": "node",
    "esModuleInterop": true,
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "declaration": true,
    "skipLibCheck": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist"]
}
</file>

</files>
