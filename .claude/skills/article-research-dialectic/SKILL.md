---
name: article-research-dialectic
description: >
  Executes adversarial research for article generation: declares a falsifiable thesis,
  runs @advocate (supporting evidence) and @skeptic (disconfirming evidence) streams,
  then synthesizes into a conflict-mapped research context and article specification.
  @skeptic may access source URLs (not claims) from advocate output to prevent redundant
  retrieval while preserving anchoring isolation. Enforces KC-3 and KC-6 kill conditions.
  Triggers on: adversarial research, research dialectic, advocate and skeptic research,
  find supporting and counter evidence, thesis-based research, generate article spec.
  Activates automatically at Step 1 of multi-agent-article-pipeline. Do NOT trigger for
  writing, auditing, SEO, triage, or fact-checking tasks.
---

# Article Research Dialectic — v4

## Pre-Execution Check

Read `pipeline_config.json`:
- `adversarial_dialectic = false` (SIMPLE): Skip Phase 2a/2b. Execute Phase 3 only with unified research pass.
- `adversarial_dialectic = true` (STANDARD/COMPLEX): Execute full three-phase protocol.
- `thesis` and `thesis_confidence` fields are pre-populated by triage — use these directly.

## Phase 1: Research Vector Decomposition

Given the thesis (from `pipeline_config.json.pipeline.thesis`), decompose into 3–7 research vectors.
Each vector = one knowledge gap or claim that the article must address.

```markdown
## Research Vectors
1. [Vector Name]: [1-sentence scope] | Priority: [HIGH|MEDIUM|LOW]
2. ...
```

Both @advocate and @skeptic receive identical vectors.

**Thesis Confidence = HIGH:** Proceed directly to Phase 2a and 2b.
**Thesis Confidence = MEDIUM:** Confirm thesis was user-approved at triage before proceeding.

## Phase 2a: @advocate Evidence Gathering

For each research vector, execute targeted searches for SUPPORTING evidence.

Source priority: Primary (official docs, papers, regulatory filings) > Authoritative secondary
(recognized experts, institutional publications) > Empirical data (benchmarks, datasets).

```markdown
### Claim ADV-[ID]
- **Statement:** [factual assertion, one sentence]
- **Source:** [Author/Org, Title, URL, Date]
- **Tier:** [T1|T2|T3]
- **Confidence:** [HIGH | MEDIUM | LOW]
- **Vector:** [which research vector this supports]
- **Strength:** [why this evidence is compelling]
```

Minimum 3 sources per vector. Flag `[INSUFFICIENT DATA]` if unmet.
Write output to `advocate_evidence.md`. Include a **Source URL Index** as the final section:
a flat list of all source URLs only (no claims, no metadata) — this is what @skeptic may read.

## Phase 2b: @skeptic Evidence Gathering

Execute AFTER Phase 2a. Permitted to read the **Source URL Index** section of
`advocate_evidence.md` only — not the extracted claims — to avoid retreading the same sources.

For each research vector, execute targeted searches for DISCONFIRMING evidence.

Prioritize: direct rebuttals, failed replications, methodological critiques, alternative
explanations, scope limitations.

```markdown
### Claim SKP-[ID]
- **Statement:** [counter-assertion, one sentence]
- **Source:** [Author/Org, Title, URL, Date]
- **Tier:** [T1|T2|T3]
- **Confidence:** [HIGH | MEDIUM | LOW]
- **Vector:** [which research vector this challenges]
- **Attack Type:** [empirical_rebuttal | methodological_critique | alternative_explanation | scope_limitation | logical_vulnerability]
```

Minimum 2 counter-sources per vector. If none: state `[NO DISCONFIRMING EVIDENCE FOUND]` — this is a valid finding.
Write output to `skeptic_evidence.md`.

## Phase 3: @synthesizer Conflict Mapping

Ingest both evidence artifacts (or unified research if SIMPLE).

Classify each claim pair per vector:

| Classification | Criteria |
|----------------|----------|
| `[CORROBORATED]` | Advocate and skeptic evidence align. High confidence. |
| `[UNCONTESTED]` | Advocate evidence exists, no skeptic counter found. Medium-high confidence. |
| `[CONFLICTING]` | Direct contradiction. Requires explicit handling in article. |
| `[WEAKENED]` | Skeptic evidence narrows but doesn't refute advocate claim. |
| `[INSUFFICIENT]` | Neither side found adequate sources. Knowledge gap. |

For every `[CONFLICTING]` classification, document the conflict axis:
Definitional | Temporal | Methodological | Empirical | Interpretive

**KC-3 Check:** If any single source > 40% of all extracted claims → HALT before writing article_spec.md. Output diagnostic dump.

**KC-6 Check:** If > 50% of vectors classified `[INSUFFICIENT]` → HALT. Output diagnostic dump.
Recommendation: "Research breadth is insufficient. Widen source search, rephrase vectors, or reduce scope."

If both checks pass, write three artifacts:

### research_context.md
Full unified evidence base by research vector. All claims tagged by classification.
Include source tier (`T1/T2/T3`) alongside each claim.

### article_spec.md
```markdown
# Article Specification
## Thesis: [from pipeline_config.json]
## Target Audience: [who + what they already know]
## Pipeline Depth: [from pipeline_config.json]
## Conflict Count: [N CONFLICTING classifications]

## Section Outline
### H2: [Section Title]
- Scope: [1 sentence]
- Word Budget: [N words]
- Key Claims: [ADV-X, SKP-Y, ...]
- Conflicts to Address: [if any]

## Source Mapping
[Which claims map to which sections]

## Risk Flags
[INSUFFICIENT vectors | high-conflict sections | T3-heavy vectors]
```

### conflict_register.md
```markdown
# Conflict Register
## Total Conflicts: [N]

### Conflict C-[N]: [Research Vector]
- Advocate Position: [claim ADV-X]
- Skeptic Position: [claim SKP-Y]
- Axis: [type]
- Conflict Confidence: [HIGH/MEDIUM — how certain is this a real contradiction vs definitional drift]
- Recommended Handling: [present both | author takes position | flag as unresolved]
```

---
> References: `references/claim-taxonomy.md`
