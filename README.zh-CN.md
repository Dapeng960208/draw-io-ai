[English](./README.md) | [简体中文](./README.zh-CN.md)

# Draw.io.ai

这是一个公共 Codex skill 仓库，核心能力是帮助 AI 快速搭建 draw.io MCP 工作流、自动在浏览区打开实时预览页面，并交付高质量的 `.drawio` + `png` 图文件。

实际 skill 内容位于：

- [`draw-io-ai/`](./draw-io-ai)

## 安装

先克隆仓库：

```bash
git clone https://github.com/Dapeng960208/draw-io-ai.git
cd draw-io-ai
```

然后把仓库里的 `draw-io-ai/` 子目录复制到本地 Codex 的 skills 目录。

### Linux / macOS

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
cp -R draw-io-ai "${CODEX_HOME:-$HOME/.codex}/skills/draw-io-ai"
```

### Windows PowerShell

```powershell
New-Item -ItemType Directory -Force "$env:USERPROFILE\.codex\skills" | Out-Null
Copy-Item -Path ".\draw-io-ai" -Destination "$env:USERPROFILE\.codex\skills\draw-io-ai" -Recurse -Force
```

复制完成后重启 Codex。

## MCP

这个 skill 主要面向以下客户端：

- Codex
- Claude Desktop
- Cursor
- VS Code

以及其他支持 MCP 的客户端，配套服务为：

- [`@next-ai-drawio/mcp-server`](https://www.npmjs.com/package/@next-ai-drawio/mcp-server)

## 许可证

[MIT](./LICENSE)
