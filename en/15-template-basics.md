---
layout: default
title: Template Basics
parent: English
nav_order: 15
---

# Template Basics

OOS templates are HTML files with special data binding attributes. OOS fills the templates with data and renders them into the Board.

## Core Principle

Templates use standard HTML. Data binding is done via a few special attributes:

| Attribute | Usage |
|---|---|
| `field="name"` | Binds a field to an element |
| `loop="@source"` | Repeats an element for each record |
| `bind="#source"` | Binds a header/wrapper to a data source |
| `single="@source.detail"` | Binds a form to a single record |

---

## Collection Template (Table)

```html
<table>
  <thead bind="#person">
    <tr>
      <th field="id">#</th>
      <th field="firstname">First Name</th>
      <th field="lastname">Last Name</th>
      <th field="net_worth">Net Worth</th>
    </tr>
  </thead>
  <tbody>
    <tr loop="@person" data-id="id">
      <td field="id"></td>
      <td field="firstname"></td>
      <td field="lastname"></td>
      <td field="net_worth"></td>
    </tr>
  </tbody>
  <tfoot>
    <tr>
      <td>Total</td>
      <td field="!sum:net_worth"></td>
    </tr>
  </tfoot>
</table>
```

**Important:**
- `bind="#person"` on `<thead>` — connects the header to the `person` data source
- `loop="@person"` on `<tr>` — repeats the row for each `person` record
- `data-id="id"` — stores the id for navigation (row click)

---

## Entity Template (Form)

```html
<fieldset single="@person.detail">
  <input type="text" field="firstname" />
  <input type="text" field="lastname"  />
  <input type="number" field="age"     />
  <input field="net_worth"             />
  <input type="text" field="id" readonly />
</fieldset>
```

**Important:**
- `single="@person.detail"` — binds the form to the `person.detail` record
- `readonly` on readonly fields — prevents editing in the UI

---

## Aggregations in Footer

```html
<tfoot>
  <tr>
    <td field="!sum:net_worth"></td>   <!-- Sum -->
    <td field="!avg:net_worth"></td>   <!-- Average -->
    <td field="!count:id"></td>        <!-- Count -->
  </tr>
</tfoot>
```

The `!` prefix marks an aggregation. OOS calculates the value from all records.

---

## Styling with DaisyUI / Tailwind

OOS templates use [DaisyUI](https://daisyui.com) on top of Tailwind CSS. All DaisyUI components are available:

```html
<tr loop="@person" class="hover:bg-base-200 cursor-pointer">
  <td field="firstname" class="font-bold"></td>
</tr>
```
