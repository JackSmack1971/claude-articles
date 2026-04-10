# Claim Taxonomy — Evidence Classification Reference

## Conflict Axis Definitions

| Axis | Definition | Detection Signal |
|------|------------|-----------------|
| Definitional | Sources use the same term to mean different things | Identical keyword, different scope or boundary |
| Temporal | Sources describe different time periods as if they're the same | Dates differ by >12 months on a fast-moving topic |
| Methodological | Sources use incompatible measurement or study design | Survey vs. experiment; self-report vs. observed behavior |
| Empirical | Sources present directly contradicting data points | Numbers contradict; one dataset refutes another |
| Interpretive | Sources agree on facts but disagree on meaning or implication | Same statistic, opposite conclusions |

## Confidence Degradation Rules

A claim's confidence degrades under these conditions:

| Condition | Downgrade |
|-----------|-----------|
| Sole T3 source (no T1/T2 corroboration) | HIGH → MEDIUM |
| Source older than freshness threshold (see verification-protocol.md) | Any → LOW |
| Claim is a secondary inference (source implies but doesn't state) | Reduce by one level |
| Claim was [WEAKENED] by skeptic evidence | HIGH → MEDIUM; MEDIUM → LOW |
| Claim was [CONFLICTING] with no axis resolution | Any → requires explicit article handling |

## Source Tier Tagging

Tag every claim with its tier:
- `T1` — Peer-reviewed, official regulatory, government dataset, primary document
- `T2` — Recognized expert, institutional report, established industry analyst
- `T3` — Quality journalism with named T1/T2 citation within the article
- `T4` — Blog, forum, undated — NOT acceptable; do not extract claims from T4 sources

## INSUFFICIENT Vector Protocol

When a vector cannot meet minimum source requirements:
1. Flag it explicitly: `[INSUFFICIENT DATA — vector: X]`
2. Note the best available evidence and its limitations.
3. Include in `article_spec.md` Risk Flags section.
4. @engineer may NOT write a substantive section on an [INSUFFICIENT] vector without sourcing.
   Option: include a brief "What We Don't Yet Know" framing if it adds editorial value.

## Conflict Confidence Field

New in v4. Signals how certain @synthesizer is that a conflict is real vs. apparent:
- **HIGH:** Sources clearly contradict on the same specific claim.
- **MEDIUM:** Contradiction may be due to definitional drift or temporal gap — could dissolve with clarification.
If MEDIUM: @synthesizer should note the possible resolution in the conflict_register.md entry.
