# Experiments

Active tests and hypotheses. **Not canonical** until promoted to cases or rules.

---

## Experiment Log

### EXP-001 — House system consistency

| Field | Value |
|-------|-------|
| Hypothesis | Placidus vs Whole Sign changes ruler hits minimally for Dzmitry WIN layer |
| Status | not started |
| Method | Recompute 3 tournament datetimes in both systems |
| Success criteria | Document if repeat catalog changes |
| Result | _pending_ |

---

### EXP-002 — Jun 2026 window cluster

| Field | Value |
|-------|-------|
| Hypothesis | 12 Jun and 19 Jun 2026 recreate functional WIN+DISCIPLINE repeats |
| Status | pending engine run |
| Method | `prompts/poker_window_search.md` on both dates |
| Files | `cases/dzmitry_poker/future_windows.md` |
| Result | _pending_ |

---

### EXP-003 — Orb threshold for exact repeat

| Field | Value |
|-------|-------|
| Hypothesis | 2° max orb (current rule) is correct for Jupiter→11th ruler repeats |
| Status | not started |
| Method | List orbs from all 3 wins; test 1° vs 2° vs 3° sensitivity |
| Result | _pending_ |

---

### EXP-004 — 11 June 2026 Moon mismatch (UTC conversion)

| Field | Value |
|-------|-------|
| Date | 2026-06-11 |
| Issue | Moon position did not match external astroprocessor |
| Root cause | Incorrect local time → UTC conversion (Hollywood, FL — EDT UTC−4 not applied correctly) |
| Fix | Corrected UTC conversion; Moon then matched external astroprocessor |
| Status | **complete — validated** |
| Promote to | `docs/operating_protocol.md`, `training/hermes_training_summary_v1.md` |
| Lesson | Always print local timestamp and UTC timestamp; Moon is first fast-body check for timestamp errors |

---

### EXP-005 — 21 November 2026 wrap-around house placement

| Field | Value |
|-------|-------|
| Date | 2026-11-21 |
| Issue | transiting Saturn and Neptune in Aries incorrectly placed in natal house 12 |
| Root cause | House-assignment logic did not handle wrap-around span (House 1 cusp Aquarius 10° → House 2 cusp Aries 16°) |
| Fix | Two-part interval logic: Aries 0°–16° → natal house 1 |
| Regression checks | transiting Saturn in Aries → natal house 1 ✓; transiting Neptune in Aries → natal house 1 ✓; transiting Pluto in Aquarius 3°23′ → natal house 12 ✓ (unchanged) |
| Status | **complete — validated** |
| Promote to | `docs/operating_protocol.md`, `training/hermes_training_summary_v1.md` |
| Lesson | Wrap-around cusp spans crossing 360°/0° must use two-part interval assignment |

---

## Template

```markdown
### EXP-XXX — Title
| Field | Value |
|-------|-------|
| Hypothesis | |
| Status | not started | in progress | complete | rejected |
| Method | |
| Result | |
| Promote to | [file if validated] |
```
