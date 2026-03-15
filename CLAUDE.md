# Edumark

Specification for the `.edm` format — a semantic Markdown extension for educational books.

## What this project is

Edumark defines an **authoring format**, not a tool. The main deliverable is `EDUMARK_SPEC.md`, the format specification. There is no code, parser, or renderer here — those are independent projects that consume this spec.

## Philosophy

> The `.edm` describes **what** the content is, never **how** it looks.

- **Purely semantic**: blocks declare type, id, title, answer — never style, position, or layout.
- **CommonMark as base**: all standard Markdown is inherited without redefinition.
- **Extended blocks with `:::`**: the only syntactic extension is fenced directive blocks `:::type`.
- **The decoder decides presentation**: colors, fonts, margins, columns, and any visual aspect are the sole responsibility of the rendering software.

## Syntax language

Block names and attributes are in **English** (like Markdown itself). Content inside blocks is written in whatever language the author chooses.

## Repository structure

```
edumark/
├── CLAUDE.md              ← this file
├── EDUMARK_SPEC.md        ← format specification
├── ejemplos/              ← example .edm files
├── prompts/               ← LLM prompts for generating .edm content
├── edumark-js/            ← submódulo: decoder JavaScript
└── edumark-beauty/        ← submódulo: temas y estilos CSS
```

## Submódulos

Este repo contiene dos submódulos que son repos independientes:
- **edumark-js**: decoder/parser en JavaScript
- **edumark-beauty**: temas visuales y estilos CSS

### Reglas para commits y push

Al hacer commit y push, seguir este orden:
1. Hacer commit y push en cada submódulo que tenga cambios
2. Volver al repo padre, hacer `git add` de los submódulos actualizados
3. Hacer commit y push del repo padre

Alternativa rápida: usar `git push --recurse-submodules=on-demand` desde el repo padre.

## What does NOT belong in the format

- Position (right, left, float)
- Width, columns, margins
- Colors, typography, sizes
- Any presentation hint

All of that lives in the decoder's configuration.
