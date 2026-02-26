# Weekly Update Skill

**Quality gates:** Before finalizing output, apply checks from `workflows/quality-gates.md`.

**Trigger**: `/weekly-update`

**Purpose**: Generate a weekly progress summary for your team channel, serving leadership, cross-functional partners, and your team with qualitative updates.

**Core Principle**: High-density, efficient read. Anyone on the team should get the key updates in under 2 minutes. No filler, no verbose explanations — just signal.

---

## Parameters

| Parameter | Default | Description |
|-----------|---------|-------------|
| `--days` | 7 | History window for Slack queries |
| `--dry-run` | false | Draft only, don't post |

---

## Timing

<!-- CUSTOMIZE: When do you send your weekly update? -->
Run [YOUR DAY — e.g., Friday afternoon], review, post to [YOUR CHANNEL] [YOUR DAY] EOD.

---

## Data Sources (Query in Parallel)

| Source | Tool | What to Extract |
|--------|------|-----------------|
| Slack project channels | `mcp__slack__conversations_history` (7d) | Status updates, blockers, decisions |
| [YOUR TRACKER] | [YOUR TOOL] | Task completion, milestones, blocked items |
| [YOUR DOC TOOL] | [YOUR TOOL] | Official status, owners, timelines |
| [YOUR SHIPPED CHANNEL] | `mcp__slack__conversations_history` | What launched this week |

<!-- CUSTOMIZE: Add or remove data sources based on your stack. -->

---

## Slack Channels to Monitor

<!-- CUSTOMIZE: List the channels Claude should query for weekly update content.
     Organize into tiers: always query vs. only if active. -->

### Tier 1 (Always Query)

| Channel | ID | Project |
|---------|-----|---------|
| #[project-channel-1] | `[CHANNEL_ID]` | [Project Name] |
| #[project-channel-2] | `[CHANNEL_ID]` | [Project Name] |
<!-- CUSTOMIZE: Add your active project channels. -->

### Tier 2 (Query if Active)

Check message count first, skip if <3 messages in window:

| Channel | ID | Project |
|---------|-----|---------|
| #[project-channel-3] | `[CHANNEL_ID]` | [Project Name] |
<!-- CUSTOMIZE: Add channels that may or may not be active week to week. -->

### Context Channels

| Channel | ID | Purpose |
|---------|-----|---------|
| #[shipped-channel] | `[CHANNEL_ID]` | What launched |
| #[team-channel] | `[CHANNEL_ID]` | Team discussion |
<!-- CUSTOMIZE: Add channels that provide context but aren't project-specific. -->

---

## Process Steps

### Step 1: Gather (Parallel Queries)

Run all channel queries in parallel:
```
mcp__slack__conversations_history(channel_id="[ID]", limit="7d")
```

Query your project tracker for task completions and milestones.

### Step 2: Extract

From Slack, look for:
- Status keywords: "shipped", "launched", "live", "blocked", "delayed", "pushed"
- Rollout indicators: "rolling out", "50%", "100%", "internal", "public"
- Decisions: "decided", "going with", "approved", "signed off"
- Timeline: "targeting", "ETA", "next week", "by Friday"

From your project tracker, extract:
- Tasks completed this week
- Upcoming milestones
- Blocked items
- Assignees (for owner attribution)

### Step 3: Synthesize

Cross-reference sources:
- Use most recent signal as authoritative
- Flag discrepancies (e.g., Slack says "blocked" but tracker shows active)
- Attribute to correct owner

Group into 4 sections (each project appears ONCE):
1. **Shipped** — Launched this week
2. **In Progress** — Active work with target dates inline
3. **In Design** — Design/scoping work, no firm dates yet
4. **Notes** — Paused items, shifts, blockers (keep brief)

### Step 4: Sensitivity Check

Before including any content, flag for explicit approval:
- Information from DMs or private channels
- Personnel/hiring discussions
- Unannounced partnerships
- Financial/pricing decisions not yet public
- Performance concerns about individuals

Present flagged content with: "This came from [source]. OK to share publicly?"

### Step 5: Generate

Build Slack-friendly output per `voice/public.md`:
- Channel name in parentheses: (#project-channel)
- Target dates inline with status
- One line per project max
- Each project appears once — no duplication across sections

### Step 6: Review

Present draft to user with:
- Any flagged sensitivity concerns
- Open questions about attribution or status

### Step 7: Post

Upon approval:
```
mcp__slack__conversations_add_message(channel_id="[YOUR_TEAM_CHANNEL_ID]", text="[formatted update]", content_type="text/plain")
```

### Step 8: Archive

Save to `weekly-updates/YYYY-MM-DD-weekly-update.md`

---

## Output Format

**Target Length**: 300-400 words (efficient 2-minute read)

**Voice**: Follow `voice/public.md` — direct, structured, no exclamation points.

```
*Weekly Update: [Month Day-Day]*

*Shipped*
* [Feature] (#channel) — [1-line impact]

*In Progress*
* _[Project]_ — [status]. *[Target date]*. @[Owner]

*In Design*
* _[Project]_ — [status]. @[Owner]

*Notes*
* _Paused_: [Project] — [reason]
```

**Key Formatting Rules**:
- Brevity is paramount — every word should add value
- One line per project max — status + target date + owner inline
- Each project appears ONCE (no duplication across sections)
- Channel name in parentheses
- Use `content_type="text/plain"` for Slack posting
- No preambles, no verbose explanations
- No exclamation points

---

## Limitations

- Slack history limited to 7 days by default
- Can't detect decisions in DMs without explicit approval
- Some projects don't have dedicated Slack channels
