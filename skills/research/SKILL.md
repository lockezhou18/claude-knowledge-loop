---
description: "Deep research on any topic — systematic inquiry using Socratic questioning, Aristotelian analysis, and abductive reasoning. Works for engineering, business, theology, markets, or any domain requiring deep understanding."
user_invocable: true
---

# /research — Systematic Inquiry Framework

Research is not search. Search finds what's known. Research discovers what's unknown and generates new understanding.

This skill can be invoked standalone (`/research <topic>`) or as Stage 0 of the `/pipeline` skill.

## When to use
- Any question that deserves more than a quick lookup
- Before making a significant decision (technical, business, personal)
- When entering unfamiliar territory
- When the user says "research", "deep dive", "explore", "investigate", "what should we know about..."
- Automatically invoked by `/pipeline` for Standard/Heavy/Research-heavy scope tasks

## Scope Assessment

Before starting, classify depth:

| Depth | Time | When | Method |
|-------|------|------|--------|
| **Quick** | 2-5 min | Known domain, specific question | Layers 1-2, one round of questioning |
| **Standard** | 5-15 min | Some unknowns, moderate complexity | All layers, one full inquiry cycle |
| **Deep** | 15-30 min | Unfamiliar territory, high stakes | All layers, parallel subagents, recursive questioning |
| **Exhaustive** | 30+ min | New domain, foundational decisions | Full literature review, multiple cycles, all 6 types of Socratic questions |

Announce the depth before starting. If the user disagrees, adjust.

---

## The Inquiry Cycle

### Step 1: DEFINE — What is the real question?

Before searching for ANYTHING, question the question itself.

**Socratic clarification:**
- "What exactly is being asked?" — Define terms precisely. Vague questions produce vague research.
- "What am I assuming?" — Surface hidden beliefs that might send research in the wrong direction.
- "Is this the RIGHT question?" — Sometimes the stated problem isn't the real problem.

**5W1H decomposition:**
- **WHAT** is the topic/problem? (Define precisely)
- **WHY** does it matter? (Purpose before mechanism)
- **HOW** does it currently work? (Mechanism, process)
- **WHO** are the stakeholders/experts? (Authority, credibility)
- **WHEN** is this relevant? (Recency, context, urgency)
- **WHERE** does it apply? (Scope, boundaries)
- **WHICH** alternatives exist? (Option space)

**Produce:** A refined question statement that's more precise than what the user originally asked.

---

### Step 2: EXPLORE — Build understanding across sources

Work through layers, matching depth to scope:

**Layer 1: What we already know**
Check your local knowledge base, memory files, team docs, and prior research notes for existing coverage of this topic.

**Layer 2: Primary sources (the thing itself)**
- For engineering: read the actual code, config, architecture
- For business: read the actual financials, market data, contracts
- For theology: read the actual Scripture, source texts
- For markets: read the actual filings, earnings, data

Apply **Aristotle's Four Causes** to build complete understanding:
| Cause | Question | What it reveals |
|-------|---------|----------------|
| **Material** | What is it made of? What are its components? | The substance — data, parts, inputs |
| **Formal** | What is its structure/design/pattern? | The architecture — how parts connect |
| **Efficient** | What brought it into being? What causes change? | The history — decisions, constraints, forces |
| **Final** | What is its PURPOSE? Why does it exist? | The telos — prevents building the wrong thing perfectly |

**Layer 3: External sources — context-dependent, not a fixed hierarchy**

There is no universal "best" source. The best source depends on your **research mode**. Classify your intent first, then follow the matrix.

**Step 1: Identify your research mode:**

| Mode | You're asking... | Signal |
|------|-----------------|--------|
| **DISCOVER** | "What exists? What's the landscape?" | You don't know what you don't know |
| **UNDERSTAND** | "Why? How does this work? What's the theory?" | You found something, now need depth |
| **BUILD** | "How do I implement this?" | You've decided what, need the how |
| **DEBUG** | "Why is this broken? How do I fix it?" | Something failed, need answers fast |
| **EVALUATE** | "Should we use X or Y? Is this a good idea?" | Making a decision, need evidence |

**Step 2: Start with the ★ sources for your mode:**

```
                    DISCOVER        UNDERSTAND       BUILD          DEBUG          EVALUATE
                    (landscape)     (theory/why)     (implement)    (fix now)      (decide)
───────────────────────────────────────────────────────────────────────────────────────────
Papers/Journals     Survey papers   ★ PRIMARY        Edge cases     Rare           ★ Evidence
                    for landscape   Deep theory       Root cause                    for/against

GitHub              ★ awesome-*     Verify claims    ★ PRIMARY      ★ Issues       Stars/adoption
                    Star counts     Read actual code  Working code   Real gotchas   as signal

Official Docs       Skim for scope  Reference        ★ PRIMARY      ★ PRIMARY      Canonical
                                    details          Canonical       Error codes    specs

Eng. Blogs          Landscape       Production       Patterns       War stories    ★ Production
                    overview        experience       at scale       Incident rpts  experience

Conference Talks    ★ Rapid         Practitioner     Rare           Rare           Talks comparing
                    landscape scan  synthesis                                      alternatives

Community/SO        Breadth         Rare             Quick answers  ★ Quick fix    Opinions
                                                                                   (verify!)

Semantic Scholar    ★ Citation      ★ Citation       Rare           Rare           ★ Consensus
/Research Rabbit    graph explore   graph deep                                     across papers

Scite/PapersFlow    Rare            ★ Find who       Rare           Rare           ★ Who disagrees?
                                    DISAGREES                                      Counter-evidence
```

★ = Start here for this mode. Work outward as needed.

**Step 3: Modes shift within a single session.** A research session often flows:
1. DISCOVER (what exists?) → 2. UNDERSTAND (why does this work?) → 3. EVALUATE (should we use it?) → 4. BUILD (how do we implement?)

Recognize the shift and change your source strategy accordingly.

**Assessing any source (regardless of mode):**
- Who wrote it? (Engineers at scale > independent bloggers > anonymous)
- When? (Recent < 2 years for fast-moving tech; timeless for fundamentals)
- Production experience? ("We run this at 10M QPS" > "I tried this in a weekend")
- Cross-reference 2-3 sources — never trust a single source for important decisions
- When paper claims X but code implements Y, trust the code

**Additional modifiers:**

Your **knowledge state** changes what's useful:
- Novice → surveys, awesome-lists, overviews FIRST (don't drown in primary sources)
- Familiar → go straight to specific papers/implementations
- Expert → primary sources, raw data, cutting-edge preprints

Your **time horizon** changes what matters:
- Fix it now → Stack Overflow, GitHub issues, docs (fastest path)
- Build for next month → Docs, GitHub implementations, blogs (practical)
- Architect for years → Papers, books, first principles (foundational)

Your **confidence requirement** changes how many sources you need:
- Exploring possibilities → 1-2 sources, breadth over rigor
- Making a recommendation → 3+ sources, need evidence
- Irreversible decision → cross-validated across papers + implementations + production experience

**Follow the citation graph (Standard+ scope):**
When you find a key source, trace the web:
1. **Backward references** — What did this source cite? The 3-5 most-cited are usually foundational.
2. **Forward citations** — Who cited this? Find what built on it, challenged it, or superseded it.
3. **Related work** — Papers explicitly map the landscape. Read "Related Work" sections.
4. **Follow surprise** — If a source says something unexpected, trace WHERE they got that claim.

**Citation-aware authority scoring:**
- 1000+ citations = foundational (shaped the field)
- 100-999 = influential (widely validated)
- 10-99 = emerging (promising, less validated)
- <10 + recent = cutting-edge (highest novelty, may not hold up)
- <10 + old = likely superseded (check for newer work)

**Search tools:**
- **Semantic Scholar API / Research Rabbit** — citation graph, influence scores, iterative chaining
- **Google Scholar / Connected Papers** — forward citations, visual relationship maps
- **Scite / PapersFlow** — contradiction detection (who DISAGREES, not just who cites)
- **GitHub** — `gh search repos "TOPIC" --sort stars`, awesome-lists, issues, dependents
- **Elicit / Consensus** — evidence-backed answers from 100M+ papers

**For Deep/Exhaustive scope:** Launch parallel research agents, each in a different mode or angle. One traces citation graphs, another searches GitHub implementations, a third explores opposing viewpoints.

---

### Step 3: QUESTION — Challenge what you found (Socratic elenchus)

This is where research becomes more than search. After exploring, actively ATTACK your own findings:

**The 6 types of Socratic questions:**

| Type | Question Pattern | Purpose |
|------|-----------------|---------|
| **Clarification** | "What exactly does this mean?" | Prevent false understanding |
| **Probing Assumptions** | "What is this source assuming?" | Expose hidden premises |
| **Reasons & Evidence** | "What evidence actually supports this?" | Demand proof, not claims |
| **Viewpoints** | "What would an opponent say?" | Seek the strongest counterargument |
| **Implications** | "If this is true, what follows?" | Trace consequences |
| **Meta-questions** | "Why are we asking this question?" | Question the framing |

**Specific challenges to apply:**
- "What CONTRADICTS what I found?" — If all sources agree, you may be in an echo chamber.
- "What would the OPPOSING VIEW say?" — Steelman the counterargument.
- "What SURPRISED me?" — Surprise = your mental model was wrong = most valuable finding.
- "What do I NOT understand yet?" — Honest confusion > false confidence.

**Contradiction-finding tools (use these to challenge your findings):**
- **Scite** (`scite.ai`): Shows whether citations SUPPORT or CONTRADICT — not just "cited by N" but "N agree, M disagree"
- **PapersFlow** (`papersflow.ai`): Counter-evidence detection — actively finds papers that disagree with your thesis
- **Google Scholar**: Search for `"CLAIM" critique OR rebuttal OR "contrary to"` to find opposing work

---

### Step 4: HYPOTHESIZE — Generate explanations (abductive reasoning)

Don't just report findings. Make the creative leap:

- "Given the evidence, what's the BEST explanation?" (Not the first — the best.)
- "What would need to be TRUE for this approach to work?"
- "What would FALSIFY this?" (If nothing could prove it wrong, it's not useful.)
- "What NEW insight emerges that no single source states?"

**Abduction is triggered by surprise.** When something doesn't match expectations, ask: "What would explain this?" The explanation IS the insight.

---

### Step 5: SYNTHESIZE — Combine into understanding

Synthesis ≠ summarizing what you found. Synthesis means:

- **Convergence**: Where do sources AGREE? (Higher confidence)
- **Contradiction**: Where do they DISAGREE? (Investigate deeper — this is where real insight hides)
- **Gaps**: What does NO source address? (The real research frontier)
- **Emergence**: What NEW understanding appears from combining sources that none states individually?

---

### Step 6: REFLECT — Know what you now know and don't know

- "What do I now know that I didn't before?"
- "What do I now KNOW THAT I DON'T KNOW?" (The most valuable output.)
- "What question should I ask NEXT?" (Research generates better questions, not just answers.)
- Write findings to your knowledge base so future research benefits.

---

## Research Artifact

Produce this artifact (scale detail to depth):

```markdown
## Research: <topic>

### Question
<The REAL question, after Step 1 clarification>

### What we know
<Existing knowledge, code, docs, patterns, prior work>

### What the research found
<Source findings, with authority assessment per source>

### What surprised us
<Unexpected findings that challenge assumptions>

### Contradictions
<Where sources disagree — and what the disagreement means>

### What we don't know
<Explicit unknowns, gaps, open questions>

### Hypothesis
<Best explanation / recommended direction, with reasoning>

### What would change our mind
<Evidence that would falsify the hypothesis>

### Next questions
<What should be researched next, if going deeper>
```

## Source Cards (Standard+ depth)

For Standard and deeper research, track source cards as you go. Each source that informs a finding or decision gets a card.

**During research** (not after), as each source is fetched and analyzed, note:
- URL and title
- Key excerpts — the specific quotes/data that matter
- Which finding or decision this source informs

**At the gate** (when user agrees research is complete), save source cards to your knowledge base.

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
- "{another key passage}"

## Used in
- {Which finding, decision, or hypothesis this source supported}
```

**Why source cards matter:**
- URLs rot — source cards preserve the substance
- Re-evaluation — in 6 months, see what the source *actually said* vs what we interpreted
- Audit trail — decision → source card → exact quote

**When to skip:** Quick depth research (2-5 min, 1-2 sources). Don't create overhead for simple lookups.

---

## Gate

The research is sufficient when you can answer:
- Can you explain the topic to a skeptic? (Not just a friendly audience)
- Can you explain WHY this approach over alternatives? (Not just what)
- Have you found at least one thing that SURPRISED you? (If not, you didn't research deeply enough)
- Can you name what you DON'T know? (If not, you're overconfident)

## Rules
- **Never skip Step 3 (QUESTION).** Exploring without questioning is just collecting, not researching.
- **Surprise is signal.** When something contradicts expectations, that's the most valuable finding — don't dismiss it.
- **Contradictions are gold.** When sources disagree, don't pick a winner — investigate WHY they disagree.
- **Gaps are the frontier.** What no source addresses is often the most important thing to figure out.
- **Depth through recursion.** For any answer, you can ask 5W1H again. The depth of research = the depth of recursive questioning.
- **Write it down.** Save research notes so the knowledge compounds.
- **Track sources.** For Standard+ depth, save source cards with key excerpts and decision links.
