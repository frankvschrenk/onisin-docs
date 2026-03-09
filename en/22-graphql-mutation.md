---
layout: default
title: Mutation Syntax
parent: English
nav_order: 22
---

# GraphQL Mutation Syntax

## Update Mutation

```graphql
mutation {
  update_person_detail(
    id: 42
    firstname: "Anna"
    lastname: "Schmidt"
    age: 35
  ) { id firstname lastname }
}
```

- `id` is always required (NOT NULL)
- All other fields are optional — only provided fields are updated
- Fields with `readonly="true"` are ignored

## Direct Mutation Calls

The AI **never calls mutations directly**. The correct workflow is:

```
1. oos_query              → load and display data
2. oos_ui_change_required → write changes as preview into the Board
3. oos_ui_save            → user confirms → OOS scrapes FormData → mutation executes
```

`oos_ui_save` automatically builds the mutation from the form data.

## What `oos_ui_save` Does

1. Scrape FormData from the current Board form
2. Ignore readonly fields
3. Read currency fields via `data-raw`
4. Assemble GraphQL mutation from the fields
5. Execute mutation
6. Reload Board with updated data
