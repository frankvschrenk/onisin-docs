---
layout: default
title: Relations
parent: English
nav_order: 10
---

# Relations

Relations link two contexts together. They describe how records are connected.

## Syntax

```xml
<relation name="notes" context="note_list" type="has_many" bind="id -> person_id"/>
```

| Attribute | Description |
|---|---|
| `name` | Relation name (for AI and navigation) |
| `context` | Target context |
| `type` | Relationship type |
| `bind` | `local_field -> foreign_field` |

## Relationship Types

### `has_many`
A person has many notes.

```xml
<!-- In person_list / person_detail: -->
<relation name="notes" context="note_list" type="has_many" bind="id -> person_id"/>
```

→ `note_list` is filtered with `person_id = <current id>`

### `has_one`
A user has one profile.

```xml
<relation name="profile" context="user_profile" type="has_one" bind="id -> user_id"/>
```

### `belongs_to`
A note belongs to a person.

```xml
<!-- In note_detail: -->
<relation name="person" context="person_detail" type="belongs_to" bind="person_id -> id"/>
```

## Interaction with Navigate

Relations describe the *data structure*. `<navigate>` describes *UI navigation*. Both can be combined:

```xml
<!-- Data structure: person has many notes -->
<relation name="notes" context="note_list" type="has_many" bind="id -> person_id"/>

<!-- UI: clicking "Notes" button opens note_list filtered by person_id -->
<navigate event="on_notes" to="note_list" bind="id -> person_id"/>
```
