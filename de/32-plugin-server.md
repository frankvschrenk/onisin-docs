---
layout: default
title: Plugin-Server
parent: Deutsch
nav_order: 32
---

# Plugin-Server (oosp)

## Was ist oosp?

Der **oosp Plugin-Server** ist ein eigenständiger MCP-Server, der vom OOS Hub als externe Datenquelle genutzt wird. Er ermöglicht es, eigene Datenquellen, Microservices oder externe Systeme in OOS einzubinden — ohne den Hub-Prozess zu verändern.

## Wann oosp verwenden?

- Daten liegen auf einem anderen Server oder in einer anderen Datenbank
- Datenquelle soll isoliert und unabhängig vom Hub laufen
- Eigene Businesslogik soll beim Datenzugriff ausgeführt werden
- Externe APIs sollen als OOS-Contexts eingebunden werden

## Plugin-Server starten

```bash
./oos --plugin /pfad/zu/ctx :9100
```

| Argument | Beschreibung |
|---|---|
| `--plugin` | Startet OOS im Plugin-Modus |
| `/pfad/zu/ctx` | Verzeichnis mit Context-Definitionen für diesen Plugin-Server |
| `:9100` | Adresse und Port |

### HTTP (unverschlüsselt)

```bash
./oos --plugin /pfad/zu/ctx http :9100
```

### HTTPS (mTLS — Standard)

```bash
./oos --plugin /pfad/zu/ctx :9100
```

Ohne explizites `http` startet der Plugin-Server immer mit HTTPS/mTLS.

## TLS / mTLS

Beim Start im HTTPS-Modus:

1. OOS prüft ob ein Zertifikat in Vault unter `OOS_PLUGIN_CERT` / `OOS_PLUGIN_KEY` existiert
2. Falls nicht: OOS generiert ein self-signed ECDSA-Zertifikat
3. Das Zertifikat wird in Vault gespeichert
4. Beim Hub-Connect: Hub lädt das Zertifikat aus Vault und pinnt es (cert-pinning)

```
[plugin] Generiere self-signed TLS Zertifikat...
[plugin] ✅ Vault gespeichert: OOS_PLUGIN_CERT
[plugin] ✅ HTTPS MCP → :9100
```

## Plugin als DSN im Context

In `infra.conf.xml`:

```xml
<dsn name="lager" type="plugin" url="https://localhost:9100/mcp"/>
```

Ein Context kann dann auf diesen Plugin-DSN verweisen:

```xml
<context name="lager_list" kind="collection" source="lager_item" dsn="lager" ...>
```

## oosp_* Tools

Der Plugin-Server stellt dem Hub eigene MCP-Tools bereit:

| Tool | Beschreibung |
|---|---|
| `oosp_query` | Daten vom Plugin laden |
| `oosp_mutation` | Daten über Plugin aktualisieren |
| `oosp_schema` | Plugin-Schema laden |
| `oosp_sources` | Verfügbare Quellen auflisten |

Diese Tools werden vom Hub automatisch aufgerufen — der Benutzer und Claude interagieren immer nur mit den Standard-OOS-Tools.
