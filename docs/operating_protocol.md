# Operating Protocol

The strict compute-first workflow for all Hermes / Money Compass astrology analysis.

---

## Core Principle

**Compute before interpret.** Never lead with narrative. Always establish astronomical facts first, then derive meaning in a fixed order.

**Primary method:** transit-to-natal.  
**Secondary (optional):** event chart — only when explicitly requested or relevant, and always separated.

---

## Workflow (Strict Order)

### Step 1 — Raw Transit Positions

Record transiting planet positions at the target datetime:

- Sign and degree (e.g., `transiting Mars 14°32′ Scorpio`)
- Retrograde status if applicable
- Source datetime and timezone (UTC preferred for storage)

**Role:** Ground truth. All subsequent steps depend on these positions. If positions are wrong, everything downstream is invalid.

Do not interpret at this step. List only.

---

### Step 2 — Exact Aspects with Orbs

For each transiting planet, calculate aspects to:

- Natal planets
- Natal house cusps (when relevant to the analysis domain)

**Notation:**

```
transiting Mars → natal Venus
  transiting: 14°32′ Scorpio
  natal:      12°18′ Taurus
  aspect:     opposition (180°)
  orb:        2°14′
```

```
transiting Jupiter → natal 11th cusp
  transiting: 22°05′ Gemini
  natal cusp: 21°40′ Sagittarius
  aspect:     opposition (180°)
  orb:        0°25′
```

**Rules:**

- Use exact angle (0°, 60°, 90°, 120°, 180°) and orb for every listed aspect
- Sort by tightness (smallest orb first) within each category
- Distinguish applying vs. separating when orb is meaningful for timing

Do not interpret at this step. List only.

---

### Step 3 — Natal House Placement of Transits

For each transiting planet, record which **natal house** it occupies at the event datetime.

```
transiting Mercury → natal 5th house (whole-sign or system used: document which)
transiting Moon   → natal 6th house
```

**Role:** Context layer — where transiting energy lands in the native's life structure.

Document the house system used (Placidus, Whole Sign, etc.) and keep it consistent within a case.

---

### Step 4 — Interpretation (Last)

Only after Steps 1–3 are complete:

1. Apply domain model (poker, casino, general money)
2. Evaluate house-ruler layer
3. Evaluate key natal planets
4. Classify support vs. risk
5. Synthesize window quality
6. Output practical instruction block (poker windows)

**Never skip to interpretation because an aspect "feels" significant.**

---

## Interpretation Priority (Poker Windows)

When analyzing poker timing windows, apply layers in this order:

| Priority | Layer | Description |
|----------|-------|-------------|
| 1 | House-ruler layer | Transits to rulers of 2, 3, 5, 6, 8, 11 |
| 2 | Key natal planets layer | Sun, Moon, Mercury, Venus, Mars, Jupiter, Saturn as natal anchors |
| 3 | Natal house placement of transits | Where transiting planets sit in natal houses |
| 4 | Natal house cusp activation | Transits aspecting natal cusps (especially 2, 5, 8, 11) |

Lower priority does not mean irrelevant — it means house-ruler and key planet signals outweigh generic cusp hits unless orbs are extremely tight.

---

## Transit-to-Natal vs. Event Chart

### Transit-to-Natal (Primary)

- Compare **current sky** to **native's birth chart**
- Used for: "How does this moment activate this person's chart?"
- Default for all Money Compass window analysis

### Event Chart (Optional)

- Chart cast for **event datetime + location** (e.g., tournament start)
- Used for: situational overlay, venue/ timing context
- **Never substitute for transit-to-natal** in window scoring

### Separation Rule

If both are used, structure output as:

```markdown
## TRANSIT-TO-NATAL ANALYSIS
[primary content]

---

## EVENT CHART OVERLAY (OPTIONAL — SEPARATE)
Chart: [datetime, location]
NOTE: This section does not replace transit-to-natal conclusions.
[event chart content]
```

Mixing event-chart conclusions into transit-to-natal sections without labeling is a **protocol violation**.

---

## Notation Checklist

Before finalizing any analysis:

- [ ] All aspects use `transiting X → natal Y` or `transiting X → natal [N] cusp`
- [ ] Degrees and orbs present for listed aspects
- [ ] House system documented
- [ ] Interpretation appears after computed lists
- [ ] Event chart (if any) is in a labeled separate section
- [ ] Practical instruction block included (poker windows)

---

## Engine vs. Agent Boundary

| Responsibility | Owner |
|----------------|-------|
| Ephemeris, positions, aspects, orbs, cusps | Deterministic engine |
| Listing Steps 1–3 in correct order | Agent (from engine output) |
| Pattern matching, case recall, practical synthesis | Agent (from this repo) |
| Inventing positions or aspects | **Forbidden** |

If engine output is unavailable, state that explicitly. Do not estimate planetary positions.

---

## Related Documents

- `training/poker_model.md` — house-ruler functional model
- `training/general_window_logic.md` — repeats, support, risk
- `training/practical_instruction_block.md` — mandatory output footer
- `prompts/strict_transit_to_natal.md` — agent prompt for this protocol
