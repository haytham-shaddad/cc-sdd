---
agent: 'agent'
description: 'Analyze implementation gap between requirements and existing codebase'
tools: ['search/codebase']
---
<meta>
description: Analyze implementation gap between requirements and existing codebase
argument-hint: <feature-name:$1>
</meta>

# Implementation Gap Validation

## Parse Arguments
- Feature name: `$1`

## Validate
Check that requirements have been completed:
- Verify `{{KIRO_DIR}}/specs/$1/` exists
- Verify `{{KIRO_DIR}}/specs/$1/requirements.md` exists

If validation fails, inform user to complete requirements phase first.

## Invoke Agent

Delegate gap analysis to the validate-gap agent.
Pass the following context to the agent at #file:.github/agents/kiro/validate-gap.prompt.md:

Feature: $1
Spec directory: {{KIRO_DIR}}/specs/$1/

File patterns to read:
- {{KIRO_DIR}}/specs/$1/spec.json
- {{KIRO_DIR}}/specs/$1/requirements.md
- {{KIRO_DIR}}/steering/*.md
- {{KIRO_DIR}}/settings/rules/gap-analysis.md

## Display Result

Show agent summary to user, then provide next step guidance:

### Next Phase: Design Generation

**If Gap Analysis Complete**:
- Review gap analysis insights
- Run `/kiro-spec-design $1` to create technical design document
- Or `/kiro-spec-design $1 -y` to auto-approve requirements and proceed directly

**Note**: Gap analysis is optional but recommended for brownfield projects to inform design decisions.
