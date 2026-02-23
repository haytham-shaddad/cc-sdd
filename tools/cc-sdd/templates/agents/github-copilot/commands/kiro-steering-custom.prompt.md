---
agent: 'agent'
description: 'Create custom steering documents for specialized project contexts'
tools: ['search/codebase', 'edit/editFiles']
---
<meta>
description: Create custom steering documents for specialized project contexts
</meta>

# Kiro Custom Steering Creation

## Interactive Workflow

This command starts an interactive process with the agent:
1. Agent asks user for domain/topic
2. Agent checks for available templates
3. Agent analyzes codebase for relevant patterns
4. Agent generates custom steering file

## Invoke Agent

Delegate custom steering creation to the steering-custom agent.
Pass the following context to the agent at #file:.github/agents/kiro/steering-custom.prompt.md:

Interactive Mode: Ask user for domain/topic

File patterns to read:
- {{KIRO_DIR}}/settings/templates/steering-custom/*.md
- {{KIRO_DIR}}/settings/rules/steering-principles.md

JIT Strategy: Analyze codebase for relevant patterns as needed

## Display Result

Show agent summary to user:
- Custom steering file created
- Template used (if any)
- Codebase patterns analyzed
- Content overview

## Available Templates

Available templates in `{{KIRO_DIR}}/settings/templates/steering-custom/`:
- api-standards.md, testing.md, security.md, database.md
- error-handling.md, authentication.md, deployment.md

## Notes

- Agent will interact with user to understand needs
- Templates are starting points, customized for project
- All steering files loaded as project memory
- Avoid documenting agent-specific tooling directories (e.g. `.cursor/`, `.gemini/`, `.claude/`, `.github/`)
- `{{KIRO_DIR}}/settings/` content should NOT be documented (it's metadata, not project knowledge)
- Light references to `{{KIRO_DIR}}/specs/` and `{{KIRO_DIR}}/steering/` are acceptable; avoid other `.kiro/` directories
