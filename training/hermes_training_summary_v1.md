# Hermes Training Summary v1

Consolidated training outcomes from Hermes / Money Compass astrology-agent sessions.

**Version:** 1  
**Status:** Active  
**Last updated:** 2026-06-11

---

## Strict Astrology Workflow

All chart analysis follows a **compute-first** workflow. Interpretation is always last.

| Step | Name | Action |
|------|------|--------|
| **1** | Raw transit positions | List transiting planet positions (sign, degree, retrograde). No interpretation. |
| **2** | Exact aspects with orbs | List every transit-to-natal aspect to planets and relevant cusps. Include exact angle and orb. No interpretation. |
| **3** | Natal house placement | Record which natal house each transiting planet occupies. No interpretation. |
| **4** | Interpretation | Apply domain model, house-ruler layers, support/risk, and synthesis. Only after Steps 1–3 are complete. |

**Rule:** Never skip steps. Never interpret before computed facts are listed.

Full detail: `docs/operating_protocol.md`

---

## Primary Method: Transit-to-Natal

**Transit-to-natal** is the primary and default method for all Money Compass / Hermes work.

Compare the **current sky** to the **native's birth chart** to determine how a moment activates that chart.

---

## Event Chart: Optional, Separate Layer

Event charts (e.g., tournament start time and location) may be used as an **optional overlay**.

**Rules:**

- Event chart is never a substitute for transit-to-natal
- If used, it must appear in a **clearly labeled separate section**
- Do not mix event-chart conclusions into transit-to-natal analysis without explicit labeling

Example section title: `EVENT CHART OVERLAY (OPTIONAL — SEPARATE)`

---

## Strict Notation Standard

All aspects use transit-to-natal arrow notation:

**Planets:**
```
transiting [planet] → natal [planet]
```

**Cusps:**
```
transiting [planet] → natal [house] cusp
```

Whenever possible, include:

- Exact degrees (transiting and natal)
- Exact angle (0°, 60°, 90°, 120°, 180°)
- Orb

---

## Poker Window Output Requirement

Every **poker window analysis** must end with the **Practical Instruction Block**.

Required sections:

1. How to play the window
2. What to avoid
3. Main edge
4. Main danger
5. Best style of play
6. Bankroll instruction
7. Emotional-control instruction

Template: `training/practical_instruction_block.md`

---

## Poker Analysis Hierarchy

When interpreting poker windows, apply layers in this order:

1. House-ruler layer (2, 3, 5, 6, 8, 11)
2. Key natal planets layer
3. Natal house placement of transits
4. Natal house cusp activation layer

Detail: `training/poker_model.md`

---

## Window Quality Framework

Poker windows are evaluated using four pattern classes:

| Class | Description |
|-------|-------------|
| **Exact repeats** | Same transit-to-natal geometry across multiple winning events |
| **Functional repeats** | Different geometry activating the same house-ruler layer |
| **Contextual support** | Configurations that strengthen the window without being repeats |
| **Contextual risks** | Configurations that weaken the window or trigger avoidance |

Detail: `training/general_window_logic.md`, `docs/pattern_taxonomy.md`

---

## Validated Case Knowledge

| Case | Key Finding |
|------|-------------|
| `dzmitry_poker` | Three tournament wins share a composite signature: **luck + cognition + aggression + discipline + timing** |
| `dimasik_shay_casino` | Casino winning/risk signatures (separate model) |

Case patterns: `cases/dzmitry_poker/repeated_patterns.md`

---

## Non-Negotiable Behaviors

| Rule | Detail |
|------|--------|
| Workflow | Positions → aspects → house placement → interpretation |
| Method | Transit-to-natal primary; event chart optional and separated |
| Notation | `transiting X → natal Y` with degrees and orb |
| Poker output | Practical instruction block required at end of every poker window analysis |
| Math | Never invent ephemeris data — use engine output or state unavailability |

---

## Domain Models

| Model | File |
|-------|------|
| Poker house-ruler model | `training/poker_model.md` |
| Casino model | `training/casino_model.md` |
| Window logic | `training/general_window_logic.md` |
| Practical block | `training/practical_instruction_block.md` |

---

## Prompt Assets

| Prompt | Purpose |
|--------|---------|
| `prompts/strict_transit_to_natal.md` | Enforce 4-step protocol |
| `prompts/technical_reset.md` | Recover from interpretation-first drift |
| `prompts/poker_window_search.md` | Window discovery and scoring |
| `prompts/pattern_match_classification.md` | Classify exact/functional/support/risk |
| `prompts/practical_block_prompt.md` | Generate mandatory footer |
| `prompts/training_consolidation.md` | Post-session knowledge capture |

---

## Structured Rules

| File | Purpose |
|------|---------|
| `rules/house_ruler_mapping.yaml` | House functions and ruler tags |
| `rules/poker_window_rules.yaml` | Window scoring references |
| `rules/risk_flags.yaml` | Contextual risk definitions |
| `rules/scoring_dimensions.yaml` | Dimension weights |

---

## Engine vs. Agent Boundary

| Layer | Owner |
|-------|-------|
| Ephemeris, positions, aspects, orbs, cusps | Deterministic engine |
| Protocol Steps 1–3 listing | Agent (from engine output) |
| Pattern matching, case recall, practical synthesis | Agent (from this repo) |
| Inventing chart data | **Forbidden** |

---

## Gaps for v2

- Engine-verified aspect lists populated in `cases/dzmitry_poker/tournament_*.md`
- Negative cases (non-winning sessions) for contrast
- Orb thresholds finalized against engine data

---

## Promotion Checklist (After Each Training Session)

1. Run `prompts/training_consolidation.md`
2. Update relevant case or training doc
3. Add validated patterns to `repeated_patterns.md`
4. Update YAML rules if new flags or dimensions emerge
5. Log experiments in `notes/experiments.md`
6. Bump this summary or create v2 when protocol changes
