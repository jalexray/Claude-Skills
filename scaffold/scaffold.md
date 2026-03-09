---
name: scaffold
description: Set up project documentation infrastructure including docs folder structure and CLAUDE.md. Use when initializing a new project or adding documentation standards to an existing one.
tools: Read, Write, Glob, Bash, AskUserQuestion
---

# Project Scaffold

This skill sets up documentation infrastructure for a project, including folder structure and a CLAUDE.md file tailored to the project's implementation approach.

## Usage

Invoke with: `/scaffold`

---

## Workflow

### Step 1: Analyze Existing Project

Before creating anything, understand what exists:

1. **Check for existing docs structure**
   ```
   Glob: docs/**/*
   ```

2. **Check for existing CLAUDE.md**
   ```
   Glob: CLAUDE.md
   ```

3. **Explore the codebase** to understand:
   - Primary language(s) and frameworks
   - Project structure and architecture patterns
   - Existing README.md content
   - Package files (package.json, requirements.txt, Cargo.toml, etc.)
   - Existing code style (naming conventions, patterns used)

### Step 2: Create Documentation Structure

Create the following folder structure if it doesn't exist:

```
docs/
├── backlog/
│   └── README.md
├── sprints/
│   └── README.md
└── technical-reference/
    └── README.md
```

#### docs/backlog/README.md

```markdown
# Backlog

This folder contains feature ideas, enhancements, and future work items.

## Structure

- Each item should be a separate markdown file
- Use descriptive filenames: `feature-name.md`
- Include priority, effort estimate, and dependencies where known

## Template

```md
# [Feature Name]

## Summary
Brief description of the feature.

## Motivation
Why this feature is needed.

## Requirements
- Requirement 1
- Requirement 2

## Dependencies
- Any blockers or prerequisites

## Notes
Additional context.
```
```

#### docs/sprints/README.md

```markdown
# Sprints

This folder contains sprint plans, PRDs, checklists, and session logs.

## Naming Conventions

- `SPRINTNAME-plan.md` - Initial sprint plan
- `SPRINTNAME-PRD.md` - Product Requirements Document
- `SPRINTNAME-checklist.md` - Development checklist
- `SPRINTNAME-logs-XX.md` - Session logs (incrementing number)

## Workflow

1. Create a sprint plan (`/prep-sprint` can help structure this)
2. Generate PRD and checklist from the plan
3. Use `/checkout` at end of sessions to create logs
4. Use `/checkin` at start of sessions to resume context
```

#### docs/technical-reference/README.md

```markdown
# Technical Reference

This folder contains technical documentation, architecture decisions, and implementation guides.

## Suggested Contents

- `architecture.md` - System architecture overview
- `api.md` - API documentation
- `data-models.md` - Database schemas and data structures
- `integrations.md` - Third-party service integrations
- `decisions/` - Architecture Decision Records (ADRs)
```

### Step 3: Generate CLAUDE.md

If no CLAUDE.md exists, create one based on the project analysis.

#### Analysis to Perform

1. **Read key files** to understand the project:
   - README.md
   - Package/dependency files
   - Main entry points
   - Config files

2. **Identify patterns**:
   - Code organization (monorepo, src/, lib/, etc.)
   - Testing approach (test locations, frameworks)
   - Build/deploy tooling
   - Linting/formatting standards

3. **Extract conventions**:
   - Naming patterns (camelCase, snake_case, etc.)
   - File organization patterns
   - Import/export patterns
   - Error handling patterns

#### CLAUDE.md Structure

```markdown
# CLAUDE.md

This file provides guidance to Claude Code when working in this repository.

## Project Overview

[Brief description of what this project does, extracted from README or inferred from code]

## Tech Stack

- **Language**: [Primary language]
- **Framework**: [If applicable]
- **Key Dependencies**: [Major libraries]

## Project Structure

```
[Directory tree of key folders with brief descriptions]
```

## Development Workflow

### Running the Project
[Commands to run/build/test]

### Testing
[Test commands and conventions]

## Code Conventions

### Naming
- [Observed naming conventions]

### File Organization
- [How files are organized]

### Patterns
- [Key patterns used in the codebase]

## Documentation

Project documentation lives in `docs/`:
- `docs/backlog/` - Feature ideas and future work
- `docs/sprints/` - Sprint plans, PRDs, and session logs
- `docs/technical-reference/` - Architecture and technical docs

When ending a development session, use `/checkout` to document progress.
When starting a session, use `/checkin` to load context.

## Important Notes

[Any critical information about the codebase - gotchas, sensitive areas, etc.]
```

### Step 4: Confirm with User

After generating, present a summary:

```markdown
## Scaffold Complete

### Created Structure
- docs/backlog/README.md
- docs/sprints/README.md
- docs/technical-reference/README.md
- CLAUDE.md

### CLAUDE.md Summary
[Brief summary of what was captured about the project]

### Next Steps
1. Review CLAUDE.md and refine any inaccuracies
2. Add a sprint plan to docs/sprints/ to get started
3. Use `/prep-sprint` to generate PRD and checklist
```

Ask the user:
1. "Does the CLAUDE.md accurately capture your project's conventions?"
2. "Any additional patterns or guidelines I should add?"

---

## Handling Existing Files

- **If docs/ structure exists**: Skip creating folders that exist, report what was skipped
- **If CLAUDE.md exists**: Read it, report its contents, and ask if user wants to update/extend it
- **If partial structure exists**: Fill in missing pieces only

---

## Notes

- Never overwrite existing files without explicit user approval
- CLAUDE.md should be specific to the project, not generic boilerplate
- Focus on patterns actually observed in the codebase, not assumed best practices
- Include commands that actually work for this project (check package.json scripts, Makefile, etc.)
