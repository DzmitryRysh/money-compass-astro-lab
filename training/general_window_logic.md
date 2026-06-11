# General Window Logic

How to evaluate timing windows for poker (and related money-risk contexts) using repeats, support, and risk.

---

## Core Insight

A good poker window is **not only luck**. It is a combination of:

| Factor | Role |
|--------|------|
| **Calculation** | Correct astro layering (protocol followed) |
| **Discipline** | 6th/Moon layer stable — tilt controlled |
| **Aggression** | 5th/Mars-Mercury — appropriate push |
| **Timing** | Exact/functional repeats, tight orbs, window clustering |
| **Bankroll control** | 2nd layer neutral or supported |
| **Winning support** | 11th/Jupiter and allied benefic flow |

Weak windows may show isolated luck (11th) without discipline or bankroll support — treat as high-variance, not validated pattern match.

---

## Pattern Types

### Exact Repeated Aspects

The same geometry recurs across winning events:

```
transiting [planet] → natal [target]: [same aspect type], orb within case threshold
```

**Strength:** Highest. Use for future window alerts and case matching.

**Documentation:** List in case `repeated_patterns.md` with tournament cross-reference.

---

### Functional Repeats

Different aspects that **activate the same functional layer**:

| Win A | Win B | Shared function |
|-------|-------|-----------------|
| trine to 11th ruler | Jupiter on 11th cusp | WIN layer |
| sextile to Mars (2nd ruler) | Venus trine 2nd cusp | BANKROLL layer |

**Strength:** High when functional mapping is explicit and case-validated.

---

### Contextual Support

Configurations that strengthen a window without being repeats:

- Transiting benefics in natal 11th or 5th
- Supportive house placement of key transits
- Key natal planets receiving tight trines/sextiles
- Absence of active flags from `rules/risk_flags.yaml`

**Effect:** Raises window score. Does not alone validate a pattern.

---

### Contextual Risks

Configurations that weaken or veto a window:

- Hard transits to 6th ruler during aggressive 5th activation
- 2nd ruler under Saturn/Neptune stress
- Multiple malefic hits to rulers without benefic mitigation
- See `rules/risk_flags.yaml` for structured flags

**Effect:** Lowers score or triggers avoid recommendation even if 11th looks bright.

---

## Window Scoring Flow

```
1. List exact aspects (Step 2 of protocol)
2. Match against case exact repeats → +weight
3. Match functional repeats → +weight
4. Add contextual support → +weight
5. Subtract contextual risks → -weight or veto
6. Check composite signature (luck+cognition+aggression+discipline+timing)
7. Generate practical instruction block
```

Weights defined in `rules/scoring_dimensions.yaml` and `rules/poker_window_rules.yaml`.

---

## Window Quality Tiers

| Tier | Criteria |
|------|----------|
| **Strong** | Multiple exact/functional repeats + support + no major risk flags + discipline layer clear |
| **Moderate** | Some support, no repeats, risks manageable |
| **Weak** | Isolated 11th or 5th signal, discipline or bankroll risk present |
| **Avoid** | Active veto flags or discipline/bankroll collapse with aggression push |

---

## Why Timing Matters

Repeats cluster in time. A future window that recreates Dzmitry's exact signatures on **12 Jun 2026** or **19 Jun 2026** (see `future_windows.md`) warrants full analysis — not because the date is magic, but because geometry may reactivate validated layers.

Always verify with engine output. Never assume a calendar date matches without computation.

---

## Related

- `docs/pattern_taxonomy.md`
- `training/practical_instruction_block.md`
- `prompts/poker_window_search.md`
- `cases/dzmitry_poker/repeated_patterns.md`
