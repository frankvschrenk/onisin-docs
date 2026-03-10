---
layout: default
title: Contacts & Mail
parent: Deutsch
nav_order: 40
---

# Contacts & Mail

## Contacts

OOS unterstützt drei Contact-Provider:

### CardDAV (iCloud, Nextcloud, etc.)

```toml
[contacts]
provider = "carddav"

[contacts.carddav]
url      = "https://p63-contacts.icloud.com"
username = "user@icloud.com"
password = "app-specific-password"
```

Für iCloud: App-spezifisches Passwort unter https://appleid.apple.com erstellen.

### Google Contacts

```toml
[contacts]
provider = "google"

[contacts.google]
token_file = "/dein/pfad/.oos/google_token.json"
```

Beim ersten Start führt OOS den OAuth-Flow für Google durch und speichert das Token in `token_file`.

**Voraussetzung:** Google Cloud Projekt mit aktivierter People API und OAuth2 Credentials.

### Microsoft / Outlook

```toml
[contacts]
provider = "microsoft"

[contacts.microsoft]
client_id     = "AZURE_CLIENT_ID"
client_secret = "AZURE_CLIENT_SECRET"
tenant_id     = "AZURE_TENANT_ID"
token_file    = "/dein/pfad/.oos/ms_token.json"
```

**Voraussetzung:** Azure App Registration mit `Contacts.Read` Permission.

---

## Mail (SMTP)

```toml
[mail]
smtp_host     = "smtp.mail.me.com"
smtp_port     = 587
smtp_username = "user@icloud.com"
smtp_password = "app-specific-password"
from_name     = "Max Mustermann"
from_email    = "user@icloud.com"
```

Port `587` mit STARTTLS wird empfohlen. Port `465` (SSL) wird ebenfalls unterstützt.

## Mail-Workflow mit Claude

```
1. "Schreibe eine Mail an Anna Schmidt über das Meeting morgen"
   → oos_contacts("Anna Schmidt")     ← E-Mail-Adresse suchen
   → oos_mail_compose(to, subject, body)  ← Preview im Board
   
2. Benutzer prüft die Mail im Board

3. "Senden" / "Send"
   → oos_mail_send()                  ← Mail wird abgeschickt
```
