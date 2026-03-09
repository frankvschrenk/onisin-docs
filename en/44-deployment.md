---
layout: default
title: Deployment
parent: English
nav_order: 44
---

# Deployment — Bridge & Claude Desktop

## Bridge Mode

Claude Desktop communicates via **stdio** (JSON-RPC). The OOS Hub is an **HTTP server**. OOS includes a built-in bridge mode — no separate tool required.

### Starting Bridge Mode

```bash
./oos --bridge
```

Or with an explicit URL (default: `http://localhost:8000/mcp`):

```bash
./oos --bridge --url http://localhost:8000/mcp
```

OOS reads from stdin and writes to stdout — exactly as Claude Desktop expects.

### Configuring Claude Desktop

In `~/Library/Application Support/Claude/claude_desktop_config.json` (macOS):

```json
{
  "mcpServers": {
    "oos": {
      "command": "/path/to/oos",
      "args": ["--bridge"],
      "env": {}
    }
  }
}
```

> **Note:** `--bridge` does not appear in `./oos --help` as it is intercepted before the normal startup process. It is available nonetheless.

Windows: `%APPDATA%\Claude\claude_desktop_config.json`

Restart Claude Desktop after saving. OOS will then appear as an available MCP server.

### Verifying the Connection

After restarting Claude Desktop, the hammer icon (🔨) should appear, showing the available OOS tools.

## Deployment Topologies

### Everything local (typical for development)

```
Claude Desktop → oos --bridge → OOS Hub (:8000) → PostgreSQL
                                      └──────────→ oosp (:9100)
```

### Hub on server

```
Claude Desktop → oos --bridge → OOS Hub (server:8000) → PostgreSQL (server)
                                      └──────────────→ oosp (server:9100)
```

In this case, specify the server URL explicitly:

```bash
./oos --bridge --url https://my-server.example.com/mcp
```

## Auto-Updater

OOS automatically checks for a new version at startup:

```
[updater] ✅ Up to date (v0.3.0)
```

or:

```
[updater] 🔄 Update available: v0.3.1 → automatic download
```

The current version and download endpoint are managed in `version.json` on the OOS homepage.
