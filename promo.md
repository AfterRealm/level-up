**Level Up v1.0.0 — An Agent Promotion Skill**

Ever lose track of what agents you have scattered across projects? Same. So I built something for it.

**Level Up** scans every `.claude/agents/` directory across your machine — global and all projects — and gives you a full inventory. Then lets you manage them without leaving your session.

**What it does:**
- **List** every agent you have, grouped by project
- **Inspect** any agent's full source
- **Promote** a project agent to global — direct copy OR **Template & Adapt** (rewrites it to strip project-specific references so it works everywhere)
- **Create** new agents with guided setup

**The cool stuff:**
- Paginated menus via AskUserQuestion — browse 30+ agents without scrolling
- Subagent architecture — the heavy scanning happens off your main context
- Template & Adapt is the standout — take a project-specific Housekeeper and Level Up rewrites it as a generic global version

**Install:**
```
claude plugin add afterrealm/level-up
```

GitHub: https://github.com/AfterRealm/level-up

Built and Blunt Cake'd (9/10, Snoop Dogg approved). 4th AfterRealm plugin.
