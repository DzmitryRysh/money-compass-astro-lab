# Prompt: Pattern Match Classification

Classify transit configurations against the pattern taxonomy.

---

## Instruction

Given a list of transit-to-natal aspects (already computed — do not recompute from memory), classify each significant configuration.

**Taxonomy:** `docs/pattern_taxonomy.md`

**Classes:**
- `exact_repeat` — same geometry as validated case event
- `functional_repeat` — different geometry, same house-ruler/layer function
- `contextual_support` — strengthens window without being a repeat
- `contextual_risk` — weakens window; may veto

**Domain tags:** BANKROLL, READ, PLAY, EXEC, POT, WIN, LUCK, EDGE, DISCIPLINE

---

## Output Format

For each configuration:

```markdown
### [short label]
- **Class:** exact_repeat | functional_repeat | contextual_support | contextual_risk
- **Aspect:** transiting X → natal Y (angle, orb)
- **Domain tags:** ...
- **Case evidence:** [case file, event]
- **Confidence:** high | medium | low
- **Notes:** ...
```

---

## Classification Rules

1. **Exact repeat** requires case documentation — do not invent repeats
2. **Functional repeat** requires explicit layer mapping (e.g., both activate WIN/11th)
3. **Contextual risk** with severity `veto` in `rules/risk_flags.yaml` overrides isolated WIN support
4. Confidence `high` only for engine-verified tight orbs (< 1°) with case cross-reference

---

## User Message Template

```
Classify these aspects for [native] poker window [datetime]:

[paste Step 2 aspect list]

Case reference: cases/dzmitry_poker/repeated_patterns.md
Risk reference: rules/risk_flags.yaml
```

---

## Summary Table (end of response)

| Config | Class | Tags | Confidence |
|--------|-------|------|------------|
| | | | |

**Composite signature present?** luck + cognition + aggression + discipline + timing — yes/partial/no
