---
layout: default
title: Mutation-Syntax
parent: Deutsch
nav_order: 22
---

# GraphQL Mutation-Syntax

## Update-Mutation

```graphql
mutation {
  update_person_detail(
    id: 42
    firstname: "Anna"
    lastname: "Schmidt"
    age: 35
  ) { id firstname lastname }
}
```

- `id` ist immer Pflicht (NOT NULL)
- Alle anderen Felder sind optional — nur übergebene Felder werden aktualisiert
- Felder mit `readonly="true"` werden ignoriert

## Direktes Aufrufen von Mutations

Die KI ruft Mutations **nie direkt** auf. Der korrekte Workflow ist:

```
1. oos_query       → Daten laden und anzeigen
2. oos_ui_change_required → Änderungen als Preview ins Board schreiben
3. oos_ui_save     → Benutzer bestätigt → OOS scraped FormData → Mutation wird ausgeführt
```

`oos_ui_save` baut die Mutation automatisch aus den Formulardaten auf.

## Was `oos_ui_save` macht

1. FormData aus dem aktuellen Board-Formular scrapen
2. Readonly-Felder ignorieren
3. Währungsfelder über `data-raw` auslesen
4. GraphQL-Mutation aus den Feldern zusammenstellen
5. Mutation ausführen
6. Board mit den aktualisierten Daten neu laden
