---
layout: default
title: Logic Rules
parent: English
nav_order: 12
---

# Logic Rules — Conditional Styling

Logic Rules allow CSS classes or styles to be applied to fields when a condition is met.

## Syntax

```xml
<logic field="fieldname">
  <rule condition="CONDITION" class="CSS_CLASS"/>
  <rule condition="CONDITION" style="CSS_STYLE"/>
</logic>
```

## Examples

### Error color for older people

```xml
<logic field="age">
  <rule condition="> 30" class="text-error"/>
</logic>
```

### Gold background for millionaires

```xml
<logic field="net_worth">
  <rule condition="> 1000000" class="bg-amber-500"/>
</logic>
```

### Multiple rules

```xml
<logic field="score">
  <rule condition="< 50"  class="text-error"/>
  <rule condition="> 90"  class="text-success"/>
</logic>
```

## Condition Operators

| Operator | Example | Meaning |
|---|---|---|
| `>` | `> 100` | greater than |
| `<` | `< 0` | less than |
| `>=` | `>= 18` | greater than or equal |
| `<=` | `<= 100` | less than or equal |
| `==` | `== 1` | equal |
| `!=` | `!= 0` | not equal |

## CSS Classes

OOS uses [Tailwind CSS](https://tailwindcss.com). All Tailwind utility classes can be used:

```xml
<rule condition="> 0"    class="text-green-600"/>
<rule condition="< 0"    class="text-red-600 font-bold"/>
<rule condition="> 9999" class="bg-yellow-100 border border-yellow-400"/>
```

## Template Integration

Classes are injected into the HTML by the OOS merge engine — the template doesn't need to do anything special. OOS finds the element with the matching `field=` attribute and adds the classes automatically.
