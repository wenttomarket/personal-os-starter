# Personal OS Starter Kit

A ready-to-use personal operating system for AI-assisted productivity. Clone it, fill in your context, and start using Claude Code as your daily copilot for meetings, Slack, team management, and weekly updates.

This starter kit gives you the architecture — you bring the content.

---

## What You Get

| Feature | What It Does |
|---------|-------------|
| **4 slash commands** | `/1-1`, `/meeting_summary`, `/weekly-update`, `/slack-backlog` — ready to customize |
| **Voice guides** | Teach Claude how you write (formal + casual) so drafts sound like you |
| **Team dossiers** | Templates for direct reports and stakeholders — Claude remembers context across sessions |
| **Quality gates** | 7-point checklist applied to every summary before it's shown to you |
| **Extensible architecture** | Add new skills in 10 minutes with the two-layer pattern |

---

## Prerequisites

Before you start, you'll need:

1. **An Anthropic account** with Claude Code access — [claude.ai/code](https://claude.ai/code)
2. **A terminal** — Terminal.app (Mac), iTerm2, or any terminal emulator
3. **Git** — Check with `git --version` in your terminal. If not installed: `xcode-select --install` (Mac)
4. **Node.js 18+** — Check with `node --version`. Install from [nodejs.org](https://nodejs.org) if needed

---

## Installation

Open your terminal and run these commands one at a time:

```bash
# 1. Navigate to where you keep projects
cd ~/projects

# 2. Clone this repo (replace with your actual repo URL)
git clone [YOUR_REPO_URL] my-os
cd my-os

# 3. Install Claude Code (if you haven't already)
npm install -g @anthropic-ai/claude-code

# 4. Launch Claude Code in this directory
claude

# 5. Claude will automatically read CLAUDE.md and know your setup
```

---

## Day 1 Checklist

Work through these in order. Each one takes 5-15 minutes.

- [ ] **Fill in CLAUDE.md basics** — Replace `[YOUR NAME]`, `[YOUR ROLE]`, `[YOUR COMPANY]` at the top
- [ ] **Add 3 operating principles** — In the Working Style section. What drives your decisions?
- [ ] **Create your self-profile** — Copy `team/stakeholders/TEMPLATE-self-profile.md` to `team/stakeholders/[your-name].md` and fill it in
- [ ] **Add your Slack IDs** — Fill in the Slack Quick Reference tables in CLAUDE.md (channels + DM IDs)
- [ ] **Add 1 direct report** — Copy `team/TEMPLATE-direct-report.md` to `team/[name].md` and fill in the basics
- [ ] **Start your voice guide** — See "Populating Your Voice" below
- [ ] **Test a slash command** — Run `/meeting_summary` to verify the setup works

---

## How Slash Commands Work

Every skill uses a **two-layer pattern**:

```
.claude/commands/[command].md     ← "Stub" — Claude sees this in the command menu
                                     Points to the full runbook

workflows/[skill-name]-skill.md   ← "Runbook" — Full step-by-step process
                                     Tools to call, output format, fallbacks
```

**Why two layers?**
- The stub keeps the command menu clean and fast
- The runbook has the full process details Claude needs to execute
- You can update the runbook without touching the command registration

When you type `/meeting_summary`, Claude reads the stub, follows the pointer to the runbook, and executes the full workflow.

---

## Adding Your First Skill

Let's say you want a `/standup` command that summarizes your team's async standup channel.

### Step 1: Create the runbook

Create `workflows/standup-skill.md`:

```markdown
# Skill: `/standup`

**Quality gates:** Before finalizing output, apply checks from `workflows/quality-gates.md`.

> Summarize today's async standup posts from the team channel.

## Trigger
/standup

## Steps

### 1. Fetch standup posts
Query the team standup channel for today's messages:
mcp__slack__conversations_history(channel_id="[YOUR_STANDUP_CHANNEL_ID]", limit="1d")

### 2. Extract updates
For each person's post, extract:
- What they did yesterday
- What they're doing today
- Any blockers

### 3. Generate summary
Synthesize into a brief team summary, organized by person.
Flag any blockers that need your attention.

## Output Format
## Team Standup: [Date]
[Name]: [today's focus]. [blocker if any]
[Name]: [today's focus].
...
Blockers: [any blockers that need attention]

## Limitations
- Only captures posts in the standup channel
- People who didn't post won't appear
```

### Step 2: Create the command stub

Create `.claude/commands/standup.md`:

```markdown
# Daily Standup Summary

Summarize today's async standup posts from the team channel.

## Usage
/standup

## Process
1. Fetch today's standup channel messages
2. Extract per-person updates and blockers
3. Generate team summary

## Full Documentation
See `workflows/standup-skill.md` for complete process details.
```

### Step 3: Register in CLAUDE.md

Add to the **Common Tasks** section:

```
**Daily standup**: Run `/standup` to summarize async standup posts. See `workflows/standup-skill.md`.
```

And add to `workflows/_index.md`:

```
- `standup-skill.md` - `/standup` command (daily async standup summary)
```

### Step 4: Test it

Restart Claude Code (or start a new conversation) and type `/standup`.

---

## Populating Your Voice

The voice guides (`voice/public.md` and `voice/private.md`) are what make Claude sound like *you* instead of generic AI. Here's how to fill them in:

### The Exercise (30 minutes)

**For your private/casual voice (`voice/private.md`):**

1. Open Slack and scroll through your recent DMs and channel replies
2. Copy 20-30 messages that feel representative of how you write
3. Look for patterns:
   - How do you say yes? ("yep", "sounds good", "done", "+1"?)
   - How do you say no? ("let's hold on that", "nah", "not sure about this"?)
   - How do you ask for things? ("can you...", "would you mind...", "hey quick ask..."?)
   - Do you use periods? Exclamation marks? Emoji?
   - How long are your typical messages?
   - Do you have any recurring abbreviations?
4. Fill in the tables in `voice/private.md` with YOUR patterns

**For your public/formal voice (`voice/public.md`):**

1. Find 10-15 examples: emails, docs, PRDs, presentations, channel announcements
2. Look for patterns:
   - How do you open documents? ("Hey team," / "Context:" / "TL;DR:")
   - How do you structure updates? (bullets, numbered lists, headers?)
   - How do you present data? (insight-first, or data-first?)
   - How do you close? (next steps, open questions, call to action?)
3. Fill in the tables in `voice/public.md`

**Quick test**: After filling in the guides, ask Claude to "draft a Slack message to [person] about [topic]" and see if it sounds like you. Iterate on the voice guide until it does.

---

## Connecting MCP Servers

**What's an MCP?** MCP (Model Context Protocol) lets Claude connect to external tools — Slack, your project tracker, meeting notes, databases. Think of them as plugins that give Claude the ability to read and write to your actual work tools.

### Common MCP Servers

| MCP Server | What It Does | Setup Link |
|-----------|-------------|-----------|
| Slack | Read/send messages, search channels | [modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers/tree/main/src/slack) |
| Linear | Read/create issues, track projects | [modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers/tree/main/src/linear) |
| Notion | Read/write pages, search databases | [modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers/tree/main/src/notion) |
| GitHub | PRs, issues, repo management | [modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers/tree/main/src/github) |

### How to Add an MCP Server

1. Install the MCP server package (usually via npm)
2. Configure it in your Claude Code settings (`~/.claude/settings.json` or project-level `.claude/settings.json`)
3. Add validation calls to the MCP Pre-Flight Check section in CLAUDE.md
4. Add tool references to your skill runbooks
5. Test with a simple query

Each MCP server has its own setup instructions. Check the links above for specifics.

---

## Repo Map

```
my-os/
├── README.md                              # You're reading it
├── CLAUDE.md                              # Master instruction file (Claude reads this first)
├── .claude/
│   └── commands/
│       ├── 1-1.md                         # /1-1 command stub
│       ├── meeting_summary.md             # /meeting_summary command stub
│       ├── weekly-update.md               # /weekly-update command stub
│       └── slack-backlog.md               # /slack-backlog command stub
├── workflows/
│   ├── _index.md                          # Workflow directory
│   ├── quality-gates.md                   # 7-point quality checklist (universal)
│   ├── 1-1-prep-skill.md                  # 1:1 prep + post-meeting follow-up runbook
│   ├── meeting-summary-skill.md           # Meeting recap + Slack share runbook
│   ├── weekly-update-skill.md             # Weekly status update runbook
│   └── slack-backlog-skill.md             # Slack backlog clearing runbook
├── team/
│   ├── _index.md                          # Team roster
│   ├── TEMPLATE-direct-report.md          # Template for direct report dossiers
│   └── stakeholders/
│       └── TEMPLATE-self-profile.md       # Template for your own profile
├── voice/
│   ├── public.md                          # Formal/external communication voice
│   └── private.md                         # Casual/DM communication voice
├── meeting-notes/
│   └── _index.md                          # Rolling decisions + action items
└── weekly-updates/
    └── _index.md                          # Archive index
```

---

## FAQ

**Do I need to know how to code?**
No. This repo has no code — it's all markdown files. If you can edit a text file, you can use this.

**What if I use Linear/Jira/Notion instead of Asana?**
The skills are tool-agnostic. Replace the placeholder MCP tool names in the runbooks with your actual tools. The workflow logic stays the same.

**What if I don't have a meeting recorder (Granola, Otter, etc.)?**
The `/meeting_summary` and `/1-1` skills will ask you to paste notes manually. You can always add a meeting MCP later.

**How do I update Claude's instructions?**
Edit `CLAUDE.md`. Changes take effect on the next conversation (or within the current one if you re-read the file).

**Can I share this with my team?**
Yes — each person should clone their own copy and fill in their own context. The architecture is the same; the content is personal.

**What's the difference between CLAUDE.md and the voice guides?**
`CLAUDE.md` is the master instruction file — it tells Claude what tools exist, how to behave, and what skills are available. Voice guides teach Claude how you *write*. Both are read automatically.

**How do I add more slash commands?**
See "Adding Your First Skill" above. Create a runbook + stub, register in CLAUDE.md, done.
