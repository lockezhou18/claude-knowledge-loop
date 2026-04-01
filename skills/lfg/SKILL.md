---
description: "LFG — Autonomous Engineering Pipeline. Full engineering pipeline with enforced gates from research to deployment to knowledge capture."
user_invocable: true
---

# LFG — Autonomous Engineering Pipeline

Full engineering pipeline with enforced gates. Each stage MUST produce a gate artifact before proceeding. No skipping. No exceptions.

```
RESEARCH ──► PROPOSE ──► PLAN ──► BUILD ────────► DEPLOY → VERIFY → COMPOUND
  discuss     discuss    discuss   [PR loop]        │                  │
  iterate     iterate    iterate   per milestone    ▼                  ▼
  ───┐        ───┐       ───┐     branch→implement ship            knowledge/
     │agree      │agree     │agree →test→CI→review  tag +           memory/
     ▼           ▼          ▼      →merge ×N        deploy          session card
  knowledge/  specs/     specs                                     aggregation
  notes/      HLD        tasks
  sources/    memory/    build-guide
              context
```

**Save at the gate.** Each stage involves discussion and iteration. Save artifacts when we **agree and move forward** — not mid-discussion. COMPOUND captures what earlier gates didn't.

| Stage | What gets saved | Where |
|-------|----------------|-------|
| RESEARCH | Research artifact + source cards | knowledge notes + sources |
| PROPOSE | HLD (self-contained with research appendices) + session context | specs + memory |
| PLAN | Specs + tasks + build-guide with engineering principles | project specs |
| BUILD | Code via PRs (each tested + reviewed + merged independently) | GitHub repo |
| DEPLOY | Release tag + deploy | GitHub + target machines |
| VERIFY | E2E test evidence | PR or specs |
| COMPOUND | Surprises, meta-patterns, session card, aggregation check | knowledge + memory |

## When to use
- Starting any non-trivial task (more than a config tweak or typo fix)
- The user says "lfg", "let's go", "ship it", or "build this"
- When you want to ensure a task goes from idea to shipped without cutting corners

## Scope Assessment

Before running the pipeline, classify the task:

| Scope | Criteria | Pipeline |
|-------|----------|----------|
| **Trivial** | One file, < 10 lines, no risk | Skip pipeline — just do it, verify, done |
| **Light** | 1-3 files, low risk, well-understood | Fast pipeline: PLAN → BUILD → VERIFY |
| **Standard** | Multiple files, moderate risk, or touches infra | Full pipeline: all 8 stages |
| **Heavy** | Cross-system, high risk, or needs stakeholder approval | Full pipeline + deep research + threat model |

Announce the scope classification before starting. If the user disagrees, adjust.

---

## Stage 0: RESEARCH

**Run `/research` on the task.** The full inquiry framework (Socratic questioning, Aristotelian analysis, abductive reasoning) lives in the `/research` skill.

**Scope mapping for /lfg:**
| LFG Scope | /research Depth | What happens |
|-----------|----------------|-------------|
| **Trivial** | Skip | Task is obvious |
| **Light** | Skip | Well-understood territory |
| **Standard** | Quick or Standard | Layers 1-2, one round of Socratic questioning |
| **Heavy** | Deep | All layers, parallel subagents, full inquiry cycle |

**Gate:** `/research` produces a research artifact with: Question, What we know, What surprised us, Contradictions, Hypothesis, What would change our mind. The test: can you explain not just WHAT to do, but WHY this approach over alternatives?

---

## Stage 1: PROPOSE

**Purpose:** Articulate what and why before touching any code.

**Produce:**
```markdown
## Proposal: <task name>
**Why:** <1 paragraph — what problem this solves and who benefits>
**Acceptance criteria:** <bullet list — how we know it's done>
**Risk:** Low / Medium / High
**Scope:** Light / Standard / Heavy
```

**Heavy scope adds:** Threat model (attack surfaces, data at risk, failure scenarios)

**Gate:** Proposal exists and user has acknowledged it. If risk is High, pause for explicit approval.

---

## Stage 2: PLAN

**Purpose:** Identify dependencies, sequence, and rollback strategy before building.

**Produce:**
- List of files to change (read them first)
- Dependencies and prerequisites — confirm each is met
- For config/infra changes: backup commands and rollback plan
- Execution order (which steps are sequential, which can parallel)

**Gate:** All prerequisites confirmed as met. Rollback plan documented for any destructive or hard-to-reverse changes.

**For Standard+ scope**, produce structured specs:
1. Generate artifacts: proposal, design, specs, tasks
2. **Apply engineering principles:**
   - Specs MUST include failure modes
   - Specs MUST include observability requirements
   - Design MUST address state management
   - Tasks MUST be ordered for incremental verification
3. Generate a build guide — project-specific engineering opinions that map principles to this build

---

## Stage 3: BUILD

**Purpose:** Implement the plan.

**Rules:**
- Follow the plan. If you discover the plan is wrong, update the plan first, don't silently diverge.
- Syntax check after every file change
- Commit at logical boundaries, not at the end
- If using sub-agents for parallel work: tasks must touch different files

**PR workflow:**
1. Group tasks into PR milestones — each PR independently mergeable and testable
2. For each PR milestone:
   - Branch from main
   - Implement tasks
   - Self-test (syntax, unit tests)
   - Push + open PR with CI
   - Review (self or cross-review depending on risk)
   - Merge to main
3. All PRs merged → BUILD complete

**PR grouping is scope-dependent:**

| Scope | PRs | Strategy |
|-------|-----|----------|
| Light | 1 | All tasks in one PR |
| Standard | 2-3 | By logical layer |
| Heavy | 3-5 | By milestone |

**Rule:** Each PR must be independently testable. If PR #2 breaks without PR #3, combine them.

**Gate:** Self-test passes.
- Syntax check: all modified files compile/parse
- Dry run: `DRY_RUN=1` or `--run-once` for services
- Functional test: the actual behavior matches acceptance criteria
- Capture test output as evidence

---

## Stage 4: REVIEW

**Purpose:** Nobody grades their own homework.

Individual PR reviews happen during BUILD (each PR's inner loop includes review). This stage is the **final integration review** on the complete, merged codebase — ensuring everything works together, not just individually.

Use `/cross-review` — two-stage spec then quality review on the full codebase.

**Gate:** Reviewer posts `APPROVED` with evidence they read actual code (not just the summary). If `CHANGES_REQUESTED`, return to BUILD, fix, re-review.

**Max review rounds:** 2. If still not approved after 2 rounds, escalate to a stakeholder.

---

## Stage 5: DEPLOY

**Purpose:** Go live with a safety net.

**Pre-deploy checklist:**
- [ ] Review approved (Stage 4 gate passed)
- [ ] Backup exists for any modified configs
- [ ] Rollback command documented and tested
- [ ] Monitoring command identified (what to watch after deploy)

**Deploy, then immediately proceed to Stage 6.**

---

## Stage 6: VERIFY

**Purpose:** Confirm it actually works in production, not just in theory.

**Post-deploy monitoring:**
- For services: watch logs for 2 minutes for errors/warnings
- For scripts: confirm output matches expected behavior
- For config changes: confirm the service restarted cleanly and is responding

**Gate:** Fresh verification evidence captured. No errors, warnings, or unexpected behavior.

**If verification fails:** Execute rollback plan. Do NOT attempt a forward fix under pressure. Rollback first, diagnose second.

---

## Stage 7: COMPOUND

**Purpose:** Capture what was learned so future work is easier.

Run `/compound` to:
- Capture knowledge notes for non-obvious learnings
- Update project docs if a new pattern emerged
- Add follow-up tasks if needed

---

## Anti-Patterns (never do these)

| Anti-pattern | Why it fails | What to do instead |
|-------------|-------------|-------------------|
| Skip RESEARCH for "I already know" | Plans built on assumptions, not facts | 5 min of research prevents hours of rework |
| Skip PROPOSE for "quick fixes" | Quick fixes cause unexpected breakage | Even 2 sentences of "why" catches bad ideas |
| Skip PLAN for "obvious changes" | Missing rollback plan → locked out | 30 seconds on rollback saves hours of recovery |
| Deploy without REVIEW | Nobody grades their own homework | At minimum, run `/cross-review` on yourself |
| Forward-fix under pressure | Compounds the problem | Rollback first, diagnose second |
| Skip VERIFY because "it compiled" | Compiling ≠ working | Run the actual verification command |
| Skip COMPOUND because "it's done" | Knowledge doesn't compound | 2 minutes now saves 2 hours later |

---

## Fast Pipeline (Light scope)

For Light scope tasks, compress to 3 stages:

1. **PLAN** — 2-3 sentences: what to change, rollback plan, test command
2. **BUILD** — Implement + self-test (capture evidence)
3. **VERIFY** — Run verification, confirm it works

Skip RESEARCH (task is well-understood), skip PROPOSE (intent is obvious), skip formal REVIEW (self-review sufficient for low-risk), skip DEPLOY (no production deployment). Still run COMPOUND if something was learned.

## Full Learning Loop

The pipeline is not linear — it's a cycle:

```
    RESEARCH ─── PROPOSE ─── PLAN ─── BUILD ─── REVIEW ─── DEPLOY ─── VERIFY ─── COMPOUND
       │save       │save      │save     │save      │          │          │          │save
       ▼           ▼          ▼         ▼         ▼          ▼          ▼          ▼
    knowledge/  specs/     specs     GitHub    (integration  ship    E2E test   knowledge/
    notes/      HLD        tasks     PRs ×N     review on    tag +   evidence   memory/
    sources/    memory/    build-    (each has   merged      deploy             session card
                context    guide     own test    main)                          aggregation
                journey              +review                                   check
                                     +merge)
       │                                                                          │
       │                          LEARNING FEEDS BACK                             │
       └──────────────────────────────────────────────────────────────────────────┘
```

Every completed task makes the next task's RESEARCH phase faster and better-informed. This is how work compounds over time — not through individual heroics, but through accumulated institutional knowledge that's actually retrievable.
