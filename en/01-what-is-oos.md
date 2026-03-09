---
layout: default
title: What is Onisin OS?
parent: English
nav_order: 1
---

# What is Onisin OS?

Onisin OS (OOS) is an **AI-first enterprise data system**. It connects structured business data with an AI assistant — without traditional GUI development.

## The Idea

Traditional business software separates data, interface, and logic into distinct layers. OOS inverts this principle: the AI is the interface. The user describes what they want to see or do — OOS handles the rest.

## Core Principles

**Contexts instead of Screens**  
Data lives in so-called *Contexts*. A Context describes a data entity (e.g. `person`, `order`) as XML — with fields, types, relations and display rules. OOS automatically generates GraphQL queries and HTML views from this definition.

**MCP as the Backbone**  
OOS implements the [Model Context Protocol (MCP)](https://modelcontextprotocol.io). Claude Desktop (or any other MCP client) connects to OOS and receives a defined set of tools: load, display, edit, and save data.

**AI Workflow — No Direct Writes**  
Data changes always follow a three-step workflow:
1. `oos_query` — load data and display it in the Board
2. `oos_ui_change_required` — show AI changes as a preview
3. `oos_ui_save` — user confirms, data is saved

No direct database writes without user confirmation.

## Components

| Component | Description |
|---|---|
| **OOS Hub** | Main process — MCP server, OAuth login, Board rendering |
| **oosp (Plugin Server)** | External MCP server for custom data sources |
| **mcp-bridge** | stdio→HTTP proxy for Claude Desktop |
| **DSL** | XML-based Context definition |
| **Board** | Display area in the Wails window |

## Technology Stack

- **Go** + **Wails** — Desktop application (macOS, Windows, Linux)
- **GraphQL** — automatically generated from DSL
- **HTML Templates** — data binding via `field=`, `loop=`, `bind=` attributes
- **HashiCorp Vault** — secret management
- **OAuth 2.0 / OIDC** — authentication (e.g. Authentik)
