---
layout: default
title: Was ist Onisin OS?
parent: Deutsch
nav_order: 1
---

# Was ist Onisin OS?

Onisin OS (OOS) ist ein **AI-first Enterprise-Datensystem**. Es verbindet strukturierte Unternehmensdaten mit einem KI-Assistenten — ohne klassische GUI-Entwicklung.

## Die Idee

Traditionelle Business-Software trennt Daten, Oberfläche und Logik in separate Schichten. OOS dreht dieses Prinzip um: Die KI ist die Oberfläche. Der Anwender beschreibt, was er sehen oder tun möchte — OOS erledigt den Rest.

## Kernprinzipien

**Contexts statt Screens**  
Daten leben in sogenannten *Contexts*. Ein Context beschreibt eine Datenentität (z.B. `person`, `order`) als XML — mit Feldern, Typen, Relationen und Anzeigeregeln. OOS generiert daraus automatisch GraphQL-Abfragen und HTML-Views.

**MCP als Rückgrat**  
OOS implementiert das [Model Context Protocol (MCP)](https://modelcontextprotocol.io). Claude Desktop (oder ein anderer MCP-Client) verbindet sich mit OOS und erhält eine definierte Werkzeugpalette: Daten laden, anzeigen, bearbeiten, speichern.

**AI-Workflow — kein direktes Schreiben**  
Änderungen an Daten folgen immer einem dreistufigen Workflow:
1. `oos_query` — Daten laden und im Board anzeigen
2. `oos_ui_change_required` — KI-Änderungen als Preview einblenden
3. `oos_ui_save` — Benutzer bestätigt, Daten werden gespeichert

Kein direktes Schreiben in die Datenbank ohne Benutzerbestätigung.

## Komponenten

| Komponente | Beschreibung |
|---|---|
| **OOS Hub** | Hauptprozess — MCP-Server, OAuth-Login, Board-Rendering |
| **oosp (Plugin-Server)** | Externer MCP-Server für eigene Datenquellen |
| **mcp-bridge** | stdio→HTTP Proxy für Claude Desktop |
| **DSL** | XML-basierte Context-Definition |
| **Board** | Anzeigebereich im Wails-Fenster |

## Technologie-Stack

- **Go** + **Wails** — Desktop-Applikation (macOS, Windows, Linux)
- **GraphQL** — automatisch aus DSL generiert
- **HTML Templates** — data-binding via `field=`, `loop=`, `bind=` Attribute
- **HashiCorp Vault** — Secret Management
- **OAuth 2.0 / OIDC** — Authentifizierung (z.B. Authentik)
