---
layout: default
title: Navigate
parent: Deutsch
nav_order: 11
---

# Navigate — Navigation zwischen Contexts

`<navigate>` definiert, wie der Benutzer von einem Context zu einem anderen navigiert.

## Syntax

```xml
<navigate event="on_select" to="person_detail" bind="id -> id"/>
```

| Attribut | Beschreibung |
|---|---|
| `event` | Auslöser (Name des Events im HTML-Template) |
| `to` | Ziel-Context |
| `bind` | `lokales_feld -> fremdes_feld` |

## Events

Events werden im HTML-Template ausgelöst, z.B. per Button-Klick oder Zeilen-Klick.

### Standard-Events

| Event | Beschreibung |
|---|---|
| `on_select` | Klick auf eine Tabellenzeile → öffnet Detail-Context |
| `on_notes` | Klick auf "Notizen"-Button → öffnet verknüpfte Liste |
| `on_back` | Zurück-Navigation |

Eigene Event-Namen sind möglich — sie müssen nur im Template und in `<navigate>` übereinstimmen.

## Bind-Syntax

```
lokales_feld -> fremdes_feld
```

Das `lokale_feld` ist ein Feldname im aktuellen Context. Das `fremde_feld` ist ein Feldname im Ziel-Context.

**Beispiele:**

```xml
<!-- person_list → person_detail: id bleibt id -->
<navigate event="on_select" to="person_detail" bind="id -> id"/>

<!-- person_detail → note_list: person.id wird zu note.person_id -->
<navigate event="on_notes" to="note_list" bind="id -> person_id"/>

<!-- note_detail → person_detail: note.person_id wird zu person.id -->
<navigate event="on_person" to="person_detail" bind="person_id -> id"/>
```

## Zusammenspiel mit dem Board

Wenn die KI eine Navigation auslöst (durch einen Benutzerklick im Board), sendet das Template ein Event an OOS. OOS löst den passenden `<navigate>`-Block auf, lädt den Ziel-Context mit dem gebundenen Feldwert und rendert ihn ins Board.
