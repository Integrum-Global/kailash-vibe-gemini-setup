# Guide 02: Agent Configuration

## 1. Migration Strategy: Agents to Personas

In Claude Code, "Agents" are specialized markdown prompt definitions (e.g., `dataflow-specialist.md`). In the Gemini/Antigravity ecosystem, we map these to **Personas** or **Knowledge Agents**.

### The Mapping

| Claude Code Component | Gemini / Antigravity Component | Location |
| :--- | :--- | :--- |
| **Specialist Agent** | Knowledge Persona | `.gemini/knowledge/` |
| **Process Agent** | Workflow | `.agent/workflows/` (See Guide 04) |

**Key Distinction**:
*   If the agent provides **advice/knowledge** (e.g., "How do I use DataFlow?"), it is a **Persona**.
*   If the agent **performs a sequence of steps** (e.g., "Implement TDD"), it is a **Workflow**.

## 2. Converting a Specialist Agent

Move the markdown file from `.claude/agents/` to `.gemini/knowledge/`.

### Example: `dataflow-specialist.md`

**Original Location**: `.claude/agents/frameworks/dataflow-specialist.md`
**New Location**: `.gemini/knowledge/dataflow-specialist.md`

You generally do not need to change the content, as Gemini can read the same markdown structure. However, ensure the frontmatter is clean.

```markdown
<!-- .gemini/knowledge/dataflow-specialist.md -->

# DataFlow Specialist Persona

You are the DataFlow Specialist, an expert in the Kailash DataFlow framework.

## Directives
1.  **Immutability**: Always prefer immutable data structures.
2.  **State**: Manage state only within `Flow` nodes.
...
```

## 3. Contextual Persona Adoption

In Gemini CLI, you "delegate" to a specialist not by a hard-coded command, but by **Contextual Adoption**.

### Usage Pattern

When you need specialist advice, explicitly reference the persona in your prompt or use the file context.

**Option A: Load Context Manually**
```bash
gemini "As the @.gemini/knowledge/dataflow-specialist.md, review this schema"
```

**Option B: Hybrid "Simulated" Delegation**
Antigravity allows defining "Knowledge Items" that can be auto-retrieved. If you place `dataflow-specialist.md` in a Knowledge Item, the system may auto-retrieve it when "DataFlow" is mentioned.

## 4. Advanced: The "Expert Panel" Workflow

To replicate the "Chain of Thought" or "Multi-Agent Review" capability:

1.  Create a workflow file (e.g., `.agent/workflows/architecture-review.md`).
2.  Define steps that adopt different personas sequentially.

```markdown
<!-- .agent/workflows/architecture-review.md -->

1.  Load context from `.gemini/knowledge/deep-analyst.md`.
2.  Ask: "Analyze the security risks of the proposed feature."
3.  Load context from `.gemini/knowledge/security-specialist.md`.
4.  Ask: "Critique the analysis and provide mitigation strategies."
```

## 5. Migration Checklist

*   [ ] Create `.gemini/knowledge/` directory.
*   [ ] Copy "Advice" agents (Analyst, Reviewer, Specialist) to `.gemini/knowledge/`.
*   [ ] Copy "Process" agents (TDD, Committer) to `.agent/workflows/` (See Guide 04).
*   [ ] Verify you can invoke a persona: `gemini "Act as @.gemini/knowledge/my-persona.md ..."`
