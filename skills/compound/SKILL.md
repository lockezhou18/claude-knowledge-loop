---
description: "Compound — Post-Task Learning Capture & Knowledge Router. After completing non-trivial work, capture learnings and route them to the right place."
user_invocable: true
---

# Compound — Post-Task Learning Capture & Knowledge Router

After completing non-trivial work, capture learnings and route them to the right place. **Suggest, not enforce** — present findings, let the user decide what to save.

## Phase 0: Classify & Suggest (Knowledge Router)

Scan the conversation and classify what happened. Present findings as suggestions — the user decides what to save.

**Classification signals:**

| Signal | Detected when | Suggest |
|--------|--------------|---------|
| **Architecture decisions** | 3+ "should we X or Y?" discussions with conclusions | Self-contained HLD → specs directory |
| **Research conducted** | WebSearch/WebFetch used, sources compared, hypothesis formed | Knowledge note (research) + source cards |
| **New domain explored** | Unfamiliar topic discussed, learning curve visible | Knowledge note (research) |
| **Reusable pattern found** | Approach that would apply beyond this specific task | Knowledge note (pattern) |
| **Surprise/failure** | "I didn't expect...", debugging, wrong assumptions corrected | Knowledge note (solution or lesson) |
| **Success validated** | User praised approach, confirmed non-obvious choice worked | Knowledge note (success) |
| **Long session with context** | Multiple stages, decisions, artifacts produced | Session card → memory |
| **Backlog items emerged** | "We should also...", "future work:", deferred decisions | Backlog items → memory |

**Present as suggestions:**
```
/compound found:
  - 2 architecture decisions → save HLD to specs/? [y/n]
  - Research on topic X → save knowledge note + source cards? [y/n]
  - Long session (2+ hours) → save session card to memory? [y/n]
  - 3 backlog items discussed → save to backlog? [y/n]
```

**User says yes/no per item.** Don't save anything without consent. Skip items the user declines.

**Context-dependent depth:**

| Session type | Typical suggestions | What to skip |
|---|---|---|
| Big architecture | All of the above | Nothing — capture everything |
| Medium feature | Knowledge note + maybe session card | HLD (code is the truth) |
| Bug fix | Knowledge note if root cause was surprising | Everything else (commit message suffices) |
| Research only | Knowledge notes + source cards | HLD, session card |
| Routine ops | Nothing | Everything (don't document the obvious) |

---

## Phase 1: Retrospective

For each item the user approved, review the conversation and extract learnings.

**Tracks (use whichever apply):**

**Bug Track** (if work involved fixing something):
- Symptoms → root cause → what didn't work → actual fix → why it works → prevention

**Knowledge Track** (if work involved building or learning):
- Key insight → why it matters → when to apply → concrete example

**Research Track** (if work involved research):
- Question → findings → surprises → actionable recommendations

**Decision Track** (if significant choices were made):
- What decided → alternatives considered → why this option → what triggers revisit

**Success Track** (if something worked particularly well):
- What specifically worked → why → conditions → how to replicate → what would improve it

---

## Phase 2: Route to Right Tier

Based on what was approved in Phase 0, save to the correct location:

### Tier 1: Memory (Claude's cross-session context)

**When:** Long sessions, significant decisions, project context needed in future conversations.

**Session card template:**
```markdown
---
name: Session — {topic}
description: {one-line summary of what happened}
type: project
---

Date: {YYYY-MM-DD}
Topic: {topic}

Decisions made:
- {decision 1 — what was chosen and why}

Artifacts produced:
- {path/to/artifact} — {what it is}

Surprises:
- {unexpected finding}

Open questions:
- {unresolved question}

Next: {what should happen next}
```

### Tier 2: Specs (self-contained HLD)

**When:** Architecture decisions were made that others need to understand.

Save as a self-contained document. Include research appendices so the document stands alone — anyone reads it, gets the full picture.

### Tier 3: Knowledge Notes

**When:** Non-obvious, reusable insights were discovered.

Follow the note-writing process (Phase 3-5 below).

### Source Cards

**When:** Research was conducted with web sources.

```markdown
---
url: {source URL}
title: "{source title}"
accessed: {YYYY-MM-DD}
type: {primary|academic|industry|community}
---

## Key excerpts
- "{quote or data point that mattered}"

## Used in
- {Which decision this informed}
```

---

## Phase 3: Overlap Check (for knowledge notes)

Before writing a new note, search your existing knowledge base for notes on the same topic.

Score overlap: same problem, same root cause, same approach, same files.
- **High overlap (3-4):** Update the existing note.
- **Low overlap (0-2):** Create a new atomic note.

---

## Phase 4: Write Atomic Notes

**One insight per note.** Multiple learnings = multiple notes with links.

Generate ID: `note-YYYYMMDD-HHMMSS`

```markdown
---
id: note-YYYYMMDD-HHMMSS
title: <descriptive title>
type: solution | research | decision | pattern | playbook | success
tags: [tag1, tag2, ...]
links: []
agent: <who discovered this>
status: active
---

## Context
<what happened — 1-2 sentences>

## Insight
<the non-obvious learning — the core of the note>

## Evidence
<data, errors, research that supports the insight>

## Action
<what to do with this knowledge>
```

| Type | When to use | Required sections |
|------|------------|-------------------|
| **solution** | Fixed a bug or solved a problem | Context, Insight, Evidence, Action |
| **research** | Explored a topic, found useful info | Context, Insight, Evidence |
| **decision** | Made a significant choice | Context, Insight (why), Action (revisit trigger) |
| **pattern** | Discovered a reusable approach | Context, Insight, Action (when to apply) |
| **playbook** | Step-by-step procedure | Context, Action (the steps) |
| **success** | Something worked well | Context, Insight (why), Action (replicate) |

---

## Phase 5: Link Notes

Identify connections to existing notes. For each related note:
1. Add its ID to your new note's `links:` array
2. Add your new note's ID to the related note's `links:` array (bidirectional)
3. Record the relationship

Relationship types: `references`, `contradicts`, `supersedes`, `extends`, `related`

If a note **contradicts** an existing one, flag explicitly.
If a note **supersedes** an existing one, mark old note's status as `superseded`.

---

## Phase 6: Update Index

Append one line per note to your knowledge index:

```markdown
- [Title](notes/<id>-<slug>.md) — tags: tag1, tag2 | agent: name | type
```

Keep the index sorted by date descending.

---

## Phase 7: Aggregation Check

After saving notes, check if synthesis is warranted:

If 3+ notes share a tag cluster, **suggest** (don't auto-create) synthesizing a meta-pattern note:
```
3 notes tagged [multi-agent, communication] → synthesize into "Distributed Agent Communication Principles"? [y/n]
```

This is how episodic knowledge (individual notes) becomes semantic knowledge (patterns). Only suggest when there's enough raw material.

---

## Phase 8: Action Items

Final check — does this session warrant any of these?

- [ ] Update project instructions or team specs?
- [ ] Add follow-up tasks?
- [ ] New hook, skill, or monitoring check needed?
- [ ] Does any finding contradict an existing decision? (flag for review)
- [ ] Backlog items to track?

---

## Rules
- **Suggest, not enforce** — present findings, user decides what to save
- **Context-dependent** — a bug fix needs nothing; a big architecture session needs everything
- **One insight per note** — atomic notes, multiple learnings = multiple notes
- **Save at the gate** — when user agrees, not mid-discussion
- Focus on **non-obvious, reusable** insights — skip anything derivable from the code
- Always add tags — this is how future research finds past work
- Always check overlap before writing — update > duplicate
- Ask before writing — never save without user consent
- Source cards preserve raw research — URL + key excerpts + decision links
- Session cards capture journey — decisions, artifacts, surprises, next steps
