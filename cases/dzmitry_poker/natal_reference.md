# Natal Reference — Dzmitry

Chart metadata and poker-relevant natal structure. **Populate degrees from engine export** in `data/natal/`.

---

## Birth Data

| Field | Value |
|-------|-------|
| Date | _YYYY-MM-DD_ |
| Time | _HH:MM (local)_ |
| Location | _City, Country_ |
| Timezone | _IANA e.g. Europe/Minsk_ |
| UTC datetime | _ISO 8601_ |
| House system | _Placidus / Whole Sign — document choice_ |

---

## Poker House Rulers (Validated Case Mapping)

| House | Sign on Cusp | Ruler | Poker Function |
|-------|----------------|-------|----------------|
| 2nd | _sign_ | **Mars** | Bankroll / personal money |
| 3rd | _sign_ | **Venus** | Tactical reading / short-range strategy |
| 5th | _sign_ | **Mercury** | Game / risk / play |
| 6th | _sign_ | **Moon** | Discipline / execution quality |
| 8th | _sign_ | **Venus** | Pot / other people's money |
| 11th | _sign_ | **Jupiter** | Win / prize / favorable result |

---

## Key Natal Planets

| Planet | Sign | Degree | House | Poker Relevance |
|--------|------|--------|-------|-----------------|
| Sun | | | | Core will, table presence |
| Moon | | | | Emotional baseline, 6th ruler |
| Mercury | | | | 5th ruler, hand logic |
| Venus | | | | 3rd & 8th ruler, reads + pot |
| Mars | | | | 2nd ruler, bankroll fight |
| Jupiter | | | | 11th ruler, win expansion |
| Saturn | | | | Structure, pressure tests |

---

## Natal Cusp Degrees (for cusp aspects)

| House | Cusp Sign | Cusp Degree |
|-------|-----------|-------------|
| 2nd | | |
| 3rd | | |
| 5th | | |
| 6th | | |
| 8th | | |
| 11th | | |

---

## Data Source

- Structured export: `data/natal/dzmitry.json` _(create from engine)_
- Last verified: _date_

---

## Notes

- Rulers above reflect training case — recompute if house system changes
- Never use this file's empty degree fields for analysis; engine output required
