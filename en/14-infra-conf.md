---
layout: default
title: infra.conf.xml
parent: English
nav_order: 14
---

# infra.conf.xml — Backbone DSNs

`infra.conf.xml` defines the database connections (DSNs) that contexts can use.

## Structure

```xml
<?xml version="1.0" encoding="UTF-8"?>
<backbone name="backbone">
  <dsn name="demo"  type="postgres" path="postgres://user:pass@localhost:5432/mydb"/>
  <dsn name="lager" type="plugin"   url="http://localhost:9200/mcp"/>
</backbone>
```

## DSN Types

### `postgres`
Direct PostgreSQL connection.

```xml
<dsn name="demo" type="postgres" path="postgres://user:pass@host:5432/dbname"/>
```

The `path` attribute is a standard PostgreSQL connection string.

### `plugin`
Data source via an external plugin server (oosp).

```xml
<dsn name="lager" type="plugin" url="http://localhost:9200/mcp"/>
```

The `url` attribute points to the MCP endpoint of the plugin server. Both HTTP and HTTPS are supported.

## Availability

At startup, OOS checks all DSNs. Contexts whose DSN is unreachable are removed from the AST and are not available to Claude. This prevents error messages for unavailable sources.

## Path Configuration

The path to `infra.conf.xml` is specified in `oos.toml` under `[oos] infra` — or overridden via the JWT claim `oos_infra_path`.

```toml
[oos]
infra = "/your/path/oosb/ctx/infra.conf.xml"
```
