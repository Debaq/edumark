Generate a complete educational chapter in Edumark format (.edm) on: $ARGUMENTS

If subject, topic, or student level is not specified, ask before generating.

Read the full format specification in EDUMARK_SPEC.md.

# Edumark Format (.edm)

## Absolute rule

The .edm describes WHAT the content is, never HOW it looks. Zero colors, margins, positions, fonts. Semantics only.

## Base

Full CommonMark. All standard Markdown is valid.

## File structure

1. YAML frontmatter: title, author, version, date, subject, level, topics
2. `# Title`
3. `:::objective` at the beginning
4. `##` sections with expository text interspersed with pedagogical blocks
5. `:::summary` at the end
6. `:::question` for self-assessment
7. `:::reference` with bibliography

## Pedagogical blocks

Syntax: `:::type attribute="value"` ... `:::`

| Block | Attributes | Purpose |
|---|---|---|
| `:::objective` | `id` | Learning objectives (beginning) |
| `:::definition` | `id`, `multiple` | Terms: `**Term** \| Definition` |
| `:::key-concept` | `id` | Core idea to retain |
| `:::note` | `id` | Supplement, trivia |
| `:::warning` | `id` | Common errors, warnings |
| `:::example` | `id`, `title` | Worked example |
| `:::exercise` | `id`, `title` | Problem (nests `:::solution`) |
| `:::application` | `id`, `title` | Theory → professional practice |
| `:::comparison` | `id`, `title` | Comparative table |
| `:::diagram` | `id`*, `title`* | Figure: text description + optional ```mermaid/d2/dot/svg code block |
| `:::image` | `id`* | Image with fields: file, title, description, source, alt |
| `:::question` | `id`, `type`* | GIFT-style: `=` correct, `~` wrong, `#` feedback |
| `:::mnemonic` | `id` | Memory aid |
| `:::history` | `id`, `title`, `characters`, `year` | Historical anecdote |
| `:::summary` | `id` | Synthesis (end) |
| `:::reference` | `id` | Bibliography |
| `:::aside` | `id`, `title` | Fun fact, free-form supplement |
| `:::math` | `id` | Display math (Unicode, one equation per line) |
| `m{...}` | — | Inline math within text |
| `:::teacher-only` | — | Teacher version only |
| `:::student-only` | — | Student version only |

\* = required

## Math

Display math block: `:::math` with one equation per line in natural Unicode.
Inline math: `m{v₀ + a·t}` within running text.

The author writes human-readable Unicode, **never** LaTeX (`\frac`, `$$`, `\sqrt`):

| Convention | Example |
|---|---|
| Subscripts | `v₀`, `x₁`, `ₙ`, `ₘ`, `ᵢ`, `ⱼ`, `ₓ`, `v_{max}` |
| Superscripts | `t²`, `v³`, `x⁴`, `ⁿ` |
| Greek | `Δ`, `α`, `β`, `γ`, `θ`, `λ`, `μ`, `π`, `σ`, `ω`, `φ`, `ε`, `τ`, `ρ` |
| Operators | `·` (product), `×`, `÷`, `±`, `∓` |
| Comparisons | `≈`, `≠`, `≤`, `≥`, `∝` |
| Symbols | `∞`, `→`, `←`, `⇒`, `∈`, `∑`, `∫`, `∂` |
| Fractions | `Δx/Δt`, `(a+b)/(c-d)`, `½`, `⅓`, `¼`, `⅔`, `¾` |
| Root / bar | `√(2·g·h)`, `√2`, `v̄`, `x̄` |
| Limit | `lím` |

## SVG in diagrams

When a diagram needs precise visual shapes (circuits, geometric figures, molecular structures), use ```svg inside `:::diagram`:

```
:::diagram id="fig-example" title="Example"
Text description as fallback.

```svg
<svg viewBox="0 0 200 100" xmlns="http://www.w3.org/2000/svg">
  <!-- shapes here -->
</svg>
```
:::
```

SVG rules:
- Always use `viewBox`, never fixed `width`/`height`
- Use `currentColor` for strokes and fills (the theme controls colors)
- No external references, no `<style>` blocks, no `style` attributes
- Use SVG presentation attributes directly (`stroke`, `fill`, `stroke-width`)
- Keep it simple: schematic/educational diagrams, not complex illustrations

## Cross-references

- `ref{id}` → reference to a block
- `ref{id text}` → with custom text
- `ref{file.edm#id}` → cross-file

## Includes

`::include file="path.edm"` (two colons, not three)

## Writing rules

1. Alternate expository text with blocks. Never 3+ blocks in a row without narrative text.
2. Use variety: history, application, mnemonic, comparison, warning — not just definitions.
3. Progressive: simple to complex.
4. Real warnings: errors students actually make on this topic.
5. Concrete applications with real data.
6. Engaging historical stories.
7. Varied questions: open, choice, case.
8. Descriptive IDs with prefixes: fig-, ex-, def-.
9. Write in the user's language.
