# Repository Overview

## Purpose

Money Compass Astro Lab externalizes astrology-agent training into a version-controlled knowledge base. It captures:

- **Protocol** — compute-first workflow, notation standards, interpretation order
- **Models** — poker and casino house-ruler frameworks
- **Cases** — real events with repeated pattern validation
- **Prompts** — agent instructions for reproducible analysis
- **Rules** — YAML structures for scoring, flags, and mappings

## Directory Map

```
money-compass-astro-lab/
├── README.md                 # Entry point, architecture, quick start
├── docs/                     # Canonical documentation
├── training/                 # Hermes training artifacts and domain models
├── cases/                    # Validated case studies + template
├── prompts/                  # Reusable agent prompt blocks
├── rules/                    # Structured YAML knowledge
├── data/                     # Natal charts, window exports (gitignored sensitive exports)
└── notes/                    # Working notes, not canonical
```

## Canonical vs. Working Material

| Type | Location | Status |
|------|----------|--------|
| Canonical protocol | `docs/operating_protocol.md` | Source of truth |
| Domain models | `training/*.md` | Source of truth |
| Validated patterns | `cases/*/repeated_patterns.md`, `winning_signatures.md` | Source of truth after review |
| Agent prompts | `prompts/*.md` | Source of truth for agent behavior |
| Structured rules | `rules/*.yaml` | Source of truth for scoring/flags |
| Brainstorming | `notes/` | Non-canonical until promoted |

## Primary Analysis Method

**Transit-to-natal** is the default and primary method for all Money Compass / Hermes work.

Event charts (e.g., tournament start, session start) are **optional** and must be:

1. Clearly labeled as `EVENT CHART — NOT TRANSIT-TO-NATAL`
2. Kept in a separate section
3. Never mixed into transit-to-natal conclusions without explicit cross-reference

## Notation Standard (Summary)

- Aspects: `transiting [planet] → natal [planet]`
- Cusps: `transiting [planet] → natal [house] cusp`
- Always include: exact degrees, exact angle, orb (when available from engine)

Full detail: `docs/operating_protocol.md`

## Key Documents by Task

| Task | Read First |
|------|------------|
| Any chart analysis | `docs/operating_protocol.md` |
| Poker window search | `training/poker_model.md`, `training/general_window_logic.md` |
| Casino session analysis | `training/casino_model.md` |
| End-of-analysis output | `training/practical_instruction_block.md`, `docs/output_templates.md` |
| New case intake | `cases/templates/case_template.md` |
| Pattern classification | `docs/pattern_taxonomy.md`, `rules/scoring_dimensions.yaml` |
| Product / implementation planning | `docs/money_compass_agent_architecture.md` |

## Architecture

How trained Hermes-style reasoning fits into the future Money Compass application — engine vs. agent boundaries, layered architecture, and near-term build order:

**[`docs/money_compass_agent_architecture.md`](money_compass_agent_architecture.md)**

## Versioning

Training summaries are versioned (`hermes_training_summary_v1.md`, etc.). When protocol or models change materially, update the summary and note the change in `docs/roadmap.md`.
