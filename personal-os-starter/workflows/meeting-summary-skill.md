# Skill: `/meeting_summary`

**Quality gates:** Before finalizing output, apply checks from `workflows/quality-gates.md`.

> Quick recap of a recent meeting, with optional Slack share.

## Trigger

```
/meeting_summary [meeting] [destination]
```

Both arguments are optional and can appear in any order:
- **meeting selector**: search term, title fragment, or "2nd last" / "3rd last" etc. Defaults to most recent.
- **destination**: `@name` (DM) or `#channel`

Examples:
- `/meeting_summary` — most recent meeting, suggest destination
- `/meeting_summary @sarah` — most recent, DM to Sarah
- `/meeting_summary product review` — find the most recent meeting matching "product review"
- `/meeting_summary product review #product` — match + post to channel
- `/meeting_summary 2nd last @mike` — second most recent meeting, DM to Mike

## Steps

### 1. Fetch the meeting

<!-- CUSTOMIZE: Replace with your meeting data tool. Options:
     - Granola MCP: get_recent_meetings(count=1)
     - Google Calendar + manual notes: ask user to paste
     - Otter.ai, Fireflies, etc.: use their MCP or API
     - Manual: ask user to paste meeting notes -->

**If you have a meeting recorder MCP:**

**Default (no selector):**
```
[your_meeting_tool]__get_recent_meetings(count=1)
```

**With ordinal** ("2nd last", "3rd last"):
```
[your_meeting_tool]__get_recent_meetings(count=N)  # then pick the Nth result
```

**With search term** (title fragment or topic):
```
[your_meeting_tool]__search_meetings(query="product review", limit=5)  # pick best match
```

**If no meeting recorder MCP:** Ask the user to paste their meeting notes or a summary. Then skip to Step 3.

If multiple matches, show a short list and let user pick. Display title, duration, and participant list so user can confirm.

### 2. Get meeting content (parallel)

Run both in parallel:
- Full metadata and participants
- AI summary and human notes

If notes are empty or thin, fall back to transcript and summarize manually.

### 3. Generate concise summary

Format depends on destination type (see Step 5), but general principles apply to all:

- Keep it tight — drop implementation details and technical noise
- Use descriptive names for workstreams
- Include @mentions for owners on every item
- Add business context on blockers (why it matters, not just what's stuck)
- Action items = what + who + why, not just what
- Cover the breadth of the meeting — don't tunnel on one topic

### 4. Resolve destination

**If destination provided:**
- Map `@name` to Slack DM channel ID using CLAUDE.md Quick Reference (Key People table)
- Map `#channel` to channel ID using CLAUDE.md Quick Reference (channel tables)
- If not found in Quick Reference, search via `channels_list`

**If no destination provided:**
- Check meeting participants against CLAUDE.md Key People table — suggest DM to participant if it's a 1:1
- If group meeting, suggest the most relevant project channel based on title/topic matching
- Present suggestion and let user confirm or redirect

### 5. Draft and send Slack message

**Before drafting:** Read `voice/private.md` for DMs. Choose format based on destination:

**DM format** (casual, matches your voice):
- Jump straight into content, no preamble
- Bulleted list (not numbered) — casual, conversational
- Optional section headers if the meeting covered distinct topics
- Close with "lmk if questions on any of this" or similar

**Channel format** (structured, for broader audience):
- Header line: "Notes from [date] [Meeting Name]"
- Three sections: **Progress** / **Blockers** / **Plans** (omit any section that has no content)
- Progress: what moved forward, shipped, or was decided
- Blockers: what's stuck and *why it matters*
- Plans: who owes what next, with @mentions and timeframes
- @mentions on every item — no orphan bullets

**Sending rules:**
- Always show draft to user for approval before sending
- Use `content_type="text/plain"` (preserves line breaks)
- Never send without explicit greenlight

## Limitations

- **Only works with recorded meetings**: If your meeting tool wasn't running, there's no data to pull. Fallback: paste notes manually.
- **Summary quality depends on notes**: Meetings with edited notes produce better recaps than transcript-only meetings.
- **Destination mapping**: Only works for people/channels listed in CLAUDE.md Quick Reference. Add new contacts there first.
