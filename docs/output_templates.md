# Output Templates

Standard output structures for Hermes / Money Compass analyses.

---

## Full Transit-to-Natal Analysis

```markdown
# Analysis: [Subject] — [Datetime UTC]

## Metadata
- Native: [reference to natal_reference.md or data/natal/]
- Event: [description]
- House system: [Placidus | Whole Sign | ...]
- Method: TRANSIT-TO-NATAL

---

## 1. Raw Transit Positions
| Planet | Sign | Degree | Rx |
|--------|------|--------|-----|
| Sun | ... | ... | |
| Moon | ... | ... | |
| ... | | | |

---

## 2. Exact Aspects (transit → natal)
Sorted by orb (tightest first).

### To natal planets
- transiting [P] → natal [P]: [aspect] [orb]° — [deg details]

### To natal cusps
- transiting [P] → natal [H] cusp: [aspect] [orb]° — [deg details]

---

## 3. Natal House Placement of Transits
| Transiting Planet | Natal House |
|-------------------|-------------|
| ... | ... |

---

## 4. Interpretation

### 4.1 House-ruler layer
[2, 3, 5, 6, 8, 11 analysis]

### 4.2 Key natal planets
[Sun, Moon, Mercury, Venus, Mars, Jupiter, Saturn]

### 4.3 Support vs. risk summary
**Support:** ...
**Risk:** ...

### 4.4 Window assessment
- Exact repeats: ...
- Functional repeats: ...
- Overall quality: [strong | moderate | weak | avoid]

---

## 5. Practical Instruction Block
[See training/practical_instruction_block.md — mandatory for poker windows]
```

---

## Poker Window Search (Summary Form)

```markdown
# Poker Window: [Date range]

## Window score: [0–100 or tier]

## Top support signals
1. ...
2. ...

## Top risk signals
1. ...
2. ...

## Repeat match
- Exact: [yes/no — which case aspects]
- Functional: [yes/no — which layers]

## Practical Instruction Block
[full block]
```

---

## Event Chart Overlay (Optional Section)

```markdown
---

## EVENT CHART OVERLAY (OPTIONAL — SEPARATE)
**Chart:** [datetime, location]
**NOTE:** This section does not replace transit-to-natal conclusions.

### Event chart angles / houses
...

### Relevance to session
...
```

---

## Case Intake (New Event)

Use `cases/templates/case_template.md` as the base. Minimum sections:

1. Event metadata
2. Steps 1–3 (computed)
3. Outcome (win/loss/break-even + notes)
4. Pattern tags
5. Link to natal reference

---

## Pattern Classification Snippet

```markdown
| Aspect / Config | Class | Tags | Confidence |
|-----------------|-------|------|------------|
| transiting Jup → natal 11th ruler | exact_repeat | WIN, LUCK | high |
| transiting Sat → natal Moon | contextual_risk | DISCIPLINE | medium |
```

---

## Related

- `docs/operating_protocol.md`
- `training/practical_instruction_block.md`
- `prompts/practical_block_prompt.md`
