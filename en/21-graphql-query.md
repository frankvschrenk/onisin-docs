---
layout: default
title: Query Syntax and Filters
parent: English
nav_order: 21
---

# GraphQL Query Syntax and Filters

## Basic Query

```graphql
{ person_list { id firstname lastname email city age net_worth } }
```

The AI always uses exactly the `list_fields` from the DSL — no more, no less.

## Filters

### Exact Value

```graphql
{ person_list(city: "Berlin") { id firstname lastname city } }
```

### LIKE Filter (substring)

```graphql
{ person_list(lastname_like: "Smi") { id firstname lastname } }
```

Finds all persons whose last name contains "Smi" (case-insensitive `ILIKE`).

### Numeric Comparisons

```graphql
# All persons older than 30
{ person_list(age_gt: 30) { id firstname lastname age } }

# All persons with net worth below 1 million
{ person_list(net_worth_lt: 1000000) { id firstname lastname net_worth } }

# Combined: older than 30 AND net worth above 500k
{ person_list(age_gt: 30, net_worth_gt: 500000) { id firstname lastname age net_worth } }
```

### Available Filter Suffixes

| Suffix | Meaning | Type |
|---|---|---|
| (no suffix) | Exact value | all |
| `_like` | ILIKE substring | string |
| `_gt` | greater than | int, float |
| `_lt` | less than | int, float |

Only fields with `filterable="true"` in the context have filter arguments.

## Entity Query

```graphql
{ person_detail(id: 42) { id firstname lastname age net_worth street zip city email phone } }
```

## Nested Queries

OOS does not support nested GraphQL queries over relations. Navigation between contexts always happens via separate `oos_query` calls with the appropriate filter.
