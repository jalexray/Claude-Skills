---
name: segment-workstreams
description: Analyze a prepped sprint (PRD + checklist) and segment it into independent workstreams for parallel execution across git worktrees. Use after prep-sprint to organize work for parallel Claude sessions.
tools: Read, Write, Glob, AskUserQuestion
---

# Segment Workstreams

This skill analyzes sprint documentation produced by `/prep-sprint` and segments the work into independent workstreams that can be executed in parallel across separate git worktrees.

## Usage

Invoke with: `/segment-workstreams <sprint-name>` or `/segment-workstreams <path-to-PRD-or-checklist>`

The skill will locate the sprint's PRD and checklist files based on the sprint name pattern.

---

## Phase 1: Load Sprint Documentation

### Actions

1. **Locate sprint documents** using Glob:
   - Search for `*<sprint-name>*-PRD.md`
   - Search for `*<sprint-name>*-checklist.md`

2. **Read both documents** completely

3. **If documents not found**: Ask user for correct paths using AskUserQuestion

4. **Extract from PRD**:
   - All user stories with their acceptance criteria
   - Technical design and architecture decisions
   - Dependencies (internal and external)
   - File/component areas mentioned

5. **Extract from checklist**:
   - All phases and their tasks
   - HUMAN vs LLM assignments
   - QA checkpoints

---

## Phase 2: Dependency Analysis

Analyze the work items to identify dependencies and potential conflicts.

### Dependency Types to Identify

| Type | Description | Impact on Parallelization |
|------|-------------|---------------------------|
| **File Conflict** | Multiple tasks modify the same file | Cannot parallelize |
| **Data Dependency** | Task B needs output from Task A | Sequential within workstream |
| **API/Interface** | Tasks share an interface contract | Can parallelize with contract |
| **Infrastructure** | Shared setup (DB, auth, etc.) | Setup workstream runs first |
| **Independent** | No shared resources | Fully parallelizable |

### Actions

1. **Map tasks to affected files/components**:
   - Identify which files each task will create or modify
   - Group tasks that touch the same areas

2. **Identify shared infrastructure**:
   - Database schemas, migrations
   - Authentication/authorization setup
   - Configuration files
   - Shared utilities or helpers

3. **Detect interface boundaries**:
   - API contracts between frontend/backend
   - Service interfaces
   - Event/message contracts

4. **Build dependency graph**:
   - Which tasks must complete before others can start
   - Which tasks are truly independent

---

## Phase 3: Workstream Segmentation

Segment work into parallelizable workstreams based on dependency analysis.

### Segmentation Principles

1. **Minimize cross-workstream dependencies**: Each workstream should be as self-contained as possible

2. **Group by domain/feature**: Keep related functionality together

3. **Separate infrastructure from features**: Setup/foundation work should be its own workstream that completes first

4. **Balance workload**: Distribute tasks roughly evenly across workstreams

5. **Respect file boundaries**: Tasks modifying the same files must be in the same workstream

### Standard Workstream Patterns

Consider these common patterns when segmenting:

| Pattern | When to Use |
|---------|-------------|
| **Infrastructure First** | Sprint has shared setup (DB, auth, config) |
| **Feature Vertical** | Each feature touches different files end-to-end |
| **Layer Horizontal** | Clear API contract allows frontend/backend split |
| **Component Isolated** | Independent UI components or services |

### Actions

1. **Identify infrastructure workstream** (if needed):
   - Tasks that establish foundations others depend on
   - Usually runs first, blocks other workstreams

2. **Group remaining tasks into feature workstreams**:
   - Each workstream should have clear boundaries
   - Aim for 2-5 workstreams (more creates coordination overhead)

3. **For each workstream, define**:
   - Name (used for worktree: `claude --worktree <name>`)
   - Description of what it accomplishes
   - Tasks included (reference checklist items)
   - Files/components it will modify
   - Dependencies on other workstreams
   - Interface contracts with other workstreams

4. **Validate segmentation**:
   - No file conflicts across parallel workstreams
   - Dependencies flow in one direction (no cycles)
   - Each workstream is meaningful (not too granular)

---

## Phase 4: Generate Workstream Documents

Create a workstreams manifest and individual workstream files.

### Workstreams Manifest Structure

Generate `SPRINTNAME-workstreams.md`:

```markdown
# [Sprint Name] - Workstreams

## Overview
Summary of how this sprint was segmented and why.

## Execution Order

```
[infrastructure] ──┬──> [feature-a]
                   ├──> [feature-b]
                   └──> [feature-c]
```

## Workstream Summary

| Workstream | Description | Depends On | Est. Complexity |
|------------|-------------|------------|-----------------|
| infrastructure | Database setup, auth config | None | Medium |
| feature-a | User profile management | infrastructure | Medium |
| feature-b | Notification system | infrastructure | Low |
| feature-c | Reporting dashboard | infrastructure | High |

## Interface Contracts

Define any contracts between workstreams that allow parallel development:

### API: User Profile Endpoint
- **Provider**: feature-a
- **Consumer**: feature-c
- **Contract**: `GET /api/users/:id` returns `{ id, name, email, role }`

(Additional contracts as needed)

## Merge Strategy

Recommended order for merging workstream branches:
1. infrastructure (base)
2. feature-a, feature-b (parallel, no conflicts)
3. feature-c (after feature-a for API dependency)
```

### Individual Workstream Files

For each workstream, generate `SPRINTNAME-workstream-NAME.md`:

```markdown
# [Sprint Name] - Workstream: [Name]

## Quick Start

```bash
claude --worktree [sprint]-[workstream-name]
```

## Overview
What this workstream delivers and its boundaries.

## Dependencies

### Blocked By
- [workstream-name]: [what must be complete]

### Blocks
- [workstream-name]: [what this enables]

## Interface Contracts

### Provides
- [Contract name]: [specification]

### Consumes
- [Contract name]: [specification]

## Scope

### In Scope
- [Component/feature 1]
- [Component/feature 2]

### Out of Scope (handled by other workstreams)
- [Component/feature] → [workstream-name]

## Tasks

From the development checklist, this workstream includes:

### Phase: [Name]
_QA Checkpoint: [verification steps]_

- [ ] **[HUMAN/LLM]** Task description
- [ ] **[HUMAN/LLM]** Task description

(Include all relevant tasks from original checklist)

## Files & Components

This workstream will create or modify:
- `path/to/file.ext` - [purpose]
- `path/to/component/` - [purpose]

## Completion Criteria

This workstream is complete when:
- [ ] All tasks checked off
- [ ] Tests pass for this workstream's scope
- [ ] Interface contracts are implemented and documented
- [ ] No linting/type errors introduced
- [ ] Ready for code review

## Merge Instructions

When complete:
1. Ensure all tests pass
2. Create PR to main branch
3. Reference workstream in PR description
4. Tag dependent workstreams in PR for visibility
```

### Phase 4 Actions

1. **Generate the workstreams manifest** (`SPRINTNAME-workstreams.md`)
2. **Generate individual workstream files** for each workstream
3. **Validate file references** - ensure no file appears in multiple parallel workstreams
4. **Report completion** with summary

---

## Final Report

When segmentation is complete, output:

```
## Workstreams Ready

Sprint: [sprint-name]
Total Workstreams: [N]

### Execution Plan

1. **[workstream-name]** (blocking)
   - [brief description]
   - Start: `claude --worktree [name]`

2. **[workstream-name]** + **[workstream-name]** (parallel)
   - [brief descriptions]
   - Start: `claude --worktree [name]`

### Generated Files

- SPRINTNAME-workstreams.md (manifest)
- SPRINTNAME-workstream-NAME.md (per workstream)

### Quick Start

To begin parallel execution:

# Terminal 1 - Start infrastructure first
claude --worktree [sprint]-infrastructure

# After infrastructure completes, start features in parallel
# Terminal 2
claude --worktree [sprint]-feature-a

# Terminal 3
claude --worktree [sprint]-feature-b
```

---

## Error Handling

- **PRD/checklist not found**: Ask user for correct sprint name or file paths
- **Cannot determine file boundaries**: Ask user to clarify which files each feature will modify
- **Circular dependencies detected**: Report the cycle and ask user how to break it
- **Single task depends on multiple parallel workstreams**: Move it to a later phase or its own workstream
- **Workstreams too imbalanced**: Suggest redistribution and confirm with user

---

## Tips for Best Results

1. **Well-structured checklists help**: The more specific the original checklist tasks, the better the segmentation

2. **Name workstreams for worktrees**: Keep names short, lowercase, hyphenated (e.g., `auth-setup`, `user-profile`, `api-endpoints`)

3. **Interface contracts enable parallelism**: When workstreams need to interact, define contracts early so both can proceed

4. **Review before executing**: The segmentation is a recommendation - adjust based on team knowledge of the codebase

5. **Start infrastructure first**: Don't begin feature workstreams until shared setup is merged to main
