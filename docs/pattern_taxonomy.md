# Pattern Taxonomy

Classification system for recurring astrology signatures in Money Compass gambling contexts.

---

## Pattern Classes

### 1. Exact Repeated Aspects

The **same** transit-to-natal aspect (same transiting planet, same natal target, same aspect type) appears across multiple winning events within tolerable orb variance.

**Example signature:**
```
transiting Jupiter → natal 11th ruler (exact trine, orb < 1°)
```

**Use:** Highest-confidence pattern tier. Flag in case `repeated_patterns.md` and `rules/poker_window_rules.yaml`.

---

### 2. Functional Repeats

Different astronomical configurations that **functionally activate the same house-ruler or domain layer**.

**Example:**
- Win A: `transiting Venus → natal Mars` (2nd ruler)
- Win B: `transiting Jupiter → natal 2nd cusp`

Both support bankroll / personal-money layer without identical geometry.

**Use:** Second-tier confidence. Requires explicit functional mapping in analysis.

---

### 3. Contextual Support

Configurations that ** reinforce** the primary winning pattern without being repeats:

- Supportive natal house placement of transits (e.g., transiting Jupiter in natal 11th)
- Tight supportive aspects to key natal planets
- Absence of major risk flags during the window

**Use:** Window quality uplift. Document as `SUPPORT:` prefixed items.

---

### 4. Contextual Risks

Configurations that ** weaken or contradict** the primary pattern:

- Hard aspects to 6th ruler (discipline breakdown)
- Afflictions to 2nd ruler (bankroll stress)
- Malefic transits through natal 8th without extraction support

**Use:** Window quality downgrade or veto. Document as `RISK:` prefixed items.

---

## Domain Tags

| Tag | Meaning |
|-----|---------|
| `BANKROLL` | 2nd house / ruler |
| `READ` | 3rd house / ruler — tactical cognition |
| `PLAY` | 5th house / ruler — game/risk |
| `EXEC` | 6th house / ruler — discipline |
| `POT` | 8th house / ruler — other people's money |
| `WIN` | 11th house / ruler — prize/result |
| `LUCK` | Jupiter, benefic flow, 11th activation |
| `EDGE` | Mercury/Venus tactical support |
| `DISCIPLINE` | Moon/6th/Saturn constructive aspects |

---

## Composite Winning Signature (Poker)

Validated across Dzmitry case (3 tournament wins):

```
luck + cognition + aggression + discipline + timing
```

| Component | Astro Correlates |
|-----------|------------------|
| Luck | 11th/Jupiter support, benefic flow |
| Cognition | 3rd ruler, Mercury, tight read windows |
| Aggression | Mars, 5th activation, controlled push |
| Discipline | 6th/Moon, absence of discipline risk flags |
| Timing | Exact repeats, tight orbs, window clustering |

A window with strong luck but weak discipline is **not** equivalent to a validated winning window.

---

## Classification Output Format

When classifying a configuration:

```markdown
### [Pattern Name]
- **Class:** exact_repeat | functional_repeat | contextual_support | contextual_risk
- **Domain tags:** WIN, DISCIPLINE, ...
- **Aspect:** transiting X → natal Y (angle, orb)
- **Confidence:** high | medium | low
- **Case evidence:** dzmitry_poker/tournament_N, ...
```

---

## Promotion Path

```
notes/experiments.md  →  case file  →  repeated_patterns.md  →  rules/*.yaml
```

A pattern is **canonical** only after appearing in `repeated_patterns.md` or `winning_signatures.md` and being referenced in rules YAML.

---

## Related

- `training/general_window_logic.md`
- `rules/scoring_dimensions.yaml`
- `rules/risk_flags.yaml`
- `prompts/pattern_match_classification.md`
