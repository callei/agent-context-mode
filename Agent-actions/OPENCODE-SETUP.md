# OpenCode Setup Guide

## What is OpenCode?

[OpenCode](https://opencode.ai) is an open-source, terminal-based AI coding assistant that supports the Model Context Protocol (MCP). It is the primary target platform for this MCP server.

---

## Prerequisites

- Node.js 18+ installed
- OpenCode installed (`npm install -g opencode-ai` or via release binary)

---

## Installation

### Option 1: npx (no install required)

Add to your OpenCode configuration at `~/.config/opencode/opencode.json`:

```json
{
  "mcp": {
    "context-mode": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "context-mode"]
    }
  }
}
```

### Option 2: Global npm install

```bash
npm install -g context-mode
```

Then in `~/.config/opencode/opencode.json`:

```json
{
  "mcp": {
    "context-mode": {
      "type": "stdio",
      "command": "context-mode"
    }
  }
}
```

### Option 3: Local clone

```bash
git clone https://github.com/callei/OpenCode-context-mode
cd OpenCode-context-mode
npm install
npm run build
```

Then in `~/.config/opencode/opencode.json`:

```json
{
  "mcp": {
    "context-mode": {
      "type": "stdio",
      "command": "node",
      "args": ["/path/to/OpenCode-context-mode/build/server.js"]
    }
  }
}
```

---

## Environment Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `PROJECT_DIR` | current working directory | Set to your project root for security policy resolution |

OpenCode automatically sets the working directory to your project root when launching MCP servers, so you typically don't need to set `PROJECT_DIR` manually.

---

## Available Tools

Once configured, OpenCode can call these MCP tools:

| Tool | Description |
|------|-------------|
| `batch_execute` | Run multiple shell commands + search all output in one call |
| `execute` | Run code in 11 languages in a sandboxed subprocess |
| `execute_file` | Process files in sandbox without loading contents into context |
| `index` | Chunk and index markdown/docs into FTS5 knowledge base |
| `search` | Query indexed content with BM25 ranking |
| `fetch_and_index` | Fetch a URL, convert to markdown, index into knowledge base |
| `stats` | Show context savings statistics for the current session |

---

## Verification

Start OpenCode in your project directory. The MCP server should appear in the server list. You can verify it is running by asking:

```
Use the context-mode stats tool to show session statistics.
```

---

## Performance Tip

Install [Bun](https://bun.sh) for 3–5× faster JavaScript/TypeScript execution inside the sandbox:

```bash
curl -fsSL https://bun.sh/install | bash
```
