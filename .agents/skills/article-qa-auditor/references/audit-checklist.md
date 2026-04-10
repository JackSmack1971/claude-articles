# Audit Checklist — Full Severity-Tagged Rubric

## Severity Definitions

| Level | Meaning | Blocks Section? |
|-------|---------|-----------------|
| [CRITICAL] | Factual error, fabricated citation, unresolved CONFLICTING claim, silent conflict resolution | Yes — SECTION BLOCKED |
| [MAJOR] | Missing citation on factual claim, [Unverified] presented as fact, spec deviation > 30%, word budget > 30% over | Yes — SECTION BLOCKED |
| [MINOR] | Citation format non-standard, word budget 15–30% over, transition quality | No — SECTION PASS WITH NOTES |
| [STYLE] | Prohibited phrase, wrong emphasis type, passive construction | No — SECTION PASS WITH NOTES |

## Full Inline Checklist

### Factual Layer
- [ ] (CRITICAL) Claims map to research_context.md claim IDs
- [ ] (CRITICAL) No claims from [INSUFFICIENT] vectors stated as established fact
- [ ] (CRITICAL) VERIFIED-UPDATED values used (not original research_context.md figures)
- [ ] (MAJOR) [Unverified] claims labeled inline with `[Unverified]` marker
- [ ] (MAJOR) No fabricated or invented citations
- [ ] (MINOR) T3 citations marked `†` per fact-checker protocol

### Citation Layer
- [ ] (CRITICAL) Silent conflict resolution (one side adopted without disclosure)
- [ ] (MAJOR) Factual assertion without inline citation `[Source: ...]`
- [ ] (MAJOR) Legislation/policy acronym without inline definition on first use
- [ ] (MINOR) Citation format non-standard (missing date, vague org name)

### Conflict Handling Layer
- [ ] (CRITICAL) [CONFLICTING] claim handled differently than `conflict_decisions.json` specifies
- [ ] (MAJOR) "Remains contested" closing without identifying structural incompatibility axis
- [ ] (MINOR) Conflict presented but recommended handling (neutral/position) not clearly signaled to reader

### Style Guide Layer
- [ ] (MAJOR) Meta-narration ("In this article we will explore...")
- [ ] (STYLE) AI-tell phrases ("It's important to note", "It's worth mentioning", "Delving into")
- [ ] (STYLE) Rhetorical question as section opener
- [ ] (STYLE) Filler transitions ("Furthermore", "Moreover", "Additionally" without logical justification)
- [ ] (STYLE) Bold used for non-first-introduction terms
- [ ] (STYLE) Unattributed superlative ("the best", "revolutionary", "the most powerful")
- [ ] (STYLE) Bullet list for narrative content (bullets permitted only for 3+ parallel enumerations)

### Structural Layer
- [ ] (MAJOR) Section scope deviates materially from article_spec.md
- [ ] (MINOR) Word budget exceeded by 15–30%
- [ ] (MAJOR) Word budget exceeded by > 30%
- [ ] (MINOR) Topic sentence does not lead first paragraph

## Holistic-Only Checks

- [ ] (CRITICAL) Thesis not stated in introduction
- [ ] (MAJOR) Thesis not reflected or resolved in conclusion
- [ ] (CRITICAL) Item from conflict_register.md not addressed anywhere in draft
- [ ] (MAJOR) Single source > 30% of total citations
- [ ] (MAJOR) Source Appendix missing or incomplete
- [ ] (MINOR) Total word count outside 1,800–3,200 range
- [ ] (MINOR) Duplicate sentences between sections
- [ ] (STYLE) Abrupt section-to-section transition (no bridging sentence)
