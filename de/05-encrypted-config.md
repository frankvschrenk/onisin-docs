---
layout: default
title: Encrypted Config
parent: Deutsch
nav_order: 5
---

# Encrypted Config (oos.enc)

OOS unterstützt verschlüsselte Konfiguration. Damit können sensible Werte aus `oos.toml` in eine verschlüsselte Datei `oos.enc` ausgelagert werden.

## Verschlüsselung

```bash
./oos crypt encrypt oos.toml
```

Das Passwort wird interaktiv abgefragt. Die verschlüsselte Datei wird als `oos.enc` gespeichert.

## Verwendung

```bash
OOS_PASSWORD=meinPasswort ./oos
```

OOS erkennt automatisch ob `oos.enc` vorhanden ist und entschlüsselt die Konfiguration beim Start.

## Sicherheitshinweis

`oos.toml` sollte nach der Verschlüsselung aus dem Dateisystem und aus der Versionskontrolle entfernt werden. Nur `oos.enc` committen.
