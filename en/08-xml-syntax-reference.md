---
layout: default
title: XML Syntax Reference
parent: English
nav_order: 8
---

# XML Syntax Reference

## `<oos>` — Root Element

Every `.ctx.xml` file starts with `<oos>` as the root.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<oos>
  <!-- Contexts, Locales, AI Prompts -->
</oos>
```

---

## `<context>` — Context Definition

| Attribute | Required | Description |
|---|---|---|
| `name` | ✅ | Unique name (e.g. `person_list`) |
| `kind` | ✅ | `collection` or `entity` |
| `source` | ✅ | Table name in the database |
| `dsn` | ✅ | DSN name from `infra.conf.xml` |
| `view` | ✅ | Path to HTML template |
| `locale` | – | Locale name from `global.conf.xml` |

```xml
<context name="person_list" kind="collection" source="person"
         dsn="demo" locale="standard"
         view="html/pages/person/person.table.html">
```

---

## `<list_fields>` — Fields for collection

Comma-separated list of fields the AI should query for a collection.

```xml
<list_fields>id, firstname, lastname, email, city, age, net_worth</list_fields>
```

Only these fields are used in the GraphQL query. No more, no less.

---

## `<field>` — Field Definition

| Attribute | Required | Description |
|---|---|---|
| `name` | ✅ | Field name (matches DB column) |
| `type` | ✅ | `int`, `float`, `string`, `bool` |
| `header` | – | Column header in template |
| `format` | – | Display format (`cur`, `num:0`, `num:2`) |
| `filterable` | – | `"true"` — field can be used as a query filter |
| `readonly` | – | `"true"` — field is ignored in mutations |

```xml
<field name="id"        type="int"    header="#"          format="num:0"/>
<field name="firstname" type="string" header="First Name" filterable="true"/>
<field name="net_worth" type="float"  header="Net Worth"  format="cur"/>
<field name="id"        type="int"    readonly="true"/>
```

---

## `<navigate>` — Navigation Between Contexts

| Attribute | Description |
|---|---|
| `event` | Trigger (e.g. `on_select`, `on_notes`) |
| `to` | Target context |
| `bind` | Binding: `local_field -> foreign_field` |

```xml
<navigate event="on_select" to="person_detail" bind="id -> id"/>
<navigate event="on_notes"  to="note_list"     bind="id -> person_id"/>
```

---

## `<relation>` — Relations

| Attribute | Description |
|---|---|
| `name` | Relation name |
| `context` | Related context |
| `type` | `has_many`, `has_one`, `belongs_to` |
| `bind` | Binding: `local_field -> foreign_field` |

```xml
<relation name="notes" context="note_list" type="has_many" bind="id -> person_id"/>
```

---

## `<logic>` — Conditional Styling

```xml
<logic field="age">
  <rule condition="> 30" class="text-error"/>
</logic>

<logic field="net_worth">
  <rule condition="> 1000000" class="bg-amber-500"/>
</logic>
```

When the condition is met, the CSS class is applied to the field in the template.

---

## `<ai>` — AI Instructions

```xml
<ai name="list_behavior">
  Always query exactly the list_fields for person_list.
</ai>
<ai name="format_hint">
  net_worth is a currency in EUR. age in years, no decimal places.
</ai>
```

---

## `<locale>` — Locale Definition (in global.conf.xml)

```xml
<locale name="standard"  language="de-DE" currency="EUR"/>
<locale name="us_market" language="en-US" currency="USD"/>
```

---

## Complete Example

```xml
<?xml version="1.0" encoding="UTF-8"?>
<oos>
  <context name="person_list" kind="collection" source="person"
           dsn="demo" locale="standard"
           view="html/pages/person/person.table.html">

    <list_fields>id, firstname, lastname, email, city, age, net_worth</list_fields>

    <field name="id"        type="int"    header="#"          format="num:0"/>
    <field name="firstname" type="string" header="First Name" filterable="true"/>
    <field name="lastname"  type="string" header="Last Name"  filterable="true"/>
    <field name="age"       type="int"    header="Age"        filterable="true" format="num:0"/>
    <field name="net_worth" type="float"  header="Net Worth"  filterable="true" format="cur"/>
    <field name="city"      type="string" header="City"       filterable="true"/>
    <field name="email"     type="string" header="E-Mail"/>

    <logic field="age">
      <rule condition="> 30" class="text-error"/>
    </logic>

    <navigate event="on_select" to="person_detail" bind="id -> id"/>
    <relation name="notes" context="note_list" type="has_many" bind="id -> person_id"/>

    <ai name="list_behavior">Always query exactly the list_fields.</ai>
  </context>
</oos>
```
