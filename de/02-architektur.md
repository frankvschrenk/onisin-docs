---
layout: default
title: Architektur
parent: Deutsch
nav_order: 2
---

# Architektur-Übersicht

## Komponenten-Diagramm

```
Claude Desktop (stdio)
        │
        ▼
mcp-bridge (:stdio → HTTP)
        │
        ▼
OOS Hub (:8000/mcp)          ← MCP-Server + Wails-Desktop
        │
        ├── DSL / AST         ← XML Context-Definitionen
        ├── GraphQL Engine    ← auto-generiert aus DSL
        ├── HTML Board        ← Wails-Fenster
        │
        └── HTTPS/mTLS ──────▶ oosp Plugin-Server (:9100)
                                      │
                                      └── eigene Datenquellen
```

## OOS Hub

Der Hub ist der zentrale Prozess. Er startet als Wails-Desktop-App und stellt gleichzeitig einen MCP-Server auf Port `8000` bereit.

**Aufgaben:**
- OAuth-Login (Authentik oder lokaler Modus)
- DSL laden und AST aufbauen
- GraphQL-Schema generieren
- Datenbankverbindungen verwalten (DSN-Registry)
- MCP-Tools für Claude bereitstellen
- HTML ins Wails-Board rendern

## oosp (Plugin-Server)

Der Plugin-Server ist ein separater Prozess, der ebenfalls als MCP-Server läuft — aber mit eigenem Context-Verzeichnis und eigenen Datenquellen.

Der Hub verbindet sich via **HTTPS/mTLS** mit dem Plugin-Server. Das TLS-Zertifikat wird automatisch generiert und in Vault gespeichert.

**Wann wird oosp benötigt?**  
Wenn Datenquellen vom Hub-Prozess getrennt laufen sollen — z.B. auf einem anderen Server oder als isolierter Microservice.

## mcp-bridge

Claude Desktop kommuniziert per **stdio** (JSON-RPC über Stdin/Stdout). Der OOS Hub ist aber ein HTTP-Server. Die `mcp-bridge` übersetzt zwischen beiden Protokollen.

```
Claude Desktop
    stdio (JSON-RPC)
         │
    mcp-bridge
         │
    HTTP POST /mcp
         │
    OOS Hub
```

## Datenfluss: Query

```
Claude → oos_query("person_list", "{ person_list { id firstname ... } }")
    → GraphQL Execute
    → SQL SELECT ... FROM person
    → JSON-Daten
    → HTML-Template rendern
    → Wails Board aktualisieren
    → "Daten im Board angezeigt"
```

## Datenfluss: Mutation

```
Claude → oos_ui_change_required("person_detail", {"lastname": "Müller"})
    → Stage-Daten mergen
    → HTML-Template rendern (Preview)
    → Wails Board aktualisieren (noch nicht gespeichert)

Benutzer bestätigt →

Claude → oos_ui_save()
    → FormData aus Board scrapen
    → GraphQL Mutation bauen
    → SQL UPDATE ... WHERE id = ?
    → Erfolg
```

## Secret-Management

OOS unterstützt zwei Secret-Backends:

| Provider | Verwendung |
|---|---|
| `env` | Umgebungsvariablen — für lokale Entwicklung |
| `vault` | HashiCorp Vault KV v2 — für Produktion |

Alle Keys werden unter einem einzigen Vault-Pfad als Felder gespeichert (z.B. `secret/oos`).
