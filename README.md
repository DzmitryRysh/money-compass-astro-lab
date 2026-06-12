# Money Compass Astro Lab

A durable external knowledge base for astrology-agent training, reusable logic, prompts, case studies, and pattern knowledge developed around **Hermes** and **Money Compass**.

This repository preserves reasoning-layer knowledge that would otherwise be lost when chats end, tools change, or local machine state resets.

---

## What This Repo Is

| Layer | Location | Purpose |
|-------|----------|---------|
| **Operating protocol** | `docs/`, `training/` | How Hermes computes and interprets charts |
| **Domain models** | `training/poker_model.md`, `training/casino_model.md` | House-ruler and window logic for gambling contexts |
| **Case studies** | `cases/` | Validated patterns from real events (poker, casino) |
| **Prompts** | `prompts/` | Reusable agent instructions for consistent output |
| **Structured rules** | `rules/*.yaml` | Machine-readable scoring, flags, and mappings |
| **Raw data** | `data/` | Natal charts, window exports, structured outputs |
| **Working notes** | `notes/` | Ideas, experiments, open questions |

---

## Relationship to Money Compass

**Money Compass** is the product surface: timing windows, risk signals, and practical guidance for money-related decisions.

**This repo** is the knowledge engine behind that product:

- It stores *how* to think about poker windows, casino sessions, and transit-to-natal analysis.
- It captures *what worked* in validated cases (e.g., Dzmitry's tournament wins, Dimasik/Shay casino sessions).
- It defines *prompts and rules* that Hermes (or future agents) must follow for consistent, auditable output.

Product code computes deterministic astrology. This repo preserves the interpretation layer, pattern taxonomy, and case-derived heuristics that sit above raw ephemeris data.

---

## Architecture: Engine vs. Agent

```
┌─────────────────────────────────────────────────────────┐
│  Agent / Hermes (reasoning layer)                       │
│  — pattern matching, case recall, practical blocks      │
│  — prompt-driven interpretation order                   │
│  — risk/support synthesis                               │
├─────────────────────────────────────────────────────────┤
│  Structured signals (this repo: rules/, training/)      │
│  — house-ruler mappings, scoring dimensions, flags      │
│  — window logic, output templates                       │
├─────────────────────────────────────────────────────────┤
│  Deterministic astro engine (external / product code)   │
│  — ephemeris, aspect math, house cusps, orbs            │
│  — raw transit positions, exact angles                  │
└─────────────────────────────────────────────────────────┘
```

**Why separate them?**

- **Deterministic math belongs in an engine** so positions, aspects, and orbs are reproducible and testable.
- **AI/agent reasoning belongs above structured signals** so interpretation follows a strict protocol without hallucinating chart data.
- **This repo preserves the reasoning layer** — prompts, case logic, pattern knowledge, and output standards — independent of any single chat session or model version.

---

## Implementation Architecture

For the full picture of how Hermes training connects to future Money Compass product code — layered stack (engine → patterns → agent → output), boundaries, example flows, and build order — see **[`docs/money_compass_agent_architecture.md`](docs/money_compass_agent_architecture.md)**.

That document is the main bridge between this knowledge repo and implementation decisions.

For the first deterministic astrology engine (Layer 1), see **[`docs/astro_engine_v1_spec.md`](docs/astro_engine_v1_spec.md)** — inputs, JSON contract, module layout, and test plan.

---

## How Hermes Training Is Stored

| Artifact | File(s) |
|----------|---------|
| Core protocol | `docs/operating_protocol.md` |
| Training summary v1 | `training/hermes_training_summary_v1.md` |
| Poker / casino models | `training/poker_model.md`, `training/casino_model.md` |
| Window logic | `training/general_window_logic.md` |
| Practical output block | `training/practical_instruction_block.md` |
| Validated cases | `cases/dzmitry_poker/`, `cases/dimasik_shay_casino/` |
| Agent prompts | `prompts/*.md` |
| Structured rules | `rules/*.yaml` |

When new training sessions produce insights, update the relevant doc, add or extend a case file, and bump versioned summaries (e.g., `hermes_training_summary_v2.md`).

---

## Quick Start

1. Read `docs/overview.md` for repository orientation.
2. Read `docs/operating_protocol.md` before any chart analysis.
3. For poker windows: `training/poker_model.md` → `training/general_window_logic.md` → `training/practical_instruction_block.md`.
4. For a new case: copy `cases/templates/case_template.md`.
5. For agent runs: use prompts in `prompts/` with rules from `rules/`.

---

## Repository Tone

Practical. Structured. Serious. Clean. Reusable. Built for future product development — not casual notes.

---

## License / Usage

Private knowledge repository. Do not commit secrets (see `.gitignore`). Chart data in `data/` should be anonymized or permissioned as appropriate.
