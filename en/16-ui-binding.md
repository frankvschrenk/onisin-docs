---
layout: default
title: UI Binding Reference
parent: English
nav_order: 16
---

# UI Binding Reference

Complete reference for all data binding attributes in OOS templates.

---

## `field="fieldname"`

Binds an HTML element to a data field. OOS writes the value into the element.

```html
<td field="firstname"></td>
<input field="lastname" />
<span field="city"></span>
```

### Inline Format
A field format can be specified inline in the `field` attribute:

```html
<td field="net_worth|cur"></td>     <!-- Currency format -->
<td field="age|num:0"></td>         <!-- Integer -->
<td field="score|num:2"></td>       <!-- 2 decimal places -->
```

The inline format overrides the format from the DSL.

---

## `loop="@source"`

Repeats the element for each record in the source.

```html
<tr loop="@person" data-id="id">
  <td field="id"></td>
  <td field="firstname"></td>
</tr>
```

- The source matches the `source` attribute of the context (e.g. `person`)
- `data-id="id"` — stores the primary key for navigation

---

## `bind="#source"`

Connects a header or wrapper to a data source. Typically used in `<thead>`.

```html
<thead bind="#person">
  <tr>
    <th field="id">#</th>
    <th field="firstname">First Name</th>
  </tr>
</thead>
```

---

## `single="@source.detail"`

Binds a form to a single record.

```html
<fieldset single="@person.detail">
  <input field="firstname" />
  <input field="lastname"  />
</fieldset>
```

---

## Aggregations (`field="!aggregation:field"`)

| Syntax | Description |
|---|---|
| `field="!sum:net_worth"` | Sum of all values |
| `field="!avg:net_worth"` | Average |
| `field="!count:id"` | Number of records |
| `field="!min:age"` | Minimum |
| `field="!max:age"` | Maximum |

```html
<tfoot>
  <tr>
    <td>Total</td>
    <td style="text-align: right" field="!sum:net_worth"></td>
  </tr>
  <tr>
    <td>Average</td>
    <td style="text-align: right" field="!avg:net_worth"></td>
  </tr>
</tfoot>
```

---

## `readonly` on Input Fields

```html
<input type="text" field="id" readonly />
<input type="text" field="created_at" readonly />
```

Readonly inputs are displayed but ignored during `oos_ui_save` scraping.

---

## `data-id="field"`

Stores the value of a field as an HTML data attribute. Used for row navigation.

```html
<tr loop="@person" data-id="id" class="cursor-pointer">
```

OOS reads `data-id` on row click and initiates navigation.

---

## Currency Fields (`data-raw`, `data-prefix`, `data-suffix`)

For currency inputs, OOS injects additional data attributes:

```html
<!-- After OOS renders: -->
<input field="net_worth"
       data-raw="1234567.89"
       data-prefix="$"
       data-suffix=""
       value="$1,234,567.89" />
```

- `data-raw` — the raw numeric value (used when saving)
- During `oos_ui_save`, `data-raw` is read instead of `value` to avoid formatting issues
