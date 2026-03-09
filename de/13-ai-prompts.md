---
layout: default
title: AI Prompts
parent: Deutsch
nav_order: 13
---

# AI Prompts im Context

OOS erlaubt es, KI-Anweisungen direkt in die Context-Definition einzubetten. Diese Prompts werden beim Start in den MCP-Schema-Export aufgenommen und von Claude automatisch berücksichtigt.

## Context-spezifische Prompts

```xml
<context name="person_list" ...>
  <ai name="list_behavior">
    Frage bei person_list immer exakt die list_fields ab – nicht mehr, nicht weniger.
  </ai>
  <ai name="navigation">
    Ein Klick auf eine Tabellenzeile öffnet person_detail mit der id dieser Person.
  </ai>
  <ai name="format_hint">
    net_worth ist Währung in EUR. age in Jahren, keine Dezimalstellen.
  </ai>
</context>
```

## Globale Prompts (global.conf.xml)

Globale Prompts gelten für alle Contexts:

```xml
<ai>
  <prompt name="system">
    OOS ist ein AI-first Enterprise-Datensystem. Alle Daten leben in Contexts.
    Ein Context ist entweder eine collection oder eine entity.
  </prompt>
  <prompt name="query_behavior">
    Nutze bei GraphQL-Abfragen immer exakt die list_fields.
  </prompt>
  <prompt name="mutation_behavior">
    Schreibe niemals direkt in die Datenbank.
    Workflow: oos_query → oos_ui_change_required → oos_ui_save.
  </prompt>
  <prompt name="language">
    Antworte immer in der Sprache des Benutzers. Standard ist Deutsch.
  </prompt>
</ai>
```

## Prompt-Hierarchie

1. **Globale Prompts** — werden zuerst an Claude übermittelt
2. **Context-spezifische Prompts** — werden beim `oos_schema`-Aufruf je Context übergeben

## Best Practices

- Prompts kurz und präzise halten
- Verhalten beschreiben, nicht Implementierungsdetails
- Readonly-Felder und Pflichtfelder explizit benennen
- Formatierungshinweise direkt beim Context angeben

```xml
<ai name="edit_behavior">
  Nur title und body sind editierbar.
  id, person_id und created_at sind readonly und dürfen nicht geändert werden.
</ai>
```
