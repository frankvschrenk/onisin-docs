---
layout: default
title: Field Types and Formats
parent: English
nav_order: 9
---

# Field Types and Formats

## Field Types (`type`)

| Type | Description | SQL Equivalent |
|---|---|---|
| `int` | Integer | `INTEGER`, `BIGINT` |
| `float` | Floating point | `NUMERIC`, `DECIMAL`, `FLOAT` |
| `string` | Text | `VARCHAR`, `TEXT` |
| `bool` | Boolean | `BOOLEAN` |

## Display Formats (`format`)

### `cur` — Currency
Formats the value as a currency according to the context's locale.

```xml
<field name="net_worth" type="float" format="cur"/>
```

- Locale `de-DE` / `EUR` → `1.234.567,89 €`
- Locale `en-US` / `USD` → `$1,234,567.89`

### `num:N` — Number with N Decimal Places

```xml
<field name="id"    type="int"   format="num:0"/>  <!-- 1234    -->
<field name="score" type="float" format="num:2"/>  <!-- 3.14    -->
<field name="qty"   type="int"   format="num:0"/>  <!-- 42      -->
```

- `num:0` — no decimal places (recommended for integers too)
- `num:2` — two decimal places

### No Format
If no `format` is specified, the value is displayed unformatted.

## Locale Binding

The format is always interpreted in combination with the context's `locale`:

```xml
<context name="person_list" locale="standard" ...>
  <!-- locale "standard" = de-DE / EUR -->
  <field name="net_worth" type="float" format="cur"/>
  <!-- Output: 1.234,56 € -->
</context>

<context name="person_detail" locale="us_market" ...>
  <!-- locale "us_market" = en-US / USD -->
  <field name="net_worth" type="float" format="cur"/>
  <!-- Output: $1,234.56 -->
</context>
```

## `readonly`

Fields with `readonly="true"` are displayed normally but ignored during mutations (updates). Typically used for `id`, `created_at`, `updated_at`.

```xml
<field name="id"         type="int"    readonly="true"/>
<field name="created_at" type="string" readonly="true"/>
```

## `filterable`

Fields with `filterable="true"` can be used by the AI as filter arguments in GraphQL queries.

```xml
<field name="lastname" type="string" filterable="true"/>
```

Generates GraphQL arguments: `lastname`, `lastname_like`.
