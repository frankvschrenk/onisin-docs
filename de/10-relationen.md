---
layout: default
title: Relationen
parent: Deutsch
nav_order: 10
---

# Relationen

Relationen verknüpfen zwei Contexts miteinander. Sie beschreiben, wie Datensätze zusammenhängen.

## Syntax

```xml
<relation name="notes" context="note_list" type="has_many" bind="id -> person_id"/>
```

| Attribut | Beschreibung |
|---|---|
| `name` | Name der Relation (für KI und Navigation) |
| `context` | Ziel-Context |
| `type` | Beziehungstyp |
| `bind` | `lokales_feld -> fremdes_feld` |

## Beziehungstypen

### `has_many`
Eine Person hat viele Notizen.

```xml
<!-- In person_list / person_detail: -->
<relation name="notes" context="note_list" type="has_many" bind="id -> person_id"/>
```

→ `note_list` wird mit `person_id = <aktuelle id>` gefiltert

### `has_one`
Ein Benutzer hat ein Profil.

```xml
<relation name="profile" context="user_profile" type="has_one" bind="id -> user_id"/>
```

### `belongs_to`
Eine Notiz gehört zu einer Person.

```xml
<!-- In note_detail: -->
<relation name="person" context="person_detail" type="belongs_to" bind="person_id -> id"/>
```

## Zusammenspiel mit Navigate

Relationen beschreiben die *Datenstruktur*. `<navigate>` beschreibt die *UI-Navigation*. Beide können kombiniert werden:

```xml
<!-- Datenstruktur: person hat viele notes -->
<relation name="notes" context="note_list" type="has_many" bind="id -> person_id"/>

<!-- UI: Klick auf "Notizen"-Button öffnet note_list gefiltert nach person_id -->
<navigate event="on_notes" to="note_list" bind="id -> person_id"/>
```
