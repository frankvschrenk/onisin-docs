---
layout: default
title: Query-Syntax und Filter
parent: Deutsch
nav_order: 21
---

# GraphQL Query-Syntax und Filter

## Basis-Query

```graphql
{ person_list { id firstname lastname email city age net_worth } }
```

Die KI verwendet immer genau die `list_fields` aus der DSL — nicht mehr, nicht weniger.

## Filter

### Exakter Wert

```graphql
{ person_list(city: "Berlin") { id firstname lastname city } }
```

### LIKE-Filter (Teilstring)

```graphql
{ person_list(lastname_like: "Müll") { id firstname lastname } }
```

Sucht alle Personen, deren Nachname "Müll" enthält (case-insensitive `ILIKE`).

### Numerische Vergleiche

```graphql
# Alle Personen älter als 30
{ person_list(age_gt: 30) { id firstname lastname age } }

# Alle Personen mit Vermögen unter 1 Million
{ person_list(net_worth_lt: 1000000) { id firstname lastname net_worth } }

# Kombination: älter als 30 UND Vermögen über 500k
{ person_list(age_gt: 30, net_worth_gt: 500000) { id firstname lastname age net_worth } }
```

### Verfügbare Filter-Suffixe

| Suffix | Bedeutung | Typ |
|---|---|---|
| (kein Suffix) | Exakter Wert | alle |
| `_like` | ILIKE Teilstring | string |
| `_gt` | größer als | int, float |
| `_lt` | kleiner als | int, float |

Nur Felder mit `filterable="true"` im Context haben Filter-Argumente.

## Entity-Query

```graphql
{ person_detail(id: 42) { id firstname lastname age net_worth street zip city email phone } }
```

## Verschachtelte Abfragen

OOS unterstützt keine verschachtelten GraphQL-Queries über Relations. Navigation zwischen Contexts erfolgt immer über separate `oos_query`-Aufrufe mit dem passenden Filter.
