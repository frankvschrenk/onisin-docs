---
layout: default
title: GraphQL Schema
parent: English
nav_order: 20
---

# GraphQL — Auto-Generated Schema

OOS automatically generates a complete GraphQL schema from the DSL. No GraphQL files need to be created manually.

## How It Works

For each context, OOS generates:

- **Query** — for fetching data
- **Mutation** — for updating data (only for `kind="entity"` with a defined mutation)

## Generated Queries

For a context `person_list` (collection, source `person`):

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

Filter arguments are only generated for fields with `filterable="true"`.

## Generated Mutations

For a context `person_detail` (entity, source `person`):

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

Fields with `readonly="true"` are not included in the mutation.

## Type Mapping

| DSL Type | GraphQL Type |
|---|---|
| `int` | `Int` |
| `float` | `Float` |
| `string` | `String` |
| `bool` | `Boolean` |

## Schema Inspection

The generated schema can be accessed via the GraphQL introspection endpoint:

```
http://localhost:8000/graphql
```
