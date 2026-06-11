# Case: Dimasik / Shay — Casino

Casino gambling case for session-based analysis. Complements poker model with chase-risk and session-limit patterns.

---

## Summary

| Field | Value |
|-------|-------|
| Subjects | Dimasik, Shay _(document relationship: same session / shared chart ref)_ |
| Context | Casino sessions (slots / table — specify in event_notes) |
| Primary method | Transit-to-natal |
| Secondary | Event chart (session start) — optional, separated |

---

## Purpose in Repository

- Validate **casino-specific** winning and risk signatures
- Contrast with poker model (less 3rd-house read layer, more 5th chase + 2nd bankroll)
- Feed `training/casino_model.md` with case evidence

---

## Files

| File | Purpose |
|------|---------|
| `natal_reference.md` | Chart data — one or two natives per setup |
| `event_notes.md` | Session chronology and outcomes |
| `winning_signatures.md` | Patterns present in winning sessions |
| `risk_signatures.md` | Patterns present in losing or chase-heavy sessions |

---

## Status

- [x] Case scaffold
- [ ] Natal data from engine
- [ ] Session events with outcomes
- [ ] Signature validation across 3+ sessions

---

## Usage

1. Record session in `event_notes.md`
2. Run transit-to-natal at session start
3. Tag configurations in winning/risk signature files
4. Cross-check `rules/risk_flags.yaml` for casino-specific flags
