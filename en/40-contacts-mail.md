---
layout: default
title: Contacts & Mail
parent: English
nav_order: 40
---

# Contacts & Mail

## Contacts

OOS supports three contact providers:

### CardDAV (iCloud, Nextcloud, etc.)

```toml
[contacts]
provider = "carddav"

[contacts.carddav]
url      = "https://p63-contacts.icloud.com"
username = "user@icloud.com"
password = "app-specific-password"
```

For iCloud: create an app-specific password at https://appleid.apple.com.

### Google Contacts

```toml
[contacts]
provider = "google"

[contacts.google]
token_file = "/your/path/.oos/google_token.json"
```

On first start, OOS runs the OAuth flow for Google and stores the token in `token_file`.

**Prerequisite:** Google Cloud project with People API enabled and OAuth2 credentials.

### Microsoft / Outlook

```toml
[contacts]
provider = "microsoft"

[contacts.microsoft]
client_id     = "AZURE_CLIENT_ID"
client_secret = "AZURE_CLIENT_SECRET"
tenant_id     = "AZURE_TENANT_ID"
token_file    = "/your/path/.oos/ms_token.json"
```

**Prerequisite:** Azure App Registration with `Contacts.Read` permission.

---

## Mail (SMTP)

```toml
[mail]
smtp_host     = "smtp.mail.me.com"
smtp_port     = 587
smtp_username = "user@icloud.com"
smtp_password = "app-specific-password"
from_name     = "John Smith"
from_email    = "user@icloud.com"
```

Port `587` with STARTTLS is recommended. Port `465` (SSL) is also supported.

## Mail Workflow with Claude

```
1. "Write an email to Anna Schmidt about tomorrow's meeting"
   → oos_contacts("Anna Schmidt")         ← find email address
   → oos_mail_compose(to, subject, body)  ← preview in Board

2. User reviews the mail in the Board

3. "Send"
   → oos_mail_send()                      ← mail is sent
```
