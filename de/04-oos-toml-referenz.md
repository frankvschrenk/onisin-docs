---
layout: default
title: oos.toml Referenz
parent: Deutsch
nav_order: 4
---

# oos.toml Referenz

`oos.toml` ist die zentrale Konfigurationsdatei. Sie liegt im selben Verzeichnis wie die OOS-Binary.

---

## [secrets]

| Key | Typ | Beschreibung |
|---|---|---|
| `provider` | string | `"env"` oder `"vault"` |
| `url` | string | Vault URL (nur bei `vault`) |
| `token` | string | Vault Token (nur bei `vault`) |
| `path` | string | Vault KV-Pfad (z.B. `"oos"`) |

```toml
[secrets]
provider = "vault"
url      = "http://localhost:8200"
token    = "hvs.xxxxxxxxxxxx"
path     = "oos"
```

---

## [oos]

| Key | Typ | Beschreibung |
|---|---|---|
| `ctx_dir` | string | Pfad zum Context-Verzeichnis (`.ctx.xml` Dateien) |
| `ctx_zip` | string | Alternativ: Pfad zu einer ZIP-Datei mit Contexts |
| `html_type` | string | `"fs"` (Dateisystem) oder `"embed"` (eingebettet) |
| `html_dir` | string | Pfad zum HTML-Template-Verzeichnis |
| `infra` | string | Pfad zur `infra.conf.xml` |
| `oosp_url` | string | URL des Plugin-Servers (wird durch JWT überschrieben) |

```toml
[oos]
ctx_dir   = "/dein/pfad/oosb/ctx"
html_type = "fs"
html_dir  = "/dein/pfad/oosb/html"
infra     = "/dein/pfad/oosb/ctx/infra.conf.xml"
oosp_url  = "http://localhost:9100"
```

---

## [auth]

| Key | Typ | Beschreibung |
|---|---|---|
| `client_id` | string | OAuth Client ID |
| `authorization_endpoint` | string | OAuth Authorization URL |
| `token_endpoint` | string | OAuth Token URL |
| `redirect_uri` | string | Callback URL (muss in OAuth App registriert sein) |
| `scope` | string | OAuth Scopes (mind. `openid profile email`) |

```toml
[auth]
client_id              = "w4GMl3g0vKLerJtg2W4OxXkgPXZBRY2vZUN8Rz35"
authorization_endpoint = "http://localhost:9000/application/o/authorize/"
token_endpoint         = "http://localhost:9000/application/o/token/"
redirect_uri           = "http://localhost:9999/callback"
scope                  = "openid profile email onisin"
```

---

## [chat]

| Key | Typ | Beschreibung |
|---|---|---|
| `ollama_url` | string | Ollama API URL für lokale LLMs |
| `model` | string | Modellname (z.B. `"qwen2.5:14b"`) |
| `mcp_url` | string | MCP-URL für den integrierten Chat |

```toml
[chat]
ollama_url = "http://localhost:11434"
model      = "qwen2.5:14b"
mcp_url    = "http://localhost:8000/mcp"
```

---

## [contacts]

| Key | Typ | Beschreibung |
|---|---|---|
| `provider` | string | `"carddav"`, `"google"` oder `"microsoft"` |

### [contacts.carddav]

```toml
[contacts.carddav]
url      = "https://p63-contacts.icloud.com"
username = "user@icloud.com"
password = "app-specific-password"
```

### [contacts.google]

```toml
[contacts.google]
token_file = "/dein/pfad/.oos/google_token.json"
```

### [contacts.microsoft]

```toml
[contacts.microsoft]
client_id     = "AZURE_CLIENT_ID"
client_secret = "AZURE_CLIENT_SECRET"
tenant_id     = "AZURE_TENANT_ID"
token_file    = "/dein/pfad/.oos/ms_token.json"
```

---

## [mail]

| Key | Typ | Beschreibung |
|---|---|---|
| `smtp_host` | string | SMTP-Server |
| `smtp_port` | int | SMTP-Port (587 für STARTTLS) |
| `smtp_username` | string | SMTP-Benutzername |
| `smtp_password` | string | SMTP-Passwort |
| `from_name` | string | Absendername |
| `from_email` | string | Absender-E-Mail |

```toml
[mail]
smtp_host     = "smtp.mail.me.com"
smtp_port     = 587
smtp_username = "user@icloud.com"
smtp_password = "app-specific-password"
from_name     = "Max Mustermann"
from_email    = "user@icloud.com"
```

---

## JWT-Überschreibung

Bestimmte `[oos]`-Werte können durch JWT-Claims aus dem OAuth-Login überschrieben werden. Das ermöglicht benutzerspezifische Konfigurationen ohne Neustart.

| JWT Claim | Überschreibt |
|---|---|
| `oos_ctx_dir` | `[oos] ctx_dir` |
| `oos_html_dir` | `[oos] html_dir` |
| `oos_html_type` | `[oos] html_type` |
| `oos_oosp_url` | `[oos] oosp_url` |
| `oos_infra_path` | `[oos] infra` |
