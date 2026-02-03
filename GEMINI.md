# GEMINI.md - Project Context

> [!NOTE]
> This project uses a **Hybrid Configuration** for Gemini CLI and Antigravity.
> - **Skills**: `.agent/skills` (Shared)
> - **Knowledge**: `.gemini/knowledge` (Personas)
> - **Context**: `GEMINI.md` (This file)

## 📌 Project Overview
This repository utilizes the **Kailash SDK Ecosystem** (Core, DataFlow, Nexus, Kaizen).
This context file serves as the **Single Source of Truth** for AI agent behavior.

## 🧠 Mental Model: The Antigravity Agent

1.  **I am an Expert**: I do not guess. I invoke specialist personas.
2.  **I am Explicit**: I follow the 4-Phase Lifecycle (Analysis -> Planning -> Implementation -> Validation).
3.  **I am Persistent**: I use `docs/` and `GEMINI.md` to maintain state across sessions.

---

## 🚀 Quick Start
- **Analyze**: Copy/paste `instructions/templates/01a-analysis-new.md`
- **Plan**: Copy/paste `instructions/templates/02-planning.md`
- **Implement**: Copy/paste `instructions/templates/03-implementation.md`
- **Validate**: Copy/paste `instructions/templates/04-validation.md`

---

## ⚡ Essential Pattern (All Frameworks)

### For Docker/FastAPI Deployment (RECOMMENDED)
```python
from kailash.workflow.builder import WorkflowBuilder
from kailash.runtime import AsyncLocalRuntime  # Docker-optimized runtime

workflow = WorkflowBuilder()
workflow.add_node("NodeName", "id", {"param": "value"})  # String-based
runtime = AsyncLocalRuntime()  # Async-first, no threading
results, run_id = await runtime.execute_workflow_async(workflow.build(), inputs={})  # Same return as LocalRuntime!
```

### For CLI/Scripts (Sync Contexts)
```python
from kailash.workflow.builder import WorkflowBuilder
from kailash.runtime import LocalRuntime

workflow = WorkflowBuilder()
workflow.add_node("NodeName", "id", {"param": "value"})  # String-based
runtime = LocalRuntime()  # Inherits from BaseRuntime with 3 mixins
results, run_id = runtime.execute(workflow.build())  # ALWAYS .build()
```

### Auto-Detection (Simplest)
```python
from kailash.runtime import get_runtime

# Automatically selects AsyncLocalRuntime for Docker/FastAPI,
# LocalRuntime for CLI/scripts
runtime = get_runtime()  # Defaults to "async" context
```

### Runtime Architecture (Internal)
Both LocalRuntime and AsyncLocalRuntime inherit from BaseRuntime and use shared mixins:

**BaseRuntime Foundation**:
- 29 configuration parameters (debug, enable_cycles, conditional_execution, connection_validation, etc.)
- Execution metadata management (run IDs, workflow caching)
- Common initialization and validation modes (strict, warn, off)

**Shared Mixins**:
- **CycleExecutionMixin**: Cycle execution delegation to CyclicWorkflowExecutor with validation and error wrapping
- **ValidationMixin**: Workflow structure validation (5 methods)
  - validate_workflow(): Checks workflow structure, node connections, parameter mappings
  - _validate_connection_contracts(): Validates connection parameter contracts
  - _validate_conditional_execution_prerequisites(): Validates conditional execution setup
  - _validate_switch_results(): Validates switch node results
  - _validate_conditional_execution_results(): Validates conditional execution results
- **ConditionalExecutionMixin**: Conditional execution and branching logic with SwitchNode support
  - Pattern detection and cycle detection
  - Node skipping and hierarchical execution
  - Conditional workflow orchestration

**LocalRuntime-Specific Features**:
- Enhanced error messages via _generate_enhanced_validation_error()
- Connection context building via _build_connection_context()
- Public validation API: get_validation_metrics(), reset_validation_metrics()

**ParameterHandlingMixin Not Used**:
LocalRuntime uses WorkflowParameterInjector for enterprise parameter handling instead of ParameterHandlingMixin (architectural boundary for complex workflows).

**Usage**:
```python
# Configuration from BaseRuntime (29 parameters)
runtime = LocalRuntime(
    debug=True,
    enable_cycles=True,                    # CycleExecutionMixin
    conditional_execution="skip_branches",  # ConditionalExecutionMixin
    connection_validation="strict"          # ValidationMixin (strict/warn/off)
)
results, run_id = runtime.execute(workflow.build())

# Validation metrics (LocalRuntime public API)
metrics = runtime.get_validation_metrics()
runtime.reset_validation_metrics()
```

This architecture ensures consistent behavior between sync and async runtimes with no API changes.

**AsyncLocalRuntime-Specific Features**:
AsyncLocalRuntime extends LocalRuntime with async-optimized execution:
- **WorkflowAnalyzer**: Analyzes workflows to determine optimal execution strategy
- **ExecutionContext**: Async execution context with integrated resource access
- **Execution Strategies**: Automatically selects pure async, mixed, or sync-only execution
- **Level-Based Parallelism**: Executes independent nodes concurrently within dependency levels
- **Thread Pool**: Executes sync nodes without blocking async loop
- **Semaphore Control**: Executes sync nodes without blocking async loop
- **Semaphore Control**: Limits concurrent executions to prevent resource exhaustion

Inherits all LocalRuntime capabilities through MRO:
- All 29 BaseRuntime configuration parameters
- All mixin methods (cycle execution, validation, conditional execution)
- Enhanced error messages and validation metrics

**Usage**:
```python
from kailash.runtime import AsyncLocalRuntime

# Same configuration as LocalRuntime
runtime = AsyncLocalRuntime(
    debug=True,
    enable_cycles=True,                    # CycleExecutionMixin
    conditional_execution="skip_branches",  # ConditionalExecutionMixin
    connection_validation="strict",         # ValidationMixin
    max_concurrent_nodes=10                 # AsyncLocalRuntime-specific
)
results, run_id = await runtime.execute_workflow_async(workflow.build(), inputs={})

# All inherited methods available
runtime.validate_workflow(workflow)  # ValidationMixin
metrics = runtime.get_validation_metrics()  # LocalRuntime
```

This inheritance ensures 100% feature parity between sync and async runtimes, including identical return structures.

## ⚠️ Critical Rules
- ALWAYS: `runtime.execute(workflow.build())`
- NEVER: `workflow.execute(runtime)`
- String-based nodes: `workflow.add_node("NodeName", "id", {})`
- Real infrastructure: NO MOCKING in Tiers 2-3 tests
- **Return Structure**: Both LocalRuntime and AsyncLocalRuntime return `(results, run_id)` - identical structure
- **Docker/FastAPI**: Use `AsyncLocalRuntime()` or `WorkflowAPI()` (defaults to async)
- **CLI/Scripts**: Use `LocalRuntime()` for synchronous execution

## 📚 Framework-Specific Guides

For detailed framework documentation, see:

| Framework | Quick Reference | Full Documentation |
|-----------|-----------------|-------------------|
| **DataFlow** | `sdk-users/apps/dataflow/GEMINI.md` | Database operations, critical gotchas, Docker deployment |
| **Kaizen** | `sdk-users/apps/kaizen/GEMINI.md` | AI agents, signatures, multi-modal, v1.0 features |
| **Nexus** | `sdk-users/apps/nexus/GEMINI.md` | Multi-channel deployment (API/CLI/MCP) |
| **Core SDK** | `.agent/skills/01-core-sdk/` | WorkflowBuilder, nodes, runtime patterns |

**Key DataFlow Gotchas** (see full guide for details):
1. NEVER manually set `created_at`/`updated_at` (auto-managed)
2. CreateNode uses FLAT params; UpdateNode uses `filter` + `fields`
3. Primary key MUST be named `id`
4. `soft_delete` only affects DELETE, NOT queries
5. Use `$null`/`$exists` operators for NULL checking

---

## 🏆 Success Factors
- **Lessons Learned** 🎓
  1. Documentation Early: Writing guides after implementation is easier
  2. Pattern Consistency: Following same structure across examples reduces errors
  3. Incremental Validation: Verifying tests pass immediately prevents compounding issues
  4. Comprehensive Coverage: Detailed documentation prevents future questions
  5. Design System as Foundation: Create design system BEFORE features to enforce consistency
  6. Mandatory Guides: Institutionalizing design patterns as "must follow" directives prevents drift
  7. Single Import Pattern: Consolidating all design system exports into one file (design_system.dart) simplifies usage
  8. Component Showcase: Building live demo app while developing components catches UX issues early
  9. Deprecation Fixes: Address all deprecations immediately to prevent tech debt accumulation
  10. Real Device Testing: Testing on actual trackpad/touch reveals issues that simulators miss
  11. Pointer Events for Touch: Low-level pointer events handle trackpad/touch better than high-level gestures
  12. Responsive Testing: Test at all three breakpoints (mobile/tablet/desktop) for every feature
