---
agent: 'agent'
description: 'Generate implementation tasks for a specification'
tools: ['search/codebase', 'edit/editFiles']
---
<meta>
description: Generate implementation tasks for a specification
argument-hint: <feature-name:$1> [-y:$2]
</meta>

# Implementation Tasks Generator

## Parse Arguments
- Feature name: `$1`
- Auto-approve flag: `$2` (optional, "-y")

## Validate
Check that design has been generated:
- Verify `{{KIRO_DIR}}/specs/$1/` exists
- Verify `{{KIRO_DIR}}/specs/$1/design.md` exists

If validation fails, inform user to complete design phase first.

## Invoke Agent

Delegate task generation to the spec-tasks agent.
Pass the following context to the agent at #file:.github/agents/kiro/spec-tasks.prompt.md:

Feature: $1
Spec directory: {{KIRO_DIR}}/specs/$1/
Auto-approve: {true if $2 == "-y", else false}
Sequential: false

File patterns to read:
- {{KIRO_DIR}}/specs/$1/*.{json,md}
- {{KIRO_DIR}}/steering/*.md
- {{KIRO_DIR}}/settings/rules/tasks-generation.md
- {{KIRO_DIR}}/settings/rules/tasks-parallel-analysis.md
- {{KIRO_DIR}}/settings/templates/specs/tasks.md

Mode: {generate or merge based on tasks.md existence}
Language: respect spec.json language for tasks.md output

## Display Result

Show agent summary to user, then provide next step guidance:

### Next Phase: Implementation

**Before Starting Implementation**:
- **IMPORTANT**: Clear conversation history and free up context before running `/kiro-spec-impl`
- This applies when starting first task OR switching between tasks
- Fresh context ensures clean state and proper task focus

**If Tasks Approved**:
- Execute specific task: `/kiro-spec-impl $1 1.1` (recommended: clear context between each task)
- Execute multiple tasks: `/kiro-spec-impl $1 1.1,1.2` (use cautiously, clear context between tasks)
- Without arguments: `/kiro-spec-impl $1` (executes all pending tasks - NOT recommended due to context bloat)

**If Modifications Needed**:
- Provide feedback and re-run `/kiro-spec-tasks $1`
- Existing tasks used as reference (merge mode)

**Note**: The implementation phase will guide you through executing tasks with appropriate context and validation.
