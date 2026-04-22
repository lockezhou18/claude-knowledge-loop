---
description: "Deep research on any topic — systematic inquiry that combines Socratic questioning, primary-source-first exploration, Karpathy-style minimal reconstruction, gstack-style live verification, and evidence synthesis. Works for engineering, business, theology, markets, or any domain requiring deep understanding."
user_invocable: true
---

# /research — Systematic Inquiry Framework

Research is not search. Search finds what's known; research discovers what's unknown and generates new understanding. This skill evolves the classical inquiry cycle with two disciplines that the 2026-04 usage audit surfaced as high-leverage: **reconstruct to understand** (Karpathy) and **verify by probing the real system** (gstack).

Invoke standalone (`/research <topic>`) or as Stage 0 of the `/lfg` pipeline.

## When to use
- Any question that deserves more than a quick lookup
- Before making a significant decision (technical, business, personal)
- When entering unfamiliar territory
- When the user says "research", "deep dive", "explore", "investigate", "what should we know about..."
- Automatically invoked by `/lfg` for Standard/Heavy scope

## Scope Assessment

Classify depth before starting:

| Depth | Time | When | Method |
|-------|------|------|--------|
| **Quick** | 2–5 min | Known domain, specific question | Steps 1, 2, 5 only (skip RECONSTRUCT + VERIFY) |
| **Standard** | 5–15 min | Some unknowns, moderate complexity | All 5 steps, one full cycle |
| **Deep** | 15–30 min | Unfamiliar territory, high stakes | All 5 steps, parallel subagents on DISCOVER |
| **Exhaustive** | 30+ min | New domain, foundational decisions | Full cycle + literature review + multiple VERIFY rounds |

Announce the depth before starting. Adjust if the user disagrees.

---

## The Inquiry Cycle

### Step 1: DEFINE — What is the real question?

Question the question itself before searching for anything.

**Socratic clarification:**
- "What exactly is being asked?" — define terms precisely
- "What am I assuming?" — surface hidden beliefs that could misdirect research
- "Is this the RIGHT question?" — sometimes the stated problem isn't the real problem

**5W1H decomposition:**
- **WHAT** is the topic/problem? (define precisely)
- **WHY** does it matter? (purpose before mechanism)
- **HOW** does it currently work? (mechanism, process)
- **WHO** are the stakeholders/experts? (authority, credibility)
- **WHEN** is this relevant? (recency, context, urgency)
- **WHERE** does it apply? (scope, boundaries)
- **WHICH** alternatives exist? (option space)

**Produce:** A refined question statement more precise than the user's original framing.

---

### Step 2: DISCOVER — Build understanding from primary sources

Work outward in layers, matching depth to scope.

**Layer 1: What we already know**
```bash
# Personal knowledge (cross-project)
grep -i "keyword" ~/.claude/knowledge/INDEX.md
grep -rl "keyword" ~/.claude/knowledge/notes/ 2>/dev/null

# Team knowledge (shared with all agents)
grep -i "keyword" ~/.openclaw/workspace/team-shared/knowledge/INDEX.md
grep -rl "keyword" ~/.openclaw/workspace/team-shared/knowledge/notes/ 2>/dev/null
```
Also: `MEMORY.md`, memory files, team-shared docs, agent-comms history.

**Layer 2: Primary sources (the thing itself)**
- Engineering → the actual code, config, architecture
- Business → the actual financials, filings, contracts
- Theology → Scripture, source texts
- Markets → actual filings, earnings, data

Apply **Aristotle's Four Causes** to build complete understanding:

| Cause | Question | What it reveals |
|-------|---------|----------------|
| **Material** | What is it made of? | The substance — data, parts, inputs |
| **Formal** | What's its structure? | The architecture — how parts connect |
| **Efficient** | What brought it into being? | The history — decisions, constraints |
| **Final** | What is its PURPOSE? | The telos — prevents building the wrong thing perfectly |

**Layer 3: External sources — matched to your research mode**

Classify mode first, then follow the ★ source:

| Mode | You're asking... | ★ Start here |
|------|-----------------|--------------|
| **DISCOVER** | "What exists?" | awesome-lists, survey papers, conference talks |
| **UNDERSTAND** | "Why? How?" | papers first, verify against GitHub implementations |
| **BUILD** | "How do I implement?" | official docs, GitHub code, engineering blogs |
| **DEBUG** | "Why is this broken?" | GitHub issues, Stack Overflow |
| **EVALUATE** | "Should we use X or Y?" | papers + Scite for contradictions + production blogs |

See `~/.claude/knowledge/authority-sources.md` for the full source hierarchy, assessment criteria, and citation tools.

**Follow the citation graph** (Standard+): backward (what did it cite?), forward (who cited it?), related work, and surprise (trace unexpected claims to their source).

**Challenge what you found** (brief Socratic elenchus):
- What CONTRADICTS this? (If all sources agree, suspect an echo chamber.)
- What would the OPPOSING VIEW say? Steelman it.
- What SURPRISED me? (Surprise = your mental model was wrong = most valuable finding.)

---

### Step 3: RECONSTRUCT — Understanding is demonstrated by building

Karpathy's "Zero to Hero" principle: if you can't rebuild it minimally, you don't understand it.

**When this step fits:**
- UNDERSTAND mode, non-trivial mechanisms
- Any topic where a minimal working version fits in < 50 lines
- Algorithms, protocols, data structures, systems with clear boundaries

**When to skip:**
- Quick-depth lookups
- Pure discovery (you haven't committed to studying a specific thing yet)
- Domains that don't admit a reduction (theology, markets, policy)

**How to reconstruct:**
1. Strip the topic to its essential mechanism. Not the optimized production version — the smallest thing that captures the IDEA.
2. Re-implement from first principles. No library magic. Names explicit.
3. Test it against one concrete example you can trace by hand.
4. **If you can't reconstruct, you haven't understood.** Go back to DISCOVER and read deeper.

**Artifact:**
```python
# Minimum viable <topic>
# Purpose: <what this demonstrates>
# What's stripped: <optimizations/edge cases I intentionally left out>

def essential_mechanism(input):
    ...  # 20 lines max
    return output

# Concrete trace:
#   input = ...   → step 1: ... → step 2: ... → output = ...
```

The artifact is NOT production code. It's a **thinking prosthetic** — the minimum that makes the idea tractable in your head.

---

### Step 4: VERIFY — Probe the real system, don't trust descriptions

`gstack`'s core discipline: *"don't trust a design without a real smoke-test."* Applied to research: before claiming X works, DEMONSTRATE X works against the actual system.

**Why this step exists:**
- Papers claim things that code doesn't implement. Docs lag behind reality. Memory rots. Reading is not verification.
- The 2026-04 usage audit flagged "Verification Before Claims" as a recurring friction — the SKILL.md that catches it must ENACT it.

**What to probe, by mode:**

| Mode | Verification method |
|------|---------------------|
| **DISCOVER** | Pull 1–2 candidate repos and run their examples. Do they actually do what's claimed? |
| **UNDERSTAND** | Run the RECONSTRUCT minimal version. Does it produce the expected output? |
| **BUILD** | Smoke-test the real integration — curl the API, run the query, hit the endpoint |
| **DEBUG** | Instrument and observe — logs, network traces, live inspection |
| **EVALUATE** | Benchmark side-by-side on a representative workload |

**For UI / front-end claims:** invoke `/gstack` to drive a real browser, take annotated screenshots, verify state.
**For APIs:** `curl -v` with real credentials; check status codes, rate-limit headers, response shape.
**For data claims:** small live query that confirms the pattern, not just a read of the schema doc.

**Skip when:** Quick-depth (lookups), or when the claim is purely historical/interpretive with no running system to probe.

**Output a verification note:**
```markdown
**Claim**: <X works like Y>
**Probe**: <exact command / URL / query you ran>
**Result**: <what actually happened>
**Matches expectation?**: yes / no — <nuance>
```

---

### Step 5: SYNTHESIZE — Convergence, contradiction, frontier, emergence

Synthesis ≠ summarizing what you found. It means combining evidence into understanding that none of the sources stated individually.

**Four lenses:**
- **Convergence**: Where do sources AGREE? → higher confidence
- **Contradiction**: Where do they DISAGREE? → investigate WHY; disagreement often hides insight
- **Gaps**: What does NO source address? → the real research frontier
- **Emergence**: What NEW understanding appears from combining sources?

**Best explanation (abductive):**
- "Given all the evidence + the reconstruction + the verification, what's the BEST explanation?" (not the first — the best)
- "What would need to be TRUE for this to work?"
- "What would FALSIFY it?" — if nothing could, the hypothesis isn't useful

**Reflect explicitly:**
- What do I now know that I didn't before?
- What do I now KNOW THAT I DON'T KNOW? (often the most valuable output)
- What question should I ask NEXT?

**Write findings to** `~/.openclaw/workspace/team-shared/knowledge/notes/` so the knowledge compounds.

---

## Research Artifact

Scale detail to depth:

```markdown
## Research: <topic>

### Question
<The REAL question, after Step 1 clarification>

### What we know (Layer 1)
<Existing knowledge, code, docs, patterns, prior work>

### What DISCOVER found
<Primary + external findings, with authority assessment per source>

### Minimum reconstruction (if applicable)
<Link to or inline the 20-line version that demonstrates the mechanism>

### What VERIFY showed
<Exact probe + result + whether it matched expectations>

### What surprised us
<Unexpected findings that challenged assumptions>

### Contradictions
<Where sources disagreed — and what the disagreement means>

### What we don't know
<Explicit unknowns, gaps, open questions>

### Hypothesis
<Best explanation / recommended direction, with reasoning>

### What would change our mind
<Evidence that would falsify the hypothesis>

### Next questions
```

## Source Cards (Standard+ depth)

For Standard and deeper research, track source cards as you go. Each source that informs a finding or decision gets a card.

**During research** (not after), as each source is fetched and analyzed, note: URL, title, key excerpts, which finding this source informs.

**At the gate** (when research is complete), save to `~/.openclaw/workspace/team-shared/knowledge/sources/{topic}-{date}/`.

**Source card format:**
```markdown
---
url: {source URL}
title: "{source title}"
accessed: {YYYY-MM-DD}
type: {primary|academic|industry|community}
---

## Key excerpts
- "{quote or data point that informed our thinking}"

## Used in
- {Which finding, decision, or hypothesis this source supported}
```

**Why:** URLs rot. Source cards preserve the substance. In 6 months, you'll see what the source *actually said* vs. what we interpreted.

---

## Gate

Research is sufficient when you can answer:
- Can you explain the topic to a **skeptic**, not just a friendly audience?
- Can you explain WHY this approach over alternatives, not just WHAT it is?
- Have you found at least one thing that **surprised** you? (If not, you didn't research deeply enough.)
- If the topic admitted RECONSTRUCT: can you rebuild the minimum version from scratch?
- If the topic admitted VERIFY: does the real system match the claim?
- Can you name what you DON'T know?

## Rules
- **DISCOVER is not synthesis.** Exploring without questioning is collecting, not researching.
- **If you didn't reconstruct it, you didn't understand it.** (When reconstruction is feasible.)
- **If you didn't probe it, you didn't verify it.** (When probing is feasible.)
- **Surprise is signal.** When something contradicts expectations, that's the most valuable finding.
- **Contradictions are gold.** When sources disagree, don't pick a winner — investigate WHY.
- **Gaps are the frontier.** What no source addresses is often most important.
- **Write it down.** Save research notes to `knowledge/notes/` so knowledge compounds.
- **Track sources.** For Standard+ depth, save source cards with key excerpts and decision links.

## History

- **v1** (March 2026) — 6-step cycle: DEFINE, EXPLORE, QUESTION, HYPOTHESIZE, SYNTHESIZE, REFLECT.
- **v2** (April 2026) — 5-step cycle: DEFINE, DISCOVER, RECONSTRUCT (new, Karpathy), VERIFY (new, gstack), SYNTHESIZE. QUESTION/HYPOTHESIZE/REFLECT folded into SYNTHESIZE. Added in response to the 2026-04 Claude Code Insights audit, which flagged "Verification Before Claims" as a recurring friction and recommended codifying the hypothesis ledger.
