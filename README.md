# Claude Knowledge Loop

Four skills for [Claude Code](https://docs.anthropic.com/en/docs/claude-code) that make your knowledge compound over time.

The inquiry cycle works for any domain — engineering, business, theology, markets, research — but the pipeline and examples lean toward software engineering. Most AI coding tools help you build faster. These help you **learn faster** — so every project makes the next one better.

## The Loop

```
  /research         /lfg            /compound        /compound-refresh
  ┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
  │ Understand│───►│  Build   │───►│ Capture  │───►│ Maintain │
  │  deeply   │    │ with     │    │ what you │    │ what you │
  │           │    │ gates    │    │ learned  │    │ know     │
  └─────▲────┘    └──────────┘    └──────────┘    └────┬─────┘
        │                                              │
        └──────── knowledge feeds back ────────────────┘
```

1. **`/research`** — Systematic inquiry before you build. Not search — research. Uses Socratic questioning, Aristotle's Four Causes, and abductive reasoning to generate understanding, not just find information.

2. **`/lfg`** — Gated engineering pipeline: RESEARCH → PROPOSE → PLAN → BUILD → REVIEW → DEPLOY → VERIFY → COMPOUND. Matches ceremony to scope — trivial tasks skip the pipeline, heavy tasks get deep research and threat modeling. No skipping stages.

3. **`/compound`** — After completing work, captures non-obvious learnings as atomic, linked notes (Zettelkasten-style). Classifies what happened, suggests what to save, routes to the right place. You decide what to keep.

4. **`/compound-refresh`** — Periodic maintenance on your knowledge base. Checks notes against current code, heals broken links, flags contradictions, archives stale insights, synthesizes patterns from 3+ related notes.

## Why This Exists

Every engineer has solved the same problem twice because they forgot the first solution. Every team has made the same mistake because the lesson lived in someone's head, not in a searchable note.

These skills close that gap. The key insight: **knowledge doesn't compound automatically — it needs a system.** Research generates understanding. Building tests that understanding. Compound captures what survived. Refresh keeps it honest.

Over weeks and months, your knowledge base grows from nothing into a searchable repository of patterns, decisions, and lessons that makes every `/research` cycle faster and every `/lfg` build more informed.

## Install

```bash
git clone https://github.com/lockezhou18/claude-knowledge-loop.git
cp -r claude-knowledge-loop/skills/* ~/.claude/commands/
```

Or install to a specific project (shared with collaborators via git):
```bash
cp -r claude-knowledge-loop/skills/* your-project/.claude/commands/
```

Start a Claude Code session and type `/research <topic>` or `/lfg`.

## Setting Up Your Knowledge Base

The loop works best with a place to store notes. Create a simple structure:

```bash
mkdir -p ~/knowledge/{notes,sources}
touch ~/knowledge/INDEX.md
```

Then tell Claude where it is by adding to your project's `CLAUDE.md`:

```markdown
Knowledge base location: ~/knowledge/
- Notes: ~/knowledge/notes/
- Source cards: ~/knowledge/sources/
- Index: ~/knowledge/INDEX.md
```

The skills will read and write notes there. Start empty — it fills up naturally as you work.

## How Each Skill Works

### `/research <topic>`

Runs a 6-step inquiry cycle:

1. **DEFINE** — Question the question. What are you actually asking? What are you assuming?
2. **EXPLORE** — Check local knowledge, then primary sources, then external sources matched to your research mode (DISCOVER / UNDERSTAND / BUILD / DEBUG / EVALUATE)
3. **QUESTION** — Attack your own findings using 6 types of Socratic questions
4. **HYPOTHESIZE** — Generate the best explanation using abductive reasoning
5. **SYNTHESIZE** — Find where sources agree, disagree, and leave gaps
6. **REFLECT** — Name what you know, what you don't, and what to ask next

Produces a research artifact with: Question, Findings, Surprises, Contradictions, Hypothesis, and What Would Change Our Mind.

**Scales to scope:** Quick (2-5 min) for known domains, Exhaustive (30+ min) for foundational decisions.

### `/lfg`

Eight-stage pipeline with enforced gates:

```
RESEARCH → PROPOSE → PLAN → BUILD → REVIEW → DEPLOY → VERIFY → COMPOUND
```

Each stage produces evidence before the next begins. Scope determines ceremony:

| Scope | Pipeline | When |
|-------|----------|------|
| Trivial | Skip — just do it | One file, < 10 lines |
| Light | PLAN → BUILD → VERIFY | 1-3 files, low risk |
| Standard | All 8 stages | Multiple files, moderate risk |
| Heavy | All 8 + deep research + threat model | Cross-system, high risk |

### `/compound`

Scans the conversation after work is done and suggests what to save:

- Architecture decisions → specs
- Research findings → knowledge notes + source cards
- Reusable patterns → pattern notes
- Surprises and failures → solution notes
- Session context → session cards

**Suggest, not enforce** — presents findings, you decide what to keep. Checks for overlap before writing (updates existing notes instead of duplicating).

### `/compound-refresh`

Monthly (or after major changes) maintenance:

1. **Inventory** — Count notes, check index sync
2. **Assess** — Do referenced files still exist? Is the insight still accurate?
3. **Classify** — Keep / Update / Consolidate / Supersede / Archive
4. **Heal** — Fix orphan notes, missing links, unresolved contradictions
5. **Report** — Summary of changes and knowledge base health

## Customization

These skills are designed to be forked. Common adjustments:

- **Knowledge location** — Point to wherever you keep notes (`~/knowledge/`, `docs/knowledge/`, etc.)
- **Pipeline stages** — `/lfg` has 8 stages. Solo projects might skip REVIEW or simplify DEPLOY.
- **Scope thresholds** — Adjust what counts as Light/Standard/Heavy for your risk tolerance.
- **Source preferences** — `/research` has a source selection matrix. Reorder for your domain.
- **Note format** — `/compound` uses a specific frontmatter schema. Adapt to your preference.

## Requirements

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI, desktop app, or IDE extension
- No external dependencies — all skills use Claude's built-in tools

## FAQ

**Do I need a knowledge base to start?**
No. `/research` and `/lfg` work standalone. The knowledge base makes them better over time, but you can start without one and add it later.

**Will these slow me down?**
For trivial tasks, `/lfg` skips the pipeline entirely. The overhead scales with risk — a one-line fix gets no ceremony, a cross-system change gets full research and review. The investment pays back when you don't solve the same problem twice.

**How many notes before it's useful?**
Around 10-15 notes, `/research` starts finding relevant past work. Around 30+, patterns emerge and `/compound-refresh` starts suggesting synthesis. The system gets better the more you use it.

**Can I use these with a team?**
Yes. Install to `.claude/commands/` in your repo (committed to git). Everyone on the team gets the same skills, and `/compound` writes notes that others can find.

## License

MIT

## Acknowledgments

See [ACKNOWLEDGMENTS.md](ACKNOWLEDGMENTS.md) for the full intellectual lineage — from Socratic method to Constitutional AI to Zettelkasten.
