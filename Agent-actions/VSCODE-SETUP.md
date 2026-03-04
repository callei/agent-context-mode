# VS Code Setup Guide

## Supported VS Code Extensions

`context-mode` can be used as an MCP server with VS Code through:

- **GitHub Copilot Chat** (VS Code 1.99+) — built-in MCP support
- **Cursor** — fork of VS Code with native MCP support
- **Continue** (`continue.dev`) — open-source AI coding extension with MCP support

---

## GitHub Copilot Chat (VS Code 1.99+)

### Project-scoped configuration

Create `.vscode/mcp.json` in your project root:

```json
{
  "servers": {
    "context-mode": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "context-mode"],
      "env": {
        "PROJECT_DIR": "${workspaceFolder}"
      }
    }
  }
}
```

### User-scoped configuration

Open VS Code settings (`Ctrl+,` / `Cmd+,`), search for `mcp`, and edit `settings.json`:

```json
{
  "mcp": {
    "servers": {
      "context-mode": {
        "type": "stdio",
        "command": "npx",
        "args": ["-y", "context-mode"]
      }
    }
  }
}
```

---

## Cursor

Add to `~/.cursor/mcp.json` (global) or `.cursor/mcp.json` (project):

```json
{
  "mcpServers": {
    "context-mode": {
      "command": "npx",
      "args": ["-y", "context-mode"],
      "env": {
        "PROJECT_DIR": "/path/to/your/project"
      }
    }
  }
}
```

---

## Continue (`continue.dev`)

Add to `~/.continue/config.json`:

```json
{
  "mcpServers": [
    {
      "name": "context-mode",
      "command": "npx",
      "args": ["-y", "context-mode"]
    }
  ]
}
```

---

## Environment Variables

| Variable | Description |
|----------|-------------|
| `PROJECT_DIR` | Optional. Set to your project root to enable security policy file resolution. VS Code's `${workspaceFolder}` variable is supported in `.vscode/mcp.json`. |

---

## Verification

Once configured, open GitHub Copilot Chat (or your extension's chat interface) and ask:

```
Use context-mode stats to show session statistics.
```

The server should respond with a context savings table.

---

## Notes

- **VS Code MCP support** requires VS Code 1.99 or later with GitHub Copilot Chat enabled
- The server communicates over **stdio** — no ports or network configuration required
- All tool calls run in isolated subprocesses; no file system writes outside the sandbox
