# Cloudflare MCP

<p align="right">
    <b>English</b> | <a href="./README_zh.md">简体中文</a>
</p>

[![GitHub last commit](https://img.shields.io/github/last-commit/ZhanZiyuan/cloudflare-mcp)](https://github.com/ZhanZiyuan/cloudflare-mcp/commits/main/)
[![GitHub License](https://img.shields.io/github/license/ZhanZiyuan/cloudflare-mcp)](https://github.com/ZhanZiyuan/cloudflare-mcp/blob/main/LICENSE)
[![GitHub Downloads (all assets, all releases)](https://img.shields.io/github/downloads/ZhanZiyuan/cloudflare-mcp/total)](https://github.com/ZhanZiyuan/cloudflare-mcp/releases)

This is an extension for the Gemini CLI that integrates Cloudflare's Model Context Protocol (MCP) servers.
It allows you to interact with various Cloudflare services (such as Workers, R2, DNS, Logs, etc.) using natural language.

This project uses mcp-remote to connect to Cloudflare-hosted remote MCP endpoints.

## Project Directory Structure

```text
cloudflare-mcp/
├── commands/
│   └── cloudflare/
│       ├── dev.toml      # Development commands (Workers, Builds, Bindings)
│       ├── ops.toml      # Operations commands (Observability, Logs, Audit)
│       └── info.toml     # Information commands (Docs, Radar, GraphQL)
├── GEMINI.md             # AI assistant system prompts and context configuration
├── gemini-extension.json # Core extension configuration and MCP server registry
├── README.md             # English documentation
└── README_zh.md          # Chinese documentation
```

## Prerequisites

- **Gemini CLI**: Ensure the Gemini CLI environment is installed and configured.
- **Node.js & NPM**: This extension relies on npx to run mcp-remote. Ensure Node.js is in your system path.
- **Cloudflare Account**: A valid Cloudflare account is required to obtain API credentials.

## Configuration and Installation

### Configure Environment Variables (Critical Step)

Setting the `CLOUDFLARE_API_TOKEN` and `CLOUDFLARE_ACCOUNT_ID` is a critical step to ensure the Gemini CLI can securely authenticate with Cloudflare via the MCP protocol. Without these credentials, the remote MCP servers will reject connection requests.

You need to obtain an API Token from the [Cloudflare Dashboard](https://dash.cloudflare.com/profile/api-tokens). It is recommended to create a Token with Read/Write permissions for the services you intend to use (e.g., Workers, DNS, Logs).

Add the following to your terminal configuration file (e.g., `.bashrc`, `.zshrc`, or `PowerShell profile`), or export them in the current session before running the Gemini CLI:

- **macOS / Linux:**

    ```bash
    export CLOUDFLARE_API_TOKEN="your_api_token"
    export CLOUDFLARE_ACCOUNT_ID="your_account_id"
    ```

- **Windows (PowerShell):**

    ```powershell
    $env:CLOUDFLARE_API_TOKEN="your_api_token"
    $env:CLOUDFLARE_ACCOUNT_ID="your_account_id"
    ```

### Link the Extension

- Navigate to the project root directory in your terminal and run the following command to link the extension to the Gemini CLI:

    ```bash
    gemini extensions link .
    ```

    Once linked, restart the Gemini CLI for changes to take effect.

- Or install the extension using the following command:

    ```bash
    gemini extensions install https://github.com/ZhanZiyuan/cloudflare-mcp
    ```

## Usage

This extension registers commands under the `cloudflare` group. You invoke them using the `/` prefix followed by `group:command`.

- **Development Tasks (Workers, Builds)**: invokes commands defined in `commands/cloudflare/dev.toml`:

    ```text
    /cloudflare:dev "List my recent Worker build statuses"
    ```

- **Operations & Monitoring (Logs, Analytics)**: invokes commands defined in `commands/cloudflare/ops.toml`:

    ```text
    /cloudflare:ops "Check for DNS resolution errors in the last hour"
    ```

- **Information Retrieval (Docs, Radar)**: invokes commands defined in `commands/cloudflare/info.toml`:

    ```text
    /cloudflare:info "How do I use KV storage in Cloudflare Workers?"
    ```

The AI will automatically detect your language preference and respond in English or Chinese
based on the configuration in [GEMINI.md](./GEMINI.md).

---

*Based on the [Gemini CLI Extensions Guide](https://geminicli.com/docs/extensions/getting-started-extensions/) and [Cloudflare Agents docs](https://developers.cloudflare.com/agents/model-context-protocol/mcp-servers-for-cloudflare/).*
