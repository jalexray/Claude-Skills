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

## Usage

Once installed, invoke a skill by typing its name as a slash command in Claude Code:

```
/skill-name
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
