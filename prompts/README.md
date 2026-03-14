# LLM Prompts

Each file is a ready-to-use prompt for its platform. Copy the content and paste it where it belongs.

| File | Platform | Where to paste |
|---|---|---|
| `edumark_claude.md` | Claude Code | `.claude/commands/edumark.md` → use with `/project:edumark topic` |
| `edumark_claude.md` | Claude Projects | In "Project instructions" |
| `edumark_claude.md` | Claude API | `system` field in the request |
| `edumark_gemini.md` | Gemini Gems | Gem instructions |
| `edumark_gemini.md` | Gemini API | `system_instruction` field |
| `edumark_chatgpt.md` | Custom GPT | "Instructions" field in the builder |
| `edumark_chatgpt.md` | OpenAI API / Assistants | `instructions` field or `system` message |
| `edumark_universal.md` | Qwen, Kimi, DeepSeek, Mistral, Llama, Ollama, LM Studio | System prompt or first chat message |

## Differences between versions

- **Claude**: uses `$ARGUMENTS` (variable that Claude Code replaces with whatever the user types after the command)
- **ChatGPT**: asks the model to deliver output inside a code block for easy copy
- **Gemini**: same as universal, adapted for Gem format
- **Universal**: generic, works on any model that reads natural language instructions
