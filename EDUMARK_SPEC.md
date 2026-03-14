# Edumark — Especificación del Formato v2.0

> Formato de autoría educativa basado en Markdown extendido.
> Diseñado para cualquier disciplina y nivel educativo.
> Extensión de archivo: `.edm`

---

## Principio fundamental

> El `.edm` describe **qué** es el contenido, nunca **cómo** se ve.
> El estilo, posición y layout son responsabilidad exclusiva del decodificador.
> Los atributos en bloques son solo semánticos (tipo, id, título, respuesta, etc.).

---

## 1. Base: CommonMark

Edumark hereda toda la sintaxis de [CommonMark](https://commonmark.org/) sin modificaciones. Todo lo que es válido en CommonMark es válido en `.edm`:

- Encabezados (`#`, `##`, etc.)
- Párrafos, énfasis (`*cursiva*`, `**negrita**`)
- Listas ordenadas y desordenadas
- Enlaces e imágenes inline (`![alt](ruta)`)
- Bloques de código (con ` ``` `)
- Tablas (extensión GFM)
- Citas (`>`)
- Líneas horizontales (`---`)

Lo que Edumark agrega son **bloques pedagógicos** y **mecanismos de composición** que no existen en CommonMark.

---

## 2. Metadatos del documento (frontmatter YAML)

Cada archivo `.edm` puede comenzar con un bloque YAML delimitado por `---`. Los campos son libres, pero se recomiendan:

```yaml
---
titulo: "Título del capítulo o documento"
autor: "Nombre del autor"
version: 1.0
fecha: 2026-01
# Campos opcionales de contexto educativo:
materia: "Nombre de la asignatura"
nivel: "Pregrado"
unidad: "I — Nombre de la unidad"
temas:
  - Tema 1
  - Tema 2
---
```

El decodificador decide qué hacer con estos metadatos (encabezados de página, portada, índice, etc.).

---

## 3. Bloques pedagógicos

Todos los bloques siguen la sintaxis de fenced directives:

```
:::tipo atributo="valor"
Contenido en Markdown estándar.
:::
```

### Reglas generales

- Los bloques se abren con `:::tipo` y se cierran con `:::` en línea propia.
- Los atributos son opcionales y solo semánticos: `id`, `titulo`, `tipo`, `respuesta`, etc.
- El contenido interno acepta Markdown estándar completo.
- Los bloques pueden anidarse cuando tiene sentido semántico (ej: `:::solucion` dentro de `:::ejercicio`).
- Cualquier atributo `id` debe ser único dentro del documento y sirve como ancla para referencias cruzadas.

### Sintaxis de atributos

```
:::tipo                              ← sin atributos
:::tipo id="mi-id"                   ← un atributo
:::tipo id="mi-id" titulo="Título"   ← múltiples atributos
```

Los valores van entre comillas dobles. Los nombres de atributos son siempre minúsculas y sin espacios (usar guión: `concepto-clave`).

---

### 3.1 `:::objetivo`

Objetivos de aprendizaje al inicio de un capítulo o sección.

```
:::objetivo
Al finalizar este tema el estudiante será capaz de:
- Describir X.
- Identificar Y.
- Comparar A con B.
:::
```

**Atributos opcionales:** `id`

---

### 3.2 `:::definicion`

Define uno o más términos técnicos. El formato interno usa `**Término** | Definición` para separar término de definición.

Término único:
```
:::definicion id="fotosintesis"
**Fotosíntesis** | Proceso por el cual las plantas convierten la luz solar en energía química.
:::
```

Múltiples términos:
```
:::definicion multiple
**Mitosis** | División celular que produce dos células hijas idénticas.
**Meiosis** | División celular que produce cuatro células haploides.
:::
```

**Atributos opcionales:** `id`, `multiple` (flag, sin valor)

---

### 3.3 `:::concepto-clave`

Destaca un concepto fundamental que el estudiante debe retener.

```
:::concepto-clave id="ley-ohm"
La corriente eléctrica es directamente proporcional al voltaje e inversamente proporcional a la resistencia: I = V/R.
:::
```

**Atributos opcionales:** `id`

---

### 3.4 `:::nota`

Información complementaria, curiosidades o aclaraciones útiles.

```
:::nota
La palabra "algoritmo" proviene del nombre del matemático persa al-Juarismi (siglo IX).
:::
```

**Atributos opcionales:** `id`

---

### 3.5 `:::alerta`

Señala errores conceptuales frecuentes, confusiones comunes o advertencias importantes.

```
:::alerta
No confundir **masa** con **peso**. La masa es una propiedad intrínseca del objeto (kg); el peso es la fuerza gravitatoria que actúa sobre él (N).
:::
```

**Atributos opcionales:** `id`

---

### 3.6 `:::ejemplo`

Un caso práctico, ejemplo resuelto o aplicación concreta de un concepto.

```
:::ejemplo titulo="Cálculo de velocidad media"
Un automóvil recorre 150 km en 2 horas.
Velocidad media = 150 km / 2 h = 75 km/h.
:::
```

**Atributos opcionales:** `id`, `titulo`

---

### 3.7 `:::ejercicio`

Actividad o problema para que el estudiante resuelva. Puede contener un bloque `:::solucion` anidado.

```
:::ejercicio id="ej-01" titulo="Ley de Ohm"
Si un circuito tiene una resistencia de 10 Ω y un voltaje de 5 V, ¿cuál es la corriente?

:::solucion
I = V / R = 5 V / 10 Ω = 0,5 A
:::
:::
```

**Atributos opcionales:** `id`, `titulo`

---

### 3.8 `:::aplicacion`

Conecta el contenido teórico con su relevancia práctica o profesional. Reemplaza bloques específicos de disciplina (como "clínica" en medicina) por un bloque genérico aplicable a cualquier campo.

```
:::aplicacion titulo="Puentes colgantes"
El cálculo de tensión en cables es fundamental en el diseño de puentes colgantes.
El puente Golden Gate usa cables de acero con una tensión calculada mediante
las mismas ecuaciones de estática que se estudian en este capítulo.
:::
```

```
:::aplicacion titulo="Isquemia cerebral"
Las neuronas toleran solo 4–6 minutos de anoxia antes de sufrir muerte irreversible.
Esto explica la urgencia del tratamiento trombolítico en el ACV.
:::
```

**Atributos opcionales:** `id`, `titulo`

---

### 3.9 `:::comparacion`

Compara dos o más conceptos, estructuras o entidades usando una tabla.

```
:::comparacion titulo="ADN vs. ARN"
| Característica | ADN | ARN |
|---|---|---|
| Azúcar | Desoxirribosa | Ribosa |
| Bases | A, T, G, C | A, U, G, C |
| Cadenas | Doble hélice | Simple cadena |
| Función | Almacenamiento | Expresión génica |
:::
```

**Atributos opcionales:** `id`, `titulo`

---

### 3.10 `:::diagrama`

Describe una figura o diagrama que debe ser creado o insertado. El decodificador decide si genera un placeholder, invoca un motor de diagramas, o muestra la descripción textual.

```
:::diagrama id="fig-ciclo-agua" titulo="El ciclo del agua"
Diagrama circular mostrando:
- Evaporación desde océanos y lagos
- Condensación en nubes
- Precipitación (lluvia, nieve)
- Escorrentía e infiltración
- Flechas indicando el flujo cíclico
:::
```

**Atributos requeridos:** `id`, `titulo`

---

### 3.11 `:::imagen`

Inserta una imagen con metadatos descriptivos. El formato interno usa pares `clave: valor` en líneas separadas.

```
:::imagen id="fig-celula"
archivo: celula_animal.jpg
titulo: "Célula animal típica"
descripcion: "Microfotografía electrónica de una célula animal mostrando núcleo, mitocondrias y retículo endoplásmico."
fuente: "Alberts et al., Molecular Biology of the Cell, 6.ª ed."
alt: "Célula animal con organelas visibles"
:::
```

**Atributos requeridos:** `id`
**Campos internos:** `archivo`, `titulo`, `descripcion`, `fuente`, `alt`

---

### 3.12 `:::pregunta`

Pregunta de estudio o autoevaluación. El atributo `tipo` indica la modalidad.

Desarrollo (respuesta abierta):
```
:::pregunta tipo="desarrollo"
¿Por qué los metales son buenos conductores de electricidad?
:::
```

Selección múltiple:
```
:::pregunta tipo="seleccion" respuesta="c"
¿Cuál es la unidad del SI para la fuerza?

a) Julio
b) Vatio
c) Newton
d) Pascal
:::
```

Caso o problema aplicado:
```
:::pregunta tipo="caso"
Una empresa produce 500 unidades diarias con un costo unitario de $12.
Si el precio de venta es $20, ¿cuál es la utilidad diaria?
¿Qué sucede si el costo de materia prima sube un 15%?
:::
```

**Atributos requeridos:** `tipo` (valores: `desarrollo`, `seleccion`, `caso`, `verdadero-falso`)
**Atributos opcionales:** `id`, `respuesta`

El atributo `respuesta` es semántico — el decodificador decide si lo muestra, lo oculta, o lo presenta según el modo (docente/estudiante).

---

### 3.13 `:::mnemonico`

Recurso nemotécnico para facilitar la memorización.

```
:::mnemonico
**Orden de los planetas → Mi Vieja Tía María Jura Saber Usar Neptuno**
- **M**ercurio
- **V**enus
- **T**ierra
- **M**arte
- **J**úpiter
- **S**aturno
- **U**rano
- **N**eptuno
:::
```

**Atributos opcionales:** `id`

---

### 3.14 `:::historia`

Contexto histórico, anécdotas o historia de un descubrimiento relevante.

```
:::historia titulo="El descubrimiento de la penicilina" personajes="Alexander Fleming" año="1928"
En septiembre de 1928, Alexander Fleming regresó de vacaciones a su laboratorio
en el St. Mary's Hospital de Londres. Encontró que un hongo había contaminado
una de sus placas de cultivo de *Staphylococcus*. Alrededor del hongo, las
bacterias habían muerto. Ese "accidente" condujo al descubrimiento del primer
antibiótico y transformó la medicina para siempre.
:::
```

**Atributos opcionales:** `id`, `titulo`, `personajes`, `año`

---

### 3.15 `:::resumen`

Síntesis al final de un tema, sección o capítulo.

```
:::resumen
- Punto esencial 1.
- Punto esencial 2.
- Punto esencial 3.
:::
```

**Atributos opcionales:** `id`

---

### 3.16 `:::referencia`

Fuentes bibliográficas del capítulo o sección.

```
:::referencia
- Autor A. *Título del libro*. Edición. Editorial; Año.
- Autor B, Autor C. *Otro libro*. Edición. Editorial; Año.
:::
```

**Atributos opcionales:** `id`

---

### 3.17 `:::recuadro`

Contenido complementario que no encaja en las otras categorías: datos curiosos, información adicional, contexto cultural, etc.

```
:::recuadro titulo="¿Sabías que...?"
El número π ha sido calculado a más de 100 billones de dígitos,
pero para la mayoría de aplicaciones de ingeniería bastan 15 decimales.
:::
```

**Atributos opcionales:** `id`, `titulo`

---

## 4. Referencias cruzadas

### 4.1 IDs

Cualquier bloque puede tener un atributo `id` que lo identifica de forma única dentro del documento:

```
:::definicion id="entropia"
**Entropía** | Medida del desorden de un sistema termodinámico.
:::
```

Los encabezados de Markdown generan IDs automáticos basados en su texto (como en CommonMark/GFM).

### 4.2 Sintaxis de referencia

Para referenciar un bloque o encabezado desde cualquier parte del texto:

```
ver{id}          → referencia a un bloque por su id
ver{id texto}    → referencia con texto personalizado
```

Ejemplos:

```
Como se definió en ver{entropia}, el desorden tiende a aumentar.
Observe la ver{fig-ciclo-agua Figura del ciclo del agua}.
Resuelva el ver{ej-01 ejercicio sobre Ley de Ohm}.
```

El decodificador decide cómo renderizar la referencia: como hipervínculo, como número de figura, como texto con página, etc. La **numeración automática** (Figura 1, Tabla 2, Ejercicio 3) es responsabilidad del decodificador.

### 4.3 Referencias entre archivos

Cuando un libro está compuesto por múltiples archivos `.edm`, las referencias cruzadas entre archivos usan la ruta relativa como prefijo:

```
ver{cap02.edm#id-del-bloque}
ver{cap02.edm#id-del-bloque texto personalizado}
```

---

## 5. Includes (composición de documentos)

Un libro se compone desde múltiples archivos `.edm` usando el bloque `::incluir`:

```
::incluir archivo="ruta/al/archivo.edm"
```

Nótese que `::incluir` usa **dos** dos-puntos (no tres), ya que no delimita contenido.

### Archivo raíz del libro

El archivo raíz define la estructura del libro en su frontmatter y/o con includes:

```yaml
---
titulo: "Fundamentos de Física"
autor: "María García"
edicion: 1
---
```

```
# Fundamentos de Física

::incluir archivo="cap01_cinematica.edm"
::incluir archivo="cap02_dinamica.edm"
::incluir archivo="cap03_energia.edm"
::incluir archivo="apendice_formulas.edm"
```

Los includes se resuelven de forma recursiva (un archivo incluido puede incluir otros). El decodificador debe detectar y rechazar inclusiones circulares.

---

## 6. Contenido condicional

### 6.1 `:::solo-docente`

Envuelve contenido que solo debe aparecer en la versión para docentes. El decodificador lo incluye o lo omite según el modo de compilación.

```
:::solo-docente
Las respuestas del examen parcial son:
1. c) Newton
2. a) Verdadero
3. I = V/R = 0,5 A
:::
```

### 6.2 `:::solo-estudiante`

Envuelve contenido exclusivo para la versión del estudiante (por ejemplo, espacios para completar, instrucciones de actividades).

```
:::solo-estudiante
Complete la siguiente tabla con los valores calculados:
| Voltaje (V) | Resistencia (Ω) | Corriente (A) |
|---|---|---|
| 10 | 5 | ___ |
| 20 | 10 | ___ |
:::
```

Estos bloques son **binarios**: el contenido existe o no existe. No hay lógica condicional compleja. El decodificador decide el modo al compilar.

---

## 7. Extensión de archivo y convenciones

- **Extensión:** `.edm` (EduMark Document)
- **Codificación:** UTF-8
- **Nombrado de archivos:** libre, pero se recomienda un esquema descriptivo (ej: `cap01_cinematica.edm`, `U1_03_medula_espinal.edm`)
- **Imágenes:** se referencian por ruta relativa al archivo `.edm`. La organización de carpetas es libre.

---

## 8. Resumen de bloques

| Bloque | Atributos | Propósito |
|---|---|---|
| `:::objetivo` | `id` | Objetivos de aprendizaje |
| `:::definicion` | `id`, `multiple` | Definición de términos |
| `:::concepto-clave` | `id` | Concepto fundamental a retener |
| `:::nota` | `id` | Información complementaria |
| `:::alerta` | `id` | Errores comunes o advertencias |
| `:::ejemplo` | `id`, `titulo` | Caso práctico o ejemplo resuelto |
| `:::ejercicio` | `id`, `titulo` | Problema para resolver (anida `:::solucion`) |
| `:::aplicacion` | `id`, `titulo` | Relevancia práctica/profesional |
| `:::comparacion` | `id`, `titulo` | Tabla comparativa |
| `:::diagrama` | `id`\*, `titulo`\* | Descripción de figura a crear |
| `:::imagen` | `id`\* | Imagen con metadatos |
| `:::pregunta` | `id`, `tipo`\*, `respuesta` | Autoevaluación |
| `:::mnemonico` | `id` | Recurso nemotécnico |
| `:::historia` | `id`, `titulo`, `personajes`, `año` | Contexto histórico |
| `:::resumen` | `id` | Síntesis de sección/capítulo |
| `:::referencia` | `id` | Bibliografía |
| `:::recuadro` | `id`, `titulo` | Contenido complementario libre |
| `:::solo-docente` | — | Contenido solo para docentes |
| `:::solo-estudiante` | — | Contenido solo para estudiantes |
| `:::solucion` | — | Solución (solo dentro de `:::ejercicio`) |

\* = atributo requerido

---

## 9. Lo que NO va en el formato

El `.edm` **nunca** contiene:

- Posición (derecha, izquierda, flotante, centrado)
- Dimensiones (ancho, alto, columnas, márgenes)
- Colores, tipografía, tamaños de fuente
- Clases CSS, estilos inline, o cualquier hint de presentación
- Instrucciones de paginación o saltos de página

Todo eso pertenece a la configuración del decodificador, no al documento.

---

*Edumark v2.0 — Formato abierto para autoría educativa. Puede extenderse con nuevos tipos de bloque según necesidades pedagógicas.*
