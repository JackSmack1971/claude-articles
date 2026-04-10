# Attack Vectors — Red Team Reference

## Logical Attack Taxonomy

| Attack | Definition | Example |
|--------|-----------|---------|
| Unstated Assumption | Thesis requires an undefended premise | "AI adoption improves productivity" assumes productivity is measurable and improvement is attributable |
| False Dichotomy | Only two options presented; more exist | "Either adopt AI or fall behind" — many companies selectively adopt with neutral outcomes |
| Scope Overreach | Evidence supports a narrower claim | Study of 50 companies cited to support claim about "enterprise-wide" behavior |
| Composition Fallacy | Component truth generalized to the whole | Individual components improve, therefore the system improves — often false in complex systems |
| Causal Overclaim | Correlation treated as directional causation | "Companies using X report higher revenue" — revenue growth may attract X adoption, not cause it |
| Cherry-Picked Timeframe | Data window chosen to flatter conclusion | "X grew 40% since 2021" — 2021 was a trough; relative to 2019 the growth is negligible |
| Survivorship Bias | Only successful cases studied | "Companies that adopted Y succeeded" — failed adopters are unrepresented in the dataset |

## Framing Vulnerability Taxonomy

| Vulnerability | Detection Question |
|---------------|-------------------|
| Metric manipulation | Which KPI was chosen, and does another KPI tell a different story? |
| Audience mismatch | Is the conclusion actionable for the stated audience, or only for a subset? |
| Temporal obsolescence | Is the conclusion based on a context that no longer exists? |
| False precision | Does the level of numerical specificity exceed the data's actual precision? |
| Anchoring bias | Does the framing of the thesis pre-dispose the reader toward one interpretation before the evidence is examined? |

## Threat Level Calibration

**LOW:** The counterargument is valid but doesn't change the article's practical utility.
A critical reader might note it; a practitioner reader would not.

**MEDIUM:** The counterargument identifies a gap the article should acknowledge. A skilled
reader in the field will notice the omission. A limitations section or a qualifying clause
in the conclusion resolves it.

**HIGH:** The counterargument, if raised by a critic, would undermine the article's core claim
in the eyes of the target audience. The thesis as stated cannot survive without addressing it.
Requires substantive revision — not just a disclaimer.

## Rules for the Red Team

1. Produce the strongest version of each attack — not a strawman.
2. Mark any empirical claim as `[RED TEAM ASSERTION — unverified]` if you cannot cite it.
3. Do not fabricate data or citations. Intellectual honesty is the constraint.
4. If no meaningful attack exists for a vector, state that explicitly — a clean bill of health
   on a vector is also a useful signal.
5. The threat level must be determined by how a well-informed critical reader of the target
   audience would respond — not by how persuasive the attack feels to you.
