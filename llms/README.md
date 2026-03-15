# LLM Prompts para Edumark

Prompts listos para que cualquier LLM genere capítulos `.edm` completos y bien estructurados.

| Archivo | Para quién |
|---|---|
| `edumark_claude.md` | Claude Code (skill / custom slash command) |
| `edumark_universal.md` | Cualquier otro LLM (ChatGPT, Gemini, Qwen, DeepSeek, Ollama, etc.) |

---

## Uso en Claude Code

El prompt `edumark_claude.md` está diseñado para usarse como **custom slash command** (skill). Así Claude Code puede generar capítulos `.edm` directamente desde la terminal.

### Configuración

1. Crear la carpeta de comandos si no existe:
   ```bash
   mkdir -p .claude/commands
   ```

2. Copiar (o enlazar) el prompt:
   ```bash
   cp llms/edumark_claude.md .claude/commands/edumark.md
   ```

3. Usar desde Claude Code:
   ```
   /edumark Cinemática en una dimensión, nivel universitario, física
   ```

Claude leerá la spec, aplicará las reglas de escritura y generará el capítulo completo.

---

## Uso en otros LLMs

El prompt `edumark_universal.md` funciona como **system prompt** en cualquier plataforma. Copiá su contenido y cargalo según tu LLM:

### ChatGPT (Custom GPT)

1. Ir a **Explore GPTs → Create**
2. En **Instructions**, pegar el contenido de `edumark_universal.md`
3. Subir `EDUMARK_SPEC.md` como archivo de conocimiento
4. Pedirle: *"Generá un capítulo sobre termodinámica, nivel universitario"*

### Gemini (Google AI Studio)

1. Ir a **AI Studio → Create new prompt**
2. En **System instructions**, pegar el contenido de `edumark_universal.md`
3. En el chat, enviar `EDUMARK_SPEC.md` como contexto y luego pedir el capítulo

### API (OpenAI, Anthropic, Google, etc.)

Usar el prompt como `system` message y la spec como contexto:

```python
messages = [
    {"role": "system", "content": open("llms/edumark_universal.md").read()},
    {"role": "user", "content": "Generá un capítulo sobre la célula, nivel secundario, biología"}
]
```

### Ollama / LLMs locales

```bash
ollama run llama3 --system "$(cat llms/edumark_universal.md)"
```

Luego pedirle el capítulo en el chat.

---

## Tip general

Para mejores resultados, siempre incluir `EDUMARK_SPEC.md` como contexto adicional (archivo adjunto, knowledge base, o en el mensaje). Los prompts son un resumen operativo; la spec es la referencia completa.
