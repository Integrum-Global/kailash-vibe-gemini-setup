# Guide 04: Workflow Automation

## 1. Migration Strategy: Hooks to Workflows

Claude Code relied heavily on **Hooks** (JavaScript scripts in `scripts/hooks/`) to enforce quality gates (e.g., `PreToolUse`, `PostToolUse`).
Antigravity and Gemini CLI use **Workflows** (`.agent/workflows/`) to define repeatable, multi-step processes that can include validation.

> [!WARNING]
> **Important Difference**: Unlike Claude Hooks which run *automatically*, Gemini Workflows typically require **explicit activation** (e.g., you must ask to run them). See [Gap Analysis](GAP_ANALYSIS.md) for details.

### The Mapping

| Claude Code Component | Gemini / Antigravity Component | Location |
| :--- | :--- | :--- |
| **Hook** (e.g., `validate-workflow.js`) | **Workflow Step** or **MCP Validation** | `.agent/workflows/` |
| **Process Agent** (e.g., `tdd-implementer`) | **Workflow** | `.agent/workflows/tdd.md` |

## 2. Converting Process Agents to Workflows

A "Process Agent" in Claude (like `tdd-implementer.md`) was essentially a set of instructions telling Claude to follow a specific sequence. In Antigravity, we make this explicit.

### Example: TDD Workflow

**Original**: An agent that says "Always write tests first."
**New**: A structured Workflow file.

```markdown
<!-- .agent/workflows/tdd-cycle.md -->
---
description: Implement a feature using Test Driven Development
---

1.  **Analyze Request**: Understand the feature requirements from the user.
2.  **Write Failing Test**: Create a test file in `tests/` that exerts the new functionality.
3.  **Run Test**: Execute the test to confirm it fails (Red).
4.  **Implement Code**: Write the minimum code in `src/` to satisfy the test.
5.  **Run Test Again**: Confirm it passes (Green).
6.  **Refactor**: Clean up the code if necessary.
```

### Usage
In Antigravity or Gemini CLI (with workflow support):
```bash
gemini "Run the TDD workflow for a new user login feature"
```

## 3. Replacing Hooks with "Check" Workflows

Claude hooks ran automatically. In the standard Gemini CLI, you may need to run checks explicitly, or configure your MCP server to reject invalid inputs.

### Explicit Validation Workflow
Instead of an invisible hook that blocks you, run a "Commit Check" workflow before finishing methods.

```markdown
<!-- .agent/workflows/pre-commit.md -->
---
description: Verify code quality before commit
---

// turbo-all
1.  Run linter: `npm run lint`
2.  Run tests: `npm test`
3.  Check for TODOs: `grep -r "TODO" src/`
4.  If all pass, git commit.
```

## 4. Automating with File Watchers (Advanced)

For true "Hook-like" behavior (running on file save), you can use external tools like `nodemon` or `chokidar-cli` to trigger Gemini workflows, though this is an advanced setup.

## 5. Migration Checklist

*   [ ] Analyze `scripts/hooks/*.js` to understand what logic needs preserving.
*   [ ] Create `.agent/workflows/` directory.
*   [ ] Create workflow files for key processes (TDD, Code Review, Deployment).
*   [ ] Test workflows using `gemini "Run [workflow name]..."`.
