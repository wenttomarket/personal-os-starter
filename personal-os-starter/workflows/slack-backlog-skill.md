# Skill: `/slack-backlog`

**Quality gates:** Before finalizing output, apply checks from `workflows/quality-gates.md`.

> Clear your Slack backlog — find unanswered mentions and DMs, draft responses, send with approval.

## Trigger

```
/slack-backlog [--days=N]
```

- Default: 2 days
- Optional `--days` parameter (1-7)

## Steps

### 1. MCP pre-flight

Verify Slack connectivity:
```
mcp__slack__channels_list(limit=1)
```

### 2. Gather mentions and DMs (parallel)

Run these searches in parallel:

<!-- CUSTOMIZE: Replace [YOUR_SLACK_USER_ID] with your Slack user ID.
     Find it by searching for yourself: mcp__slack__users_search(query="[your name]") -->

**a) @mentions in channels**
```
mcp__slack__conversations_search_messages(search_query="<@[YOUR_SLACK_USER_ID]>", filter_date_after="[N days ago]")
```

**b) DMs where others messaged you**
Loop through Key People DM channel IDs from CLAUDE.md Quick Reference. For each:
```
mcp__slack__conversations_history(channel_id="[DM ID]", limit="[N]d")
```
Filter to messages NOT from you (`[YOUR_SLACK_USER_ID]`) — these are messages you may need to respond to.

**c) Threads you're tagged in**
```
mcp__slack__conversations_search_messages(search_query="<@[YOUR_SLACK_USER_ID]>", filter_date_after="[N days ago]", filter_threads_only=true)
```

### 3. Deduplicate and filter

- Merge results from all three searches
- Remove messages you already replied to — check thread for your user ID in subsequent messages via `mcp__slack__conversations_replies`
- Remove bot messages and automated posts
- Group by thread/conversation — each "item" = one thread or DM exchange needing a response

### 4. Read thread context

For each item:
- **Channel threads**: fetch full thread via `mcp__slack__conversations_replies(channel_id, thread_ts)`
- **DMs**: use the already-fetched DM history

Capture: who said what, what's being asked/discussed, any deadlines or urgency signals. For very long threads, show last ~10 messages.

### 5. Draft responses

**Before drafting:** Read `voice/private.md`.

For each item, draft a response in your voice:
- Match your tone and patterns from the voice guide
- Action-oriented (what's the next step?)
- Use your signature tokens
- No exclamation points, no corporate speak

Apply quality gates from `workflows/quality-gates.md`.

### 6. Present backlog summary

Show a numbered summary table:

```
## Slack Backlog: [date range]

| # | Channel/DM | From | Topic | Age |
|---|-----------|------|-------|-----|
| 1 | #project-channel | @person | Question about X | 1d |
| 2 | DM @person | Person | Feedback on Y | 6h |
| 3 | #team | @person | Request for Z | 2d |
...

[N] items need responses. Walking through each one now.
```

### 7. One-by-one approval loop

For each item:
1. Show the thread context (key messages, who said what)
2. Show the drafted response
3. Ask user: **Send as-is / Edit / Skip / Skip all remaining**
4. If "Edit" — user provides modified text, confirm, then send
5. If "Send" — post via `mcp__slack__conversations_add_message` with `content_type="text/plain"` and appropriate `thread_ts` (for channel threads) or `channel_id` (for DMs)
6. Move to next item

### 8. Summary

After all items processed:
```
Done — sent N/M responses, skipped K.
```

## Limitations

- Slack search API may miss some mentions in private channels Claude can't access
- DM scanning limited to Key People list in CLAUDE.md — won't catch DMs from unlisted people
- Thread context truncated for very long threads (last ~10 messages shown)
- Rate limits may slow down if 20+ threads need context fetching
