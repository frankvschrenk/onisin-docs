---
layout: default
title: OAuth / Authentik Setup
parent: Deutsch
nav_order: 6
---

# OAuth / Authentik Setup

OOS verwendet OAuth 2.0 / OIDC zur Authentifizierung. Empfohlen wird [Authentik](https://goauthentik.io), es funktioniert aber mit jedem OIDC-konformen Provider.

## Authentik Konfiguration

### 1. OAuth2/OIDC Provider anlegen

In Authentik unter **Applications → Providers → Create**:

- Type: `OAuth2/OpenID Provider`
- Client ID: notieren (kommt in `oos.toml`)
- Client Secret: nicht benötigt (PKCE)
- Redirect URIs: `http://localhost:9999/callback`
- Scopes: `openid`, `profile`, `email` + custom scope `onisin`

### 2. Custom Scope `onisin` definieren

Unter **System → Scopes → Create**:

```
Name: onisin
Expression:
return {
  "oos_ctx_dir":   "/Users/frank/oosb/ctx",
  "oos_html_dir":  "/Users/frank/oosb/html",
  "oos_html_type": "fs",
  "oos_oosp_url":  "https://localhost:9100",
  "oos_infra_path": "/Users/frank/oosb/ctx/infra.conf.xml"
}
```

### 3. oos.toml befüllen

```toml
[auth]
client_id              = "DEINE_CLIENT_ID"
authorization_endpoint = "http://localhost:9000/application/o/authorize/"
token_endpoint         = "http://localhost:9000/application/o/token/"
redirect_uri           = "http://localhost:9999/callback"
scope                  = "openid profile email onisin"
```

## JWT Claims

Nach erfolgreichem Login liest OOS folgende Claims aus dem JWT:

| Claim | Bedeutung |
|---|---|
| `oos_ctx_dir` | Context-Verzeichnis |
| `oos_html_dir` | HTML-Template-Verzeichnis |
| `oos_html_type` | `"fs"` oder `"embed"` |
| `oos_oosp_url` | Plugin-Server URL (http oder https) |
| `oos_infra_path` | Pfad zur infra.conf.xml |

JWT-Claims haben Vorrang vor `oos.toml`. So können verschiedene Benutzer unterschiedliche Contexts und Datenquellen erhalten.

## HTTPS für Plugin-Server

Wenn `oos_oosp_url` mit `https://` beginnt, aktiviert OOS automatisch mTLS für die Verbindung zum Plugin-Server. Das Zertifikat wird aus Vault geladen.
