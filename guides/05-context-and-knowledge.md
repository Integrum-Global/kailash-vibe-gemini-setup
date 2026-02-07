# Guide 05: Context and Knowledge Management

## 1. Migration Strategy: CLAUDE.md to GEMINI.md

The heart of the project context in Claude Code was `CLAUDE.md`. In the Gemini ecosystem, this file is named `GEMINI.md`.

### The Mapping

| Claude Code | Gemini / Antigravity | Function |
| :--- | :--- | :--- |
| `CLAUDE.md` | `GEMINI.md` | Root Project Context & Rules |
| `.claude/rules/*.md` | Embedded Directives | Mandatory constraints |
| `.claude/kailash-learning/` | Native Memory | Learned patterns |

## 2. Setting up GEMINI.md

Simply rename your existing `CLAUDE.md` to `GEMINI.md`, but update the header and key directives.

### Structure of a Good GEMINI.md

```markdown
# Project Context

> [!NOTE]
> This project uses a **Hybrid Configuration** for Gemini CLI and Antigravity.
> - Skills: `.gemini/skills`
> - Knowledge: `.gemini/knowledge`

## Core Directives

1.  **Architecture**: We use the Kailash SDK (DataFlow, Nexus).
2.  **Testing**: NO MOCKING allowed in integration tests.
3.  **Imports**: Always use absolute imports.

## Common Commands (Reference)

- Build: `npm run build`
- Test: `npm test`
```

## 3. Migrating Rules

Claude had separate rule files (`rules/security.md`, etc.).
In Gemini:
1.  **Critical Rules**: Embed them directly in `GEMINI.md` under a "Directives" or "Constraints" section.
2.  **Domain Rules**: Keep them in specific Specialist Personas (e.g., `security-specialist.md` contains the detailed security rules).

## 4. The Learning System

Claude Code used a complex script-based learning system (`/learn`, `/evolve`).

### Antigravity Users
Antigravity has **Native Implicit Learning**. The system indexes conversations and persists memory automatically.

### Pure Gemini CLI Users
Standard Gemini CLI does **not** persist memory across sessions automatically in the same way.

**Recommended Strategy**:
*   **Manual Updates**: Treat `GEMINI.md` as your "Long Term Memory".
*   **Learned Lessons**: Create a `knowledge/lessons-learned.md` and manually add new patterns there.

### Migration Action
You can delete the `scripts/learning/` directory.

## 5. Migration Checklist

*   [ ] Rename `CLAUDE.md` to `GEMINI.md`.
*   [ ] Update `GEMINI.md` header to mention Hybrid Setup.
*   [ ] Copy critical "MUST" constraints from `.claude/rules/` into `GEMINI.md`.
*   [ ] Remove `scripts/learning/` and `scripts/hooks/` (once workflows are ready).
