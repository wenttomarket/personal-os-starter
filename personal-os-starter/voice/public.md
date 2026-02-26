# Voice Profile: Public (Presentations, Docs & External)

> **For Claude**: Use this profile when drafting presentations, PRDs, company-wide updates, external communications, and formal documents on my behalf.

<!-- CUSTOMIZE: This guide teaches Claude how you write formally. To populate it:
     1. Collect 20-30 examples of your writing: Slack channel posts, PRDs, presentations, emails
     2. Look for patterns: How do you open? How do you close? What phrases recur?
     3. Fill in the tables and sections below with YOUR patterns
     See README.md "Populating Your Voice" section for the full exercise. -->

## Signature Tokens (Quick Reference)

Use these characteristic phrases to nail the voice:

| Context | I Say | Not This |
|---------|-------|----------|
| Opening | [YOUR OPENER — e.g., "Hey team,"] | [ANTI-PATTERN — e.g., "I hope this message finds you well"] |
| Framing | [YOUR FRAMING — e.g., "Strategic context:"] | [ANTI-PATTERN — e.g., "Overview"] |
| Data | [HOW YOU CITE DATA — e.g., "Drove ~10% increase"] | [ANTI-PATTERN — e.g., "We saw some improvement"] |
| Emphasis | [YOUR EMPHASIS — e.g., "KEY TAKEAWAY"] | [ANTI-PATTERN — e.g., "Important!"] |
| Uncertainty | [HOW YOU HEDGE — e.g., "Open question:"] | [ANTI-PATTERN — e.g., "We're not really sure but..."] |
| Forward | [HOW YOU LOOK AHEAD — e.g., "Next steps:"] | [ANTI-PATTERN — e.g., "Going forward we will try to..."] |
| Closing | [YOUR CLOSER — e.g., "LFG"] | [ANTI-PATTERN — e.g., "Please let me know if you have questions"] |

<!-- CUSTOMIZE: Add or remove rows as needed. The key is capturing YOUR specific patterns. -->

---

## Quick Reference

| Attribute | Pattern |
|-----------|---------|
| Typical format | [e.g., Structured with clear headers, bullets, numbered lists] |
| Formality | [1-5 scale — e.g., 3/5 - Professional but direct] |
| Visual emphasis | [e.g., Bold headers, callout boxes] |
| Punctuation | [e.g., Standard punctuation; rarely uses exclamation points] |
| Capitalization | [e.g., Proper case for headers] |

---

## Tone Characteristics

<!-- CUSTOMIZE: Replace these with 3-5 characteristics of YOUR formal writing voice. -->

1. **[CHARACTERISTIC — e.g., Data-forward but not dry]**
   - [Explanation and examples]

2. **[CHARACTERISTIC — e.g., Direct and action-oriented]**
   - [Explanation and examples]

3. **[CHARACTERISTIC — e.g., Audience-aware framing]**
   - [Explanation and examples]

4. **[CHARACTERISTIC — e.g., Confident with appropriate hedging]**
   - [Explanation and examples]

---

## Structure Patterns

### Document Organization
<!-- CUSTOMIZE: How do you typically structure documents? -->
- [YOUR PATTERN — e.g., Strategic context section up front]
- [YOUR PATTERN — e.g., Clear headers and subheaders]
- [YOUR PATTERN — e.g., Bullet points for lists, numbered for sequences]

### Headlines That Work
<!-- CUSTOMIZE: What makes a good header in your style? -->
- [YOUR PATTERN — e.g., Snappy headlines that state the insight, not describe the content]
- [GOOD EXAMPLE — e.g., "Retention drops 40% in week 2"]
- [BAD EXAMPLE — e.g., "Week 2 Retention Analysis"]

---

## Content Approach

### How to Frame Updates
<!-- CUSTOMIZE: Your preferred structure for updates. -->
1. [STEP — e.g., Strategic context (why this matters now)]
2. [STEP — e.g., What we learned / key findings]
3. [STEP — e.g., Opportunities identified]
4. [STEP — e.g., Recommended next steps]
5. [STEP — e.g., Open questions / risks]

### Finishing Strong
- Always end with clear next steps
- Assign ownership where possible
- Include timeline/urgency context
- Call out dependencies and blockers

---

## Words/Phrases I Use

<!-- CUSTOMIZE: List phrases you actually use, organized by context. -->

### Framing
- [YOUR PHRASE]
- [YOUR PHRASE]

### Emphasis
- [YOUR PHRASE]
- [YOUR PHRASE]

### Transitions
- [YOUR PHRASE]
- [YOUR PHRASE]

---

## Words/Phrases I Avoid

<!-- CUSTOMIZE: What should Claude NEVER say on your behalf? -->
- [ANTI-PATTERN — e.g., Corporate jargon: "synergies", "leverage"]
- [ANTI-PATTERN — e.g., Vague headers: "Overview", "Summary"]
- [ANTI-PATTERN — e.g., Exclamation points]
- [ANTI-PATTERN — e.g., Apologetic framing: "This is just a rough draft but..."]

---

## Format Examples

### Good Header
> **[YOUR EXAMPLE of a header that works]**

### Bad Header
> **[YOUR EXAMPLE of a header that doesn't work]**

### Good Bullet
> - **[YOUR EXAMPLE of a well-written bullet point]**

### Bad Bullet
> - [YOUR EXAMPLE of a vague or weak bullet point]

---

## Context-Specific Adjustments

<!-- CUSTOMIZE: How does your voice shift for different audiences? -->

### Company-wide presentations
- [YOUR PATTERN]

### Executive updates
- [YOUR PATTERN]

### External communications
- [YOUR PATTERN]

---

## Slack Posting Format (via API)

When posting to public Slack channels via Claude/MCP:
- Use `content_type="text/plain"` - preserves blank lines between sections
- Do NOT use `text/markdown` - it collapses line breaks and mangles formatting
- Add blank line between each section header for readability
- Bold (`*text*`) gets stripped by API - manually bold in Slack after posting if needed
- Bullets (`*`) and italics (`_text_`) work in text/plain

---

*Last updated: [DATE]*
*To populate: collect 20-30 writing samples and extract patterns. See README.md for the exercise.*
