---
description: Validate project structure and code compliance
---

# Validation Workflow

This workflow runs standard validation checks to ensure project health and compliance with Gold Standards.

1. **Validate Workflow Structure**
   Run the workflow validator to check for common errors in Antigravity workflows.
   ```bash
   node scripts/hooks/validate-workflow.js
   ```

2. **Auto-Format Code**
   Run the auto-formatter to ensure code style consistency.
   ```bash
   node scripts/hooks/auto-format.js
   ```

3. **Validate Bash Commands** (Optional usage)
   Note: This is typically a pre-hook, but can be run manually to check command safety patterns.
   ```bash
   node scripts/hooks/validate-bash-command.js
   ```

## Usage
Run this workflow before committing changes or when you suspect structural issues.
