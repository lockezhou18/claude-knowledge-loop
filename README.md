# Claude Knowledge Loop

Stop solving the same problem twice. Four Claude Code skills that make your knowledge compound over time.

## Install

```bash
git clone https://github.com/lockezhou18/claude-knowledge-loop.git
cp -r claude-knowledge-loop/skills/* ~/.claude/commands/
```

Then type `/research <topic>` or `/lfg` in any Claude Code session.

## Skills

| Skill | Command | When to use |
|-------|---------|-------------|
| **Research** | `/research <topic>` | Before any non-trivial decision. Explores a question using Socratic inquiry, finds contradictions, surfaces what you don't know. |
| **LFG** | `/lfg` | When building anything beyond a one-line fix. Gated pipeline from research to deployment. Calls `/research` and `/compound` automatically. |
| **Compound** | `/compound` | After completing work. Captures non-obvious learnings as linked notes. You choose what to keep. |
| **Compound Refresh** | `/compound-refresh` | Monthly, or after major changes. Audits your notes against current code, heals broken links, archives stale insights. |

## Example: What `/research` produces

```
> /research should we use NATS or Redis for agent messaging?
```

```markdown
## Research: Agent Messaging Transport

### Question
Not "NATS vs Redis" but "what transport properties does a multi-agent
system actually need, and which tool best fits those properties?"

### What surprised us
Redis Streams can do pub/sub, but its consumer group model requires
careful partition management that NATS handles natively. Most "Redis
for messaging" blog posts benchmark happy-path throughput and ignore
consumer failure recovery.

### Contradictions
Paper X claims NATS has higher latency than Redis. But the benchmark
used single-node Redis vs clustered NATS — not a fair comparison.
NATS single-node benchmarks show 12M msg/sec vs Redis's 1M msg/sec.

### What we don't know
How either behaves under network partitions with our specific
message sizes (8-32KB agent payloads). Need to benchmark ourselves.

### Hypothesis
NATS fits better — native pub/sub, no partition management, built-in
JetStream for persistence. Redis would require building what NATS
gives out of the box.

### What would change our mind
If our message patterns turn out to be primarily point-to-point (not
pub/sub), Redis Streams' simpler operational model might win.
```

The key output isn't the answer — it's the "What surprised us" and "What we don't know" sections. Those push your understanding beyond what a simple search would find.

## Example: What `/compound` captures

After a debugging session:

```
> /compound

/compound found:
  - Surprising root cause (DNS cache, not API timeout) → save knowledge note? [y/n]
  - Pattern: 3rd time we've seen DNS issues after deploy → synthesize pattern? [y/n]
  - 2 backlog items discussed → save to backlog? [y/n]
```

Each note is atomic, tagged, and linked to related notes. After 30+ notes, patterns emerge automatically.

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

`/lfg` is the all-in-one: it runs `/research` at stage 0 and `/compound` at stage 7 automatically. Use `/research` or `/compound` standalone for inquiry or learning capture outside a build task. Run `/compound-refresh` on a regular schedule.

## Why This Approach

Inspired by [**compound engineering**](https://every.to/guides/compound-engineering) (Kieran Klaassen / Every Inc.) — the idea that each unit of engineering work should make subsequent units *easier*, not harder. Just as compound interest grows wealth, compound engineering grows capability. See also their [compound-engineering-plugin](https://github.com/EveryInc/compound-engineering-plugin) and [compound-knowledge-plugin](https://github.com/EveryInc/compound-knowledge-plugin).

Most tools operate within your current knowledge boundary: you ask, you get an answer. This loop pushes **beyond** that boundary. `/research` uses Socratic questioning to surface hidden assumptions and find contradictions. `/compound` captures surprises — moments where reality didn't match your mental model. Over time, the system maps not just what you know, but what you *know you don't know*.

The inquiry cycle works for any domain — engineering, business, research, markets — but the pipeline and examples lean toward software engineering.

<details>
<summary><b>Setting up a knowledge base (optional)</b></summary>

The loop works best with a place to store notes:

```bash
mkdir -p ~/knowledge/{notes,sources}
touch ~/knowledge/INDEX.md
```

Add to your project's `CLAUDE.md`:

```markdown
Knowledge base location: ~/knowledge/
- Notes: ~/knowledge/notes/
- Source cards: ~/knowledge/sources/
- Index: ~/knowledge/INDEX.md
```

Start empty — it fills up naturally as you work. Around 10-15 notes, `/research` starts finding relevant past work. Around 30+, `/compound-refresh` starts suggesting pattern synthesis.

</details>

<details>
<summary><b>How /lfg scales to scope</b></summary>

| Scope | Pipeline | When |
|-------|----------|------|
| Trivial | Skip — just do it | One file, < 10 lines |
| Light | PLAN → BUILD → VERIFY | 1-3 files, low risk |
| Standard | All 8 stages | Multiple files, moderate risk |
| Heavy | All 8 + deep research + threat model | Cross-system, high risk |

Full pipeline: RESEARCH → PROPOSE → PLAN → BUILD → REVIEW → DEPLOY → VERIFY → COMPOUND. Each stage produces evidence before the next begins.

</details>

<details>
<summary><b>Customization</b></summary>

These skills are designed to be forked:

- **Knowledge location** — Point to wherever you keep notes
- **Pipeline stages** — Solo projects might skip REVIEW or simplify DEPLOY
- **Scope thresholds** — Adjust what counts as Light/Standard/Heavy
- **Source matrix** — `/research` has a source selection matrix by research mode. Reorder for your domain
- **Note format** — `/compound` uses a frontmatter schema. Adapt to your preference

</details>

## FAQ

**Do I need a knowledge base to start?**
No. `/research` and `/lfg` work standalone. Add a knowledge base later when you want notes to persist.

**Will these slow me down?**
`/lfg` skips the pipeline for trivial tasks. The overhead scales with risk. The investment pays back when you don't solve the same problem twice.

**Can I use these with a team?**
Yes. Install to `.claude/commands/` in your repo. Everyone gets the same skills, and `/compound` writes notes others can find.

## License

MIT — see [ACKNOWLEDGMENTS.md](ACKNOWLEDGMENTS.md) for intellectual lineage (Socratic method, Zettelkasten, Constitutional AI, and more).
