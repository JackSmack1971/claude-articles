---
name: multi-agent-article-pipeline
description: >
  Orchestrates a full adversarial multi-agent article generation pipeline with eight skills:
  complexity triage, research dialectic, fact-check gate, streamed drafting with inline audit,
  red team validation, audience simulation, SEO optimization, and cross-run learning.
  Routes depth (SIMPLE/STANDARD/COMPLEX) based on scored triage. Triggers on: generate article,
  write article, research and write, multi-agent article, adversarial article, long-form content
  with research, fact-checked blog post, article pipeline, write with sources. Composes with
  article-complexity-triage, article-research-dialectic, article-fact-checker, article-qa-auditor,
  article-red-team, article-reader-simulation, article-seo-optimizer automatically. Do NOT
  trigger for short posts, social content, or tasks without research requirements.
---

# Multi-Agent Article Pipeline — v4

## Architecture

Eight skills, six personas, four human checkpoints. All inter-agent data passes through named
artifacts. No implicit context bleed between persona shifts.

**Always read before starting:** `article_style_guide.md` (project file) — governs all
prose, citation format, and conflict presentation standards.

**Load references on trigger:**
- `references/personas.md` — persona constitutions (load once)
- `references/pipeline-schemas.md` — JSON artifact schemas (reference as needed)

---

## Step 0: Complexity Triage & Learning Ingestion

**Activate:** `article-complexity-triage` skill.

1. Triage skill reads `pipeline_learnings.md` and applies cross-run calibration.
2. Scores topic across novelty, contentiousness, scope.
3. Extracts or formulates thesis; halts for confirmation if `thesis_confidence = MEDIUM`.
4. Writes `pipeline_config.json` with all routing flags.
5. Display routing confirmation to user (format defined in triage skill).

**Proceed only after user confirms thesis (if MEDIUM) or triage completes (if HIGH).**

---

## Step 1: Research Phase

Read `pipeline_config.json.pipeline.adversarial_dialectic`.

**Activate:** `article-research-dialectic` skill.

### IF adversarial_dialectic = true (STANDARD or COMPLEX):

**→ @advocate:** Phase 2a — thesis-supporting evidence. Writes `advocate_evidence.md`.

**→ @skeptic:** Phase 2b — disconfirming evidence.
- May read source URLs only from `advocate_evidence.md` (not claims) to avoid redundant retrieval.
- Writes `skeptic_evidence.md`.

**→ @synthesizer:** Phase 3 — conflict mapping.
- Writes `research_context.md`, `article_spec.md`, `conflict_register.md`.
- **KC-3 Check:** If any single source > 40% of total claims → HALT.
- **KC-6 Check:** If > 50% of research vectors classified `[INSUFFICIENT]` → HALT.

### IF adversarial_dialectic = false (SIMPLE):

**→ @synthesizer:** Unified research pass.
- Writes `research_context.md`, `article_spec.md`. No `conflict_register.md`.

---

## Step 1.5: Fact-Check Gate

Read `pipeline_config.json.pipeline.fact_check`.

**IF fact_check = true:**

**Activate:** `article-fact-checker` skill.

**→ @fact-checker:**
1. Extracts HIGH/MEDIUM confidence claims from `research_context.md`.
2. Verifies each via web search per verification protocol.
3. Writes `fact_check_report.md` and `dispute_register.md` (if disputes found).
4. If > 3 DISPUTED claims: surface `dispute_register.md` to user with inline note.

**@engineer MUST read `fact_check_report.md` before Step 3. VERIFIED-UPDATED values
supersede `research_context.md` originals.**

---

## Step 2: Approval Gate — ⛔ HALT

Present to user:
- `article_spec.md` — section hierarchy, token budget, target audience.
- `conflict_register.md` — (STANDARD/COMPLEX) conflicts requiring decision.
- `dispute_register.md` — (if populated) verified disputes from fact-check gate.

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
⛔ APPROVAL GATE — Pipeline Paused
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Depth: [SIMPLE|STANDARD|COMPLEX]
Sections: [N] | Conflicts: [N] | Fact Disputes: [N]
Token Budget: [N] tokens

Conflict handling required for each [CONFLICTING] item:
  (a) Present both positions neutrally.
  (b) Author takes position — specify: advocate or skeptic.
  (c) Flag as unresolved ("remains contested").

Actions:
  ✅ "approved" — conflicts default to (a) if not individually specified.
  ✏️ "revise: <feedback>" — amend spec. Max 3 cycles.
  ❌ "abort" — terminate pipeline.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

On `approved`: write `conflict_decisions.json` (schema in `references/pipeline-schemas.md`).
Record each conflict with: id, handling decision (neutral/author_position/unresolved), position if applicable.
**This JSON is the authoritative source for @engineer — not the free-text conversation.**

KC-2 Check: third non-substantive consecutive revision → HALT.

---

## Step 3: Streamed Drafting + Inline Audit

**@engineer** reads: `article_spec.md`, `research_context.md`, `fact_check_report.md`,
`conflict_decisions.json`, `article_style_guide.md`.

**Activate:** `article-qa-auditor` skill (inline mode).

Loop per section:
```
@engineer writes section → yields
@qa audits section (inline mode) → verdict to audit_log.md

SECTION PASS or PASS WITH NOTES → advance
SECTION BLOCKED →
  REVISION_COUNT += 1
  IF REVISION_COUNT ≥ 2 → KC-4: HALT
  ELSE → @engineer revises → re-audit
```

**KC-5 Check (mid-draft):** After each section, check cumulative telemetry. If utilization
> 85% and sections remain → HALT. Do not wait for KC-1 (which only fires pre-draft).

After all sections pass:
- **@qa final holistic mode:** cross-section coherence, narrative arc, source diversity
  (>30% single source = CRITICAL finding), thesis-conclusion alignment.
- Writes `audit_report.md`.
- If holistic verdict = FAIL: one revision cycle on CRITICAL/MAJOR sections only, then re-audit.

---

## Step 4: Red Team (COMPLEX only)

Read `pipeline_config.json.pipeline.red_team`.

**IF red_team = true:**

**Activate:** `article-red-team` skill.

**→ @adversary:** Receives ONLY thesis statement + conclusion section (no full draft — anchoring prevention).
Writes `red_team_report.md`.

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Red Team Report — Threat Level: [LOW|MEDIUM|HIGH]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  ✅ "accept" — proceed as-is.
  ✏️ "address" — @engineer adds/modifies to respond.
  📝 "acknowledge" — add limitations paragraph only.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

If "address": @engineer revises → @qa re-audits affected sections only.

---

## Step 5: Reader Simulation

**Activate:** `article-reader-simulation` skill.

**→ @reader:** Reads `article_draft.md` from declared target audience perspective.
Writes `reader_questions.md`.

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Reader Simulation — Rating: [ACCESSIBLE|MOSTLY ACCESSIBLE|INACCESSIBLE]
Comprehension Gaps: [N]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  ✅ "ship" — accept as-is.
  ✏️ "polish" — address top 3 priority gaps.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

If "polish": @engineer addresses gaps → @qa inline-audits modified sections.

---

## Step 6: Delivery

Present final `article_draft.md`.

---

## Step 6.5: SEO Pass

Read `pipeline_config.json.pipeline.seo_pass`.

**IF seo_pass = true:**

**Activate:** `article-seo-optimizer` skill.

Generates `seo_package.md` with title variants, meta description, slug, keyword analysis,
JSON-LD blocks, and on-page checklist. Present to user alongside final article.

---

## Step 7: Cross-Run Learning

**→ @qa** appends structured entry to `pipeline_learnings.md`:

```markdown
## Run: [ISO Date] — [Topic Title]
### Depth: [SIMPLE|STANDARD|COMPLEX] | Composite Score: [N.N]
### Metrics
- Sections: [N] | Audit blocks: [N] | Revision cycles: [N]
- Kill switch triggers: [N] | Conditions: [list or none]
- Conflicts: [N] | Disputes resolved: [N]
- Red team threat: [N/A|LOW|MEDIUM|HIGH]
- Reader accessibility: [rating]
- SEO keyword density: [N%]
### Lessons
- [What worked]
- [What to change]
### Calibration
[CALIBRATION] novelty_bias: [±N if applicable] | budget_kc1_topics: [topic keywords if KC-1 fired]
```

The `[CALIBRATION]` tag is required. If no calibration needed, write: `[CALIBRATION] none`.

---
> References: `references/personas.md`, `references/pipeline-schemas.md`
