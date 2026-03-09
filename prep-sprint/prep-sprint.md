---
name: prep-sprint
description: Review sprint plans for clarity and completeness, generate PRDs, and create technical development checklists. Use when starting a new sprint, reviewing sprint docs, or preparing for development.
tools: Read, Write, Edit, Glob, AskUserQuestion
---

# Sprint Planning Workflow

This skill guides a sprint from plan through development-ready checklist in three phases. Each phase must pass before proceeding to the next.

## Usage

Invoke with: `/prep-sprint <path-to-sprint-plan>`

The sprint plan filename should follow the pattern `SPRINTNAME-plan.md` or similar. The SPRINTNAME will be extracted and used for generated documents:
- `SPRINTNAME-PRD.md` - Product Requirements Document
- `SPRINTNAME-checklist.md` - Technical Development Checklist

---

## Phase 1: Sprint Plan Review

Read the sprint plan document provided by the user and evaluate it against these criteria:

### Clarity Checklist

- [ ] Goals are specific and measurable
- [ ] Language is unambiguous (no "should," "might," "could consider")
- [ ] Technical terms are defined or commonly understood
- [ ] Scope boundaries are explicit

### Completeness Checklist

- [ ] Use cases are fully described with actors, triggers, and outcomes
- [ ] User stories follow "As a [user], I want [action], so that [benefit]" format
- [ ] Acceptance criteria exist for each deliverable
- [ ] Dependencies are identified
- [ ] Risks and mitigations are noted

### Consistency Checklist

- [ ] Internal logic is sound (no contradicting requirements)
- [ ] Aligns with project README and existing architecture
- [ ] Terminology is used consistently throughout
- [ ] Priorities don't conflict

### Phase 1 Actions

1. **Read the sprint plan** provided as argument
2. **Read the project README** (if exists) to verify alignment
3. **Evaluate** against all checklists above
4. **If issues found**: Use AskUserQuestion to clarify. Help refine the document using Edit.
5. **If passes**: Report "Phase 1 Complete - Sprint plan is clear, complete, and consistent." Proceed to Phase 2.

---

## Phase 2: Product Requirements Document

Generate or review `SPRINTNAME-PRD.md` based on the sprint plan.

### PRD Structure

```markdown
# [Sprint Name] - Product Requirements Document

## Overview
Brief description of what this sprint delivers and why.

## Goals
- Primary goal
- Secondary goals
- Success metrics

## Assumptions
- List all assumptions the team is making
- Technical assumptions
- User behavior assumptions
- Resource assumptions

## User Stories

### Story 1: [Title]
**As a** [user type]
**I want** [action/feature]
**So that** [benefit/outcome]

**Acceptance Criteria:**
- [ ] Criterion 1
- [ ] Criterion 2

(Repeat for all stories)

## Design

### User Flow
Description or diagram of user interactions.

### Technical Design
High-level technical approach, architecture decisions, data models.

### UI/UX Considerations
Wireframe descriptions, component requirements, responsive behavior.

## Out of Scope
Explicit list of what this sprint does NOT include:
- Item 1
- Item 2

## Dependencies
- External services
- Other team deliverables
- Third-party integrations

## Risks & Mitigations
| Risk | Impact | Mitigation |
|------|--------|------------|
| ... | ... | ... |
```

### Phase 2 Actions

1. **Check if PRD exists** using Glob for `*-PRD.md` matching the sprint name
2. **If PRD does not exist**: Generate it from the sprint plan using the structure above. Save as `SPRINTNAME-PRD.md`.
3. **If PRD exists**: Review for completeness and consistency with sprint plan.
4. **If issues found**: Use AskUserQuestion to clarify gaps or conflicts. Refine using Edit.
5. **If passes**: Report "Phase 2 Complete - PRD is thorough and aligned with sprint plan." Proceed to Phase 3.

---

## Phase 3: Technical Development Checklist

Generate or review `SPRINTNAME-checklist.md` for human+LLM pair programming execution.

### Checklist Structure

```markdown
# [Sprint Name] - Development Checklist

## Overview
Quick reference for the development team on what we're building.

---

## Phase 1: [Foundation/Setup]
_QA Checkpoint: [What to verify before proceeding]_

- [ ] **[HUMAN]** Task description
- [ ] **[LLM]** Task description
- [ ] **[HUMAN]** Task description

## Phase 2: [Core Feature]
_QA Checkpoint: [What to verify before proceeding]_

- [ ] **[LLM]** Task description
- [ ] **[HUMAN]** Task description

(Continue phases as needed)

## Phase N: [Integration/Polish]
_QA Checkpoint: [Final verification steps]_

- [ ] **[HUMAN]** Task description
- [ ] **[LLM]** Task description

---

## Assignment Guidelines

**Assign to HUMAN:**
- Decisions requiring business context or user empathy
- External account setup, API key generation, service configuration
- Design review and UX decisions
- Final approval on user-facing copy
- Manual testing requiring subjective judgment
- Security-sensitive configurations
- Stakeholder communication

**Assign to LLM:**
- Boilerplate code generation
- Writing tests based on defined acceptance criteria
- Implementing well-specified functions
- Refactoring with clear requirements
- Documentation generation
- Code review for patterns and bugs
- Data transformation and migrations with clear schemas
```

### Phase 3 Actions

1. **Check if checklist exists** using Glob for `*-checklist.md` matching the sprint name
2. **If checklist does not exist**: Generate it from the PRD. Sequence tasks in logical, incremental phases with QA checkpoints. Assign each task to HUMAN or LLM based on guidelines.
3. **If checklist exists**: Review for:
   - Consistency with sprint plan and PRD
   - Completeness (all requirements covered)
   - Logical sequencing
   - Appropriate HUMAN/LLM assignments
   - QA checkpoints between phases
4. **If conflicts found**: Use AskUserQuestion to resolve discrepancies between documents.
5. **If passes**: Report completion status.

### Final Report

When all three phases pass, output:

```
## Sprint Ready for Development

- Sprint Plan: [filename] - Reviewed and approved
- PRD: [filename] - Complete
- Checklist: [filename] - Ready for execution

The sprint documentation is consistent, complete, and ready for the development team.
```

---

## Error Handling

- If the sprint plan file cannot be found, ask the user for the correct path
- If the sprint name cannot be extracted from the filename, ask the user to provide it
- If README.md is missing, note this but continue (flag as a risk in PRD)
- If any phase reveals fundamental issues with the sprint concept itself, pause and discuss with the user before generating downstream documents
