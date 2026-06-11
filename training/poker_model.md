# Poker Model

House-ruler framework for poker timing and window analysis in Money Compass / Hermes.

**Native reference:** Dzmitry (validated case — three tournament wins)

---

## Overview

Poker is modeled through **six natal houses** and their **rulers**. Transits to these rulers and related cusps indicate support or risk for bankroll, reads, play quality, discipline, pot extraction, and winning outcomes.

**Method:** transit-to-natal (primary). See `docs/operating_protocol.md`.

---

## House Map and Rulers (Dzmitry)

| House | Poker Function | Ruler |
|-------|----------------|-------|
| **2nd** | Bankroll / personal money | **Mars** |
| **3rd** | Tactical reading / short-range strategy | **Venus** |
| **5th** | Game / risk / play | **Mercury** |
| **6th** | Discipline / execution quality | **Moon** |
| **8th** | Other people's money / the pot / extraction of value | **Venus** |
| **11th** | Win / prize / favorable result | **Jupiter** |

### What each house means in poker

| House | Function |
|-------|----------|
| **2nd** | Chips behind, rebuy capacity, personal financial pressure at the table |
| **3rd** | Hand reading, ranging, short-range tactical decisions, table dynamics |
| **5th** | Gamble impulse, bold lines, creative aggression, play energy |
| **6th** | Execution quality, tilt control, stamina, routine at the table |
| **8th** | The pot, value from opponents' stacks, shared-money / extraction dynamics |
| **11th** | Prize, placement, favorable result, winning variance |

### What each ruler means functionally

| Ruler | Houses ruled | Functional role |
|-------|--------------|-----------------|
| **Mars** | 2nd | Bankroll fight, stack pressure, controlled chip aggression |
| **Venus** | 3rd, 8th | Tactical grace and reads (3rd); pot value and extraction (8th) |
| **Mercury** | 5th | Game intellect, hand selection, quick tactical adjustments |
| **Moon** | 6th | Emotional rhythm, discipline baseline, tilt susceptibility |
| **Jupiter** | 11th | Win expansion, prize luck, favorable variance |

**Note:** Rulers above are from the validated Dzmitry chart. For other natives, resolve rulers from their natal chart. Store per-native mappings in `cases/*/natal_reference.md`.

---

## Interpretation Hierarchy

Apply in this order during Step 4 (interpretation). Higher priority outweighs lower unless lower-layer orbs are extremely tight.

### 1. House-ruler layer (highest priority)

Transits aspecting the rulers of houses 2, 3, 5, 6, 8, and 11.

This is the primary signal layer for poker windows. Each ruler activation maps to a specific poker function (bankroll, read, play, discipline, pot, win).

### 2. Key natal planets layer

Transits to core natal planets: Sun, Moon, Mercury, Venus, Mars, Jupiter, Saturn.

These planets anchor the chart. Their transit activations modify or reinforce house-ruler signals.

### 3. Natal house placement of transits

Where each transiting planet falls in the native's natal houses at the event time.

Shows which life area receives transiting energy — e.g., transiting Jupiter through natal 11th adds WIN-layer context even without a direct ruler aspect.

### 4. Natal house cusp activation layer (lowest priority in hierarchy)

Transits aspecting natal house cusps, especially cusps of 2, 3, 5, 6, 8, and 11.

Cusp hits activate the house directly. They support the analysis but do not override clear house-ruler or key-planet signals unless orbs are very tight.

---

## Support vs. Risk

### Support

A layer shows **support** when transits to its ruler (or related cusp/placement) are constructively activating that house's poker function — e.g., WIN layer active, READ layer clear, DISCIPLINE layer stable.

Support can appear as:

- Direct constructive transits to the house ruler
- Functional activation of the same layer through a different geometry (see `training/general_window_logic.md`)

### Risk

A layer shows **risk** when transits afflict its ruler or undermine that house's poker function — e.g., discipline breakdown (6th/Moon), bankroll stress (2nd/Mars), spew impulse (5th/Mercury).

### Synthesis rules (from validated training)

| Condition | Assessment |
|-----------|------------|
| WIN (11th) support + DISCIPLINE (6th) clear | Strong window candidate |
| WIN support without DISCIPLINE | High-variance — not a full validated signature |
| PLAY (5th) aggression push + DISCIPLINE risk | Dangerous — spew pattern |
| BANKROLL (2nd) risk active | Tighter bankroll guidance regardless of other support |

A strong poker window requires **multiple layers working together**, not a single bright signal.

---

## Composite Winning Signature (Validated — Dzmitry, 3 wins)

```
luck + cognition + aggression + discipline + timing
```

| Component | Astro correlate (Dzmitry) |
|-----------|---------------------------|
| **Luck** | 11th house / Jupiter ruler — win and prize flow |
| **Cognition** | 3rd house / Venus ruler — reads and short-range strategy |
| **Aggression** | 5th house / Mercury ruler — game and controlled risk |
| **Discipline** | 6th house / Moon ruler — execution and tilt control |
| **Timing** | Exact and functional repeats across winning events |

No single house alone defines a winning window. The **combination** of all five components is the validated pattern.

Case detail: `cases/dzmitry_poker/repeated_patterns.md`

---

## Poker Window Output

Every poker window analysis must end with the **Practical Instruction Block**.

Template: `training/practical_instruction_block.md`

---

## Related

- `training/general_window_logic.md` — exact repeats, functional repeats, support, risk
- `training/hermes_training_summary_v1.md` — protocol summary
- `rules/house_ruler_mapping.yaml` — structured house/ruler data
- `rules/poker_window_rules.yaml` — window scoring references
