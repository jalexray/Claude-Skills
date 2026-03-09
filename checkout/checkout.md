---
name: checkout
description: Document session progress and create detailed logs for context preservation. Use when ending a development session or when you want to checkpoint your work.
tools: Read, Write, Edit, Glob, Bash, AskUserQuestion
---

# End of Session Checkout

This skill documents all progress made during a development session, creating a detailed log that allows work to be resumed if context is lost.

## Usage

Invoke with: `/checkout` or `/checkout <sprint-name>`

If no sprint name is provided, defaults to the most recently modified sprint in `docs/sprints/`.

---

## Workflow

### Step 1: Determine Log File Name

1. **Find the sprint name**: Look for the most recent sprint plan in `docs/sprints/` (e.g., `v1-POC.md` → sprint name is `v1-POC`)
2. **Find existing logs**: Search for `docs/sprints/<sprint-name>-logs-*.md`
3. **Increment log number**: If `v1-POC-logs-01.md` exists, create `v1-POC-logs-02.md`
4. **If no logs exist**: Create `<sprint-name>-logs-01.md`

### Step 2: Gather Session Information

Collect the following information by:
- Reviewing the conversation history
- Checking git status for changed files
- Querying the database for current state
- Reviewing any task lists

**Information to gather:**

1. **Session metadata**
   - Date
   - Approximate duration
   - Phases/milestones completed

2. **Current system state**
   - What's in the database (record counts)
   - What processes are running or completed
   - What's been deployed/configured

3. **Progress made**
   - Features implemented
   - Files created/modified
   - Tests written/passing
   - Dependencies added

4. **Scope changes**
   - Original plan vs what was actually done
   - Features deferred or removed
   - New requirements discovered

5. **Design decisions**
   - Technical choices made and rationale
   - Trade-offs considered
   - Alternatives rejected and why

6. **Discoveries**
   - API behaviors different from docs
   - Bugs or limitations found
   - Performance characteristics
   - Integration gotchas

7. **Known issues**
   - What's currently broken or incomplete
   - Workarounds in place
   - Technical debt incurred

### Step 3: Write the Log File

Create the log file with this structure:

```markdown
# <Sprint Name> Development Log - Session XX

**Date**: YYYY-MM-DD
**Duration**: ~X hours
**Phases Completed**: X-Y (Description)

---

## Current State & How to Resume

### System State (as of session end)
- Database status and record counts
- Processes running or completed
- Configuration state

### To Verify State
```bash
# Commands to check current state
```

### To Resume Development
Step-by-step instructions for picking up where we left off.

### Environment Configuration
All env vars and their current values/defaults.

### Checklist Status
| Phase | Status | Notes |
|-------|--------|-------|
| ... | ✅/⏳/❌ | ... |

---

## Executive Summary

2-3 paragraph summary of what was accomplished and key decisions.

---

## Detailed Progress

### [Phase/Feature Name]

#### Completed
- Bullet points of what was done

#### Technical Decisions
**[Decision Name]**
- Context: Why this decision was needed
- Options considered: What alternatives existed
- Decision: What we chose
- Rationale: Why we chose it

#### Discoveries
- Things learned that weren't in the plan

---

## Scope Changes

### [Change Name]
**Original**: What the plan said
**Changed to**: What we actually did
**Rationale**: Why we changed it
**Impact**: What this affects going forward

---

## Files Created/Modified

### New Files
```
path/to/files/
├── file1.py    # Purpose
└── file2.py    # Purpose
```

### Modified Files
- `path/to/file.py` - What changed

---

## Dependencies Added

| Package | Version | Purpose |
|---------|---------|---------|
| ... | ... | ... |

---

## Known Issues & Technical Debt

### [Issue Name]
- **Status**: Open/Workaround/Deferred
- **Description**: What's wrong
- **Impact**: What it affects
- **Workaround**: If any

---

## Lessons Learned

1. **[Lesson]** - Explanation

---

## Next Steps

### Immediate (Next Session)
1. First thing to do
2. Second thing to do

### Upcoming Phases
- Phase X: Description
- Phase Y: Description

---

## Quick Reference

### Key Commands
```bash
# Commands frequently used
```

### Key Files
- `path/to/important/file.py` - What it does

### Key Endpoints
| Endpoint | Method | Purpose |
|----------|--------|---------|
| ... | ... | ... |
```

### Step 4: Verify Completeness

Before finishing, verify the log includes:

- [ ] Enough context for an engineering manager to understand all decisions
- [ ] Verification commands to check current system state
- [ ] Clear resume instructions for next session
- [ ] All scope changes documented with rationale
- [ ] All technical decisions documented with alternatives considered
- [ ] Known issues and workarounds listed
- [ ] Next steps clearly defined

### Step 5: Confirm with User

After writing the log, ask the user:
1. "Is there anything else from this session that should be documented?"
2. "Should I commit this log file to git?"

---

## Example Invocations

```
/checkout
```
Creates log for the most recent sprint (auto-detected).

```
/checkout v1-POC
```
Creates log specifically for the v1-POC sprint.

---

## Notes

- Be thorough - it's better to over-document than under-document
- Include actual commands and code snippets, not just descriptions
- Focus on the "why" behind decisions, not just the "what"
- Make resume instructions specific enough that someone unfamiliar could follow them
- Include database queries to verify state, not just descriptions of state
