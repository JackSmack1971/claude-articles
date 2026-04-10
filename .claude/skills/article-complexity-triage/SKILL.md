---
name: article-complexity-triage
description: >
  Scores an article topic brief across novelty, contentiousness, and scope dimensions
  to route the pipeline to SIMPLE, STANDARD, or COMPLEX depth. Reads pipeline_learnings.md
  to apply cross-run calibration before scoring. Writes pipeline_config.json with all
  routing flags, token budget, and thesis confidence level. Triggers on: complexity triage,
  pipeline routing, topic scoring, depth classification, article pipeline start. Required
  first step before any article generation pipeline run. Do NOT use standalone for research,
  writing, auditing, or SEO tasks.
---

# Article Complexity Triage

## Purpose

Routes each pipeline run to the minimum viable depth that produces a credible, defensible
article. Over-routing wastes tokens; under-routing produces thin output. Scoring is
deterministic — every field has an explicit rubric with numeric thresholds.

## Step 0: Pre-Triage Learning Ingestion

Before scoring, read `pipeline_learnings.md` (project file):
- Scan for any LESSONS entries flagged `[CALIBRATION]` — these override default thresholds.
- If no learnings exist or file is empty, proceed with defaults below.
- Note any topics similar to the current brief that ran into KC-1 (token overrun) — bump
  budget ceiling by 8k for this run.

## Step 1: Thesis Extraction

From the topic brief, extract or formulate:

```markdown
Proposed Thesis: [one declarative sentence asserting a specific position]
Confidence: [HIGH | MEDIUM]
```

- **HIGH:** Topic is factual, bounded, and well-attested. Concurrent research allowed.
- **MEDIUM:** Topic is contested, rapidly evolving, or requires definitional setup.
  HALT before research. Present thesis to user for confirmation before proceeding.

## Step 2: Dimension Scoring

Score each dimension 1–5 using the rubric in `references/triage-rubric.md`.

| Dimension | Score | Criteria Summary |
|-----------|-------|-----------------|
| Novelty | 1–5 | How much of the topic postdates 2023 or is domain-niche? |
| Contentiousness | 1–5 | How much do credible sources disagree on facts or interpretation? |
| Scope | 1–5 | How many distinct sub-topics must be covered for adequate treatment? |

**Composite score** = (Novelty × 0.35) + (Contentiousness × 0.40) + (Scope × 0.25)

## Step 3: Routing Decision

| Composite Score | Depth | Adversarial Dialectic | Red Team | Fact Check | SEO Pass | Token Budget |
|-----------------|-------|-----------------------|----------|------------|----------|--------------|
| 1.0 – 2.4 | SIMPLE | false | false | false | true | 24,000 |
| 2.5 – 3.7 | STANDARD | true | false | true | true | 32,000 |
| 3.8 – 5.0 | COMPLEX | true | true | true | true | 48,000 |

Token budget increases 8k if pipeline_learnings.md contains a KC-1 entry for similar topics.

## Step 4: Write pipeline_config.json

```json
{
  "pipeline": {
    "depth": "SIMPLE|STANDARD|COMPLEX",
    "adversarial_dialectic": true,
    "red_team": false,
    "fact_check": true,
    "seo_pass": true,
    "token_budget": 32000,
    "topic_brief": "<verbatim user input>",
    "thesis": "<extracted thesis>",
    "thesis_confidence": "HIGH|MEDIUM",
    "novelty_score": 3,
    "contentiousness_score": 4,
    "scope_score": 2,
    "composite_score": 3.1,
    "budget_adjusted": false,
    "routed_at": "<ISO timestamp>"
  }
}
```

All fields required. No partial writes.

## Step 5: Confirm to User

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🔀 COMPLEXITY TRIAGE — Routing Complete
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Depth:               [SIMPLE|STANDARD|COMPLEX]
Composite Score:     [N.N] (Novelty: N | Contention: N | Scope: N)
Thesis:              [one sentence]
Thesis Confidence:   [HIGH → proceeding | MEDIUM → awaiting confirmation]
Adversarial Dialectic: [enabled|disabled]
Fact-Check Gate:     [enabled|disabled]
Red Team:            [enabled|disabled]
Token Budget:        [N] tokens
Learning Calibration: [applied|none]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

If `thesis_confidence` is MEDIUM, display:

```
⚠️ THESIS CONFIDENCE: MEDIUM
Proposed thesis: "[thesis]"
Confirm, rephrase, or provide alternative:
  ✅ "confirmed" — proceed with this thesis
  ✏️ "rephrase: <new thesis>" — update and proceed
```

Proceed only on explicit user confirmation.

---
> References: `references/triage-rubric.md`
