---
layout: default
title: Plugin Server
parent: English
nav_order: 32
---

# Plugin Server (oosp)

## What is oosp?

The **oosp plugin server** is a standalone MCP server used by the OOS Hub as an external data source. It allows custom data sources, microservices, or external systems to be integrated into OOS — without modifying the Hub process.

## When to Use oosp?

- Data lives on a different server or in a different database
- Data source should run isolated and independently from the Hub
- Custom business logic should run during data access
- External APIs should be integrated as OOS contexts

## Starting the Plugin Server

```bash
./oos --plugin /path/to/ctx :9100
```

| Argument | Description |
|---|---|
| `--plugin` | Starts OOS in plugin mode |
| `/path/to/ctx` | Directory with context definitions for this plugin server |
| `:9100` | Address and port |

### HTTP (unencrypted)

```bash
./oos --plugin /path/to/ctx http :9100
```

### HTTPS (mTLS — default)

```bash
./oos --plugin /path/to/ctx :9100
```

Without an explicit `http`, the plugin server always starts with HTTPS/mTLS.

## TLS / mTLS

When starting in HTTPS mode:

1. OOS checks whether a certificate exists in Vault under `OOS_PLUGIN_CERT` / `OOS_PLUGIN_KEY`
2. If not: OOS generates a self-signed ECDSA certificate
3. The certificate is stored in Vault
4. On Hub connect: Hub loads the certificate from Vault and pins it (cert-pinning)

```
[plugin] Generating self-signed TLS certificate...
[plugin] ✅ Vault stored: OOS_PLUGIN_CERT
[plugin] ✅ HTTPS MCP → :9100
```

## Plugin as DSN in Context

In `infra.conf.xml`:

```xml
<dsn name="warehouse" type="plugin" url="https://localhost:9100/mcp"/>
```

A context can then reference this plugin DSN:

```xml
<context name="warehouse_list" kind="collection" source="warehouse_item" dsn="warehouse" ...>
```

## oosp_* Tools

The plugin server provides the Hub with its own MCP tools:

| Tool | Description |
|---|---|
| `oosp_query` | Load data from plugin |
| `oosp_mutation` | Update data via plugin |
| `oosp_schema` | Load plugin schema |
| `oosp_sources` | List available sources |

These tools are called automatically by the Hub — users and Claude always interact only with the standard OOS tools.
