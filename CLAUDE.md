## Article Pipeline Project — v4

Eight skills installed: multi-agent-article-pipeline (orchestrator),
article-complexity-triage, article-research-dialectic, article-fact-checker,
article-qa-auditor, article-red-team, article-reader-simulation, article-seo-optimizer.

Project files:
- article_style_guide.md — authoritative style rules, enforced by all agents
- pipeline_learnings.md — cross-run calibration log, read by triage at each run start

Default audience: practitioner
Default depth: auto-triage (scored by complexity-triage skill)

To run the full pipeline: "Generate an article about [topic]"
Sub-skills work standalone. Fact-checker and SEO optimizer can be invoked independently.
