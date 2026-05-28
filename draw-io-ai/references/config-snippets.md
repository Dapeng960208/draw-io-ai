# Config Snippets

## Codex Desktop on Windows

Add this to the user's Codex config, typically `$CODEX_HOME/config.toml` or `~/.codex/config.toml`:

```toml
[mcp_servers.drawio]
command = 'npx'
args = ['-y', '@next-ai-drawio/mcp-server@latest']
startup_timeout_sec = 120
```

If the client cannot launch plain `npx` on Windows, resolve the full path with:

```powershell
where.exe npx
```

Then replace `command = 'npx'` with the resolved `npx.cmd` path if needed.

## Codex Desktop on Linux or macOS

Add this to the user's Codex config, typically `$CODEX_HOME/config.toml` or `~/.codex/config.toml`:

```toml
[mcp_servers.drawio]
command = 'npx'
args = ['-y', '@next-ai-drawio/mcp-server@latest']
startup_timeout_sec = 120
```

If `npx` is not on the PATH, resolve it first:

```bash
which npx
```

## Claude Desktop / Cursor / VS Code

```json
{
  "mcpServers": {
    "drawio": {
      "command": "npx",
      "args": ["@next-ai-drawio/mcp-server@latest"]
    }
  }
}
```

## Optional Private Draw.io Base URL

```json
{
  "mcpServers": {
    "drawio": {
      "command": "npx",
      "args": ["@next-ai-drawio/mcp-server@latest"],
      "env": {
        "DRAWIO_BASE_URL": "https://drawio.your-company.com"
      }
    }
  }
}
```

## Optional Port Override

```json
{
  "mcpServers": {
    "drawio": {
      "command": "npx",
      "args": ["@next-ai-drawio/mcp-server@latest"],
      "env": {
        "PORT": "6003"
      }
    }
  }
}
```
