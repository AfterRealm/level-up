# Level Up — Agent Management Skill

Inspect, promote, merge, and analyze usage of your Claude Code agents across projects and global scope.

## What It Does

- **Scan** every `.claude/agents/` directory on your machine — global + all Desktop projects — and list them grouped by project
- **Inspect** any agent's full source
- **Promote & Adapt** a project agent to global (direct copy, or rewrite to strip project-specific references)
- **Merge** two agents into one, with section-by-section combination
- **Stats** — analyze real token costs per agent from your session history, including turn counts, work vs cache breakdown, and your current weekly budget utilization
- **Optimize** — after stats, get AI-generated audit suggestions for any agent to reduce turns, cache growth, or bloat

## How It Works

Level Up is built around a **minimal-interaction** philosophy: one menu per invocation, plain chat for follow-ups. Every picker is a full context re-read, so the skill keeps them to a minimum.

Heavy work (scanning, inspecting, merging, stats) runs in subagents to save your main session's context window. User interaction stays in the main thread.

### The Stats Feature

`agent_stats.py` scans your Claude Code session JSONL files — parent sessions for agent identity, subagent files for real token costs — and gives you the full picture:

- **Work tokens** — what the agent actually input/output
- **Cache tokens** — the context-loading overhead that counts against your weekly usage cap
- **Turns** — how many tool-call round trips an agent makes per invocation (the main cost driver)
- **Weekly budget** — your current 7-day utilization from Anthropic's usage API

This makes it trivial to see *why* an agent is expensive, not just *that* it is. The optimization audit feature then reads the agent's definition and suggests specific edits — batched reads, scope caps, turn reduction — to bring the cost down.

## Install

```bash
claude plugin add afterrealm/level-up
```

Or manually copy to `~/.claude/skills/level-up/`.

## Usage

Just say:
- "level up" or "manage agents"
- "list my agents" or "show my agents"
- "inspect [agent name]"
- "promote [agent] to global"
- "merge agents"
- "agent stats" or "agent usage"
- "which agents are unused"

## Requirements

- Python 3.x (for the scanner scripts)
- Claude Code CLI

## Notes

Per-agent token costs are approximate — matched by timestamp proximity between parent invocations and subagent files (no direct link exists in the JSONL format). Totals and invocation counts are accurate.

Agent creation is handled natively by Claude — no skill needed. Just describe what you want.

## Author

Built by [AfterRealm](https://github.com/AfterRealm)
