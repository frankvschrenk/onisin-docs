---
layout: default
title: MCP Tools
parent: Deutsch
nav_order: 23
---

# MCP Tools — Übersicht

OOS stellt Claude eine definierte Menge von MCP-Tools zur Verfügung. Diese Tools sind das einzige Interface zwischen Claude und OOS.

## Tool-Liste

| Tool | Beschreibung |
|---|---|
| `oos_schema` | AST laden — Contexts, Felder, Prompts |
| `oos_query` | Daten laden und ins Board rendern |
| `oos_render` | JSON direkt ins Board rendern |
| `oos_ui_change_required` | Änderungen als Preview ins Board |
| `oos_ui_save` | Daten speichern |
| `oos_system_status` | Systemstatus abrufen |
| `oos_contacts` | Kontakte laden |
| `oos_mail_compose` | Mail vorbereiten |
| `oos_mail_send` | Mail senden |

---

# `oos_schema`

Lädt den vollständigen OOS-AST und gibt ihn an Claude zurück.

**Claude verwendet dieses Tool immer zuerst**, bevor er Daten abfragt oder ändert.

**Rückgabe:**
- Alle verfügbaren Contexts mit Feldern, Typen, list_fields
- Globale und context-spezifische AI-Prompts
- Locales, Relationen, Navigate-Definitionen

**Wichtig:** Das Schema wird intern verarbeitet. Claude gibt keine Schema-Rohdaten im Chat aus.

---

# `oos_query`

Lädt Daten per GraphQL und rendert sie ins Board.

**Parameter:**
- `context` — Name des Contexts (z.B. `person_list`)
- `query` — GraphQL-Query-String

**Beispiel:**
```
context: "person_list"
query:   "{ person_list { id firstname lastname email city age net_worth } }"
```

**Rückgabe:** `"All data was successfully displayed in the UI."`

**Regeln:**
- Immer `oos_schema` zuerst aufrufen
- Exakt die `list_fields` des Contexts verwenden
- Kein `oos_query` ohne vorheriges Schema-Laden

---

# `oos_render`

Rendert JSON-Daten direkt ins Board — ohne GraphQL.

**Parameter:**
- `context` — Context für das HTML-Template
- `json` — Daten als JSON-String

**Verwendung:** Wenn die Daten bereits vorliegen (z.B. aus einer externen Quelle) und nur angezeigt werden sollen.

```
context: "person_list"
json:    "[{\"id\":1,\"firstname\":\"Max\",\"lastname\":\"Mustermann\"}]"
```

---

# `oos_ui_change_required`

Schreibt KI-Änderungen als Preview ins Board. Die Daten werden noch **nicht** gespeichert.

**Parameter:**
- `context` — Context des aktuellen Formulars
- `changes` — Geänderte Felder als JSON-Objekt

**Beispiel:**
```
context: "person_detail"
changes: {"lastname": "Schmidt", "age": 35}
```

**Workflow:** Immer nach `oos_query`, immer vor `oos_ui_save`.

---

# `oos_ui_save`

Speichert die aktuellen Board-Daten in die Datenbank.

**Keine Parameter.**

OOS scraped automatisch die FormData aus dem Board, baut die GraphQL-Mutation und führt sie aus.

**Voraussetzung:** Der Benutzer muss explizit bestätigt haben, dass er speichern möchte.

---

# `oos_system_status`

Gibt den aktuellen Systemstatus zurück:
- Aktiver Context
- Verfügbare Tools
- DSN-Status

---

# `oos_contacts`

Lädt Kontakte aus dem konfigurierten Provider (CardDAV, Google, Microsoft).

**Parameter:**
- `query` — Suchbegriff (Name, E-Mail)

**Rückgabe:** Liste von Kontakten mit Name, E-Mail, Telefon.

---

# `oos_mail_compose`

Bereitet eine E-Mail vor und zeigt eine Preview im Board.

**Parameter:**
- `to` — Empfänger (Name oder E-Mail aus Kontakten)
- `subject` — Betreff
- `body` — Nachrichtentext

---

# `oos_mail_send`

Sendet die vorbereitete E-Mail aus der aktuellen Session.

**Keine Parameter.**

**Voraussetzung:** `oos_mail_compose` wurde aufgerufen und der Benutzer hat mit "senden" bestätigt.

---

# Der vollständige AI-Workflow

```
1. oos_schema              → Schema laden (einmal pro Session)
2. oos_query               → Daten laden und anzeigen
3. [Benutzer möchte etwas ändern]
4. oos_ui_change_required  → Änderungen als Preview
5. [Benutzer bestätigt]
6. oos_ui_save             → Speichern
```
