# Gemini CLI & Antigravity Migration Guide

## Introduction

This guide provides a comprehensive roadmap for migrating from a **Claude Code** setup (specifically the *Kailash Vibe CC Setup*) to a hybrid **Gemini CLI** and **Google Antigravity** environment.

The goal is to replicate the sophisticated agentic workflows, knowledge management, and quality enforcement of the Claude setup using Gemini's native capabilities.

## Terminology & Concept Mapping

| Feature | Claude Code | Gemini CLI / Antigravity |
| :--- | :--- | :--- |
| **Interface** | Terminal (`claude`) | Terminal (`gemini`) + Antigravity UI |
| **Project Context** | `CLAUDE.md` | `GEMINI.md` |
| **Specialists** | Agents (`.claude/agents/*.md`) | Personas / Knowledge Agents (`.gemini/knowledge/`) |
| **Knowledge Base** | Skills (`.claude/skills/`) | Agent Skills (`.agent/skills/` symlinked to `.gemini/skills/`) |
| **Automation** | Hooks (`scripts/hooks/*.js`) | Workflows (`.agent/workflows/`) & MCP Servers |
| **Constraints** | Rules (`.claude/rules/*.md`) | Directives in `GEMINI.md` & Workflows |
| **Shortcuts** | Commands (`/sdk`, `/db`) | Skills (`/skills`) & Default Tools |

## Guide Index

This documentation is split into 5 parts:

### [01 - Overview & Setup](01-overview-and-setup.md)
*   Architecture of the hybrid setup.
*   Installation instructions.
*   Repository structure.

### [02 - Agent Configuration](02-agent-configuration.md)
*   Migrating specialist agents to personas.
*   Contextual persona adoption.
*   Simulating delegation.

### [03 - Skills & Tools](03-skills-and-tools.md)
*   Migrating skills.
*   Using Model Context Protocol (MCP).
*   Skill usage commands.

### [04 - Workflow Automation](04-workflow-automation.md)
*   Replacing hooks with Antigravity workflows.
*   Automating repeatable processes (TDD, Review).
*   Enforcing quality gates.

### [05 - Context & Knowledge Management](05-context-and-knowledge.md)
*   Migrating `CLAUDE.md` to `GEMINI.md`.
*   Embedding rules.
*   Leveraging the learning system.

### [Gap Analysis & Reality Check](GAP_ANALYSIS.md)
*   **Crucial Read**: Understand where "100% Replicability" is approximate.
*   Differences in Automation, Concurrency, and Learning.

