# Hermes Training Summary v1

Consolidated training outcomes from early Hermes / Money Compass astrology-agent sessions.

**Version:** 1  
**Status:** Active  
**Last updated:** 2026-06-11

---

## What Was Trained

Hermes was trained to perform **transit-to-natal** analysis for money/gambling timing with:

1. Strict compute-first ordering (positions → aspects → houses → interpret)
2. Standard notation for aspects and cusp activations
3. Poker-specific house-ruler model (2/3/5/6/8/11)
4. Window quality framework (exact repeats, functional repeats, support, risk)
5. Mandatory practical instruction block for poker windows
6. Case-based pattern validation (Dzmitry poker, Dimasik/Shay casino)

---

## Non-Negotiable Behaviors

| Rule | Detail |
|------|--------|
| Method | Transit-to-natal primary; event chart optional and separated |
| Order | Never interpret before listing positions, aspects, and house placements |
| Notation | `transiting X → natal Y` with degrees and orb |
| Poker priority | House rulers → key natal planets → transit house placement → cusp activation |
| Output | Practical instruction block at end of every poker window analysis |
| Math | Never invent ephemeris data — use engine output or state unavailability |

---

## Domain Models Trained

| Model | File | Context |
|-------|------|---------|
| Poker | `poker_model.md` | Tournament / cash timing windows |
| Casino | `casino_model.md` | Session-based gambling |
| Window logic | `general_window_logic.md` | Cross-domain repeat/support/risk |
| Practical block | `practical_instruction_block.md` | User-facing action guidance |

---

## Validated Case Knowledge

| Case | Key Finding |
|------|-------------|
| `dzmitry_poker` | Repeated winning signature across 3 tournaments: luck + cognition + aggression + discipline + timing |
| `dimasik_shay_casino` | Separate winning/risk signature sets for casino context |

---

## Prompt Assets

Located in `prompts/`:

- `strict_transit_to_natal.md` — enforces protocol
- `technical_reset.md` — recenter agent on compute-first workflow
- `poker_window_search.md` — window discovery and scoring
- `pattern_match_classification.md` — taxonomy application
- `practical_block_prompt.md` — footer generation
- `training_consolidation.md` — post-session knowledge capture

---

## Structured Rules

Located in `rules/`:

- `house_ruler_mapping.yaml` — poker house functions and ruler tags
- `poker_window_rules.yaml` — window scoring logic references
- `risk_flags.yaml` — contextual risk definitions
- `scoring_dimensions.yaml` — dimension weights for window quality

---

## Gaps for v2

- Engine-verified aspect lists for all three Dzmitry tournaments
- Negative cases (lost sessions) for contrast
- Orb thresholds locked with product team
- Casino model validation beyond initial Dimasik/Shay notes

---

## Promotion Checklist (After Each Training Session)

1. Run `prompts/training_consolidation.md`
2. Update relevant case or training doc
3. Add patterns to `repeated_patterns.md` or signatures files if validated
4. Update YAML rules if new flags or dimensions emerge
5. Log experiments in `notes/experiments.md`
6. Bump this summary or create v2 when protocol changes
