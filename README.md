# Personal OS

A starter kit for building your own AI-powered operating system with [Claude Code](https://claude.ai/code).

I'm [Darrell Stone](https://www.linkedin.com/in/darrellstone/), VP of Product at [Fi](https://tryfi.com). I built a personal OS repo (darrell-os) that runs my weekly cadence — 1:1 prep, meeting recaps, weekly updates, Slack triage — all through Claude Code slash commands that know my voice, my team, and my tools. This is the genericized version of that system. Clone it, fill in your context, and start using it.

## The Idea

The best use of AI isn't asking it questions. It's giving it enough context about you — how you write, who you manage, what tools you use — that it can do real work on your behalf. Meeting recaps that sound like you. 1:1 prep that pulls from Slack and your team files. Weekly updates that write themselves.

This repo is the architecture for that. No code, just markdown. The value isn't the content — it's the structure.

## What's Inside

```
personal-os-starter/
├── CLAUDE.md                    # Master instruction file — Claude reads this first
├── .claude/commands/            # Slash command stubs (4 starter commands)
├── workflows/                   # Skill runbooks + quality gates
├── voice/                       # How you write (formal + casual templates)
├── team/                        # Direct report dossiers + self-profile
├── meeting-notes/               # Rolling decisions + action items
└── weekly-updates/              # Archived weekly updates
```

## Starter Skills

| Command | What It Does |
|---------|-------------|
| `/1-1 [name]` | Prep for 1:1s (pulls Slack DMs, meetings, team file, projects) — then auto-updates team file and drafts DM recap after |
| `/meeting_summary` | Recap any meeting and share via Slack. Supports search, ordinals ("2nd last"), DM or channel destination |
| `/weekly-update` | Generate a team-wide progress summary from Slack channels and your project tracker |
| `/slack-backlog` | Find unanswered @mentions and DMs, draft responses in your voice, send one-by-one with approval |

Each skill uses a **two-layer pattern**: a lightweight command stub (`.claude/commands/`) points to a full runbook (`workflows/`) with step-by-step instructions, tool calls, output format, and fallbacks. Add new skills in 10 minutes.

## Key Concepts

**CLAUDE.md** — The master instruction file. Claude reads this at the start of every conversation. It contains your working style, tool references, Slack channel IDs, and skill registry. Think of it as your config file.

**Voice guides** — Templates that teach Claude how you actually write. Casual voice for DMs, formal voice for docs. You populate them by reviewing 20-30 of your own messages and extracting patterns (there's a guided exercise in the repo).

**Team dossiers** — Markdown files for each person you manage. Growth areas, working style, 1:1 notes. The `/1-1` skill reads and writes to these automatically.

**Quality gates** — A 7-point checklist (completeness, length, tone, sensitivity, etc.) applied before any summary is shown to you. Universal across all skills.

**MCP servers** — Plugins that connect Claude to your tools (Slack, Linear, Notion, etc.). The skills reference these for reading messages, posting updates, and querying project status.

## Getting Started

```bash
# Clone and rename
git clone https://github.com/wenttomarket/personal-os-starter.git my-os
cd my-os

# Install Claude Code if you haven't
npm install -g @anthropic-ai/claude-code

# Launch
claude
```

Then work through the Day 1 checklist:

1. Fill in `CLAUDE.md` — name, role, company, 3 operating principles
2. Create your self-profile from the template in `team/stakeholders/`
3. Add your Slack channel IDs to the quick-reference tables
4. Create 1 direct report file from the template
5. Start populating your voice guide (the 30-minute exercise)
6. Test `/meeting_summary`

Every template file includes `<!-- CUSTOMIZE: ... -->` comments explaining what to fill in.

## Adding Skills

The architecture is designed to grow. To add a new skill:

1. Create the runbook: `workflows/[name]-skill.md`
2. Create the stub: `.claude/commands/[name].md`
3. Register in the Common Tasks section of `CLAUDE.md`

There's a full walkthrough with a `/standup` example in the repo.

## Prerequisites

- [Claude Code](https://claude.ai/code) (Anthropic account required)
- Git
- Node.js 18+
- A terminal

## Tool-Agnostic

The skills work with whatever tools you use. Slack MCP is the only assumed integration. Everything else — project tracker, meeting recorder, docs platform — uses placeholder references you swap in for your stack (Linear, Jira, Asana, Notion, Granola, Otter, etc.).

## FAQ

**Do I need to know how to code?** No. It's all markdown.

**Can my team use this?** Each person clones their own copy and fills in their own context. Same architecture, personal content.

**What if I don't have a meeting recorder?** The skills fall back to asking you to paste notes. Add a meeting MCP later if you want.

---

Built with [Claude Code](https://claude.ai/code).
