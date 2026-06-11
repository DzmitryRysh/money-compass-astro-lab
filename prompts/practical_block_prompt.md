# Prompt: Practical Block Generation

Generate the mandatory Practical Instruction Block for poker window analyses.

---

## Instruction

Using the completed interpretation above, produce the **Practical Instruction Block** exactly as defined in `training/practical_instruction_block.md`.

**Required sections (in order):**

1. How to play the window
2. What to avoid
3. Main edge
4. Main danger
5. Best style of play
6. Bankroll instruction
7. Emotional-control instruction

---

## Rules

- Every bullet must trace to a support or risk signal from the analysis
- Use direct, practical language — no astro jargon in user-facing lines unless briefly clarifying
- Include bankroll and emotional-control even for strong windows
- No hype ("guaranteed win", "cannot lose")
- Tone: serious, structured, actionable

---

## User Message Template

```
Generate practical instruction block for:
- Native: [name]
- Window tier: [strong | moderate | weak | avoid]
- Top support: [list]
- Top risks: [list]
- House layers: 2nd [...], 3rd [...], 5th [...], 6th [...], 8th [...], 11th [...]
```

---

## Weak / Avoid Tier Guidance

For **weak** or **avoid** windows:

- "How to play" may be "Do not play" or "Observe only / study session"
- Bankroll instruction emphasizes **zero or minimal exposure**
- Emotional-control focuses on **not entering** or **leaving early**

---

## Reference

Full template: `training/practical_instruction_block.md`
