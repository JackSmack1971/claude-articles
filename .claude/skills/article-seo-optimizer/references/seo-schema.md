# SEO Schema — On-Page Checklist & Standards

## On-Page Checklist (Scored Pass/Fail)

| # | Check | Criteria |
|---|-------|----------|
| 1 | Title length | ≤ 60 characters |
| 2 | Primary keyword in title | Must appear verbatim or as close variant |
| 3 | Meta description length | 150–160 characters |
| 4 | Primary keyword in meta | Within first 60 characters of meta |
| 5 | H1 uniqueness | Exactly one H1; matches or close-matches title |
| 6 | Keyword in intro | Primary keyword within first 100 words |
| 7 | Keyword in at least one H2 | Verbatim or semantically equivalent |
| 8 | No keyword stuffing | Density < 2.0% |
| 9 | Article word count | ≥ 1,800 words (flags if below) |
| 10 | Source Appendix present | Must exist and contain ≥ 3 citations |
| 11 | Internal link suggestions | ≥ 3 identified (actual URLs not required) |
| 12 | Image alt text fields | Populated if images referenced; [TODO] if none |
| 13 | No orphan H3s | Every H3 must have a parent H2 |
| 14 | FAQ schema eligible | ≥ 2 question patterns found in body |
| 15 | Structured data complete | All non-TODO JSON-LD fields populated |

## Keyword Density Standards

- **Primary keyword:** 0.8%–1.5% (flag outside range; CRITICAL if > 2.5%)
- **Secondary keywords:** 0.3%–0.8% each; no secondary should exceed primary density
- **Forbidden:** Keyword repetition in consecutive sentences; keyword in every heading

## Title Variant Formulas

| Type | Formula |
|------|---------|
| Data-led | `[Stat or Finding]: [What It Means for X]` |
| Outcome-led | `How [X] [Achieves/Prevents/Changes] [Y]` |
| Contrarian | `Why [Common Belief] Is [Wrong/Incomplete/Overstated]` |

## Slug Construction Rules

1. Extract primary keyword phrase.
2. Remove stop words (the, a, an, of, in, for, to, and, or, but).
3. Convert to lowercase kebab-case.
4. Maximum 5 tokens after stop word removal.
5. No dates, no version numbers, no brand names unless they are the primary keyword.

## JSON-LD Validation Notes

- `datePublished`: Use ISO 8601 format. If unknown, mark `[TODO: publication date]`.
- `author`: If multi-author, use `@type: "Organization"` for the publisher field instead.
- `FAQPage`: Only generate if 2+ distinct question-answer pairs can be extracted from body text. Do not fabricate Q&A pairs.
- `wordCount`: Count body words only; exclude headings, footnotes, source appendix.
