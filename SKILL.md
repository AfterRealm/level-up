---
name: level-up
description: >-
  Use this skill when the user wants to manage, inspect, create, promote, or organize their Claude Code agents.
  Triggers: "list my agents", "show agents", "create an agent", "promote agent to global",
  "copy agent", "what agents do I have", "agent manager", "level up", "manage agents",
  "move agent to global", "make this agent available everywhere".
  Do NOT trigger for: running an existing agent (/agents), asking about agent capabilities,
  general questions about what agents are.
---

# Level Up — An Agent Promotion Skill

Manage, inspect, create, and promote your Claude Code agents across projects and global scope.

## Architecture

This skill uses a **split approach** to save context:

- **Heavy work** (scanning, inspecting, adapting) runs in a **subagent** so the large JSON payloads and file contents stay out of the main session context
- **User interaction** (menus, approval) stays in the **main thread** since AskUserQuestion only works there
- The subagent returns a compact summary; the main thread presents it and handles navigation

## Flow

### Step 1: Scan (subagent)

Spawn an Agent to run the scanner and return a compact summary:

```
Agent(description: "Scan agents", prompt: "Run: python \"SKILL_DIR/scripts/scan_agents.py\" --action list
Parse the JSON. Return ONLY a compact summary in this format:
GLOBAL: agent1, agent2
PROJECT_NAME (count): agent1, agent2, agent3
...one line per project, agents comma-separated. Include model in parens if set. Nothing else.")
```

Present the summary to the user.

### Step 2: Action Menu

AskUserQuestion with 4 options:

1. **Inspect** — View the full source of an agent
2. **Promote & Adapt** — Copy or adapt an agent to a different scope
3. **Create** — Build a new agent from scratch
4. **Demote** — Move a global agent back to a project

### Agent Picker (used by Inspect, Promote, Demote)

Two-step paginated picker using AskUserQuestion (max 4 options per question):

**Step A — Pick the project.** Use the summary from Step 1. Show up to 3 projects per page. If more than 3, the 4th option is "More projects..." for the next page.

**Step B — Pick the agent.** List that project's agents from the summary. Show up to 3 per page with brief descriptions. If more than 3, the 4th option is "More agents..." to paginate.

### If Inspect (subagent)

After agent picker, spawn a subagent:

```
Agent(description: "Inspect agent", prompt: "Run: python \"SKILL_DIR/scripts/scan_agents.py\" --action inspect --name \"AGENT_NAME\"
Show the full agent markdown content. Format it readably.")
```

### If Promote & Adapt

After agent picker, AskUserQuestion:

1. **Direct copy** — Promote as-is
2. **Template & adapt** — Rewrite for the new scope

**Direct copy** — spawn subagent:
```
Agent(description: "Promote agent", prompt: "Run: python \"SKILL_DIR/scripts/scan_agents.py\" --action promote --name \"AGENT_NAME\" --project \"PROJECT\"
Report the result.")
```

**Template & adapt** — spawn subagent:
```
Agent(description: "Adapt agent", prompt: "Run: python \"SKILL_DIR/scripts/scan_agents.py\" --action inspect --name \"AGENT_NAME\"
Read the full agent source. Identify project-specific references (hardcoded paths, project names, project-specific tools, data references). Rewrite the agent to be generic — replace specific paths with general instructions, broaden scope, keep core role and personality. Return ONLY the adapted markdown content, nothing else.")
```

Show the adapted version to the user. AskUserQuestion: Save it / Edit first. If approved, write to `~/.claude/agents/AGENT_NAME.md`.

### If Create

Walk the user through building a new agent (main thread — needs user input at each step):

1. Ask what the agent should do (role, personality, tools, restrictions)
2. AskUserQuestion for scope: Global / pick a project
3. Ask for a name
4. Write the `.md` file with standard agent frontmatter
5. Save to the appropriate `.claude/agents/` directory

### If Demote

Agent picker filtered to Global agents only (skip Step A). Then project picker for destination. Spawn subagent:
```
Agent(description: "Demote agent", prompt: "Run: python \"SKILL_DIR/scripts/scan_agents.py\" --action demote --name \"AGENT_NAME\" --project \"PROJECT\"
Report the result.")
```

## Important Notes

- Global agents: `~/.claude/agents/` — available in every project
- Project agents: `<project>/.claude/agents/` — only work in that project
- Agent files are `.md` with YAML frontmatter
- Never delete an agent without explicit user confirmation
- Always check for name collisions before promoting
- AskUserQuestion max 4 options — paginate with "More..." as 4th when needed
- Heavy operations go to subagents to preserve main session context
