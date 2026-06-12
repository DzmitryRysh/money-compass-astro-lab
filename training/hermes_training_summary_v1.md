# Hermes Training Summary v1

Consolidated training outcomes from Hermes / Money Compass astrology-agent sessions.

**Version:** 1  
**Status:** Active  
**Last updated:** 2026-06-11 (technical lessons added)

---

## Technical Lessons Learned

Validated fixes from Hermes training sessions. Permanent rules — not one-off corrections.

### UTC conversion bug (corrected)

A previous **Moon position error** was caused by incorrect local time → UTC conversion.

**Hollywood, FL offset rules:**

| Season | Zone | UTC offset |
|--------|------|------------|
| Summer (DST) | EDT | UTC−4 |
| Winter | EST | UTC−5 |

**Permanent rule:** Every technical verification must explicitly show:

- **Local timestamp used**
- **UTC timestamp used**

Documented in `docs/operating_protocol.md`. Regression logged in `notes/experiments.md` (11 June 2026 Moon mismatch).

### Wrap-around natal house placement bug (corrected)

House assignment failed when a natal house **crosses the 360°/0° boundary** (Pisces → Aries).

**Validated natal chart reference (Dzmitry case):**

| Cusp | Degree |
|------|--------|
| House 1 | Aquarius 10°0′ |
| House 2 | Aries 16°0′ |

**House 1 span:** Aquarius 10° → Pisces → Aries 16°

Therefore any **Aries position from 0° to 16°** belongs to **natal house 1**, not house 12.

**Corrections:**

| Transit | Previous (wrong) | Corrected |
|---------|------------------|-----------|
| transiting Saturn in Aries | natal house 12 | **natal house 1** |
| transiting Neptune in Aries | natal house 12 | **natal house 1** |
| transiting Pluto in Aquarius 3°23′ | natal house 12 | natal house 12 ✓ (unchanged) |

Wrap-around houses must be treated as **two-part intervals**. See `docs/operating_protocol.md`.

Regression logged in `notes/experiments.md` (21 November 2026 wrap-around test).

### Skill created: strict-technical-astrological-verification

Hermes training produced an internal verification skill enforcing:

1. **Compute-first** — no interpretation before math
2. **Raw positions first** — list transiting positions before aspects or houses
3. **UTC verification** — local and UTC timestamps both shown
4. **House-placement verification** — including wrap-around cusp logic
5. **Aspect verification** — exact angles and orbs confirmed
6. **Interpretation last** — only after all math is confirmed

This skill codifies the permanent verification layer above the interpretation workflow.

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
| UTC | Always show local timestamp and UTC timestamp used |
| Houses | Verify wrap-around cusp spans (360°/0° boundary) before assigning natal house |

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
- Automated regression tests for UTC conversion and wrap-around house assignment (see `notes/open_questions.md`)

---

## Promotion Checklist (After Each Training Session)

1. Run `prompts/training_consolidation.md`
2. Update relevant case or training doc
3. Add validated patterns to `repeated_patterns.md`
4. Update YAML rules if new flags or dimensions emerge
5. Log experiments in `notes/experiments.md`
6. Bump this summary or create v2 when protocol changes
