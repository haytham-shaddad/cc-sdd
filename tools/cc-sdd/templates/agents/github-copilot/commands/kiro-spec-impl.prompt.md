---
agent: 'agent'
description: 'Execute spec tasks using TDD methodology'
tools: ['search/codebase', 'edit/editFiles']
---
<meta>
description: Execute spec tasks using TDD methodology
argument-hint: <feature-name:$1> [task-numbers:$2]
</meta>

# Implementation Task Executor

## Parse Arguments
- Feature name: `$1`
- Task numbers: `$2` (optional)
  - Format: "1.1" (single task) or "1,2,3" (multiple tasks)
  - If not provided: Execute all pending tasks

## Validate
Check that tasks have been generated:
- Verify `{{KIRO_DIR}}/specs/$1/` exists
- Verify `{{KIRO_DIR}}/specs/$1/tasks.md` exists

If validation fails, inform user to complete tasks generation first.

## Task Selection Logic

**Parse task numbers from `$2`** (perform this before invoking agent):
- If `$2` provided: Parse task numbers (e.g., "1.1", "1,2,3")
- Otherwise: Read `{{KIRO_DIR}}/specs/$1/tasks.md` and find all unchecked tasks (`- [ ]`)

## Invoke Agent

Delegate TDD implementation to the spec-impl agent.
Pass the following context to the agent at #file:.github/agents/kiro/spec-impl.prompt.md:

Feature: $1
Spec directory: {{KIRO_DIR}}/specs/$1/
Target tasks: {parsed task numbers or "all pending"}

File patterns to read:
- {{KIRO_DIR}}/specs/$1/*.{json,md}
- {{KIRO_DIR}}/steering/*.md

TDD Mode: strict (test-first)

## Display Result

Show agent summary to user, then provide next step guidance:

### Task Execution

**Execute specific task(s)**:
- `/kiro-spec-impl $1 1.1` - Single task
- `/kiro-spec-impl $1 1,2,3` - Multiple tasks

**Execute all pending**:
- `/kiro-spec-impl $1` - All unchecked tasks

**Before Starting Implementation**:
- **IMPORTANT**: Clear conversation history and free up context before running `/kiro-spec-impl`
- This applies when starting first task OR switching between tasks
- Fresh context ensures clean state and proper task focus
