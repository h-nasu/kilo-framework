# Kilo Framework

A markdown-based framework for defining how AI assistants execute software development tasks. Kilo provides a structured instruction model that guides AI behavior through layered configuration, specialized agents, workflows, and skills.

## Overview

Kilo Framework treats AI execution as a configurable process. Instead of relying on implicit behavior, every decision—rules, roles, workflows, and procedures—is explicitly defined in version-controlled markdown files. This makes AI-assisted development predictable, auditable, and consistent across projects.

The framework operates in three modes:
- **In-process**: Instructions are injected directly into the AI's context
- **CLI**: Instructions are applied via the Kilo Code extension
- **VS Code extension**: Instructions are managed through Agent Manager

## Core Concepts

### Instruction Layers

Kilo applies instructions in a strict, ordered hierarchy. Later layers override earlier ones.

| Layer | Source | Purpose |
|-------|--------|---------|
| Global instructions | `.kilo/instructions/global.md` | Framework-wide defaults |
| Project instructions | `.kilo/instructions/project.md` | Project-specific overrides |
| Global rules | `.kilo/rules/*.md` | Coding, testing, git, documentation standards |
| Project rules | `.kilo/rules/project.md` | Project-specific rule overrides |
| Skills | `.kilo/skills/*/SKILL.md` | Reusable procedures for common tasks |
| Workflows | `.kilo/commands/*.md` | Structured multi-step processes |
| Agents | `.kilo/agents/*.md` | Specialized AI role definitions |
| Subagents | `.kilo/agents/subagents/*.md` | Hidden supporting agents |

### Document Model

Every task follows an 8-phase document structure. Documents are stored under `.kilo/documents/` with indexed prefixes to enforce execution order:

1. **01-plans/** — Problem understanding, assumptions, constraints, proposed approach
2. **02-specifications/** — UI, APIs, behavior, and constraints
3. **03-designs/** — Architecture, file structure, class design, technology stack
4. **04-tasks/** — Executable task breakdown with acceptance criteria
5. **05-developments/** — Execution results and completed work summaries
6. **06-tests/** — Test plans, results, failures, and fixes
7. **07-changes/** — Post-planning changes and rationale
8. **08-issues/** — Bugs, root causes, countermeasures, lessons learned

### Agents

Agents are specialized AI roles with defined responsibilities, processes, and output formats. The framework includes built-in agents:

| Agent | Role | Color |
|-------|------|-------|
| ai | General-purpose task execution | Purple |
| architect | System design and technology decisions | Blue |
| planner | Task decomposition and sequencing | Amber |
| backend | Server-side implementation | Indigo |
| frontend | Client-side implementation | Cyan |
| debugger | Error diagnosis and resolution | Red |
| qa | Quality assurance and testing | Amber |
| reviewer | Code review and standards enforcement | Red |
| documentation | Client-facing documentation | Green |

### Workflows

Workflows define repeatable, structured processes for common development activities. They map to primary agents and enforce before/after task hooks.

| Workflow | Agent | Purpose |
|----------|-------|---------|
| bug-fix | code | Diagnose, reproduce, fix, and verify bugs |
| feature-development | code | Plan, design, implement, test, and document features |
| hotfix | code | Emergency fixes with minimal scope |
| refactor | planner | Code restructuring without behavior changes |
| release | architect | Versioned release planning and coordination |

### Skills

Skills are reusable, step-by-step procedures injected into the AI context when relevant. They standardize how common tasks are performed across all projects.

| Skill | Purpose |
|-------|---------|
| create-api | Implement new API endpoints |
| create-feature | Implement new features |
| create-tests | Write tests for new or existing functionality |
| debug-error | Diagnose and resolve errors |
| design-schema | Design data schemas and models |
| review-code | Review code changes for quality |
| write-docs | Create or update project documentation |

## Rules

The framework enforces standards through layered rule files:

- **architecture.md** — Component design, data flow, dependency management
- **coding.md** — Code style, naming conventions, quality gates
- **testing.md** — Test-driven development, organization, best practices
- **git.md** — Commit standards, branching, PR guidelines
- **documentation.md** — Documentation requirements, client-facing docs, separation of concerns

Project-specific rules in `.kilo/rules/project.md` override global defaults.

## Configuration

The root `kilo.jsonc` file configures the framework for a project:

```json
{
  "$schema": "https://app.kilo.ai/config.json",
  "instructions": [
    ".kilo/instructions/**/*.md"
  ]
}
```

## Safety Principles

- **Explicit instructions over assumptions**: Every behavior must be documented
- **Indexed overrides**: Override order is unambiguous
- **Documented decisions**: All choices are recorded
- **Halt on low confidence**: Execution stops when confidence drops below acceptable thresholds

## File Structure

```
.kilo/
├── agents/           # Agent role definitions
├── commands/         # Workflow definitions
├── documents/        # Task execution documents (8 phases)
├── instructions/     # Global and project instructions
├── node_modules/     # Framework dependencies
├── rules/            # Global and project rules
├── skills/           # Reusable procedure skills
├── templates/        # Document templates
└── plans/            # Planning artifacts
```

## Getting Started

1. Create a `kilo.jsonc` in your project root pointing to `.kilo/instructions/**/*.md`
2. Define global instructions in `.kilo/instructions/global.md`
3. Add project-specific overrides in `.kilo/instructions/project.md`
4. Configure rules, agents, workflows, and skills in their respective directories
5. Execute tasks through the Kilo Code extension or CLI

## Requirements

- Node.js (for dependency management)
- Kilo Code extension (for in-editor execution)
- Git (for version control of instructions)
