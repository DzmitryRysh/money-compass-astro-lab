# Open Questions

Unresolved decisions blocking product integration or case validation.

---

## Protocol

- [ ] **Default house system for Money Compass product** — Placidus, Whole Sign, or user-selectable?
- [ ] **Event chart policy** — never in v1 product, or optional advanced mode only?
- [ ] **Applying vs separating orbs** — document when separation matters for window timing

---

## Scoring

- [ ] **Exact repeat orb threshold** — is 2° correct for all planets or planet-specific?
- [ ] **Minimum window score for user-facing "play" recommendation** — 50 moderate or 75 strong only?
- [ ] **Veto flag override** — can any support combination override DISCIPLINE_COLLAPSE?

---

## Cases

- [ ] **Dzmitry birth time accuracy** — Rodden rating? Rectification notes?
- [ ] **Dimasik / Shay** — one chart or two? Relationship to session outcomes?
- [ ] **Negative poker sessions** — do we have datetimes for non-winning events?

---

## Product

- [ ] **Engine location** — separate repo or monorepo with Money Compass?
- [ ] **Chart JSON schema** — who owns schema definition?
- [ ] **Anonymization** — how natal data in `data/natal/` is stored for git

---

## Technical (Engine & Verification)

Open questions following UTC and wrap-around house bug fixes (see `notes/experiments.md` EXP-004, EXP-005).

- [ ] **UTC conversion regression tests** — how to keep permanent automated tests for local → UTC conversion (Hollywood FL EDT/EST, other venues)?
- [ ] **Wrap-around house regression tests** — how to keep permanent tests for natal house assignment when cusps cross 360°/0° (validated case: House 1 Aquarius 10° → Aries 16°)?
- [ ] **External astroprocessor cross-check** — should future engine outputs always be compared against external astroprocessor samples for Moon and other fast-moving planets before interpretation?

---

## Resolved

| Question | Decision | Date |
|----------|----------|------|
| Primary method | Transit-to-natal | 2026-06-11 |
| Poker houses | 2/3/5/6/8/11 | 2026-06-11 |
| Practical block mandatory | Yes, all poker windows | 2026-06-11 |
| UTC verification | Local + UTC timestamps required in every technical verification | 2026-06-11 |
| Wrap-around houses | Two-part interval assignment when cusp span crosses 360°/0° | 2026-06-11 |
| Hollywood FL offsets | Summer EDT UTC−4; winter EST UTC−5 | 2026-06-11 |
