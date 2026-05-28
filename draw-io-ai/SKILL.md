---
name: draw-io-ai
description: Install, configure, verify, and use the Next AI Draw.io MCP server for Codex, Claude Desktop, Cursor, VS Code, and similar MCP clients. Use when the user asks to set up `@next-ai-drawio/mcp-server`, fix drawio MCP configuration, open a live diagram preview in the right-side review/browser pane, or quickly create, edit, and export `.drawio` diagrams with real-time browser preview.
---

# Draw.io.ai

## Overview

Use this skill for the fastest path from "I want draw.io MCP" to a working live diagram session.

Prefer the MCP workflow over manual XML-only workflows when the client can expose the `drawio` MCP tools.

## Core Rule

- In Codex or another client that supports an embedded review/browser pane, call `start_session` yourself as soon as the MCP tools are available so the diagram page opens on the right side and the user can immediately see live updates.
- Do not wait for the user to open the URL manually unless `start_session` is unavailable.
- If the client does not have an embedded pane, open the returned browser URL in the normal browser flow.

## Fast Path

1. Determine whether the `drawio` MCP tools are already available.
2. If the tools are missing, install or configure the MCP server first.
3. After the tools become available, immediately call `start_session`.
4. Create or edit the diagram.
5. Export both `.drawio` and `png` when the task is delivering a finished figure.

## Install And Configure

### Codex Desktop

- Edit the user's active Codex config file, typically `$CODEX_HOME/config.toml` or `~/.codex/config.toml`.
- Add an `mcp_servers.drawio` entry.
- On Linux and macOS, prefer plain `npx` in the MCP command.
- On Windows, prefer the resolved `npx.cmd` path if the client cannot launch plain `npx`.
- Use `args = ['-y', '@next-ai-drawio/mcp-server@latest']`.
- Set `startup_timeout_sec = 120`.
- Ask the user to restart Codex after the config change.

Read `references/config-snippets.md` for exact config examples.

### Claude Desktop / Cursor / VS Code

- Add the same MCP server using the client's JSON config format.
- Use `command = npx` and `args = ["@next-ai-drawio/mcp-server@latest"]` unless the client needs a Windows-specific `npx.cmd` path.
- On Linux, do not hardcode Windows executable paths; keep the command portable unless the client requires an absolute path.
- After editing the config, restart the client before verification.

## Verification Workflow

### Server Package Check

- If you need to verify installation without waiting for the client to reload, run:

```powershell
npx -y @next-ai-drawio/mcp-server@latest --help
```

- Treat logs such as `Starting MCP server` and `MCP server running on stdio` as proof that the package can launch.

### Tool Availability Check

- Confirm the client now exposes the `drawio` MCP tools.
- In Codex, once the tool is available, use it directly instead of continuing with shell-only verification.

### Session Check

- Call `start_session` immediately after the tools appear.
- Confirm that the browser page opens and returns a session URL.
- Keep that session alive while editing.

## Diagram Workflow

### Start

- Always call `start_session` before diagram creation when a visible live preview is helpful.
- If the user wants to see changes live, this is mandatory.

### Delivery Standard

- Unless the user explicitly asks otherwise, deliver both:
  - one editable `.drawio` source file
  - one matching `png` export
- Keep filenames aligned so the pair is obviously related.
- Do not deliver only a screenshot or only a browser preview when the task is asking for a finished diagram artifact.
- If revising an existing figure, update the editable source and the exported image together.

### Create

- Use `create_new_diagram` when starting from scratch.
- Use concise but complete XML with explicit geometry and connector anchors.
- Finish the layout to a presentation-ready state instead of stopping at rough boxes and arrows.

### Edit

- Always call `get_diagram` immediately before `edit_diagram`.
- Do not skip this. The MCP server rejects edit calls when the latest browser state was not fetched first.
- Preserve the user's meaning unless the user explicitly asks for a redesign.

### Export

- Export `.drawio` and `png` together when finishing a figure.
- If export fails with `ENOENT`, create the target directory first and retry.
- Prefer a white or very light background unless the user explicitly requests transparency.
- Ensure labels, arrowheads, and outer shapes are not clipped at the export bounds.
- Keep a small outer margin so the image does not look cramped.

## Diagram Quality Rules

### Layout

- Prefer one dominant reading direction per figure: top-to-bottom or left-to-right.
- Keep sibling nodes aligned to a visible grid rather than almost-aligned by eye.
- Use concise noun phrases inside nodes; shorten wording before shrinking readability.
- Keep related elements visually balanced and similarly sized unless size is encoding meaning.
- Use containers or background bands when grouped subsystems need stronger visual boundaries.

### Connectors

- Use orthogonal routing by default for structured architecture and workflow diagrams.
- Attach connectors to explicit side anchors so flow direction is visually obvious.
- When a connector needs turns, use explicit bend points or waypoints rather than accepting awkward auto-routing.
- Do not let connectors run through text, cut across boxes, or hide arrowheads.
- Reduce line crossings aggressively. If a connector becomes crowded, move nodes instead of compressing the path.
- Keep connector stroke style and arrowhead style consistent for the same relationship type.
- Use connector labels only when direction and layout are not enough to explain the meaning.

### Typography And Readability

- Use a clean sans-serif style consistently across the figure.
- Make titles and major section labels visibly stronger than node text.
- Resize boxes to fit text cleanly instead of forcing text against borders.
- Keep terminology consistent across all nodes; do not switch between synonyms for the same concept.
- Ensure the final `png` is readable at normal viewing scale, not only after zooming.

## Final Quality Gate

Do not consider the task complete until all of these are true:

- The `.drawio` and `png` outputs both exist and correspond to the same final content.
- The browser preview and exported files match.
- The reading order is obvious without extra explanation.
- There are no avoidable line crossings, cramped labels, or clipped elements.
- The diagram looks intentional and presentation-ready, not like a raw auto-layout draft.

## Quick Usage Patterns

### User wants setup only

- Configure the MCP server.
- Restart the client.
- Verify the tool exists.

### User wants a visible diagram quickly

- Ensure the MCP tool exists.
- Call `start_session` so the page opens in the right-side review/browser pane.
- Create a small sample diagram first if needed to prove the pipeline works.

### User wants to revise a live diagram

- Call `get_diagram`.
- Apply targeted `edit_diagram` updates.
- Re-export only after the live view looks correct.

## Common Pitfalls

- `drawio` tool missing after config change:
  Restart the client. Config edits are not picked up by the current session.
- `You must call get_diagram first before edit_diagram`:
  Fetch the current diagram immediately before editing.
- Export `ENOENT`:
  Create the destination directory first.
- No live page visible:
  Call `start_session` yourself instead of only sharing the URL.
- Port conflict on `6002`:
  The server may auto-select another port, or you can set `PORT` explicitly in MCP env.

## References

- Read `references/config-snippets.md` for exact Codex and Claude config blocks.
- Read `references/quickstart-prompts.md` for short prompts that are useful for smoke tests and first-run demos.
