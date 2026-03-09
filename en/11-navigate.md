---
layout: default
title: Navigate
parent: English
nav_order: 11
---

# Navigate — Navigation Between Contexts

`<navigate>` defines how the user navigates from one context to another.

## Syntax

```xml
<navigate event="on_select" to="person_detail" bind="id -> id"/>
```

| Attribute | Description |
|---|---|
| `event` | Trigger (event name in the HTML template) |
| `to` | Target context |
| `bind` | `local_field -> foreign_field` |

## Events

Events are triggered in the HTML template, e.g. via button click or row click.

### Standard Events

| Event | Description |
|---|---|
| `on_select` | Click on a table row → opens detail context |
| `on_notes` | Click on "Notes" button → opens linked list |
| `on_back` | Back navigation |

Custom event names are possible — they just need to match between the template and `<navigate>`.

## Bind Syntax

```
local_field -> foreign_field
```

The `local_field` is a field name in the current context. The `foreign_field` is a field name in the target context.

**Examples:**

```xml
<!-- person_list → person_detail: id stays id -->
<navigate event="on_select" to="person_detail" bind="id -> id"/>

<!-- person_detail → note_list: person.id becomes note.person_id -->
<navigate event="on_notes" to="note_list" bind="id -> person_id"/>

<!-- note_detail → person_detail: note.person_id becomes person.id -->
<navigate event="on_person" to="person_detail" bind="person_id -> id"/>
```

## Interaction with the Board

When the AI triggers a navigation (through a user click in the Board), the template sends an event to OOS. OOS resolves the matching `<navigate>` block, loads the target context with the bound field value, and renders it into the Board.
