# Google Search Engineer Skill

An SEO advisor skill for Claude Code, grounded in the leaked Google API Content Warehouse documentation (v0.4.0).

Unlike typical SEO advice based on speculation and correlation studies, this skill references **actual Google ranking infrastructure** — the internal signal names, quality metrics, spam detection systems, and ranking modules that were exposed in the leak.

## Capabilities

- **SEO Advisor**: Actionable recommendations grounded in real ranking signals
- **Ranking Signal Reference**: Explains Google's internal signals (CompressedQualitySignals, PerDocData, NavBoost, NSR, etc.)
- **Content Audit**: Structured audit against known ranking factors

## Installation

```bash
claude plugins add /path/to/claude-skills/google-search-engineer
```

## Source

All ranking signals sourced from the Google API Content Warehouse v0.4.0 documentation:
https://hexdocs.pm/google_api_content_warehouse/0.4.0/api-reference.html
