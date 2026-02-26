# CLAUDE.md

<!-- CUSTOMIZE: This is the master instruction file. Claude reads this at the start of every conversation.
     Fill in your name, role, company, and preferences. The structure matters more than the content —
     keep every section, replace placeholders with your own context. -->

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

<!-- CUSTOMIZE: Replace the bracketed values with your own info. -->

This is **[YOUR NAME]**'s personal operating system — a context repository for AI-assisted productivity as **[YOUR ROLE]** at **[YOUR COMPANY]**. It contains no code to build or test.

## Repository Structure

- `workflows/` - Skill runbooks and reference docs (see `workflows/_index.md`)
- `team/` - Team roster, direct reports, and stakeholder profiles (see `team/_index.md`)
  - `team/stakeholders/` - Key stakeholder profiles
- `voice/` - Communication style guides for drafting content
- `meeting-notes/` - Meeting notes by date, with decisions and action items
- `weekly-updates/` - Archived weekly team updates (see `weekly-updates/_index.md`)

## Key Context Files

When helping me, reference these files as needed:

| File | Use When |
|------|----------|
| `team/stakeholders/[YOUR-NAME].md` | Understanding my decision-making style, principles, strengths |
| `voice/public.md` | Drafting presentations, PRDs, external communications |
| `voice/private.md` | Drafting Slack messages, DMs, internal team communication |
| `team/_index.md` | Finding team member files, understanding structure |
| `workflows/_index.md` | Finding workflow files, understanding available skills |
| `workflows/quality-gates.md` | Quality checks applied before finalizing any summary output |

<!-- CUSTOMIZE: Add rows for your own key files as you create them. Examples:
| `workflows/weekly-sync-template.md` | Weekly planning sessions |
| `workflows/current-week.md` | Active weekly plan (rolling file) |
-->

## Source of Truth

<!-- CUSTOMIZE: Where does your team's canonical data live? Fill in the table below.
     Common options: Google Drive, Notion, Confluence, Linear, Jira, Asana, GitHub. -->

| Data Type | Source | Access Method |
|-----------|--------|---------------|
| Projects / PRDs | [YOUR TOOL — e.g., Notion, Google Drive, Confluence] | [MCP or direct file path] |
| OKRs / Roadmap | [YOUR TOOL] | [MCP or direct read] |
| Task Tracking | [YOUR TOOL — e.g., Linear, Jira, Asana] | [MCP or API] |
| Company Context | [YOUR TOOL] | [Direct read] |

## Working Style

<!-- CUSTOMIZE: Replace these with 3-5 principles that describe how YOU work.
     These help Claude match your decision-making style and communication preferences.
     Examples below are generic — make them yours. -->

My operating principles to respect:

### 1. [YOUR PRINCIPLE — e.g., "Bias Toward Action"]
<!-- Explain in 1-2 sentences what this means for how you work. -->
[Description]

### 2. [YOUR PRINCIPLE — e.g., "Data-Informed, Not Data-Driven"]
[Description]

### 3. [YOUR PRINCIPLE — e.g., "Overcommunicate Context"]
[Description]

<!-- Optional: add up to 5 total. -->

When making recommendations, come with options and a recommendation, not just problems. I value directness.

---

## Voice Guides (Mandatory for Drafting)

**Voice guides are mandatory for document drafting.** Before writing or editing any document, presentation, Slack message, or other content on my behalf:
1. Read the appropriate voice guide first (`voice/public.md` or `voice/private.md`)
2. Apply the tone, structure, and signature phrases from the guide
3. Avoid the anti-patterns listed in the guide

---

## MCP Pre-Flight Check

<!-- CUSTOMIZE: Before running any workflow that depends on MCPs, verify auth is working.
     Replace tool names below with your actual MCP tool prefixes. -->

Before running any workflow that depends on MCPs, make a lightweight read-only call to each required integration to verify auth is working. If any call fails, immediately tell me which integration is down and suggest re-auth — do not silently fall back or continue without the data.

Quick validation calls:
- **Slack**: `mcp__slack__channels_list` (limit=1)
<!-- CUSTOMIZE: Add validation calls for your other MCPs. Examples:
- **Linear**: `mcp__linear__viewer` (whoami check)
- **Notion**: `mcp__notion__search` (limit=1)
- **Jira**: `mcp__jira__get_projects` (limit=1)
-->

Only check the integrations the current workflow actually needs. Report results as a quick status line before proceeding (e.g., "Slack OK — running workflow.").

---

## MCP Servers

<!-- CUSTOMIZE: Document each MCP server you connect. This helps Claude know what tools are available.
     See README.md for how to add MCP servers. -->

### Slack MCP

See Slack Quick Reference section for channel IDs and usage.

**Slack Posting Rules**:
- **Always show the draft message and recipient(s) to the user for approval before sending.** Do not call the Slack send tool until the user explicitly greenlights the message.
- Use `content_type="text/plain"` - preserves blank lines between sections
- Do NOT use `text/markdown` - it collapses line breaks and converts bold to italic
- Add blank line between each section for readability
- Bold formatting (`*text*`) gets stripped by API - manually bold in Slack after posting if needed
- Use `*` for bullet points, `_text_` for italics (these work in text/plain)

<!-- CUSTOMIZE: Add sections for your other MCP servers. Common ones:
### Linear MCP
### Notion MCP
### Jira MCP
### Google Drive MCP
-->

---

## Slack Quick Reference

<!-- CUSTOMIZE: Fill in your actual channel IDs and DM IDs. To find a channel ID:
     mcp__slack__channels_list with channel_types="public_channel" and search for the name.
     To find a DM ID:
     mcp__slack__channels_list with channel_types="im" and search for the person. -->

### Top Channels

| Channel | ID | Purpose |
|---------|-----|---------|
| #general | `[CHANNEL_ID]` | Company-wide |
| #[your-team-channel] | `[CHANNEL_ID]` | Your team's main channel |
<!-- CUSTOMIZE: Add 5-10 channels you reference most. -->

### Key People (DMs)

| Name | Channel ID | Role |
|------|------------|------|
| [Direct Report 1] | `[DM_ID]` | [Role] |
| [Your Manager] | `[DM_ID]` | [Role] |
<!-- CUSTOMIZE: Add people you DM frequently. -->

### Active Project Channels

| Channel | ID |
|---------|-----|
| #[project-1] | `[CHANNEL_ID]` |
<!-- CUSTOMIZE: Add active project channels. -->

### Adding New Contacts

To find a new channel ID:
```
mcp__slack__channels_list with channel_types="im" and search for the name
```
Then add the ID to this table for future quick access.

---

## Tool Quick Reference

<!-- CUSTOMIZE: If you use a project tracker (Asana, Linear, Jira, etc.), add quick-reference IDs here
     so Claude doesn't have to search every time. -->

### [YOUR TOOL — e.g., Linear, Asana, Jira]

| Resource | ID | Purpose |
|----------|----|---------|
| [Your team/project] | `[ID]` | [Description] |
<!-- CUSTOMIZE: Add workspace IDs, project IDs, board IDs, etc. -->

---

## Slack Message Style Guide

- **Length:** 5-8 bullet points max for summaries. Short messages stay short.
- **Voice:** Read `voice/private.md` before drafting. Casual, direct. No corporate fluff.
- **Channel posts:** Use **Progress / Blockers / Plans** structure (not Key Points / Decisions / Action Items)
- **DMs:** Casual bulleted (not numbered) summaries. Optional section headers only if it helps organize distinct topics.
- **Separation:** Never merge distinct workstreams or projects into one bullet. Each project = its own line.
- **Specificity:** Include @mentions/owners on every item. Action items = what + who + why.
- **Detail level:** No implementation details unless explicitly asked. Focus on outcomes and status.
- **Blockers:** Move blocking items to a dedicated Blockers section — don't mix into status updates. Add business context (why it matters).
- **Coverage:** Cover the breadth of topics discussed — don't tunnel on one section.

---

## Skill Execution Rules

- **Execute immediately.** When a skill/slash command is invoked, run it end-to-end. Do not just plan, scaffold, or describe what you would do — actually do it.
- **Default scope: past 7 days.** Unless told otherwise, scope all Slack history and data pulls to the last 7 days.
- **Check MCP auth first.** Before a workflow that depends on MCPs, make a lightweight read call to verify connectivity. If auth fails, tell the user immediately and suggest re-auth — do not silently fall back to a different approach.
- **Show, don't ask.** Generate the output and show it for review. Don't ask "should I proceed?" at every step — just execute and present results.

---

## Common Tasks

<!-- CUSTOMIZE: As you add skills, list them here so Claude knows what's available.
     Format: **Skill name**: Run `/command` to [description]. See `workflows/[file].md`. -->

**1:1 prep + follow-up**: Run `/1-1 [name]` to prepare for 1:1 meetings. Pulls Slack DM history, recent meetings, team file context, and project status. After the meeting, paste notes and Claude auto-updates the team file and drafts a DM recap. See `workflows/1-1-prep-skill.md`.

**Meeting summary**: Run `/meeting_summary` to recap a recent meeting and share via Slack. Defaults to most recent; supports search terms or ordinals ("2nd last"). See `workflows/meeting-summary-skill.md`.

**Weekly update**: Run `/weekly-update` to generate a team-wide product/progress summary. Posts to your team channel after review. See `workflows/weekly-update-skill.md`.

**Slack backlog**: Run `/slack-backlog` to find unanswered Slack mentions and DMs, draft responses in your voice, and send with one-by-one approval. See `workflows/slack-backlog-skill.md`.

---

## Adding Your First Skill

The starter kit comes with 4 universal skills. To add your own:

### 1. Create the runbook

Create `workflows/[skill-name]-skill.md` with this structure:

```markdown
# Skill: `/[command-name]`

**Quality gates:** Before finalizing output, apply checks from `workflows/quality-gates.md`.

> One-line description.

## Trigger
/[command-name] [parameters]

## Steps
### 1. [First step]
[What to do, which tools to call]

### 2. [Next step]
...

## Output Format
[Template of what the output looks like]

## Limitations
[What can't it do]
```

### 2. Create the command stub

Create `.claude/commands/[command-name].md`:

```markdown
# [Skill Name]

[1-2 sentence description]

## Usage
/[command-name] [args]

## Process
[Brief summary of steps — reference the runbook for details]

## Full Documentation
See `workflows/[skill-name]-skill.md` for complete process details.
```

### 3. Register in CLAUDE.md

Add a line to the **Common Tasks** section above:

```
**[Skill name]**: Run `/[command]` to [description]. See `workflows/[file].md`.
```

### 4. Test it

Open Claude Code and type `/[command-name]`. Claude should execute the full workflow.
