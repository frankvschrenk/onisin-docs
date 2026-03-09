---
layout: default
title: AI Prompts
parent: English
nav_order: 13
---

# AI Prompts in Context

OOS allows AI instructions to be embedded directly into context definitions. These prompts are included in the MCP schema export at startup and are automatically observed by Claude.

## Context-Specific Prompts

```xml
<context name="person_list" ...>
  <ai name="list_behavior">
    Always query exactly the list_fields for person_list — no more, no less.
  </ai>
  <ai name="navigation">
    A click on a table row opens person_detail with the id of that person.
  </ai>
  <ai name="format_hint">
    net_worth is a currency in EUR. age in years, no decimal places.
  </ai>
</context>
```

## Global Prompts (global.conf.xml)

Global prompts apply to all contexts:

```xml
<ai>
  <prompt name="system">
    OOS is an AI-first enterprise data system. All data lives in contexts.
    A context is either a collection or an entity.
  </prompt>
  <prompt name="query_behavior">
    Always use exactly the list_fields in GraphQL queries.
  </prompt>
  <prompt name="mutation_behavior">
    Never write directly to the database.
    Workflow: oos_query → oos_ui_change_required → oos_ui_save.
  </prompt>
  <prompt name="language">
    Always respond in the user's language. Default is German.
  </prompt>
</ai>
```

## Prompt Hierarchy

1. **Global prompts** — sent to Claude first
2. **Context-specific prompts** — passed per context during `oos_schema` call

## Best Practices

- Keep prompts short and precise
- Describe behavior, not implementation details
- Explicitly name readonly fields and required fields
- Put formatting hints directly in the context

```xml
<ai name="edit_behavior">
  Only title and body are editable.
  id, person_id and created_at are readonly and must not be changed.
</ai>
```
