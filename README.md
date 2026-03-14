# Edumark

**Escribir un libro educativo debería ser tan simple como escribir un apunte.**

Edumark es una extensión de Markdown para crear libros educativos. El autor escribe contenido con una sintaxis familiar y marca *qué* es cada cosa — una definición, un ejercicio, una alerta, un mnemónico — sin preocuparse jamás por cómo se verá. La presentación es problema del software que lo renderice, no del autor.

La extensión de archivo es `.edm`. La sintaxis de bloques y atributos es en inglés (como Markdown mismo). El contenido se escribe en el idioma que el autor quiera.

## La idea

Los formatos de autoría educativa existentes obligan al autor a pensar en dos cosas al mismo tiempo: el contenido y la presentación. Word mezcla texto con formato. LaTeX exige decidir márgenes, tipografías y layout antes de escribir la primera oración. HTML es un lenguaje de programación disfrazado de documento.

Markdown resolvió esto para texto plano: escribís contenido, y el renderizador decide cómo se ve. Pero Markdown no sabe qué es una definición, un objetivo de aprendizaje, un ejercicio con solución o una pregunta de selección múltiple.

Edumark agrega ese vocabulario. Nada más.

## Cómo se ve

```
:::definition id="celula"
**Célula** | Unidad estructural y funcional de todos los seres vivos.
:::

:::warning
No confundir célula procariota con eucariota.
La diferencia clave es la presencia de núcleo delimitado por membrana.
:::

:::exercise title="Identificación celular"
Observe la microfotografía y determine si corresponde a una célula
procariota o eucariota. Justifique su respuesta.

:::solution
Es una célula eucariota: se observa un núcleo definido con
membrana nuclear, mitocondrias y retículo endoplásmico.
:::
:::
```

El autor declara intención semántica (`definition`, `warning`, `exercise`). Un decodificador puede convertir esto en un PDF con cajas de colores, en una web interactiva con soluciones colapsables, en un EPUB accesible, o en lo que sea necesario. El mismo `.edm` sirve para todos.

## Principio fundamental

> El `.edm` describe **qué** es el contenido, nunca **cómo** se ve.

Esto significa que en un archivo Edumark nunca vas a encontrar colores, márgenes, tipografías, anchos de columna ni instrucciones de posicionamiento. Si necesitás una caja azul con borde redondeado a la derecha, eso se configura en el decodificador. El documento solo dice "esto es una definición" — y el decodificador decide que las definiciones son cajas azules con borde redondeado a la derecha.

## Qué hay en este repositorio

Este repositorio contiene la **especificación del formato**, no un parser ni un renderer.

```
edumark/
├── README.md              ← estás aquí
├── EDUMARK_SPEC.md        ← especificación completa del formato
├── ejemplos/
│   └── capitulo_ejemplo.edm   ← capítulo de ejemplo usando todos los bloques
└── prompts/
    ├── edumark_claude.md      ← prompt para Claude Code / Projects / API
    ├── edumark_chatgpt.md     ← prompt para Custom GPTs / OpenAI API
    ├── edumark_gemini.md      ← prompt para Gems / Gemini API
    └── edumark_universal.md   ← prompt para cualquier otro LLM
```

La spec define 19 bloques pedagógicos, un sistema de referencias cruzadas, includes para componer libros desde múltiples archivos, y bloques condicionales para versiones docente/estudiante. Todo construido sobre CommonMark.

## Generar contenido con IA

En `prompts/` hay prompts listos para usar en distintos LLMs. Cargás el prompt como system prompt o instrucción, y el modelo genera capítulos `.edm` completos. Funcionan con Claude, ChatGPT, Gemini, Qwen, Kimi, DeepSeek, Ollama y cualquier modelo que acepte instrucciones.

## Para quién es

Para docentes, autores de material educativo y equipos editoriales que quieran escribir contenido una vez y publicarlo en múltiples formatos — sin quedar atados a un software, sin perder tiempo en diseño, y sin necesitar conocimientos técnicos más allá de Markdown.

## Estado

Especificación v2.0 — estable para uso y experimentación. Los decodificadores son proyectos independientes que consumen esta spec.
