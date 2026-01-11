---
description: Sync damage-control hooks from dotfiles and port changes to TypeScript
---

# Purpose

Sync the latest damage-control changes from ~/.dotfiles/claude/hooks/damage-control/ into this repo. This includes copying Python files, updating patterns.yaml, and porting any Python logic changes to the TypeScript implementation.

## Source Locations

- Python hooks: `~/.dotfiles/claude/hooks/damage-control/`
- patterns.yaml: `~/.dotfiles/claude/skills/damage-control/patterns.yaml`
- Tests: `~/.dotfiles/claude/hooks/damage-control/tests/`

## Target Locations

- Python hooks: `plugins/python/hooks/`
- TypeScript hooks: `plugins/typescript/hooks/`
- Shared patterns: `shared/patterns.yaml`

## Workflow

### Step 1: Show Differences

Diff each Python file to understand what changed:

```bash
diff ~/.dotfiles/claude/hooks/damage-control/bash-tool-damage-control.py plugins/python/hooks/bash-tool-damage-control.py
diff ~/.dotfiles/claude/hooks/damage-control/edit-tool-damage-control.py plugins/python/hooks/edit-tool-damage-control.py
diff ~/.dotfiles/claude/hooks/damage-control/write-tool-damage-control.py plugins/python/hooks/write-tool-damage-control.py
diff ~/.dotfiles/claude/hooks/damage-control/test-damage-control.py plugins/python/hooks/test-damage-control.py
diff ~/.dotfiles/claude/skills/damage-control/patterns.yaml plugins/python/hooks/patterns.yaml
```

### Step 2: Copy Python Files

```bash
cp ~/.dotfiles/claude/hooks/damage-control/bash-tool-damage-control.py plugins/python/hooks/
cp ~/.dotfiles/claude/hooks/damage-control/edit-tool-damage-control.py plugins/python/hooks/
cp ~/.dotfiles/claude/hooks/damage-control/write-tool-damage-control.py plugins/python/hooks/
cp ~/.dotfiles/claude/hooks/damage-control/test-damage-control.py plugins/python/hooks/
cp ~/.dotfiles/claude/hooks/damage-control/log_rotate.py plugins/python/hooks/
cp ~/.dotfiles/claude/hooks/damage-control/benchmark.py plugins/python/hooks/
```

### Step 3: Copy Test Files

```bash
cp ~/.dotfiles/claude/hooks/damage-control/tests/*.py plugins/python/hooks/tests/
cp ~/.dotfiles/claude/hooks/damage-control/tests/*.yaml plugins/python/hooks/tests/
```

### Step 4: Copy patterns.yaml to All Locations

```bash
cp ~/.dotfiles/claude/skills/damage-control/patterns.yaml plugins/python/hooks/patterns.yaml
cp ~/.dotfiles/claude/skills/damage-control/patterns.yaml plugins/typescript/hooks/patterns.yaml
cp ~/.dotfiles/claude/skills/damage-control/patterns.yaml shared/patterns.yaml
```

### Step 5: Port Python Changes to TypeScript

Review the diffs from Step 1. For any **logic changes** (not just patterns.yaml), update the corresponding TypeScript files:

- `plugins/typescript/hooks/bash-tool-damage-control.ts`
- `plugins/typescript/hooks/edit-tool-damage-control.ts`
- `plugins/typescript/hooks/write-tool-damage-control.ts`

Common changes to look for:
- Return value changes (blocked vs ask)
- New regex patterns or matching logic
- Path matching improvements
- New helper functions

### Step 6: Run Tests

```bash
cd plugins/python/hooks && uv run --with pytest --with pyyaml pytest tests/ -v --tb=short
```

### Step 7: Fix Test Failures

If tests fail, update the test files or fixtures to match the new patterns/behavior.

## Report

After completing the sync, summarize:

1. **Files synced** - List files copied with line count changes
2. **Python changes** - Brief description of logic changes (not just patterns)
3. **TypeScript updates** - What was ported to TypeScript
4. **Test results** - Pass/fail count
5. **Action items** - Any remaining manual work needed
