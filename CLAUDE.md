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
├── docs/                  ← translated READMEs (README_ES.md, etc.)
├── ejemplos/              ← example .edm files
├── llms/                  ← LLM prompts for generating .edm content
├── edumark-js/            ← submódulo: decoder JavaScript
└── edumark-beauty/        ← submódulo: temas y estilos CSS
```

## Submódulos

Este repo contiene dos submódulos que son repos independientes:
- **edumark-js**: decoder/parser en JavaScript
- **edumark-beauty**: editor/visor web + app de escritorio (Tauri)

### Reglas para commits y push

Al hacer commit y push, seguir este orden:
1. Hacer commit y push en cada submódulo que tenga cambios
2. Volver al repo padre, hacer `git add` de los submódulos actualizados
3. Hacer commit y push del repo padre

Alternativa rápida: usar `git push --recurse-submodules=on-demand` desde el repo padre.

### edumark-js: siempre build antes de push

`edumark-beauty` consume `edumark-js` desde GitHub (`github:Debaq/edumark-js`), no como dependencia local. Por lo tanto, cuando haya cambios en `edumark-js`:

1. Hacer build (`npm run build`) para regenerar `dist/`
2. Commit incluyendo los archivos de `dist/`
3. Push a GitHub

Si no se hace build+push, edumark-beauty (tanto en local con `npm install` como en CI/GitHub Actions) seguirá usando la versión anterior.

## edumark-beauty: web + Tauri (escritorio)

`edumark-beauty` compila como app web y como app de escritorio (Tauri v2) desde el mismo código. Las diferencias se manejan con un adaptador en runtime, no con branches.

### Arquitectura web/Tauri

- **`src/lib/fileAdapter.ts`**: adaptador que detecta `window.__TAURI__` en runtime
  - **Web**: usa `file-saver` (saveAs) y `<input type="file">` del navegador
  - **Tauri**: usa `@tauri-apps/plugin-dialog` y `@tauri-apps/plugin-fs` (diálogos nativos del SO)
- **`src-tauri/`**: configuración Rust, plugins y manifesto de la app de escritorio
- **`vite.config.ts`**: `base: '/'` para Tauri, `'/edumark/'` para web (auto-detectado)

### Reglas para cambios web vs Tauri

- El código compartido (editor, preview, temas, exportación) es idéntico en ambos targets
- Las operaciones de archivo **siempre** pasan por `fileAdapter.ts` — nunca importar `file-saver` directamente
- Si se agrega nueva funcionalidad de I/O (abrir, guardar, etc.), implementar ambas ramas en el adaptador
- `npm run dev` / `npm run build` = web; `./tauri.sh dev` / `./tauri.sh build` = escritorio

### Scripts Tauri (`./tauri.sh`)

| Comando | Acción |
|---------|--------|
| `./tauri.sh dev` | Dev mode (busca puerto libre automáticamente) |
| `./tauri.sh build` | Compilar binario release |
| `./tauri.sh deb` | Generar .deb |
| `./tauri.sh rpm` | Generar .rpm |
| `./tauri.sh package` | Generar .deb + .rpm |
| `./tauri.sh release` | Crear tag + trigger GitHub Actions (Windows .msi/.exe) |

## Sincronización spec ↔ prompts LLM

Cuando se modifica `EDUMARK_SPEC.md`, **siempre** actualizar también los prompts en `llms/` para que reflejen los cambios. Los prompts deben estar alineados con la spec vigente.

## What does NOT belong in the format

- Position (right, left, float)
- Width, columns, margins
- Colors, typography, sizes
- Any presentation hint

All of that lives in the decoder's configuration.
