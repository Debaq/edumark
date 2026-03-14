# Edumark

Especificación del formato `.edm` — una extensión semántica de Markdown para crear libros educativos.

## Qué es este proyecto

Edumark define un **formato de autoría**, no una herramienta. El entregable principal es `EDUMARK_SPEC.md`, la especificación del formato. No hay código, parser ni renderer aquí — esos serán proyectos independientes que consuman esta spec.

## Filosofía

> El `.edm` describe **qué** es el contenido, nunca **cómo** se ve.

- **Puramente semántico**: los bloques declaran tipo, id, título, respuesta — jamás estilo, posición ni layout.
- **CommonMark como base**: todo lo que es Markdown estándar se hereda sin redefinir.
- **Bloques extendidos con `:::`**: la única extensión sintáctica son los bloques delimitados por `:::tipo`.
- **El decodificador decide la presentación**: colores, tipografía, márgenes, columnas y cualquier aspecto visual son responsabilidad exclusiva del programa que renderice el `.edm`.

## Estructura del repositorio

```
edumark/
├── CLAUDE.md              ← este archivo
├── EDUMARK_SPEC.md        ← especificación del formato
└── ejemplos/              ← archivos .edm de ejemplo
```

## Convenciones

- Documentación y contenido en español
- Los archivos de ejemplo usan extensión `.edm`
- Los cambios a la spec deben mantener coherencia con el principio semántico

## Lo que NO va en el formato

- Posición (derecha, izquierda, flotante)
- Ancho, columnas, márgenes
- Colores, tipografía, tamaños
- Cualquier hint de presentación

Todo eso vive en la configuración del decodificador.
