---
name: article-qa-auditor
description: >
  Audits article drafts for factual accuracy, citation integrity, style guide compliance,
  conflict traceability, source tier quality, and fact-check report adherence. Operates in
  two modes: inline (per-section during drafting, enforces KC-5 mid-draft token check) and
  holistic (full-document final review). Also serves as the drafting execution engine
  (@engineer) when paired with approved research artifacts. Triggers on: audit article,
  QA draft, fact-check and style-check, editorial audit, review article for compliance,
  draft article from spec, inline audit section, holistic audit. Activates automatically
  at Step 3 of multi-agent-article-pipeline. Do NOT trigger for grammar-only proofreading,
  SEO optimization, or tasks without a research context artifact.
---

# Article QA Auditor — v4

## Modes

**Inline mode:** Activated after @engineer writes each section. Audits that section only.
**Holistic mode:** Activated after all sections pass inline. Full-document review.

---

## Inline Audit Protocol

### Audit Checklist (per section)

Load `references/audit-checklist.md` for the full rubric. Key checks:

**Factual Accuracy**
- [ ] Every factual claim maps to a claim ID in `research_context.md`
- [ ] Statistical values match `fact_check_report.md` VERIFIED-UPDATED entries (not originals)
- [ ] No claims from [INSUFFICIENT] vectors presented as established fact
- [ ] No `[Unverified]` claims without explicit `[Unverified]` label in prose

**Citation Integrity**
- [ ] Every factual assertion has inline citation `[Source: Author/Org, Date]`
- [ ] Legislation/policy acronyms have mandatory inline definition on first use
- [ ] T3 citations marked `†` with documented T1/T2 re-search attempt

**Conflict Handling**
- [ ] [CONFLICTING] claims handled per `conflict_decisions.json` (not improvised)
- [ ] Sections closing with "remains contested" must identify the structural incompatibility axis
- [ ] No silent conflict resolution (one side adopted without disclosure)

**Style Guide Compliance**
- [ ] No prohibited patterns (meta-narration, AI-tell phrases, rhetorical questions as openers)
- [ ] Bold used for key terms on first introduction only
- [ ] No unattributed superlatives

**Structural**
- [ ] Section matches spec scope and word budget (±15% tolerance)
- [ ] Topic sentence leads the first paragraph

### Verdict

```
SECTION PASS — all checks clear
SECTION PASS WITH NOTES — [MINOR]/[STYLE] findings only; @engineer may note and advance
SECTION BLOCKED — one or more [CRITICAL]/[MAJOR] findings; @engineer must revise before advancing
```

**KC-5 Check (mid-draft):** After issuing verdict, check cumulative telemetry footer.
If `utilization > 85%` and sections remain → output KC-5 diagnostic dump. Pipeline halts.

Append findings to `audit_log.md`:
```markdown
## Section [N] — [Title] — [VERDICT]
### Findings
- [CRITICAL|MAJOR|MINOR|STYLE] [finding description] | Claim ref: [ADV/SKP ID if applicable]
```

---

## Holistic Audit Protocol

Read complete `article_draft.md` after all sections pass inline.

### Holistic Checklist

**Narrative Arc**
- [ ] Thesis stated explicitly in introduction
- [ ] Thesis reflected or resolved in conclusion
- [ ] Section sequence builds logically (no orphaned arguments)

**Cross-Section Coherence**
- [ ] No contradictory claims between sections
- [ ] No repeated identical sentences or paragraphs
- [ ] Transitions between sections are explicit (no abrupt topic jumps)

**Aggregate Citation Metrics**
- [ ] Source diversity: no single source > 30% of total citations
- [ ] Minimum 5 distinct sources for STANDARD/COMPLEX depth articles
- [ ] Source Appendix present and complete

**Word Count**
- [ ] Total within 1,800–3,200 words (or spec override)
- [ ] No section > 30% over its spec word budget

**Conflict Resolution Audit**
- [ ] Every item in `conflict_register.md` is addressed in the draft
- [ ] No new conflicts introduced in prose that weren't in the register

### Holistic Verdict

```
PASS — article is ready for validation phases
FAIL — [list CRITICAL/MAJOR findings with section references]
```

On FAIL: one revision cycle targeting CRITICAL/MAJOR sections only. Maximum 1 full-document cycle.

Write `audit_report.md`.

---

## @engineer Drafting Mode

When activated as drafting engine with an approved spec:

1. Read `article_spec.md`, `research_context.md`, `fact_check_report.md`, `conflict_decisions.json`.
2. Read `article_style_guide.md` (always-on project rule).
3. Draft section by section. Yield after each for inline audit.
4. On VERIFIED-UPDATED claims: use the updated value from `fact_check_report.md`, not `research_context.md`.
5. On CONFLICTING claims: implement exactly as specified in `conflict_decisions.json`.

---
> References: `references/audit-checklist.md`
