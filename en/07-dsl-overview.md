---
layout: default
title: DSL Overview
parent: English
nav_order: 7
---

# DSL Overview

The OOS DSL (Domain Specific Language) describes data contexts in XML. Each `.ctx.xml` file defines one or more contexts.

## What is a Context?

A Context is the central data unit in OOS. It describes:
- Which database table (or source) is used
- Which fields are displayed
- How data is formatted and presented
- Which navigation is possible
- Which rules the AI should follow when handling this data

## Two Kinds of Context

### collection
A **collection** is a list of records — equivalent to a multi-row `SELECT`.

```xml
<context name="person_list" kind="collection" source="person" dsn="demo"
         view="html/pages/person/person.table.html">
  <list_fields>id, firstname, lastname, email</list_fields>
  ...
</context>
```

- Requires `<list_fields>` — defines which fields the AI queries
- Typically renders a table

### entity
An **entity** is a single record — equivalent to `SELECT WHERE id = ?`.

```xml
<context name="person_detail" kind="entity" source="person" dsn="demo"
         view="html/pages/person/person.detail.html">
  ...
</context>
```

- No `<list_fields>` needed
- Typically renders a detail form
- Can have a `gql_mutation` (for updates)

## File Structure

```
ctx/
  person.ctx.xml      ← person_list + person_detail
  note.ctx.xml        ← note_list + note_detail
  global.conf.xml     ← locales, global AI prompts
  infra.conf.xml      ← DSN definitions (backbone)
```

All `.ctx.xml` files in `ctx_dir` are automatically loaded on startup and merged into a single AST.

## global.conf.xml

Contains locales and global AI instructions that apply to all contexts:

```xml
<oos>
  <ai>
    <prompt name="system">OOS is an AI-first enterprise data system...</prompt>
    <prompt name="query_behavior">Always use exactly the list_fields...</prompt>
  </ai>

  <locale name="standard"  language="de-DE" currency="EUR"/>
  <locale name="us_market" language="en-US" currency="USD"/>
</oos>
```
