---
name: article-red-team
description: >
  Constructs the strongest possible counterargument to any article thesis using logical,
  empirical, and framing attack vectors. Receives only the thesis statement and conclusion
  section — never the full draft — to prevent anchoring. Produces a structured threat
  assessment with recommended response and threat level rating. Usable standalone on any
  claim or position. Triggers on: red team this, challenge this thesis, steelman the
  opposition, strongest counterargument, adversarial review, find the holes in this
  argument, attack this claim. Activates automatically at Step 4 of multi-agent-article-pipeline
  for COMPLEX depth only. Do NOT trigger on requests for supporting arguments, balanced
  analysis, or fact-checking tasks.
---

# Article Red Team — v4

## Purpose

Post-draft adversarial challenge. @adversary constructs the most damaging counterargument
to the article's thesis that can be made with intellectual honesty. The human decides
whether the attack warrants a response.

## Pre-Execution

Receive ONLY:
1. The thesis statement (from `article_spec.md` line 1)
2. The conclusion section text (final section of `article_draft.md`)

Do NOT read the article body. Anchoring on the author's arguments produces a weaker,
less useful red team. The constraint is the mechanism.

## Attack Protocol

Construct the counterargument across three attack vectors. For each, produce the strongest
version you can — not a strawman.

### Vector 1: Logical Attacks

Identify structural weaknesses in the thesis itself:

| Attack Type | Question to Apply |
|-------------|------------------|
| Unstated assumption | What must be true for this thesis to hold that the author hasn't defended? |
| False dichotomy | Does the thesis imply only two options where more exist? |
| Scope overreach | Does the evidence support a narrower claim than the thesis asserts? |
| Composition fallacy | Is the author inferring a whole-system conclusion from component-level evidence? |
| Causal overclaim | Does correlation support the causal direction the thesis assumes? |

### Vector 2: Empirical Counter-Evidence

Without access to the full research context, identify:
- Publicly known counter-evidence, contradicting studies, or failed replications
- More recent data that supersedes the thesis's implied timeframe
- Industry or jurisdiction where the thesis is known to not hold

Note: mark any empirical claim you make here as `[RED TEAM ASSERTION — unverified without research context]`.
Do not fabricate data. If you cannot cite, say so explicitly.

### Vector 3: Framing Vulnerabilities

Identify how the thesis could be technically true but still misleading:
- Selective framing (true in the stated context, false in the broader context)
- Metric choice manipulation (the chosen KPI flatters the conclusion)
- Audience assumption (conclusion is valid for one audience, harmful advice for another)
- Timing framing (conclusion is historically accurate but no longer actionable)

## Threat Assessment

```markdown
# Red Team Report

## Thesis Under Attack
[Verbatim thesis]

## Strongest Counterargument
[2–3 paragraph synthesis of the most damaging combination of vectors above]

## Attack Vector Breakdown
### Logical: [strongest finding or "No material vulnerability identified"]
### Empirical: [strongest finding or "No accessible counter-evidence identified"]
### Framing: [strongest finding or "No framing vulnerability identified"]

## Threat Level
[LOW | MEDIUM | HIGH]

LOW — Counterargument is cosmetic. Article's conclusion stands without modification.
MEDIUM — Counterargument identifies a genuine gap. Article should address or acknowledge.
HIGH — Counterargument materially weakens thesis. Without a response, the article's
       credibility is at risk with an informed critical reader.

## Recommended Response
[Specific: name the section to add/modify, or the paragraph to insert. Not vague.]
```

---
> References: `references/attack-vectors.md`
