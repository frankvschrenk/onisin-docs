---
layout: default
title: Feldtypen und Formate
parent: Deutsch
nav_order: 9
---

# Feldtypen und Formate

## Feldtypen (`type`)

| Typ | Beschreibung | SQL-Entsprechung |
|---|---|---|
| `int` | Ganzzahl | `INTEGER`, `BIGINT` |
| `float` | Gleitkommazahl | `NUMERIC`, `DECIMAL`, `FLOAT` |
| `string` | Zeichenkette | `VARCHAR`, `TEXT` |
| `bool` | Wahrheitswert | `BOOLEAN` |

## Anzeigeformate (`format`)

### `cur` — Währung
Formatiert den Wert als Währung gemäß dem Locale des Contexts.

```xml
<field name="net_worth" type="float" format="cur"/>
```

- Locale `de-DE` / `EUR` → `1.234.567,89 €`
- Locale `en-US` / `USD` → `$1,234,567.89`

### `num:N` — Zahl mit N Dezimalstellen

```xml
<field name="id"    type="int"   format="num:0"/>  <!-- 1234    -->
<field name="score" type="float" format="num:2"/>  <!-- 3,14    -->
<field name="qty"   type="int"   format="num:0"/>  <!-- 42      -->
```

- `num:0` — keine Dezimalstellen (auch für Ganzzahlen empfohlen)
- `num:2` — zwei Dezimalstellen

### Kein Format
Wenn kein `format` angegeben ist, wird der Wert unformatiert angezeigt.

## Locale-Bindung

Das Format wird immer in Kombination mit dem `locale` des Contexts interpretiert:

```xml
<context name="person_list" locale="standard" ...>
  <!-- locale "standard" = de-DE / EUR -->
  <field name="net_worth" type="float" format="cur"/>
  <!-- Ausgabe: 1.234,56 € -->
</context>

<context name="person_detail" locale="us_market" ...>
  <!-- locale "us_market" = en-US / USD -->
  <field name="net_worth" type="float" format="cur"/>
  <!-- Ausgabe: $1,234.56 -->
</context>
```

## `readonly`

Felder mit `readonly="true"` werden in der Anzeige normal dargestellt, aber bei Mutations (Updates) ignoriert. Typischerweise für `id`, `created_at`, `updated_at`.

```xml
<field name="id"         type="int"    readonly="true"/>
<field name="created_at" type="string" readonly="true"/>
```

## `filterable`

Felder mit `filterable="true"` können von der KI als Filterargument in GraphQL-Queries verwendet werden.

```xml
<field name="lastname" type="string" filterable="true"/>
```

Generiert GraphQL-Argumente: `lastname`, `lastname_like`.
