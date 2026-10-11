**Level Up — Agent Management Skill**

Ever lose track of what agents you have scattered across projects? Worse — ever wonder which ones are quietly burning your weekly token budget? Same. So I built something for it.

**Level Up** scans every `.claude/agents/` directory across your machine and gives you a full inventory. Then lets you manage them without leaving your session — one menu, no picker chains.

**What it does:**
- **List** every agent you have, grouped by project
- **Inspect** any agent's full source
- **Promote & Adapt** — direct copy OR rewrite it to strip project-specific references so it works everywhere
- **Merge** two agents into one
- **Stats** — real token costs per agent from your session history, with turn counts and your current weekly budget
- **Optimize** — AI-audited suggestions to bring down an expensive agent's cost

**The standout:** the Stats + Optimize combo. You see exactly which agents are burning your usage cap (and why — is it turns? cache growth? bloat?), then get concrete edits to fix them. I took my own Psychologist agent from 35 turns → 8 turns in one pass.

**One menu, plain chat for everything else.** Every AskUserQuestion is a full context re-read, so this skill uses exactly one. Follow-ups happen in the regular chat.

**Install:**
```
claude plugin marketplace add AfterRealm/marketplace
claude plugin install level-up@afterrealm
```

Marketplace: https://github.com/AfterRealm/marketplace
GitHub: https://github.com/AfterRealm/level-up

4th AfterRealm plugin.
