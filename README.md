[English](./README.md) | [简体中文](./README.zh-CN.md)

# Draw.io.ai

Public Codex skill for setting up a draw.io MCP workflow, automatically opening the live preview in the browser pane, and delivering high-quality `.drawio` + `png` diagram outputs.

The actual skill is in:

- [`draw-io-ai/`](./draw-io-ai)

## Quick Start

Install with:

- `quick start:install [Dapeng960208/draw-io-ai](https://github.com/Dapeng960208/draw-io-ai)`

## Install

Clone this repository:

```bash
git clone https://github.com/Dapeng960208/draw-io-ai.git
cd draw-io-ai
```

Copy the inner `draw-io-ai/` skill folder into your local Codex skills directory.

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

Restart Codex after copying the skill.

When the `drawio` MCP tools are available, this skill is designed to make the agent call `start_session` first instead of manually opening a generic draw.io URL.

## Quick Prompts

Use these prompts for smoke tests or first-run demos.

- `Check whether the drawio MCP is available and open the live preview in the right-side browser pane.`
- `Create a user login flowchart that includes login, MFA, and session management.`
- `Create a React + AWS authentication architecture diagram and export both drawio and png files.`
- `Read the current diagram and convert all connectors into animated flow lines.`

## MCP

This skill is designed for:

- Codex
- Claude Desktop
- Cursor
- VS Code

and other MCP-capable clients using:

- [`@next-ai-drawio/mcp-server`](https://www.npmjs.com/package/@next-ai-drawio/mcp-server)

## License

[MIT](./LICENSE)
