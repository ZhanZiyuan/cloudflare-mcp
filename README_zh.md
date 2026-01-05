# Cloudflare MCP

<p align="right">
    <a href="./README.md">English</a> | <b>简体中文</b>
</p>

[![GitHub last commit](https://img.shields.io/github/last-commit/ZhanZiyuan/cloudflare-mcp)](https://github.com/ZhanZiyuan/cloudflare-mcp/commits/main/)
[![GitHub License](https://img.shields.io/github/license/ZhanZiyuan/cloudflare-mcp)](https://github.com/ZhanZiyuan/cloudflare-mcp/blob/main/LICENSE)
[![GitHub Downloads (all assets, all releases)](https://img.shields.io/github/downloads/ZhanZiyuan/cloudflare-mcp/total)](https://github.com/ZhanZiyuan/cloudflare-mcp/releases)

这是一个为Gemini CLI设计的扩展，集成了Cloudflare的模型上下文协议（MCP）服务器。
通过此扩展，你可以使用自然语言直接与Cloudflare的各项服务（如Workers、R2、DNS、Logs等）进行交互。

此项目利用mcp-remote连接到Cloudflare托管的远程MCP端点。

## 项目目录结构

```text
cloudflare-mcp/
├── commands/
│   └── cloudflare/
│       ├── dev.toml      # 开发相关命令（Workers, Builds, Bindings）
│       ├── ops.toml      # 运维相关命令（Observability, Logs, Audit）
│       └── info.toml     # 信息查询命令（Docs, Radar, GraphQL）
├── GEMINI.md             # AI 助手系统提示词与上下文配置
├── gemini-extension.json # 扩展核心配置及 MCP 服务器注册表
├── README.md             # 英文说明文档
└── README_zh.md          # 中文说明文档
```

## 前置要求

- **Gemini CLI**：确保已安装并配置好 Gemini CLI 环境。
- **Node.js & NPM**：本扩展依赖 npx 来运行 mcp-remote，请确保系统路径中包含 Node.js。
- **Cloudflare 账户**：需要有效的 Cloudflare 账户以获取 API 凭证。

## 配置与安装

### 配置环境变量 (关键步骤)

设置`CLOUDFLARE_API_TOKEN`和`CLOUDFLARE_ACCOUNT_ID`是确保Gemini CLI能够通过MCP协议安全通过Cloudflare身份验证的关键步骤。
没有这些凭证，远程MCP服务器将拒绝连接请求。

你需要从[Cloudflare Dashboard](https://dash.cloudflare.com/profile/api-tokens)获取API Token。
建议创建一个具有你所需服务（如 Workers、DNS、Logs）读写权限的Token。

在你的终端配置文件（如`.bashrc`、`.zshrc`或`PowerShell profile`）中添加以下内容，或在运行Gemini CLI前在当前会话中导出：

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

### 链接扩展

- 在终端中导航到项目根目录，运行以下命令将扩展链接到 Gemini CLI：

    ```bash
    gemini extensions link .
    ```

    链接成功后，重新启动 Gemini CLI 即可生效。

- 或者通过以下命令安装：

    ```bash
    gemini extensions install https://github.com/ZhanZiyuan/cloudflare-mcp
    ```

## 使用方法

此扩展在`cloudflare`组下注册了多个命令。你需要使用`/`前缀加上`组名:命令名`的格式来调用。

- **开发任务（Workers, Builds）**：调用`cloudflare`目录下的`dev.toml`定义的命令：

    ```text
    /cloudflare:dev "列出我最近的Worker构建状态"
    ```

- **运维监控（Logs, Analytics）**：调用`cloudflare`目录下的`ops.toml`定义的命令：

```text
/cloudflare:ops "检查过去一小时的DNS解析错误"
```

- **信息查询（Docs, Radar）**：调用`cloudflare`目录下的`info.toml`定义的命令：

```text
/cloudflare:info "如何在Cloudflare Workers中使用KV存储？"
```

AI会根据[GEMINI.md](./GEMINI.md)中的设定，自动识别你的语言偏好并以中文或英文回复。

---

*基于[Gemini CLI扩展指南](https://geminicli.com/docs/extensions/getting-started-extensions/)与[Cloudflare Agents文档](https://developers.cloudflare.com/agents/model-context-protocol/mcp-servers-for-cloudflare/)构建。*
