---
layout: default
title: Template Basics
parent: Deutsch
nav_order: 15
---

# Template Basics

OOS-Templates sind HTML-Dateien mit speziellen Data-Binding-Attributen. OOS füllt die Templates mit Daten und rendert sie ins Board.

## Grundprinzip

Templates verwenden Standard-HTML. Die Bindung an Daten erfolgt über wenige spezielle Attribute:

| Attribut | Verwendung |
|---|---|
| `field="name"` | Bindet ein Feld an ein Element |
| `loop="@quelle"` | Wiederholt ein Element für jeden Datensatz |
| `bind="#quelle"` | Bindet Header/Wrapper an eine Datenquelle |
| `single="@quelle.detail"` | Bindet ein Formular an einen einzelnen Datensatz |

---

## Collection-Template (Tabelle)

```html
<table>
  <thead bind="#person">
    <tr>
      <th field="id">#</th>
      <th field="firstname">Vorname</th>
      <th field="lastname">Nachname</th>
      <th field="net_worth">Vermögen</th>
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
      <td>Gesamt</td>
      <td field="!sum:net_worth"></td>
    </tr>
  </tfoot>
</table>
```

**Wichtig:**
- `bind="#person"` im `<thead>` — verbindet den Header mit der Datenquelle `person`
- `loop="@person"` im `<tr>` — wiederholt die Zeile für jeden `person`-Datensatz
- `data-id="id"` — speichert die id für Navigation (Klick auf Zeile)

---

## Entity-Template (Formular)

```html
<fieldset single="@person.detail">
  <input type="text" field="firstname" />
  <input type="text" field="lastname"  />
  <input type="number" field="age"     />
  <input field="net_worth"             />
  <input type="text" field="id" readonly />
</fieldset>
```

**Wichtig:**
- `single="@person.detail"` — bindet das Formular an den `person.detail`-Datensatz
- `readonly` auf readonly-Feldern — verhindert Bearbeitung im UI

---

## Aggregationen im Footer

```html
<tfoot>
  <tr>
    <td field="!sum:net_worth"></td>   <!-- Summe -->
    <td field="!avg:net_worth"></td>   <!-- Durchschnitt -->
    <td field="!count:id"></td>        <!-- Anzahl -->
  </tr>
</tfoot>
```

Das `!`-Präfix kennzeichnet eine Aggregation. OOS berechnet den Wert aus allen Datensätzen.

---

## Styling mit DaisyUI / Tailwind

OOS-Templates verwenden [DaisyUI](https://daisyui.com) auf Basis von Tailwind CSS. Alle DaisyUI-Komponenten stehen zur Verfügung:

```html
<tr loop="@person" class="hover:bg-base-200 cursor-pointer">
  <td field="firstname" class="font-bold"></td>
</tr>
```
