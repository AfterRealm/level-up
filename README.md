# Level Up — An Agent Promotion Skill

Manage, inspect, create, and promote your Claude Code agents across projects and global scope.

## What It Does

- **List** all your agents across every project and global scope
- **Inspect** any agent's full source
- **Promote** a project agent to global (direct copy or template & adapt)
- **Create** new agents with guided setup
- **Demote** a global agent back to a specific project

## How It Works

Level Up scans your `~/.claude/agents/` (global) and all Desktop project `.claude/agents/` directories to find every agent you have. It presents them through paginated menus so you can browse, inspect, and manage them without leaving your session.

Heavy operations (scanning, inspecting, adapting) run in subagents to save your main session's context window.

### Template & Adapt

The standout feature: take any project-specific agent and intelligently adapt it for global use. Level Up reads the source, strips project-specific references (hardcoded paths, project names, scoped tools), rewrites it as a generic version, and shows you the draft for approval before saving.

## Install

```bash
claude plugin add afterrealm/level-up
```

Or manually copy to `~/.claude/skills/level-up/`.

## Usage

Just say:
- "level up"
- "list my agents"
- "show my agents"
- "promote an agent to global"
- "create a new agent"
- "manage agents"

## Requirements

- Python 3.x (for the scanner script)
- Claude Code CLI

## Author

Built by [AfterRealm](https://github.com/AfterRealm)
