---
layout: default
title: OAuth / Authentik Setup
parent: English
nav_order: 6
---

# OAuth / Authentik Setup

OOS uses OAuth 2.0 / OIDC for authentication. [Authentik](https://goauthentik.io) is recommended, but any OIDC-compliant provider works.

## Authentik Configuration

### 1. Create OAuth2/OIDC Provider

In Authentik under **Applications → Providers → Create**:

- Type: `OAuth2/OpenID Provider`
- Client ID: note this down (goes into `oos.toml`)
- Client Secret: not required (PKCE)
- Redirect URIs: `http://localhost:9999/callback`
- Scopes: `openid`, `profile`, `email` + custom scope `onisin`

### 2. Define Custom Scope `onisin`

Under **System → Scopes → Create**:

```
Name: onisin
Expression:
return {
  "oos_ctx_dir":   "/your/path/oosb/ctx",
  "oos_html_dir":  "/your/path/oosb/html",
  "oos_html_type": "fs",
  "oos_oosp_url":  "https://localhost:9100",
  "oos_infra_path": "/your/path/oosb/ctx/infra.conf.xml"
}
```

### 3. Fill in oos.toml

```toml
[auth]
client_id              = "YOUR_CLIENT_ID"
authorization_endpoint = "http://localhost:9000/application/o/authorize/"
token_endpoint         = "http://localhost:9000/application/o/token/"
redirect_uri           = "http://localhost:9999/callback"
scope                  = "openid profile email onisin"
```

## JWT Claims

After a successful login, OOS reads the following claims from the JWT:

| Claim | Meaning |
|---|---|
| `oos_ctx_dir` | Context directory |
| `oos_html_dir` | HTML template directory |
| `oos_html_type` | `"fs"` or `"embed"` |
| `oos_oosp_url` | Plugin server URL (http or https) |
| `oos_infra_path` | Path to infra.conf.xml |

JWT claims take precedence over `oos.toml`. This allows different users to receive different contexts and data sources.

## HTTPS for the Plugin Server

When `oos_oosp_url` starts with `https://`, OOS automatically enables mTLS for the connection to the plugin server. The certificate is loaded from Vault.
