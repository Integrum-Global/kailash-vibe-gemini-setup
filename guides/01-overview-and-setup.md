# Guide 01: Overview & Setup

## 1. The Hybrid Architecture

The target architecture is a **Hybrid Configuration** that satisfies both the Gemini CLI (for terminal-based, fast interactions) and Google Antigravity (for complex, multi-step autonomous tasks).

### Why Hybrid?
*   **Gemini CLI**: Best for "Vibe Coding"—quick questions, rapid prototyping, and focused file edits in the terminal.
*   **Antigravity**: Best for "Heavy Lifting"—orchestrating multi-agent workflows, managing long-running tasks, and maintaining deep project state.

To achieve this, we use a shared knowledge base where:
1.  **Skills** are stored in `.agent/skills` (Antigravity standard) and symlinked to `.gemini/skills` (Gemini CLI standard).
2.  **Context** is centralized in `GEMINI.md`.

## 2. Installation

### Prerequisites
*   Node.js v18+
*   Google Cloud Account (for authentication)

### Step 1: Install Gemini CLI
Use `npm` to install the official CLI tool:

```bash
npm install -g @google/gemini-cli
```

### Step 2: Authenticate
Login to your Google account to enable model access:

```bash
gemini auth login
```

### Step 3: Antigravity Setup
Ensure you have the Agent Manager configured. If using the Antigravity VS Code extension or web interface, ensure it is connected to the same workspace root.

## 3. Repository Structure

We will migrate the `.claude` structure to a dual `.gemini` and `.agent` structure.

### Final Directory Layout

```text
.
├── .agent/                  # Antigravity Root
│   ├── skills/              # SHARED: Domain knowledge (migrated from .claude/skills)
│   └── workflows/           # Process definitions (migrated from Hooks/Agents)
│
├── .gemini/                 # Gemini CLI Root
│   ├── skills/              # SYMLINK -> ../.agent/skills
│   ├── knowledge/           # Personas (migrated from .claude/agents)
│   └── settings.json        # CLI Configuration & MCP setup
│
├── GEMINI.md                # Project Context (migrated from CLAUDE.md)
└── ... (Source code)
```

## 4. Initialization Steps

Run the following commands in your project root to set up the structure:

```bash
# 1. Create directories
mkdir -p .agent/skills .agent/workflows .gemini/knowledge

# 2. Create the Symlink for Skills
# This ensures both platforms see the same skills
ln -s ../.agent/skills .gemini/skills

# 3. Create initial GEMINI.md
touch GEMINI.md
```

## 5. Verification

To verify the setup, run:

```bash
gemini --version
ls -la .gemini/skills
```

The `ls` command should show that `.gemini/skills` points to `.agent/skills`.

---
**Next**: [Migrate your Agents to Personas in Guide 02](02-agent-configuration.md)
