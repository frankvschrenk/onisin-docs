---
layout: default
title: Quick Start
parent: Deutsch
nav_order: 3
---

# Quick Start

## Voraussetzungen

- OOS Binary (`oos` / `oos.exe`)
- Claude Desktop mit MCP-Unterstützung
- PostgreSQL-Datenbank (oder Plugin-Server als Datenquelle)
- OAuth 2.0 Provider (z.B. Authentik) — oder lokaler Dev-Modus

## 1. Konfigurationsdatei anlegen

Erstelle `oos.toml` im selben Verzeichnis wie die Binary:

```toml
[secrets]
provider = "env"   # oder "vault" für Produktion

[oos]
ctx_dir  = "/pfad/zu/deinen/contexts"
html_dir = "/pfad/zu/deinen/templates"
html_type = "fs"
infra    = "/pfad/zu/infra.conf.xml"

[auth]
client_id              = "DEINE_CLIENT_ID"
authorization_endpoint = "http://localhost:9000/application/o/authorize/"
token_endpoint         = "http://localhost:9000/application/o/token/"
redirect_uri           = "http://localhost:9999/callback"
scope                  = "openid profile email onisin"
```

## 2. Infrastruktur definieren

Erstelle `infra.conf.xml`:

```xml
<backbone name="backbone">
  <dsn name="demo" type="postgres" path="postgres://user:pass@localhost:5432/mydb"/>
</backbone>
```

## 3. Ersten Context anlegen

Erstelle `contexts/person.ctx.xml`:

```xml
<oos>
  <context name="person_list" kind="collection" source="person" dsn="demo"
           view="html/pages/person/person.table.html">
    <list_fields>id firstname lastname email</list_fields>
    <field name="id"        type="int"    header="#"/>
    <field name="firstname" type="string" header="Vorname"/>
    <field name="lastname"  type="string" header="Nachname"/>
    <field name="email"     type="string" header="E-Mail"/>
  </context>
</oos>
```

## 4. OOS starten

```bash
./oos
```

Der Browser öffnet sich automatisch für den OAuth-Login. Nach erfolgreichem Login ist OOS bereit.

## 5. Claude Desktop verbinden

Füge in `claude_desktop_config.json` hinzu:

```json

{
  "mcpServers": {
    "oos": {
      "command": "/pfad/zu/oos",
      "args": ["--bridge"]
    }
  }
}
```

## 6. Ersten Query stellen

In Claude Desktop:

> „Zeige mir alle Personen"

Claude lädt das Schema, fragt die Daten ab und rendert sie ins Board.
