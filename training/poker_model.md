# Poker Model

House-ruler framework for poker timing and window analysis in Money Compass / Hermes.

---

## Overview

Poker is modeled through **six natal houses** and their **rulers**. Transits to these rulers and related cusps indicate support or risk for bankroll, reads, play quality, discipline, pot extraction, and winning outcomes.

Analysis priority (see `docs/operating_protocol.md`):

1. House-ruler layer
2. Key natal planets layer
3. Natal house placement of transits
4. Natal house cusp activation layer

---

## House Map

| House | Domain | Poker Function |
|-------|--------|----------------|
| **2nd** | Personal money, resources | Bankroll — chips behind, rebuy capacity, financial pressure |
| **3rd** | Communication, short trips, tactics | Tactical reading — hand reading, short-range strategy, table talk |
| **5th** | Play, risk, creativity | Game — gamble impulse, bold lines, creative aggression |
| **6th** | Daily work, routine, health | Discipline — execution quality, tilt control, stamina |
| **8th** | Shared resources, other people's money | The pot — value extraction, ICM pressure, opponent stacks |
| **11th** | Gains, hopes, networks | Win — prize, placement, favorable result |

---

## House Rulers (Dzmitry Reference Chart)

Rulers are **native-specific**. Example mapping from validated Dzmitry case:

| House | Ruler | Functional Role in Poker |
|-------|-------|--------------------------|
| 2nd | **Mars** | Bankroll aggression, stack pressure, fight for chips |
| 3rd | **Venus** | Smooth reads, social/table dynamics, tactical grace |
| 5th | **Mercury** | Game intellect, hand selection variance, quick adjustments |
| 6th | **Moon** | Emotional discipline, rhythm, tilt susceptibility |
| 8th | **Venus** | Pot value, extraction from others, shared-money flow |
| 11th | **Jupiter** | Win expansion, prize luck, favorable variance |

**Important:** Always resolve rulers from the native's chart. Do not copy Dzmitry's rulers to other natives.

Store per-native rulers in `cases/*/natal_reference.md` and `data/natal/`.

---

## Functional Interpretation Guide

### 2nd — Bankroll (Mars in Dzmitry case)

**Support signals:**
- Benefic transits to 2nd ruler (trine/sextile, tight orb)
- Transiting Jupiter/Venus supporting Mars
- Constructive Mars transits (controlled aggression, not reckless affliction)

**Risk signals:**
- Hard transits to 2nd ruler (square/opposition from Saturn, Neptune, afflicted Mars)
- Transiting malefics through natal 2nd without mitigation
- Bankroll stress → tighter play required regardless of other support

### 3rd — Tactical Reading (Venus)

**Support:** Mercury/Venus/Jupiter aiding 3rd ruler — clear reads, accurate ranging  
**Risk:** Neptune/Saturn afflictions — misreads, overconfidence in marginal spots

### 5th — Game / Risk (Mercury)

**Support:** Smart aggression, creative lines, favorable gamble spots  
**Risk:** Mercury afflicted — spew, unbalanced bluff frequency, boredom plays

### 6th — Discipline (Moon)

**Support:** Moon well-aspected — steady execution, emotional baseline  
**Risk:** Moon/Saturn/Neptune hard aspects — tilt, fatigue, routine breakdown

### 8th — Pot / OPM (Venus)

**Support:** Value-heavy spots, successful extraction, ICM-positive pressure  
**Risk:** Afflicted 8th ruler — overcommitting to pots, bad pot odds pursuit

### 11th — Win / Prize (Jupiter)

**Support:** Jupiter transits to 11th ruler or cusp — variance tilt favorable, deep runs  
**Risk:** Afflicted 11th — final-table friction, bubble bad beats without structural support

---

## Support vs. Risk Synthesis

A **strong poker window** typically shows:

| Layer | Minimum Requirement |
|-------|---------------------|
| WIN (11th) | At least one clear support signal |
| DISCIPLINE (6th) | No major active risk flag |
| BANKROLL (2nd) | Neutral or supportive |
| PLAY (5th) + READ (3rd) | At least one support for cognition/aggression balance |

A **weak or avoid window** often has:

- Strong 11th luck signal **without** 6th discipline support (boom-bust, tilt-prone)
- Active 2nd risk during aggressive 5th push (spew with stack pressure)
- Multiple hard aspects to rulers with no benefic mitigation

---

## Composite Signature (Validated)

From Dzmitry's three tournament wins:

```
luck (11th/Jupiter) + cognition (3rd/Mercury-Venus) + aggression (5th/Mars-Mercury)
+ discipline (6th/Moon) + timing (exact/functional repeats, tight orbs)
```

No single house alone defines a winning window. The **combination** matters.

---

## Related

- `training/general_window_logic.md`
- `training/practical_instruction_block.md`
- `rules/house_ruler_mapping.yaml`
- `rules/poker_window_rules.yaml`
- `cases/dzmitry_poker/repeated_patterns.md`
