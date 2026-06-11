# Practical Instruction Block

Standard final output section Hermes **must** produce at the end of every future poker-window analysis.

---

## Purpose

Convert astro synthesis into **actionable guidance** for the player. The practical block is user-facing product output — not optional commentary.

---

## Required Structure

Every poker window analysis ends with this block, in this order:

```markdown
---

## Practical Instruction Block

### How to play the window
[Specific tactical and strategic guidance tied to support signals]

### What to avoid
[Concrete behaviors, spot types, or session patterns tied to risk signals]

### Main edge
[Primary advantage this window provides — e.g., read clarity, aggression license, variance support]

### Main danger
[Primary failure mode — e.g., tilt, spew, bankroll overcommit]

### Best style of play
[e.g., tight-aggressive, controlled aggression, patience-first then push — linked to 3rd/5th/6th layers]

### Bankroll instruction
[Buy-in level, rebuy rules, stop-loss, session cap — linked to 2nd layer]

### Emotional-control instruction
[Tilt triggers to watch, break rules, discipline anchors — linked to 6th/Moon layer]
```

---

## Content Rules

1. **Ground every line in analysis above** — reference house layers or aspects when helpful, but keep language practical
2. **No vague luck language** — "play well" is insufficient; specify how
3. **Risk section is mandatory** even in strong windows — every window has a failure mode
4. **Bankroll and emotional sections are non-negotiable** — product safety requirements
5. **Tone:** direct, serious, no hype

---

## Example (Illustrative — replace with computed window)

```markdown
---

## Practical Instruction Block

### How to play the window
Push in spots where fold equity and hand reading align — 3rd-house support favors accurate ranging. Take calculated 5th-house gambles when stack depth allows; avoid marginal opens from early position.

### What to avoid
Do not chase pots when pot odds don't justify — 8th risk is elevated. No rebuy spirals if first buy-in busts in first hour. Avoid table talk that pulls you off rhythm.

### Main edge
Jupiter-supported 11th layer — variance favors deep runs. Reads are sharper than average; trust first instinct on opponent types.

### Main danger
Moon affliction to 6th ruler — tilt after bad beats. Aggression without discipline becomes spew.

### Best style of play
Controlled aggression: tight early, widen selectively in position mid-late. Pick spots; don't force action.

### Bankroll instruction
1.5× standard buy-in max. One rebuy only if stack lost to cooler (not spew). Hard stop at −2 buy-ins.

### Emotional-control instruction
Mandatory 10-minute walk after any hand where stack drops 30%+ from peak. No " revenge " spots. Set phone timer for session end before sitting down.
```

---

## Agent Prompt Reference

Use `prompts/practical_block_prompt.md` to generate this block consistently.

---

## Product Mapping (Future)

| Field | JSON key (proposed) |
|-------|---------------------|
| How to play | `play_guidance` |
| What to avoid | `avoid` |
| Main edge | `main_edge` |
| Main danger | `main_danger` |
| Best style | `style` |
| Bankroll | `bankroll` |
| Emotional control | `emotional_control` |

---

## Related

- `docs/output_templates.md`
- `training/general_window_logic.md`
- `prompts/practical_block_prompt.md`
