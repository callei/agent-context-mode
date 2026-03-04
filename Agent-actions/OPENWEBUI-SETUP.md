# Ollama + OpenWebUI Setup Guide

## Overview

[Ollama](https://ollama.ai) is a local model inference server. [OpenWebUI](https://openwebui.com) is a self-hosted web interface for Ollama (and other backends) that includes MCP tool support via an MCP-to-OpenAI proxy bridge.

---

## Architecture

```
OpenWebUI (browser) → MCP Proxy → context-mode (stdio MCP server)
                              ↑
                     OpenWebUI's built-in bridge
```

OpenWebUI proxies MCP tool calls into OpenAI-compatible function calls, then routes them to the local MCP server over stdio.

---

## Prerequisites

- Ollama installed and running (`ollama serve`)
- OpenWebUI 0.4.0+ (Docker or local install)
- Node.js 18+ available in the environment where OpenWebUI runs
- A tool-capable model pulled (e.g., `ollama pull qwen2.5-coder:7b`)

---

## Step 1: Install context-mode

On the machine running OpenWebUI:

```bash
npm install -g context-mode
```

---

## Step 2: Configure OpenWebUI MCP

Open OpenWebUI → **Admin Panel** → **Settings** → **Tools** → **MCP Servers**.

Add a new server:

| Field | Value |
|-------|-------|
| Server Name | `context-mode` |
| Command | `npx` |
| Arguments | `-y context-mode` |
| Environment Variables | `PROJECT_DIR=/path/to/project` (optional) |

If you installed globally:

| Field | Value |
|-------|-------|
| Command | `context-mode` |

### Alternative: config file

In your OpenWebUI data directory, edit or create `config/mcp.json`:

```json
{
  "mcpServers": {
    "context-mode": {
      "command": "npx",
      "args": ["-y", "context-mode"],
      "env": {
        "PROJECT_DIR": "/workspace"
      }
    }
  }
}
```

---

## Step 3: Enable tools for a model

In a chat session, click the **Tools** icon in the toolbar and enable **context-mode**. Then select a tool-capable Ollama model.

---

## Docker Compose Example

If running OpenWebUI in Docker, ensure Node.js is available in the container:

```yaml
services:
  openwebui:
    image: ghcr.io/open-webui/open-webui:main
    environment:
      - OLLAMA_BASE_URL=http://ollama:11434
    volumes:
      - openwebui_data:/app/backend/data
    # Install Node.js + context-mode at startup
    command: >
      sh -c "curl -fsSL https://deb.nodesource.com/setup_20.x | bash - &&
             apt-get install -y nodejs &&
             npm install -g context-mode &&
             bash start.sh"
```

Or build a custom image with Node.js pre-installed.

---

## Recommended Models (Ollama)

```bash
ollama pull qwen2.5-coder:7b      # best for code tasks
ollama pull mistral:7b-instruct   # good tool calling
ollama pull llama3.1:8b           # solid general purpose
```

Use models tagged `instruct` or `function-calling` for best MCP tool support.

---

## Environment Variables

| Variable | Description |
|----------|-------------|
| `PROJECT_DIR` | Optional. Set to the project directory to enable security policy resolution. |

---

## Verification

In OpenWebUI with a tool-capable model, type:

```
Call the context-mode stats tool.
```

---

## Troubleshooting

**"npx command not found"** — Node.js is not in the PATH of the OpenWebUI process. Install Node.js in the container or use the full path to `npx`.

**"Tool not available"** — Make sure the model supports function calling and that context-mode is enabled in the Tools panel.

**Empty output** — Some models don't reliably format MCP tool calls. Try a different model or use `qwen2.5-coder`.
