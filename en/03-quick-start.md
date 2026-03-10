---
layout: default
title: Quick Start
parent: English
nav_order: 3
---

# Quick Start

## Prerequisites

- OOS binary (`oos` / `oos.exe`)
- Claude Desktop with MCP support
- PostgreSQL database (or plugin server as data source)
- OAuth 2.0 provider (e.g. Authentik) — or local dev mode

## 1. Create the Configuration File

Create `oos.toml` in the same directory as the binary:

```toml
[secrets]
provider = "env"   # or "vault" for production

[oos]
ctx_dir  = "/path/to/your/contexts"
html_dir = "/path/to/your/templates"
html_type = "fs"
infra    = "/path/to/infra.conf.xml"

[auth]
client_id              = "YOUR_CLIENT_ID"
authorization_endpoint = "http://localhost:9000/application/o/authorize/"
token_endpoint         = "http://localhost:9000/application/o/token/"
redirect_uri           = "http://localhost:9999/callback"
scope                  = "openid profile email onisin"
```

## 2. Define Infrastructure

Create `infra.conf.xml`:

```xml
<backbone name="backbone">
  <dsn name="demo" type="postgres" path="postgres://user:pass@localhost:5432/mydb"/>
</backbone>
```

## 3. Create Your First Context

Create `contexts/person.ctx.xml`:

```xml
<oos>
  <context name="person_list" kind="collection" source="person" dsn="demo"
           view="html/pages/person/person.table.html">
    <list_fields>id firstname lastname email</list_fields>
    <field name="id"        type="int"    header="#"/>
    <field name="firstname" type="string" header="First Name"/>
    <field name="lastname"  type="string" header="Last Name"/>
    <field name="email"     type="string" header="E-Mail"/>
  </context>
</oos>
```

## 4. Start OOS

```bash
./oos
```

The browser opens automatically for OAuth login. Once logged in, OOS is ready.

## 5. Connect Claude Desktop

Add to `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "oos": {
      "command": "/path/to/oos",
      "args": ["--bridge"]
    }
  }
}
```

## 6. Run Your First Query

In Claude Desktop:

> "Show me all people"

Claude loads the schema, queries the data, and renders it into the Board.
