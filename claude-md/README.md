# CLAUDE.md Templates

Pre-built CLAUDE.md templates for different project types. Copy the one that matches your project, then customize.

## Available Templates

| Template | For | Key rules |
|----------|-----|-----------|
| `vue-frontend.md` | Vue 3 + Vite frontend projects | Composition API, Pinia, Ant Design Vue conventions |
| `h5-mobile.md` | H5 mobile apps (WebView) | Mobile-first, vw/vh units, native bridge awareness |

## How to use

1. Pick the template closest to your project type
2. Copy it to your project root as `CLAUDE.md`
3. Customize the project-specific sections (name, API patterns, etc.)
4. The AI agent will automatically read this file at the start of each session

```bash
cp claude-md/vue-frontend.md /path/to/your-project/CLAUDE.md
```

## What goes in CLAUDE.md vs CONTEXT.md?

| CLAUDE.md | CONTEXT.md |
|-----------|------------|
| **Technical rules**: coding style, naming, framework conventions | **Domain language**: business terms, relationships |
| **What the AI must/must not do** | **What words mean in this project** |
| Rarely changes | Grows as the project evolves |
| One per project type | Unique per project |
