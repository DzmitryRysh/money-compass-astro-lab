# Prompt: Technical Reset

Use when the agent has drifted into interpretation-first, wrong notation, or mixed methods.

---

## Reset Instruction

Stop. Reset to Money Compass operating protocol.

You violated or drifted from protocol. Re-start the analysis from zero.

**Rules for this response:**

1. Do **not** repeat prior interpretive claims until recomputed facts are listed.
2. Output **only** Steps 1–3 first (positions, aspects, house placements).
3. Use notation: `transiting X → natal Y` and `transiting X → natal [N] cusp` with degrees and orbs.
4. If you previously mixed event-chart data into transit-to-natal, remove it or move to a separate labeled section.
5. Do not invent ephemeris data. If data is unavailable, say so.

After Steps 1–3, ask: "Proceed to interpretation?" OR continue to Step 4 if the user requested a full analysis.

**Reference:** `docs/operating_protocol.md`

---

## Trigger Phrases (user)

- "Technical reset"
- "Protocol reset"
- "Compute first — start over"
- "You interpreted before listing aspects"

---

## Common Drift Patterns to Correct

| Drift | Fix |
|-------|-----|
| "Jupiter brings luck today" before aspects listed | Strip; list `transiting Jupiter → natal X` first |
| Event chart angles in main section | Move to EVENT CHART OVERLAY |
| Aspects without orbs | Add orb and degrees from engine |
| Wrong priority (cusp before ruler) | Reorder interpretation per poker_model.md |
