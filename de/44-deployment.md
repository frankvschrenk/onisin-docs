---
layout: default
title: Deployment
parent: Deutsch
nav_order: 44
---

# Deployment — Bridge & Claude Desktop

## Bridge-Modus

Claude Desktop kommuniziert per **stdio** (JSON-RPC). Der OOS Hub ist ein **HTTP-Server**. OOS bringt den Bridge-Modus direkt mit — kein separates Tool nötig.

### Bridge starten

```bash
./oos --bridge
```

Oder mit expliziter URL (Standard: `http://localhost:8000/mcp`):

```bash
./oos --bridge --url http://localhost:8000/mcp
```

OOS liest von stdin und schreibt nach stdout — genau wie es Claude Desktop erwartet.

### Claude Desktop konfigurieren

In `~/Library/Application Support/Claude/claude_desktop_config.json` (macOS):

```json
{
  "mcpServers": {
    "oos": {
      "command": "/pfad/zu/oos",
      "args": ["--bridge"],
      "env": {}
    }
  }
}
```

> **Hinweis:** `--bridge` erscheint nicht in `./oos --help` da es vor dem normalen Start-Prozess abgefangen wird. Es ist aber vorhanden.

Windows: `%APPDATA%\Claude\claude_desktop_config.json`

Nach dem Speichern Claude Desktop neu starten. OOS erscheint dann als verfügbarer MCP-Server.

### Verbindung prüfen

In Claude Desktop sollte nach dem Neustart das Hammer-Symbol (🔨) erscheinen, das die verfügbaren OOS-Tools anzeigt.

## Deployment-Topologie

### Alles lokal (typisch für Entwicklung)

```
Claude Desktop → mcp-bridge → OOS Hub (:8000) → PostgreSQL
                                    └──────────→ oosp (:9100)
```

### Hub auf Server

```
Claude Desktop → mcp-bridge → OOS Hub (server:8000) → PostgreSQL (server)
                                    └──────────────→ oosp (server:9100)
```

In diesem Fall muss `mcp-bridge` mit der Server-URL gestartet werden:

```bash
./mcp-bridge https://mein-server.example.com/mcp
```

## Auto-Updater

OOS prüft beim Start automatisch ob eine neue Version verfügbar ist:

```
[updater] ✅ Aktuell (v0.3.0)
```

oder:

```
[updater] 🔄 Update verfügbar: v0.3.1 → automatischer Download
```

Die aktuelle Version und der Download-Endpunkt werden in `version.json` auf der OOS-Homepage verwaltet.
