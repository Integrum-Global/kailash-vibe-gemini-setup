# PHASE 4: Validation

> **Usage**: Paste this template into a FRESH Conversation after all todos are implemented.
> **Prerequisite**: All todos in `todos/active/` marked completed.
> **Next Phase**: Final delivery / Merge.

---

## HUMAN INPUT

### Deployment Info (If Applicable)

- [ ] Staging environment deployed
- [ ] Production environment ready

### Manual Validation Credentials

- URL:
- Test User:
- Password (or ref to env):

---

## GEMINI/ANTIGRAVITY INSTRUCTIONS

### Validation Tasks

#### 1. Test Coverage Analysis

1. **Run All Test Tiers**

   ```bash
   # Unit tests
   pytest tests/unit/ --timeout=1 -v

   # Integration tests (Docker must be running - verify with test-env status)
   pytest tests/integration/ --timeout=5 -v

   # E2E tests (Docker must be running - verify with test-env status)
   pytest tests/e2e/ --timeout=10 -v
   ```

2. **Coverage Report**

   ```bash
   pytest --cov=src --cov-report=term-missing --cov-report=html
   ```

3. **NO MOCKING Compliance**
   - Verify Tier 2-3 tests use real infrastructure
   - Flag any mocks in integration/E2E tests

#### 2. Workflow Documentation

For each user workflow tested:

1. **Step-by-Step Documentation**
   - Document each step in detail
   - Include expected state at each step
   - Document transitions between steps

2. **Test Generation**
   - Generate tests for each step
   - Generate tests for transitions
   - Include metrics collection

#### 3. Parity Validation (If Applicable)

**3.1 Old System Baseline**

1. Test run the OLD system through all required workflows
2. Document outputs for each workflow
3. Run multiple times to determine output type:
   - **Deterministic**: Labels, numbers, fixed values
   - **Natural Language**: Generated text, summaries, responses

**3.2 Natural Language Evaluation**

**CRITICAL: DO NOT test NLP outputs with simple keyword/regex assertions**

For natural language outputs, use LLM evaluation via `llm-evaluator` or similar tool/script.

**3.3 Parity Report**
Document:

- Feature parity: What old system did vs what new system does
- Output parity: Comparison results with confidence levels
- Gaps identified: Any functionality lost in transition

#### 4. Self-Sustainability Validation

**EXIT CRITERIA**: Created personas and skills must work WITHOUT instruction templates.

**⚠️ HARD PREREQUISITE - FAIL IF MISSING:**

```
Required: .gemini/knowledge/project/ with domain-specific personas
Required: .agent/skills/project/SKILL.md with project skills
Required: docs/00-developers/ with developer documentation
```

**If any of these are missing or empty:**

1. **STOP VALIDATION** - Do not proceed
2. **Create rework todos** in `todos/active/`:
   ```
   TODO-REWORK-001: Create [project]-analyst persona for [gap]
   TODO-REWORK-002: Expand SKILL.md with [missing pattern]
   TODO-REWORK-003: Document [feature] in docs/00-developers/
   ```
3. **Return to Phase 3 template** with rework todos
4. **Complete all rework todos**
5. **Resume Phase 4** only after rework todos complete

**Validation Test:**

1. Start a fresh conversation
2. Ask: "Using only the personas in `.gemini/knowledge/project/` and skills in `.agent/skills/project/`, implement [simple feature X]"
3. Agent MUST be able to (all required):
   - Understand what to do (personas)
   - Know how to do it (skills)
   - Reference knowledge base (docs/00-developers/)

**FAIL CRITERIA:**
If agent cannot complete without instruction templates:

1. **Identify gaps**: What did agent need that wasn't in personas/skills?
2. **Create rework todos** for each gap identified
3. **Return to Phase 3** with rework todos
4. **Re-run validation test** after rework complete
5. Do NOT declare validation complete until test passes

**REWORK LIMITS:**
- Maximum 2 rework cycles allowed
- If still failing after 2nd rework: **ESCALATE to human for architectural review**

#### 5. Human Validation Checklist Generation

Generate a detailed checklist for human manual validation:

```markdown
## Manual Validation Checklist

### Pre-Conditions

- [ ] System is deployed/running
- [ ] Test data is populated
- [ ] User accounts are set up

### User Journey: [Name]

#### Steps

1. [ ] Navigate to [page/screen]
   - Expected: [what you should see]
   - Actual: **\*\***\_\_\_**\*\***

2. [ ] Perform [action]
   - Expected: [result]
   - Actual: **\*\***\_\_\_**\*\***

3. [ ] Verify [outcome]
   - Expected: [state]
   - Actual: **\*\***\_\_\_**\*\***

#### Edge Cases

- [ ] Empty input: **\*\***\_\_\_**\*\***
- [ ] Invalid input: **\*\***\_\_\_**\*\***
- [ ] Concurrent access: **\*\***\_\_\_**\*\***
```

---

## PHASE 4 COMPLETE - FINAL GATE

Present to human:

### 1. Test Execution Summary

- Unit tests: X/Y passed (THRESHOLD: 100% required)
- Integration tests: X/Y passed (THRESHOLD: 100% required)
- E2E tests: X/Y passed (THRESHOLD: 100% required)
- Coverage: X% (THRESHOLD: 80% minimum, 100% for critical paths)

**FAIL VALIDATION if any threshold not met.**

### 2. Parity Validation Results (REQUIRED if parity validation selected)

- Overview: [summary]
- NLP outputs: Confidence scores

### 3. Self-Sustainability Assessment

- Personas created: [list] (REQUIRED: minimum 1)
- Skills created: [list] (REQUIRED: SKILL.md + minimum 1 pattern file)
- Validation test result: PASS/FAIL (REQUIRED: PASS)

**FAIL VALIDATION if any REQUIRED not met.**

### 4. Manual Validation Checklist

- Provide complete checklist for human execution

### 5. Artifacts Summary

- Code: [list of modules/files]
- Tests: [test file count by tier]
- Documentation: [docs created]
- Personas/Skills: [list if created]

---

## ⚠️ HUMAN VALIDATION GATE ⚠️

Human must manually validate using the checklist and respond:

- **APPROVED** - Proceed with PR/deployment
- **REVISE: [description]** - Agent fixes and re-validates
- **ABORT** - Implementation rejected, rollback

**Silence is NOT approval.** Wait for explicit response.

**This is the final quality gate before release.**

---

_Template Version: 2.0_
_Phase: 4 of 4 (Validation)_
