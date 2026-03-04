# LM Studio Setup Guide

## Overview

[LM Studio](https://lmstudio.ai) is a desktop application for running local language models. LM Studio 0.3.6+ includes an MCP client that allows local models to call MCP tools.

---

## Prerequisites

- LM Studio 0.3.6 or later
- Node.js 18+ installed on your system

---

## Configuration

### Step 1: Install context-mode

```bash
npm install -g context-mode
```

Or use npx without installing (requires internet on first use):

```bash
npx -y context-mode --version
```

### Step 2: Add to LM Studio

Open LM Studio → **Developer** tab → **MCP Servers** → **Add Server**.

Fill in the form:

| Field | Value |
|-------|-------|
| Name | `context-mode` |
| Command | `npx` |
| Args | `-y context-mode` |
| Environment | `PROJECT_DIR=/path/to/your/project` (optional) |

Or, if globally installed:

| Field | Value |
|-------|-------|
| Name | `context-mode` |
| Command | `context-mode` |
| Args | _(leave blank)_ |

### Step 3: Enable for a model

In the **Chat** view, select a model, then enable **context-mode** under the tool panel.

---

## Configuration File

LM Studio stores MCP configurations in `~/.lmstudio/mcp.json`. You can also edit it directly:

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

## Environment Variables

| Variable | Description |
|----------|-------------|
| `PROJECT_DIR` | Optional. Project root directory for security policy resolution. |

---

## Recommended Models

The `batch_execute` and `execute` tools work best with models that have strong instruction-following and code generation capabilities:

- **Qwen2.5-Coder** (7B or 14B) — strong code execution reasoning
- **Mistral 7B Instruct** — good tool-calling support
- **Llama 3.1 8B Instruct** — solid general-purpose tool use
- Any model with `function_calling` or `tool_use` support

---

## Verification

In LM Studio Chat, with a tool-capable model selected, ask:

```
Use context-mode stats to show session statistics.
```

---

## Notes

- LM Studio's MCP support requires a model that supports tool/function calling
- The stdio transport is used — no network ports needed
- Performance depends on the local model's ability to correctly format tool calls
