# skills-cli

A CLI tool for managing and syncing **agent skills** across AI coding tools. Share skills between your team and keep every tool in sync — Claude Code, Cursor, OpenClaw, Codex, Windsurf, and [40+ others](#supported-tools).

## The Problem

AI coding tools each have their own skills/rules directory. If you write a great skill for Claude Code, it's stuck in `~/.claude/skills`. Your teammate on Cursor can't use it. Your other agent running OpenClaw doesn't know it exists.

## The Solution

`skills` manages a central git-backed skills repo and symlinks skills into every tool's directory automatically.

```
~/.team-skills/skills/       ← central repo (git-backed)
    ├── my-skill/
    │   └── SKILL.md
    └── another-skill/
        ├── SKILL.md
        └── scripts/

~/.claude/skills/my-skill → symlink
~/.cursor/skills/my-skill → symlink
~/.moltbot/skills/my-skill → symlink
```

## Install

```bash
# Download the script
curl -fsSL https://raw.githubusercontent.com/duskylong/skills-cli/main/skills -o ~/.local/bin/skills
chmod +x ~/.local/bin/skills

# Or clone and link
git clone https://github.com/duskylong/skills-cli.git
ln -s "$PWD/skills-cli/skills" ~/.local/bin/skills
```

Make sure `~/.local/bin` is in your `PATH`.

## Quick Start

```bash
# Initialize with a shared git repo
skills init git@github.com:your-org/team-skills.git

# Or start fresh (local only)
skills init

# Pull latest skills and sync to all detected tools
skills pull

# Check what's installed
skills status
```

## Commands

| Command | Description |
|---|---|
| `skills init [<git-url>]` | Initialize skills repo (clone or create new) |
| `skills pull` | Fetch latest from remote + sync to all detected tools |
| `skills push [msg] [--pr]` | Commit & push changes (`--pr` to create a pull request) |
| `skills publish <name> [--from <path>]` | Copy a project skill into the team repo |
| `skills use <name> [...]` | Symlink team skill(s) into the current project |
| `skills list` | Show all skills and their sync status |
| `skills remove <name>` | Remove a skill from the team repo |
| `skills status` | Show repo info and detected tools |
| `skills agent add <dir> [name]` | Register a custom agent's skills directory |
| `skills agent remove <dir>` | Unregister an agent skills directory |
| `skills agent list` | Show registered agent directories |

## Workflow

### Share a skill you wrote

```bash
# You wrote a skill in your project's .claude/skills/debug-helper
skills publish debug-helper

# Push to the team
skills push "Add debug-helper skill"
```

### Use a teammate's skill

```bash
# Pull latest
skills pull

# Or link a specific skill into your project
cd my-project
skills use debug-helper
```

### Custom agent directories

For agents that don't use standard paths:

```bash
skills agent add /home/nvidia/agents/architect/workspace/skills "Architect"
skills pull  # now syncs to this dir too
```

## Supported Tools

The CLI auto-detects installed tools and syncs skills to them:

- **Claude Code** (`~/.claude/skills`)
- **Cursor** (`~/.cursor/skills`)
- **Codex** (`~/.codex/skills`)
- **OpenClaw** (`~/.moltbot/skills`)
- **Gemini CLI** (`~/.gemini/skills`)
- **Windsurf** (`~/.codeium/windsurf/skills`)
- **GitHub Copilot** (`~/.copilot/skills`)
- **Amp** (`~/.config/agents/skills`)
- **Goose** (`~/.config/goose/skills`)
- **Cline** (`~/.cline/skills`)
- **Roo Code** (`~/.roo/skills`)
- **Kilo Code** (`~/.kilocode/skills`)
- And 30+ more — see the `TOOL_REGISTRY` in the script

## Configuration

| Variable | Default | Description |
|---|---|---|
| `SKILLS_HOME` | `~/.team-skills` | Location of the central skills repo |

Config is stored in `$SKILLS_HOME/.skillsrc`.

## Requirements

- Bash 4+
- Git
- `gh` CLI (optional, for `--pr` workflow)

## License

MIT
