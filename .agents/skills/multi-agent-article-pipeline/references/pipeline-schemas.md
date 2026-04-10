# Pipeline Schemas — v4 Artifact Contracts

## pipeline_config.json

Written by `article-complexity-triage` at Step 0. Read by all subsequent skills.

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
    "thesis": "<extracted thesis, one sentence>",
    "thesis_confidence": "HIGH|MEDIUM",
    "novelty_score": 3,
    "contentiousness_score": 4,
    "scope_score": 2,
    "composite_score": 3.1,
    "budget_adjusted": false,
    "budget_adjustment_reason": null,
    "routed_at": "2026-04-08T12:00:00Z"
  }
}
```

**Field rules:**
- `depth`: SIMPLE (1.0–2.4) | STANDARD (2.5–3.7) | COMPLEX (3.8–5.0)
- `fact_check`: always false for SIMPLE, always true for STANDARD/COMPLEX
- `red_team`: only true for COMPLEX
- `token_budget`: 24000 / 32000 / 48000 base; +8000 if KC-1 history detected
- `budget_adjusted`: true if base was incremented; `budget_adjustment_reason` explains why

---

## conflict_decisions.json

Written by orchestrator after Approval Gate `approved` response. Read by @engineer before drafting.

```json
{
  "decisions": [
    {
      "conflict_id": "C-1",
      "research_vector": "<vector name>",
      "advocate_claim_id": "ADV-3",
      "skeptic_claim_id": "SKP-2",
      "axis": "empirical|definitional|temporal|methodological|interpretive",
      "handling": "neutral|author_position|unresolved",
      "position": "advocate|skeptic|null",
      "note": "<optional human clarification>"
    }
  ],
  "default_handling": "neutral",
  "approved_at": "2026-04-08T13:00:00Z",
  "revision_cycle": 0
}
```

**Field rules:**
- `handling`: `neutral` = present both positions; `author_position` = take a side; `unresolved` = flag explicitly
- `position`: required if `handling = author_position`; null otherwise
- `default_handling`: "neutral" — applied to any conflict not individually specified
- `revision_cycle`: increments on each `revise:` response at Approval Gate

---

## Kill Switch Conditions — v4

| Code | Trigger | When Checked |
|------|---------|--------------|
| KC-1 | Token utilization > 85% BEFORE Step 3 | Post-research, pre-draft |
| KC-2 | Third non-substantive revision at Approval Gate | Step 2 |
| KC-3 | Single source > 40% of all extracted claims | End of @synthesizer Phase 3 |
| KC-4 | @engineer fails inline audit 2× on same section | Step 3 loop |
| KC-5 | Token utilization > 85% DURING Step 3 (mid-draft) | After each section in Step 3 loop |
| KC-6 | > 50% of research vectors classified [INSUFFICIENT] | End of @synthesizer Phase 3 |

KC-5 adds mid-draft protection that KC-1 cannot provide (KC-1 only fires pre-draft).
KC-6 catches breadth failure before it becomes a drafting quality problem.

---

## Artifact Map — All Pipeline Outputs

| Artifact | Written By | Read By |
|----------|-----------|---------|
| `pipeline_config.json` | @triage | all skills |
| `advocate_evidence.md` | @advocate | @skeptic (URLs only), @synthesizer |
| `skeptic_evidence.md` | @skeptic | @synthesizer |
| `research_context.md` | @synthesizer | @fact-checker, @engineer, @qa |
| `article_spec.md` | @synthesizer | @engineer, @qa, @reader, @seo-optimizer |
| `conflict_register.md` | @synthesizer | human (Approval Gate) |
| `fact_check_report.md` | @fact-checker | @engineer, @qa |
| `dispute_register.md` | @fact-checker | human (Approval Gate, if > 3 disputes) |
| `conflict_decisions.json` | orchestrator | @engineer |
| `article_draft.md` | @engineer | @qa, @adversary (conclusion only), @reader, @seo-optimizer |
| `audit_log.md` | @qa (inline) | orchestrator |
| `audit_report.md` | @qa (holistic) | orchestrator |
| `red_team_report.md` | @adversary | human |
| `reader_questions.md` | @reader | human |
| `seo_package.md` | @seo-optimizer | human |
| `pipeline_learnings.md` | @qa | @triage (next run) |
