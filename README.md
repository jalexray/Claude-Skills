# Claude Skills

A collection of reusable skills for [Claude Code](https://claude.ai/claude-code).

## What are Skills?

Skills are custom commands that extend Claude Code's capabilities. They allow you to define reusable prompts and workflows that can be invoked with slash commands (e.g., `/my-skill`).

## Installation

Copy any skill's `.md` file to your Claude Code skills directory:

```bash
# Global (available in all projects)
cp checkin/checkin.md ~/.claude/skills/

# Project-specific
cp checkin/checkin.md .claude/skills/
```

## Available Skills

### Sprint Workflow

| Skill | Description |
|-------|-------------|
| [prep-sprint](prep-sprint/prep-sprint.md) | Review sprint plans for clarity and completeness, generate PRDs, and create technical development checklists |
| [segment-workstreams](segment-workstreams/segment-workstreams.md) | Analyze a prepped sprint and segment it into independent workstreams for parallel execution across git worktrees |
| [launch-workstreams](launch-workstreams/launch-workstreams.md) | Launch Claude sessions in worktrees for parallel workstream execution |

### Session Management

| Skill | Description |
|-------|-------------|
| [checkin](checkin/checkin.md) | Load context from previous session logs to resume work |
| [checkout](checkout/checkout.md) | Document session progress into log files for future sessions |

### Project Setup

| Skill | Description |
|-------|-------------|
| [scaffold](scaffold/scaffold.md) | Set up project documentation structure and CLAUDE.md guidelines |

## Usage

Once installed, invoke a skill by typing its name as a slash command in Claude Code:

```
/skill-name
```

### Example: Parallel Sprint Workflow

```bash
# 1. Prep your sprint
/prep-sprint docs/sprints/myfeature-plan.md

# 2. Segment into parallel workstreams
/segment-workstreams myfeature

# 3. Launch worktrees for each workstream
/launch-workstreams myfeature
```

## Contributing

Contributions welcome! To add a skill:

1. Fork this repository
2. Create a folder with your skill name (e.g., `my-skill/`)
3. Add your skill as `my-skill/my-skill.md`
4. Include frontmatter with `name`, `description`, and `tools`
5. Submit a pull request

## License

MIT
