---
agent: 'agent'
description: 'Generate comprehensive requirements for a specification'
tools: ['search/codebase', 'edit/editFiles']
---
<meta>
description: Generate comprehensive requirements for a specification
argument-hint: <feature-name:$1>
</meta>

# Requirements Generation

## Parse Arguments
- Feature name: `$1`

## Validate
Check that spec has been initialized:
- Verify `{{KIRO_DIR}}/specs/$1/` exists
- Verify `{{KIRO_DIR}}/specs/$1/spec.json` exists

If validation fails, inform user to run `/kiro-spec-init` first.

## Invoke Agent

Delegate requirements generation to the spec-requirements agent.
Pass the following context to the agent at #file:.github/agents/kiro/spec-requirements.prompt.md:

Feature: $1
Spec directory: {{KIRO_DIR}}/specs/$1/

File patterns to read:
- {{KIRO_DIR}}/specs/$1/spec.json
- {{KIRO_DIR}}/specs/$1/requirements.md
- {{KIRO_DIR}}/steering/*.md
- {{KIRO_DIR}}/settings/rules/ears-format.md
- {{KIRO_DIR}}/settings/templates/specs/requirements.md

Mode: generate

## Display Result

Show agent summary to user, then provide next step guidance:

### Next Phase: Design Generation

**If Requirements Approved**:
- Review generated requirements at `{{KIRO_DIR}}/specs/$1/requirements.md`
- **Optional Gap Analysis** (for existing codebases):
  - Run `/kiro-validate-gap $1` to analyze implementation gap with current code
  - Identifies existing components, integration points, and implementation strategy
  - Recommended for brownfield projects; skip for greenfield
- Then `/kiro-spec-design $1 [-y]` to proceed to design phase

**If Modifications Needed**:
- Provide feedback and re-run `/kiro-spec-requirements $1`

**Note**: Approval is mandatory before proceeding to design phase.
