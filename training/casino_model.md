# Casino Model

House-ruler and session framework for casino gambling analysis (slots, table games, mixed sessions).

---

## Scope

Casino analysis differs from poker:

| Factor | Poker | Casino |
|--------|-------|--------|
| Opponent skill | High | Low / none |
| Decision density | Continuous | Bursty or passive |
| Bankroll arc | Hours, rebuys | Session-based |
| Primary houses | 2/3/5/6/8/11 | 2/5/8/11 + venue/timing overlay |

Transit-to-natal remains primary. Event chart (session start) may add context but must stay separated.

---

## Core Houses (Casino)

| House | Function |
|-------|----------|
| **2nd** | Session bankroll, stop-loss discipline |
| **5th** | Risk appetite, chase impulse, "one more spin" |
| **8th** | House edge context, other-entity money flow |
| **11th** | Win outcome, favorable variance |
| **6th** | Session stamina, walk-away discipline |

3rd house (reads) is less central unless table games with dealer/player dynamics matter.

---

## Session Analysis Layers

1. **Pre-session window** — transits at planned start time
2. **Peak risk interval** — transits during known high-chase periods
3. **Walk-away signal** — discipline transits (6th) degrading

---

## Winning Signatures (Starter)

See `cases/dimasik_shay_casino/winning_signatures.md` for case-specific patterns.

General support indicators:

- Jupiter/Venus supporting 11th ruler or cusp
- Constructive 5th activation without Neptune fog
- 2nd ruler supported — bankroll holds
- 6th/Moon stable — session limits respected

---

## Risk Signatures (Starter)

See `cases/dimasik_shay_casino/risk_signatures.md`.

General risk indicators:

- Neptune afflicting 5th or 2nd ruler (chase, denial)
- Saturn hard aspect to 11th ruler ( grinding loss, false hope)
- Moon afflicted in 6th context (emotional session extension)
- Strong 5th push without 11th support (entertainment spend, not edge)

---

## Output Requirements

Casino sessions do not use the full poker practical block but should include:

- Session recommendation (play / short session / avoid)
- Bankroll cap guidance
- Chase-risk warning if 5th/Neptune risk active
- Walk-away time or transit-based stop trigger if applicable

---

## Related

- `cases/dimasik_shay_casino/`
- `rules/risk_flags.yaml`
- `docs/operating_protocol.md`
