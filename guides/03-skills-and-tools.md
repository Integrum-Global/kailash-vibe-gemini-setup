# Guide 03: Skills and Tools

## 1. Migration Strategy: Skills

Skills in Claude Code (`.claude/skills/`) are structured directories containing markdown knowledge (`SKILL.md`) and sometimes snippets.
Gemini CLI and Antigravity use an identical concept called **Agent Skills**.

### The Mapping

| Claude Code | Gemini / Antigravity | Status |
| :--- | :--- | :--- |
| `.claude/skills/` | `.agent/skills/` | Direct Copy |
| `/sdk` (Command) | `/skills` (CLI Command) | Supported |

## 2. Setting Up Skills

Because we created a symlink in Guide 01 (`.gemini/skills -> .agent/skills`), any skill you migrate to `.agent/skills` is immediately available to both platforms.

### Step 1: Copy Skills
Move your skill directories:

```bash
cp -r .claude/skills/* .agent/skills/
```

### Step 2: Verify Structure
Ensure each skill has a `SKILL.md` file.

```text
.agent/skills/
└── 02-dataflow/
    ├── SKILL.md  <-- Required by Gemini/Antigravity
    └── examples/
```

## 3. Using Skills in Gemini CLI

Gemini CLI has native support for skill discovery.

### Listing Skills
```bash
gemini /skills list
```

### Activating a Skill
Unlike Claude's slash commands which are hardcoded (e.g., `/db`), Gemini allows you to activate any skill by name or description match, or explicitly:

```bash
# Explicit activation
gemini /skills enable 02-dataflow

# Natural language activation (Auto-discovery)
gemini "How do I create a model using DataFlow?"
# System auto-loads 02-dataflow skill if description matches
```

## 4. MCP Integration (Replacing Hooks)

Claude Code used "Hooks" (`scripts/hooks/`) for tool execution and validation. Gemini uses the **Model Context Protocol (MCP)** to expose tools.

### Configuration
Edit `~/.gemini/settings.json` (or project-local settings if supported) to add MCP servers.

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "."]
    },
    "git": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-git"]
    }
  }
}
```

### Migrating Custom Tools
If you had custom scripts (e.g., a specific database migration script used by Claude):
1.  Wrap the script in a simple MCP server.
2.  Or simpler: Place the script in `.agent/skills/your-skill/scripts/` and instruct the agent how to run it via `run_command`.

## 5. Migration Checklist

*   [ ] Copy all content from `.claude/skills/` to `.agent/skills/`.
*   [ ] Verify the symlink exists (`ls -l .gemini/skills`).
*   [ ] Test skill discovery: `gemini /skills list`.
*   [ ] Configure basic MCP servers (Filesystem, Git) in Gemini settings.
