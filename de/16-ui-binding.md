---
layout: default
title: UI Binding Referenz
parent: Deutsch
nav_order: 16
---

# UI Binding Referenz

Vollständige Referenz aller Data-Binding-Attribute in OOS-Templates.

---

## `field="feldname"`

Bindet ein HTML-Element an ein Datenfeld. OOS schreibt den Wert in das Element.

```html
<td field="firstname"></td>
<input field="lastname" />
<span field="city"></span>
```

### Inline-Format
Ein Feldformat kann inline im `field`-Attribut angegeben werden:

```html
<td field="net_worth|cur"></td>     <!-- Währungsformat -->
<td field="age|num:0"></td>         <!-- Ganzzahl -->
<td field="score|num:2"></td>       <!-- 2 Dezimalstellen -->
```

Das Inline-Format überschreibt das Format aus der DSL.

---

## `loop="@quelle"`

Wiederholt das Element für jeden Datensatz in der Quelle.

```html
<tr loop="@person" data-id="id">
  <td field="id"></td>
  <td field="firstname"></td>
</tr>
```

- Die Quelle entspricht dem `source`-Attribut des Contexts (z.B. `person`)
- `data-id="id"` — speichert den Primärschlüssel für Navigation

---

## `bind="#quelle"`

Verbindet einen Header oder Wrapper mit einer Datenquelle. Wird typischerweise im `<thead>` verwendet.

```html
<thead bind="#person">
  <tr>
    <th field="id">#</th>
    <th field="firstname">Vorname</th>
  </tr>
</thead>
```

---

## `single="@quelle.detail"`

Bindet ein Formular an einen einzelnen Datensatz.

```html
<fieldset single="@person.detail">
  <input field="firstname" />
  <input field="lastname"  />
</fieldset>
```

---

## Aggregationen (`field="!aggregation:feld"`)

| Syntax | Beschreibung |
|---|---|
| `field="!sum:net_worth"` | Summe aller Werte |
| `field="!avg:net_worth"` | Durchschnitt |
| `field="!count:id"` | Anzahl der Datensätze |
| `field="!min:age"` | Minimum |
| `field="!max:age"` | Maximum |

```html
<tfoot>
  <tr>
    <td>Gesamt</td>
    <td style="text-align: right" field="!sum:net_worth"></td>
  </tr>
  <tr>
    <td>Durchschnitt</td>
    <td style="text-align: right" field="!avg:net_worth"></td>
  </tr>
</tfoot>
```

---

## `readonly` auf Input-Feldern

```html
<input type="text" field="id" readonly />
<input type="text" field="created_at" readonly />
```

Readonly-Inputs werden angezeigt, aber beim `oos_ui_save`-Scraping ignoriert.

---

## `data-id="feld"`

Speichert den Wert eines Felds als HTML-Data-Attribut. Wird für Row-Navigation verwendet.

```html
<tr loop="@person" data-id="id" class="cursor-pointer">
```

OOS liest `data-id` beim Klick auf die Zeile und leitet die Navigation ein.

---

## Währungsfelder (`data-raw`, `data-prefix`, `data-suffix`)

Für Währungs-Inputs injiziert OOS zusätzliche Data-Attribute:

```html
<!-- Nach dem Rendern durch OOS: -->
<input field="net_worth"
       data-raw="1234567.89"
       data-prefix=""
       data-suffix=" €"
       value="1.234.567,89 €" />
```

- `data-raw` — der numerische Rohwert (wird beim Speichern verwendet)
- Beim `oos_ui_save` wird `data-raw` statt `value` gelesen, um Formatierungsprobleme zu vermeiden
