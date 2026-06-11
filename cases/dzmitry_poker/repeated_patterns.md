# Repeated Patterns — Dzmitry Poker

Consolidated winning patterns from **three validated tournament wins**.

Use this file when scoring future poker windows for Dzmitry. Populate exact aspect rows in tournament files when engine data is available.

---

## Core Poker-Winning Signature

All three wins share this composite:

```
luck + cognition + aggression + discipline + timing
```

| Component | Poker meaning | Primary layer (Dzmitry) |
|-----------|---------------|-------------------------|
| **Luck** | Favorable variance, prize flow, deep-run potential | 11th / Jupiter |
| **Cognition** | Hand reading, ranging, short-range tactical clarity | 3rd / Venus |
| **Aggression** | Controlled push, game energy, value-seeking play | 5th / Mercury |
| **Discipline** | Execution quality, tilt control, stamina | 6th / Moon |
| **Timing** | Recurring transit patterns across winning events | Exact and functional repeats |

**Validation rule:** A future window that shows **luck without discipline** is not a full signature match — even if the 11th/WIN layer looks strong.

---

## Pattern Classes

Four classes are used to evaluate any poker window against this case.

### 1. Exact repeats

The **same** transit-to-natal configuration recurs across multiple winning events:

- Same transiting planet
- Same natal target (planet or cusp)
- Same aspect type
- Orb within agreed tolerance (document per match)

**Use:** Highest-confidence signal. Flag first when scanning future windows.

**Documentation format:**
```
transiting [planet] → natal [target]: [aspect], orb [X]°
Present in: tournament_1, tournament_2, tournament_3
```

Populate specific rows from engine output in `tournament_1.md`, `tournament_2.md`, `tournament_3.md`.

---

### 2. Functional repeats

**Different** transit geometry that activates the **same house-ruler layer** across wins.

Examples of functional equivalence (layer level — not specific claims):

| Layer | Functional repeat means… |
|-------|--------------------------|
| WIN (11th / Jupiter) | Different transits both supporting win/prize flow |
| READ (3rd / Venus) | Different transits both supporting tactical clarity |
| PLAY (5th / Mercury) | Different transits both supporting game/risk energy |
| DISCIPLINE (6th / Moon) | Different transits both supporting stable execution |
| BANKROLL (2nd / Mars) | Different transits both supporting stack/bankroll poise |
| POT (8th / Venus) | Different transits both supporting value extraction |

**Use:** Second-highest confidence. Requires explicit mapping to a house-ruler layer.

---

### 3. Contextual support

Configurations that **strengthen** the window but are not exact or functional repeats:

- Constructive transits to key natal planets (Sun, Moon, Mercury, Venus, Mars, Jupiter, Saturn)
- Favorable natal house placement of transiting planets (e.g., benefic transit through natal 11th)
- Tight supportive aspects to house cusps
- Multiple poker layers showing support in the same window

**Use:** Raises window quality. Does not alone validate a winning pattern.

---

### 4. Contextual risks

Configurations that **weaken** the window or argue for caution:

- DISCIPLINE layer (6th / Moon) under stress while PLAY layer (5th / Mercury) is pushing
- BANKROLL layer (2nd / Mars) afflicted during aggressive session
- WIN layer (11th / Jupiter) bright without DISCIPLINE support — "false luck" pattern
- Any active risk that contradicts the composite signature

**Use:** Downgrades window tier or triggers avoid recommendation. Document even in otherwise strong windows.

Cross-reference: `rules/risk_flags.yaml`

---

## Strongest Support Indicators (Across 3 Wins)

Ranked by consistent presence in the validated winning pattern:

| Rank | Layer | Ruler | Support means… |
|------|-------|-------|----------------|
| 1 | WIN | Jupiter (11th) | Prize flow, favorable variance, deep-run support |
| 2 | READ | Venus (3rd) | Tactical clarity, accurate ranging |
| 3 | PLAY | Mercury (5th) | Smart aggression, correct gamble frequency |
| 4 | DISCIPLINE | Moon (6th) | Stable execution, tilt held in check |
| 5 | BANKROLL | Mars (2nd) | Stack pressure managed |
| 6 | POT | Venus (8th) | Value extraction from opponents' money |

All six layers contribute; the **composite signature** requires luck + cognition + aggression + discipline + timing working together — not an isolated WIN signal.

---

## Strongest Risk Indicators (Flag in Future Windows)

Risks to watch for even when other layers look supportive:

| Risk | Layer | Why it matters |
|------|-------|----------------|
| Discipline breakdown | 6th / Moon | Tilt, execution collapse — vetoes full signature |
| Spew / over-aggression | 5th / Mercury | Unbalanced gamble frequency |
| Bankroll stress | 2nd / Mars | Stack pressure, rebuy spiral |
| False luck | 11th / Jupiter without 6th | Bright WIN signal without discipline backing |

When a risk is active, reflect it in the **Practical Instruction Block** (bankroll and emotional-control sections especially).

---

## How to Apply This Case to Future Analysis

1. Run strict 4-step protocol (`docs/operating_protocol.md`)
2. Evaluate house-ruler layers in hierarchy order (`training/poker_model.md`)
3. **Exact repeats** — match against tournament_1/2/3 aspect lists (when populated)
4. **Functional repeats** — map aspects to layers 2/3/5/6/8/11
5. Add **contextual support** and subtract **contextual risks**
6. Check **composite signature**: luck + cognition + aggression + discipline + timing — full, partial, or absent
7. End with **Practical Instruction Block** (`training/practical_instruction_block.md`)

Upcoming targets: `future_windows.md` (12 Jun 2026, 19 Jun 2026)

---

## Exact Repeat Log

_Populate from engine-verified data in tournament files. Do not mark an aspect as an exact repeat until confirmed in 2+ tournament analyses._

| ID | Aspect (transit → natal) | T1 | T2 | T3 | Layer |
|----|--------------------------|----|----|-----|-------|
| | _pending engine data_ | | | | |

---

## Maintenance

- [ ] Populate exact repeat log from `tournament_1.md`, `tournament_2.md`, `tournament_3.md`
- [ ] Add negative case (non-winning session) for contrast
- [ ] Sync confirmed exact repeats to `rules/poker_window_rules.yaml`
