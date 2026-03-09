---
layout: default
title: XML Syntax Referenz
parent: Deutsch
nav_order: 8
---

# XML Syntax Referenz

## `<oos>` — Root-Element

Jede `.ctx.xml`-Datei beginnt mit `<oos>` als Root.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<oos>
  <!-- Contexts, Locales, AI-Prompts -->
</oos>
```

---

## `<context>` — Context-Definition

| Attribut | Pflicht | Beschreibung |
|---|---|---|
| `name` | ✅ | Eindeutiger Name (z.B. `person_list`) |
| `kind` | ✅ | `collection` oder `entity` |
| `source` | ✅ | Tabellenname in der Datenbank |
| `dsn` | ✅ | DSN-Name aus `infra.conf.xml` |
| `view` | ✅ | Pfad zum HTML-Template |
| `locale` | – | Locale-Name aus `global.conf.xml` |

```xml
<context name="person_list" kind="collection" source="person"
         dsn="demo" locale="standard"
         view="html/pages/person/person.table.html">
```

---

## `<list_fields>` — Felder für collection

Komma-separierte Liste der Felder, die die KI bei einer collection abfragen soll.

```xml
<list_fields>id, firstname, lastname, email, city, age, net_worth</list_fields>
```

Nur diese Felder werden im GraphQL-Query verwendet. Nicht mehr, nicht weniger.

---

## `<field>` — Felddefinition

| Attribut | Pflicht | Beschreibung |
|---|---|---|
| `name` | ✅ | Feldname (entspricht DB-Spalte) |
| `type` | ✅ | `int`, `float`, `string`, `bool` |
| `header` | – | Spaltenüberschrift im Template |
| `format` | – | Anzeigeformat (`cur`, `num:0`, `num:2`) |
| `filterable` | – | `"true"` — Feld kann als Query-Filter verwendet werden |
| `readonly` | – | `"true"` — Feld wird bei Mutations ignoriert |

```xml
<field name="id"        type="int"    header="#"        format="num:0"/>
<field name="firstname" type="string" header="Vorname"  filterable="true"/>
<field name="net_worth" type="float"  header="Vermögen" format="cur"/>
<field name="id"        type="int"    readonly="true"/>
```

---

## `<navigate>` — Navigation zwischen Contexts

| Attribut | Beschreibung |
|---|---|
| `event` | Auslöser (z.B. `on_select`, `on_notes`) |
| `to` | Ziel-Context |
| `bind` | Binding: `lokales_feld -> fremdes_feld` |

```xml
<navigate event="on_select" to="person_detail" bind="id -> id"/>
<navigate event="on_notes"  to="note_list"     bind="id -> person_id"/>
```

---

## `<relation>` — Relationen

| Attribut | Beschreibung |
|---|---|
| `name` | Name der Relation |
| `context` | Verbundener Context |
| `type` | `has_many`, `has_one`, `belongs_to` |
| `bind` | Binding: `lokales_feld -> fremdes_feld` |

```xml
<relation name="notes" context="note_list" type="has_many" bind="id -> person_id"/>
```

---

## `<logic>` — Bedingte Darstellung

```xml
<logic field="age">
  <rule condition="> 30" class="text-error"/>
</logic>

<logic field="net_worth">
  <rule condition="> 1000000" class="bg-amber-500"/>
</logic>
```

Wenn die Bedingung erfüllt ist, wird die CSS-Klasse auf das Feld im Template angewendet.

---

## `<ai>` — KI-Anweisungen

```xml
<ai name="list_behavior">
  Frage bei person_list immer exakt die list_fields ab.
</ai>
<ai name="format_hint">
  net_worth ist Währung in EUR. age in Jahren, keine Dezimalstellen.
</ai>
```

---

## `<locale>` — Locale-Definition (in global.conf.xml)

```xml
<locale name="standard"  language="de-DE" currency="EUR"/>
<locale name="us_market" language="en-US" currency="USD"/>
```

---

## Vollständiges Beispiel

```xml
<?xml version="1.0" encoding="UTF-8"?>
<oos>
  <context name="person_list" kind="collection" source="person"
           dsn="demo" locale="standard"
           view="html/pages/person/person.table.html">

    <list_fields>id, firstname, lastname, email, city, age, net_worth</list_fields>

    <field name="id"        type="int"    header="#"        format="num:0"/>
    <field name="firstname" type="string" header="Vorname"  filterable="true"/>
    <field name="lastname"  type="string" header="Nachname" filterable="true"/>
    <field name="age"       type="int"    header="Alter"    filterable="true" format="num:0"/>
    <field name="net_worth" type="float"  header="Vermögen" filterable="true" format="cur"/>
    <field name="city"      type="string" header="Stadt"    filterable="true"/>
    <field name="email"     type="string" header="E-Mail"/>

    <logic field="age">
      <rule condition="> 30" class="text-error"/>
    </logic>

    <navigate event="on_select" to="person_detail" bind="id -> id"/>
    <relation name="notes" context="note_list" type="has_many" bind="id -> person_id"/>

    <ai name="list_behavior">Frage immer exakt die list_fields ab.</ai>
  </context>
</oos>
```
