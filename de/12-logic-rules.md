---
layout: default
title: Logic Rules
parent: Deutsch
nav_order: 12
---

# Logic Rules — Bedingte Darstellung

Logic Rules ermöglichen es, CSS-Klassen oder Stile auf Felder anzuwenden, wenn eine Bedingung erfüllt ist.

## Syntax

```xml
<logic field="feldname">
  <rule condition="BEDINGUNG" class="CSS_KLASSE"/>
  <rule condition="BEDINGUNG" style="CSS_STYLE"/>
</logic>
```

## Beispiele

### Fehlerfarbige Anzeige für ältere Personen

```xml
<logic field="age">
  <rule condition="> 30" class="text-error"/>
</logic>
```

### Goldener Hintergrund für Millionäre

```xml
<logic field="net_worth">
  <rule condition="> 1000000" class="bg-amber-500"/>
</logic>
```

### Mehrere Regeln

```xml
<logic field="score">
  <rule condition="< 50"  class="text-error"/>
  <rule condition="> 90"  class="text-success"/>
</logic>
```

## Bedingungsoperatoren

| Operator | Beispiel | Bedeutung |
|---|---|---|
| `>` | `> 100` | größer als |
| `<` | `< 0` | kleiner als |
| `>=` | `>= 18` | größer oder gleich |
| `<=` | `<= 100` | kleiner oder gleich |
| `==` | `== 1` | gleich |
| `!=` | `!= 0` | ungleich |

## CSS-Klassen

OOS verwendet [Tailwind CSS](https://tailwindcss.com). Alle Tailwind-Utility-Klassen können verwendet werden:

```xml
<rule condition="> 0"    class="text-green-600"/>
<rule condition="< 0"    class="text-red-600 font-bold"/>
<rule condition="> 9999" class="bg-yellow-100 border border-yellow-400"/>
```

## Template-Integration

Die Klassen werden vom OOS Merge-Engine ins HTML injiziert — das Template muss dafür nichts besonderes tun. OOS sucht das Element mit dem passenden `field=`-Attribut und ergänzt die Klassen automatisch.
