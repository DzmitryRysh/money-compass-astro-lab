# Prompt: Poker Window Search

Discover and score poker timing windows for a native against validated case patterns.

---

## Instruction

Search for poker window quality at the given datetime(s) for the specified native.

**Method:** transit-to-natal only (strict protocol).

**Domain model:** `training/poker_model.md` — houses 2, 3, 5, 6, 8, 11 and their rulers.

**Pattern matching:** Compare against `cases/[native]_poker/repeated_patterns.md` and `rules/poker_window_rules.yaml`.

**Output must include:**
1. Full protocol Steps 1–4 (or reference prior computation)
2. Exact repeat matches (if any)
3. Functional repeat matches (if any)
4. Contextual support and risk lists
5. Window score / tier (strong | moderate | weak | avoid)
6. **Practical Instruction Block** (mandatory)

---

## User Message Template

```
Poker window search:
- Native: [name]
- Datetime(s) UTC: [list]
- Case reference: cases/dzmitry_poker/repeated_patterns.md
- Compare checkpoints: [session start / +6h / final table]

Score window and output practical instruction block.
```

---

## Scoring Procedure

1. Complete Steps 1–3 (compute)
2. For each ruler of 2/3/5/6/8/11, list active transit aspects
3. Match to case exact repeats → apply weight from `rules/scoring_dimensions.yaml`
4. Match functional repeats → apply weight
5. Add contextual support; subtract risks from `rules/risk_flags.yaml`
6. Check composite signature: luck + cognition + aggression + discipline + timing
7. Assign tier; generate practical block via `prompts/practical_block_prompt.md`

---

## Multi-Date Search

When scanning a date range:

```markdown
| Datetime UTC | Score | Tier | Top repeat match |
|--------------|-------|------|------------------|
| ... | | | |
```

Recommend best window with rationale — not every date is equal.

---

## References

- `training/general_window_logic.md`
- `cases/dzmitry_poker/future_windows.md`
- `prompts/pattern_match_classification.md`
