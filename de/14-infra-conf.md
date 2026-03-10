---
layout: default
title: infra.conf.xml
parent: Deutsch
nav_order: 14
---

# infra.conf.xml — Backbone DSNs

`infra.conf.xml` definiert die Datenbankverbindungen (DSNs), die von Contexts verwendet werden können.

## Struktur

```xml
<?xml version="1.0" encoding="UTF-8"?>
<backbone name="backbone">
  <dsn name="demo"  type="postgres" path="postgres://user:pass@localhost:5432/mydb"/>
  <dsn name="lager" type="plugin"   url="http://localhost:9200/mcp"/>
</backbone>
```

## DSN-Typen

### `postgres`
Direkte PostgreSQL-Verbindung.

```xml
<dsn name="demo" type="postgres" path="postgres://user:pass@host:5432/dbname"/>
```

Das `path`-Attribut ist eine Standard-PostgreSQL-Connection-String.

### `plugin`
Datenquelle über einen externen Plugin-Server (oosp).

```xml
<dsn name="lager" type="plugin" url="http://localhost:9200/mcp"/>
```

Das `url`-Attribut zeigt auf den MCP-Endpunkt des Plugin-Servers. HTTP und HTTPS werden unterstützt.

## Verfügbarkeit

Beim Start prüft OOS alle DSNs. Contexts, deren DSN nicht erreichbar ist, werden aus dem AST entfernt und stehen Claude nicht zur Verfügung. So entstehen keine Fehlermeldungen für nicht-verfügbare Quellen.

## Pfad-Konfiguration

Der Pfad zu `infra.conf.xml` wird in `oos.toml` unter `[oos] infra` angegeben — oder per JWT-Claim `oos_infra_path` überschrieben.

```toml
[oos]
infra = "/dein/pfad/oosb/ctx/infra.conf.xml"
```
