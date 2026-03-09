---
layout: default
title: Secret Management
parent: Deutsch
nav_order: 37
---

# Secret Management

## Übersicht

OOS unterstützt zwei Secret-Backends:

| Provider | Verwendung |
|---|---|
| `env` | Umgebungsvariablen — einfach, für lokale Entwicklung |
| `vault` | HashiCorp Vault KV v2 — sicher, für Produktion |

## Provider: `env`

```toml
[secrets]
provider = "env"
```

OOS liest Secrets aus Umgebungsvariablen mit dem Präfix `OOS_`:

```bash
export OOS_PLUGIN_CERT="-----BEGIN CERTIFICATE-----..."
export OOS_PLUGIN_KEY="-----BEGIN EC PRIVATE KEY-----..."
```

Einfach für lokale Entwicklung — nicht für Produktion empfohlen.

## Provider: `vault`

```toml
[secrets]
provider = "vault"
url      = "http://localhost:8200"
token    = "hvs.xxxxxxxxxxxx"
path     = "oos"
```

OOS verwendet HashiCorp Vault KV v2. Alle Secrets werden unter einem einzigen Pfad als Felder gespeichert.

## Vault KV v2 Setup

### 1. KV v2 Mount aktivieren

```bash
vault secrets enable -path=secret kv-v2
```

### 2. Policy erstellen

```hcl
# oos-policy.hcl
path "secret/data/oos" {
  capabilities = ["create", "read", "update"]
}
```

```bash
vault policy write oos-policy oos-policy.hcl
```

### 3. Token erstellen

```bash
vault token create -policy=oos-policy -ttl=8760h
```

### 4. In `oos.toml` eintragen

```toml
[secrets]
provider = "vault"
url      = "http://localhost:8200"
token    = "hvs.TOKEN_AUS_SCHRITT_3"
path     = "oos"
```

## Automatisches Cert-Management

OOS verwaltet TLS-Zertifikate für den Plugin-Server automatisch über Vault:

| Key | Inhalt |
|---|---|
| `OOS_PLUGIN_CERT` | PEM-kodiertes TLS-Zertifikat |
| `OOS_PLUGIN_KEY` | PEM-kodierter privater Schlüssel |

Beim Start des Plugin-Servers:
1. OOS prüft ob `OOS_PLUGIN_CERT` in Vault existiert
2. Falls nicht: generiert selbst-signiertes Zertifikat und speichert es
3. Hub lädt das Zertifikat beim Connect und pinnt es

Es müssen keine Zertifikate manuell erstellt oder verwaltet werden.
