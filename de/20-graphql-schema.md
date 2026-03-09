---
layout: default
title: GraphQL Schema
parent: Deutsch
nav_order: 20
---

# GraphQL — Auto-generiertes Schema

OOS generiert automatisch ein vollständiges GraphQL-Schema aus der DSL. Es müssen keine GraphQL-Dateien manuell erstellt werden.

## Wie es funktioniert

Für jeden Context generiert OOS:

- **Query** — zum Abrufen von Daten
- **Mutation** — zum Aktualisieren von Daten (nur bei `kind="entity"` mit definierter Mutation)

## Generierte Queries

Für einen Context `person_list` (collection, source `person`):

```graphql
type Query {
  person_list(
    id: Int
    firstname: String
    firstname_like: String
    lastname: String
    lastname_like: String
    age: Int
    age_gt: Int
    age_lt: Int
    net_worth: Float
    net_worth_gt: Float
    net_worth_lt: Float
    city: String
    city_like: String
  ): [Person]
}
```

Filter-Argumente werden nur für Felder mit `filterable="true"` generiert.

## Generierte Mutations

Für einen Context `person_detail` (entity, source `person`):

```graphql
type Mutation {
  update_person_detail(
    id: Int!
    firstname: String
    lastname: String
    age: Int
    net_worth: Float
    street: String
    zip: String
    city: String
    email: String
    phone: String
  ): Person
}
```

Felder mit `readonly="true"` werden in der Mutation nicht aufgeführt.

## Type-Mapping

| DSL-Typ | GraphQL-Typ |
|---|---|
| `int` | `Int` |
| `float` | `Float` |
| `string` | `String` |
| `bool` | `Boolean` |

## Schema-Inspektion

Das generierte Schema kann über den GraphQL-Introspection-Endpunkt abgerufen werden:

```
http://localhost:8000/graphql
```
