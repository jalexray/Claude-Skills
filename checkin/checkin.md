---
name: checkin
description: Load context from the most recent session log to resume development work. Use when starting a new session or returning after a break.
tools: Read, Glob, Bash, AskUserQuestion
---

# Session Check-In

This skill loads context from the most recent development session log, helping you quickly resume work where you left off.

## Usage

Invoke with: `/checkin` or `/checkin <sprint-name>`

If no sprint name is provided, defaults to the most recently modified sprint log in `docs/sprints/`.

---

## Workflow

### Step 1: Find the Latest Session Log

1. **If sprint name provided**: Look for `docs/sprints/<sprint-name>-logs-*.md`
2. **If no sprint name**: Find the most recently modified `*-logs-*.md` file in `docs/sprints/`
3. **If no logs exist**: Report that no session logs were found and suggest using `/checkout` at end of sessions

### Step 2: Load and Parse the Log

Read the session log and extract:

1. **Session metadata**
   - Date of last session
   - Phases/milestones completed
   - Current checklist status

2. **Current system state**
   - Database status
   - Processes that should be running
   - Configuration state

3. **Resume instructions**
   - What to do next
   - Any pending tasks or blockers

4. **Known issues**
   - What's broken or incomplete
   - Workarounds in place

### Step 3: Verify Current State

Run the verification commands from the log to check:
- Database record counts match expectations
- Required processes are running
- Environment is properly configured

### Step 4: Present Context Summary

Output a concise briefing:

```markdown
## Session Check-In Complete

### Last Session
- **Date**: [date]
- **Completed**: [phases/features]
- **Duration**: [time]

### Current State
[Quick status of database, services, etc.]

### Verification Results
[Output of state verification commands]

### Ready to Resume
[Next steps from the log]

### Watch Out For
[Known issues or blockers]
```

### Step 5: Offer Next Actions

Ask the user:
1. "Should I run the verification commands to confirm system state?"
2. "Ready to continue with [next task from log]?"

---

## Example Invocations

```
/checkin
```
Loads the most recent session log from any sprint.

```
/checkin v1-POC
```
Loads the most recent session log for the v1-POC sprint.

---

## Notes

- If verification commands fail, report discrepancies and suggest remediation
- If the log is outdated (>7 days), warn that state may have drifted
- If multiple sprints have recent logs, ask which one to load
- Cross-reference with the sprint checklist to show overall progress
