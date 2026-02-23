---
agent: 'agent'
description: 'Validate implementation against requirements, design, and tasks'
tools: ['search/codebase', 'edit/editFiles']
---
<meta>
description: Validate implementation against requirements, design, and tasks
argument-hint: [feature-name:$1] [task-numbers:$2]
</meta>

# Implementation Validation

## Parse Arguments
- Feature name: `$1` (optional)
- Task numbers: `$2` (optional)

## Auto-Detection Logic

**Perform detection before invoking agent**:

**If no arguments** (`$1` empty):
- Parse conversation history for `/kiro-spec-impl <feature> [tasks]` patterns
- OR scan `{{KIRO_DIR}}/specs/*/tasks.md` for `[x]` checkboxes
- Pass detected features and tasks to agent

**If feature only** (`$1` present, `$2` empty):
- Read `{{KIRO_DIR}}/specs/$1/tasks.md` and find all `[x]` checkboxes
- Pass feature and detected tasks to agent

**If both provided** (`$1` and `$2` present):
- Pass directly to agent without detection

## Invoke Agent

Delegate implementation validation to the validate-impl agent.
Pass the following context to the agent at #file:.github/agents/kiro/validate-impl.prompt.md:

Feature: {$1 or auto-detected}
Target tasks: {$2 or auto-detected}
Mode: {auto-detect, feature-all, or explicit}

File patterns to read:
- {{KIRO_DIR}}/specs/{feature}/*.{json,md}
- {{KIRO_DIR}}/steering/*.md

Validation scope: {based on detection results}

## Display Result

Show agent summary to user, then provide next step guidance:

### Next Steps Guidance

**If GO Decision**:
- Implementation validated and ready
- Proceed to deployment or next feature

**If NO-GO Decision**:
- Address critical issues listed
- Re-run `/kiro-spec-impl <feature> [tasks]` for fixes
- Re-validate with `/kiro-validate-impl [feature] [tasks]`

**Note**: Validation is recommended after implementation to ensure spec alignment and quality.
