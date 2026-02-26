# 1:1 Meeting Prep & Follow-Up Skill

**Trigger**: `/1-1 [name]`

**Purpose**: Prepare context for 1:1 meetings (Steps 1-4), then handle post-meeting follow-up — updating team files and drafting DM recaps (Steps 5-7).

---

## Process

### Step 1: Identify Person

Parse name from command argument. Match against the mapping table below.

<!-- CUSTOMIZE: Add your direct reports and key contacts. Format:
     | Input aliases | Slack DM ID | Team File path |
     Find DM IDs via: mcp__slack__channels_list with channel_types="im" -->

| Input | Slack DM ID | Team File |
|-------|-------------|-----------|
| `[name]`, `[alias]` | `[DM_CHANNEL_ID]` | `team/[name].md` |
<!-- CUSTOMIZE: Add a row for each person you have 1:1s with. Example:
| `sarah`, `sarah-chen` | `D06ABCDEF12` | `team/sarah-chen.md` |
| `mike`, `mike-boss` | `D05GHIJKL34` | `team/stakeholders/mike-boss.md` |
-->

If no name provided, ask for one.

### Step 2: Pull Recent Context

Fetch in parallel:

1. **Slack DMs** - Last 7 days of conversation
   - Tool: `mcp__slack__conversations_history(channel_id=[DM ID], limit="7d")`
   - Extract: key topics, open questions, action items

2. **Recent meetings** - Last 2-3 meetings with this person
   <!-- CUSTOMIZE: Replace with your meeting tool. Examples:
   - Granola: search_meetings(participant=[name], from_date="30d")
   - Google Calendar: check calendar MCP
   - Manual: user pastes notes -->
   - Extract: decisions, action items, discussion topics

3. **Team file** - Read their team file for context
   - Growth areas, working style, pending feedback
   - Recent 1:1 notes if captured there

### Step 3: Pull Project Context

<!-- CUSTOMIZE: Replace with your project tracking tool. Examples:
- Linear: mcp__linear__issues(assignee=[name])
- Jira: mcp__jira__search(assignee=[name])
- Asana: asana_search_tasks(assignee_any=[gid])
- Notion: mcp__notion__search(query=[name]) -->

Query your project tracker for:
- What projects does this person own?
- Current status and any blockers
- Upcoming deadlines

### Step 4: Generate Prep Document

Synthesize all context into actionable prep output.

---

## Output Format

```markdown
## 1:1 Prep: [Name]
**Date**: [Today's date]
**Last 1:1**: [Date or "Unknown"]

---

### Recent Conversation Themes
- [Summary of Slack DM topics from last 7 days]
- [Any open questions or items pending response]
- [Commitments made in recent messages]

### Their Active Projects

| Project | Status | Notes |
|---------|--------|-------|
| [Project 1] | [On Track/At Risk/Blocked] | [Key updates or blockers] |
| [Project 2] | [Status] | [Notes] |

### Topics to Discuss
1. [Topic from DMs or project updates needing alignment]
2. [Topic from recent meeting follow-ups]
3. [Growth area check-in based on team file]
4. [Any blockers requiring your help]

### From Last 1:1
- [Key decisions made]
- [Action items and their status]
- [Topics that were tabled for later]

### Suggested Questions
- [Based on growth areas from team file]
- [Based on project status/challenges]
- [Based on recent conversation themes]

---

### Reference
- **Working style**: [Key notes from team file]
- **Growth areas**: [From team file - what to check in on]
```

---

## Post-Meeting Flow (Steps 5-7)

After the 1:1, the user will paste notes, a summary, or raw context from the meeting. When this happens, run Steps 5-7 automatically in sequence.

### Detecting Post-Meeting Mode

Trigger post-meeting flow when the user:
- Pastes meeting notes or a summary (multi-line text with discussion topics, action items, decisions)
- Says "here are my notes", "meeting notes", "just finished my 1:1", or similar
- Pastes a meeting tool export or transcript excerpt

**Cold start**: If `/1-1` wasn't run earlier in this conversation, ask who the 1:1 was with before proceeding.

### Step 5: Update Team File

1. Read the current team file (`team/[name].md`)
2. Add a new dated entry under `## 1:1 Notes`, placed **above** previous entries (newest first)
3. Match the existing date format in that file
4. Organize by topic with **bold headers**, details as sub-bullets
5. Add `**Action Items:**` section at the end with checkbox items (`- [ ]`), each with owner and clear description
6. Refresh `### Next 1:1 Agenda` at the top of the 1:1 Notes section:
   - Add follow-up items from today's action items
   - Carry forward any undiscussed items from the previous agenda
   - Remove items that were covered
7. Update `*Last updated*` timestamp at the bottom of the file to today's date

### Step 6: Draft DM Recap

1. Read `voice/private.md` before drafting
2. Write a casual bulleted (not numbered) summary of the meeting
3. Use optional section headers only if it organizes distinct topics better
4. Follow these formatting rules:
   - Action items with owners on every item — no orphan bullets
   - Use descriptive names for workstreams
   - Drop overly granular implementation details
   - Add business context to blockers
   - Cover breadth of meeting topics — don't tunnel on one section
   - No preamble, no sign-off, close with "lmk" or nothing

### Step 7: Send DM

1. Show the draft message and recipient to the user for approval
2. **Never send without explicit greenlight**
3. Use `content_type="text/plain"` (preserves blank lines, bullet formatting)
4. Send to the person's Slack DM channel ID from the Step 1 mapping table

---

## Data Sources

| Source | Tool | What to Pull |
|--------|------|--------------|
| Slack DMs | `mcp__slack__conversations_history` | Last 7 days of messages |
| Meetings | [YOUR MEETING TOOL] | Recent 1:1s with this person |
| Team file | Read `team/[name].md` | Growth areas, working style, notes |
| Projects | [YOUR PROJECT TRACKER] | Project ownership, status, details |

---

## Fallbacks

- **No Slack DMs found**: Note "No recent DM history" and continue
- **No meetings found**: Note it — may have met without recording
- **No team file exists**: Skip team context section, note gap
- **No project ownership found**: They may not be a project owner

---

## Limitations

- Only captures Slack DMs, not other communication channels
- Meeting data requires your meeting tool to be connected
- Team files must be kept up to date for growth areas
- Cannot see calendar to know if 1:1 is actually scheduled
- DM recap quality depends on the detail level of pasted notes
