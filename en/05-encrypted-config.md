---
layout: default
title: Encrypted Config
parent: English
nav_order: 5
---

# Encrypted Config (oos.enc)

OOS supports encrypted configuration. Sensitive values from `oos.toml` can be stored in an encrypted file `oos.enc`.

## Encryption

```bash
./oos crypt encrypt oos.toml
```

The password is prompted interactively. The encrypted file is saved as `oos.enc`.

## Usage

```bash
OOS_PASSWORD=myPassword ./oos
```

OOS automatically detects whether `oos.enc` is present and decrypts the configuration on startup.

## Security Note

`oos.toml` should be removed from the filesystem and version control after encryption. Only commit `oos.enc`.
