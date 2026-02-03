# Gemini/Antigravity Instruction Templates

These templates are the **definitive interface** between humans and the Gemini/Antigravity hybrid setup. They implement a **multi-session, phase-gated workflow** where:

- Each phase produces **artifacts that survive session restarts** (persisted in `GEMINI.md` and `docs/`)
- Human approval is a **BLOCKING GATE**
- The goal is to build a **self-sustaining system** (Personas + Skills)
- Fresh Conversations are a **feature, not a bug**

---

## Template Selection Guide

### For New Projects

```
01a-analysis-new.md → 02-planning.md → 03-implementation.md → 04-validation.md
```

### For Existing Projects

```
01b-analysis-existing.md → 02-planning.md → 03-implementation.md → 04-validation.md
```

### Quick Reference

| Phase | Template | Purpose | Gate |
|-------|----------|---------|------|
| **1** | `01a-analysis-new.md` | New project analysis (VP, USP, AAA, Platform) | Human verifies artifacts |
| **1** | `01b-analysis-existing.md` | Existing codebase trace (80/15/5, personas, skills) | Human verifies artifacts |
| **2** | `02-planning.md` | Todo creation, architecture | **HARD: APPROVED/REVISE/ABORT** |
| **3** | `03-implementation.md` | Implementation loop (paste repeatedly) | All todos complete |
| **4** | `04-validation.md` | E2E testing, parity, manual checklist | Human validates |

---

## How to Use

### Step 1: Copy Entire Template
Don't excerpt - copy the **entire file** including all sections.

### Step 2: Fill HUMAN INPUT Section
This is YOUR domain:
- Objectives and vision
- Domain knowledge
- Constraints
- Credentials location

### Step 3: Paste into Terminal
Paste the content into your Gemini CLI or Antigravity chat interface.

### Step 4: Wait for GATE
The agent will stop at the gate and present a summary.

### Step 5: Provide Explicit Approval
Type one of:
- **APPROVED** - Proceed to next phase
- **REVISE: [feedback]** - Make adjustments
- **ABORT** - Stop work

### Step 6: Paste Next Template
When ready, paste the next phase template.

---

## Important Notes

### GEMINI.md is the Source of Truth
Instead of just `docs/`, we heavily leverage `GEMINI.md` as the root context file that persists across sessions. Updates to the project status are recorded there or in `docs/00-context/`.

### docs/04-context/ Is Always Created
Phase 2 always creates `docs/04-context/00-session-context.md` as the base context loading document for fresh sessions.

---

## Self-Sustainability Goal

The end state is Personas/Skills that work WITHOUT these templates.

**Validation Test** (in Phase 4):
1. Start fresh Gemini session
2. Ask agent to implement a new feature using ONLY:
   - `.gemini/knowledge/` (Personas)
   - `.agent/skills/` (Skills)
   - `docs/00-developers/`
3. If it can complete the task, system is self-sustaining
