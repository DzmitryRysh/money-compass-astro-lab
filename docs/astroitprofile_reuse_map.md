# AstroITProfile → Astro Engine v1 Reuse Map

Practical migration guide comparing existing code in **AstroITProfile** (`github.com/DzmitryRysh/AstroITProfile`) with the target design in **`docs/astro_engine_v1_spec.md`**.

**Status:** Planning — no code changes yet  
**Source repo path (local):** `C:\Users\cools\OneDrive\Documents\GitHub\AstroITProfile`

---

## Summary verdict

| Category | Assessment |
|----------|------------|
| **Reuse almost directly** | `_julian_day_utc`, `_planet_lon_ut`, `_house_index_from_lon`, cusp normalization from `swe.houses_ex`, `to_utc_birth_moment` pattern |
| **Adapt** | Timezone resolution, house/cusp functions (generalize to all 12 cusps + all bodies), FastAPI route → `engine.compute_facts` |
| **Do not carry over** | `astro.py` sun-sign tables, `it_profile.py`, `day_night.py`, city-only `places.json` lookup as primary input |

AstroITProfile is a **partial seed** for Layer 1 — not a drop-in engine.

---

## 1. Reuse almost directly

These functions are deterministic, Swiss-Ephemeris-backed, and align with v1 spec requirements (especially wrap-around house placement).

| Source | Function / block | Target module | Notes |
|--------|------------------|---------------|-------|
| `astro_calc.py` | `_julian_day_utc(dt_utc)` | `ephemeris.py` | Same `swe.julday` + GREG_CAL pattern; shared by all body/cusp calls |
| `astro_calc.py` | `_planet_lon_ut(jd_ut, planet)` | `ephemeris.py` | Core longitude helper; extend to return speed + retrograde from `swe.calc_ut` flags |
| `astro_calc.py` | `_sign_from_lon(lon)` + `SIGNS` | `ephemeris.py` or shared `constants.py` | Sign/degree formatting for JSON output |
| `astro_calc.py` | `_house_index_from_lon(lon, cusps)` | `houses.py` | **Critical reuse** — already implements two-part wrap-around (EXP-005) |
| `astro_calc.py` | Cusp normalization block (lines 67–73, 92–97) | `houses.py` | Handles `swe.houses_ex` returning length 12 or 13 |
| `astro_calc.py` | `swe.houses_ex(jd, lat, lon, house_system, swe.FLG_SWIEPH)` | `houses.py` | Placidus `b'P'` today; v1 makes `house_system` configurable input |
| `timezones.py` | `to_utc_birth_moment(...)` | `time_utils.py` | Rename/generalize to birth + analysis moments |
| `timezones.py` | `BirthMoment` dataclass | `schemas.py` | Extend with verification note fields |

**Confidence:** High — port with tests, not redesign.

---

## 2. Adapt before reuse

| Source | What exists | Required adaptation for v1 |
|--------|-------------|----------------------------|
| `timezones.py` | `timezone_name_from_coords(lat, lon)` via `timezonefinder` | v1 spec prefers **explicit IANA timezone** on input; keep coord→tz as optional helper only |
| `timezones.py` | Birth-only UTC conversion | Add **analysis moment** conversion (poker window datetime); birth tz ≠ event tz |
| `timezones.py` | No verification output | Emit `verification_notes`: offset string (EDT UTC−4 / EST UTC−5), DST flag |
| `astro_calc.py` | `calc_mercury_sign` | Replace with generic `body_position(body, utc_dt)` for all 10 v1 bodies |
| `astro_calc.py` | `calc_uranus_house` | Generalize to `house_placement(body_lon, cusps)` for **every** transiting body |
| `astro_calc.py` | `calc_house_signs` (6th/10th only) | Replace with `compute_natal_cusps()` returning **all 12** cusps with longitude + sign |
| `astro_calc.py` | Hardcoded `house_system=b'P'` | Read from `EngineInput.house_system`; map string → `bytes` for `swe.houses_ex` |
| `places.py` | `find_coordinates(place)` → JSON lookup | v1 requires **explicit lat/lon** on input; keep places helper for UX, not engine core |
| `profile.py` route | Orchestrates birth → IT profile | Replace with `engine.compute_facts()` → facts JSON (no IT logic) |
| `schemas/profile.py` | `ProfileRequest` (date, time, place string) | Replace with `EngineInput` per v1 spec (birth + analysis + config) |

---

## 3. Do not carry over

| Source | Reason |
|--------|--------|
| `astro.py` — `get_sun_sign(date)` | Date-range table, not Swiss Ephemeris; contradicts deterministic engine principle |
| `it_profile.py` — `build_it_profile(...)` | Layer 3/4 business logic; belongs outside engine |
| `day_night.py` — `is_day_chart(local_dt)` | Fixed 06:00–18:00 heuristic; not astronomical; not in v1 spec |
| `app/api/routes/profile.py` response shape | IT archetype output, not facts JSON |
| City-only input without coordinates | Ambiguous; spec requires documented lat/lon for cusp reproducibility |
| README claim “all via Swiss Ephemeris” for Sun | Sun currently bypasses ephemeris — do not replicate |

---

## 4. Mapping table: old → new module layout

Target layout from `docs/astro_engine_v1_spec.md`:

```
astro_engine/
├── schemas.py
├── time_utils.py
├── ephemeris.py
├── natal.py
├── houses.py
├── aspects.py
├── rulership.py
├── tags.py
└── engine.py
```

### Full mapping

| AstroITProfile | Function / artifact | v1 module | Action |
|----------------|---------------------|-----------|--------|
| `timezones.py` | `BirthMoment` | `schemas.py` | Adapt → `TimeMoment` + output fields |
| `timezones.py` | `to_utc_birth_moment` | `time_utils.py` | Adapt → `to_utc_moment(local_dt, tz_name)` |
| `timezones.py` | `timezone_name_from_coords` | `time_utils.py` | Optional helper; not primary path |
| — | *(missing)* | `time_utils.py` | **New:** verification note builder (offset, DST) |
| `astro_calc.py` | `_julian_day_utc` | `ephemeris.py` | Reuse directly |
| `astro_calc.py` | `_planet_lon_ut` | `ephemeris.py` | Reuse; extend return type |
| `astro_calc.py` | `_sign_from_lon`, `SIGNS` | `ephemeris.py` | Reuse directly |
| `astro_calc.py` | `calc_mercury_sign` | `ephemeris.py` | Replace → `transit_positions(all bodies)` |
| — | *(missing)* | `ephemeris.py` | **New:** retrograde + speed from `swe.calc_ut` |
| `astro_calc.py` | `swe.houses_ex` + cusp normalize | `houses.py` | Extract → `compute_cusps(jd, lat, lon, house_system)` |
| `astro_calc.py` | `_house_index_from_lon` | `houses.py` | Reuse directly |
| `astro_calc.py` | `calc_uranus_house` | `houses.py` | Adapt → `assign_house(lon, cusps)` + batch for all bodies |
| `astro_calc.py` | `calc_house_signs` | `houses.py` | Adapt → full `natal_cusps[]` output |
| — | *(missing)* | `houses.py` | **New:** `wrap_segment` label in output when placement uses wrap |
| — | *(missing)* | `natal.py` | **New:** natal planet longitudes at birth UTC + birth coordinates |
| — | *(missing)* | `aspects.py` | **New:** separation, aspect type, orb, filter (max 4° default) |
| — | *(missing)* | `rulership.py` | **New:** traditional rulers for houses 2,3,5,6,8,11 |
| — | *(missing)* | `tags.py` | **New:** optional structural tags (`BANKROLL`, `WIN`, …) |
| — | *(missing)* | `schemas.py` | **New:** `EngineInput`, `FactsOutput` per v1 JSON contract |
| `profile.py` route | orchestration | `engine.py` | Rewrite → `compute_facts(input) -> FactsOutput` |
| `places.py` | `find_coordinates` | *(outside engine)* | Product/UX layer only |
| `it_profile.py` | all | *(none)* | Do not migrate |
| `astro.py` | `get_sun_sign` | *(none)* | Do not migrate |

---

## 5. Module-by-module: what goes where

### `time_utils.py`

**From AstroITProfile:**

| Import | Source lines | v1 role |
|--------|--------------|---------|
| `to_utc_birth_moment` | `timezones.py:25–29` | Core local → UTC via `ZoneInfo` |
| `timezone_name_from_coords` | `timezones.py:18–22` | Optional fallback when tz not supplied |

**Add (not in AstroITProfile):**

- `format_verification_notes(local_dt, utc_dt, tz_name)` → e.g. `"America/New_York EST (UTC-5) applied for 2026-11-21"`
- Separate helpers: `birth_utc_moment(...)` and `analysis_utc_moment(...)` — same math, different inputs
- Hollywood FL regression cases (EXP-004): summer EDT UTC−4, winter EST UTC−5

**Does not belong here:** Julian day, ephemeris, cusps.

---

### `houses.py`

**From AstroITProfile:**

| Import | Source | v1 role |
|--------|--------|---------|
| `_house_index_from_lon` | `astro_calc.py:35–58` | Assign any longitude to house 1–12 with wrap-around |
| Cusp normalization | `astro_calc.py:67–73` | Normalize `swe.houses_ex` 12/13-length arrays |
| `swe.houses_ex(...)` call | `astro_calc.py:65, 89` | Deterministic cusp source of truth |

**Adapt:**

- `calc_uranus_house` → `def house_for_longitude(lon: float, cusps: list[float]) -> int` (public)
- `calc_house_signs` → `def compute_natal_cusps(jd, lat, lon, house_system) -> list[CuspRecord]` (all 12 houses)
- `house_system`: parameterize (`placidus` → `b'P'`, document mapping)
- Add `wrap_segment` metadata when `start > end` for house N (verification_notes)

**Does not belong here:** Planet positions (except using cusps for placement input), aspects, rulers.

**Cusp source of truth in v1:** `compute_natal_cusps()` wrapping `swe.houses_ex` — same as AstroITProfile, generalized.

---

### `ephemeris.py`

**From AstroITProfile:**

| Import | Source | v1 role |
|--------|--------|---------|
| `_julian_day_utc` | `astro_calc.py:17–20` | UTC datetime → Julian day |
| `_planet_lon_ut` | `astro_calc.py:29–33` | Body longitude at JD |
| `_sign_from_lon` | `astro_calc.py:12–14` | Sign name from longitude |
| `swe.calc_ut` usage | `astro_calc.py:25–26, 31` | Pattern for all bodies |

**Adapt:**

- `calc_mercury_sign` → `def body_state(jd, body_id) -> BodyRecord` with `longitude_deg`, `sign`, `degree`, `retrograde`, `speed_deg_per_day`
- Define v1 body set: Sun, Moon, Mercury, Venus, Mars, Jupiter, Saturn, Uranus, Neptune, Pluto
- `transit_positions`: all bodies at **analysis** UTC
- Natal planets: same helpers at **birth** UTC (called from `natal.py`)

**Does not belong here:** House cusps, aspects, timezone.

---

### `aspects.py`

**From AstroITProfile:** *Nothing — aspects not implemented (README: planned).*

**Build new:**

| Function | v1 spec requirement |
|----------|---------------------|
| `separation_deg(lon_a, lon_b)` | Shortest arc 0–180° |
| `aspect_type(separation, orb_max)` | Map to conjunction/sextile/square/trine/opposition |
| `orb_deg(separation, exact_angle)` | **Not** the same as separation |
| `transit_natal_aspects(transit_bodies, natal_bodies, orb_max)` | `planet_aspects[]` |
| `transit_cusp_aspects(transit_bodies, cusps, houses, orb_max)` | `cusp_aspects[]` for poker houses 2,3,5,6,8,11 |
| Sort by `orb_deg` ascending | Operating protocol |

Default `orb_max`: **4°** (v1 spec); tighter filters live in pattern layer (`rules/poker_window_rules.yaml`).

---

### `engine.py`

**From AstroITProfile:**

| Import | Source | v1 role |
|--------|--------|---------|
| Orchestration pattern only | `profile.py:22–56` | Sequence: coords → tz → UTC → calc → response |

**Replace pipeline with:**

```
compute_facts(input: EngineInput) -> FactsOutput:

  1. time_utils  → birth UTC + analysis UTC + verification_notes
  2. ephemeris   → natal planets (birth UTC) + transit positions (analysis UTC)
  3. houses      → natal_cusps (birth jd, birth lat/lon) + house_placements (transits)
  4. aspects     → planet_aspects + cusp_aspects
  5. rulership   → house_rulers for poker houses
  6. tags        → derived_tags (optional)
  7. schemas     → assemble FactsOutput JSON
```

**Does not belong here:** IT profile, interpretation, window tier scoring, practical blocks.

---

### Other v1 modules (no AstroITProfile source)

| Module | Purpose |
|--------|---------|
| `natal.py` | Glue: birth inputs → natal planet longitudes via `ephemeris` |
| `rulership.py` | Sign on cusp → traditional ruler; resolve ruler longitude from natal planets |
| `tags.py` | Optional `BANKROLL`, `WIN`, etc. from structural hits — no interpretation |
| `schemas.py` | Pydantic models matching v1 JSON contract |

---

## 6. Line-by-line: `astro_calc.py` → v1

| Lines | Code | Destination | Action |
|-------|------|-------------|--------|
| 5–9 | `SIGNS` | `ephemeris.py` | Copy |
| 12–14 | `_sign_from_lon` | `ephemeris.py` | Copy |
| 17–20 | `_julian_day_utc` | `ephemeris.py` | Copy |
| 23–27 | `calc_mercury_sign` | — | **Drop** — superseded by generic body calc |
| 29–33 | `_planet_lon_ut` | `ephemeris.py` | Copy + extend |
| 35–58 | `_house_index_from_lon` | `houses.py` | Copy + unit test (EXP-005) |
| 60–76 | `calc_uranus_house` | `houses.py` | Refactor: extract `compute_cusps` + `house_for_longitude` |
| 79–106 | `calc_house_signs` | `houses.py` | Refactor: return all 12 cusps, not just 6/10 signs |

---

## 7. Line-by-line: `timezones.py` → v1

| Lines | Code | Destination | Action |
|-------|------|-------------|--------|
| 11–15 | `BirthMoment` | `schemas.py` | Rename; add `verification_notes: list[str]` |
| 18–22 | `timezone_name_from_coords` | `time_utils.py` | Optional helper |
| 25–29 | `to_utc_birth_moment` | `time_utils.py` | Generalize name; used for birth and analysis |

---

## 8. Migration risks

| Risk | Detail | Mitigation |
|------|--------|------------|
| **Birth tz inferred from coords only** | AstroITProfile derives tz from birth lat/lon; analysis event may use different tz (Hollywood FL vs birth city) | v1: explicit `analysis_timezone` on input |
| **No tests in AstroITProfile** | Wrap-around and UTC logic unverified in CI | Port functions with EXP-004 / EXP-005 golden tests first |
| **Placidus hardcoded** | `b'P'` only | Configurable `house_system`; echo in output |
| **Cusp duplication** | `calc_uranus_house` and `calc_house_signs` each call `swe.houses_ex` | v1 engine: compute cusps **once** per chart moment |
| **Sun sign table** | Easy to accidentally reintroduce from `astro.py` | Ban date-table positions in engine; ephemeris only |
| **13 vs 12 cusp array** | Normalization logic must stay | Keep normalization block verbatim; test both shapes |
| **Boundary condition `lon < end`** | Wrap segment uses `lon >= start or lon < end` | Test Aries 0°, 16°, 16.01° edge cases |
| **External engine mismatch** | “Placidus” label insufficient without matching inputs | Store all settings in output; cross-check Moon vs astroprocessor |
| **places.json gaps** | Hollywood FL not in list | Do not depend on city JSON for engine core |

---

## 9. Missing capabilities (AstroITProfile → v1 gap)

| v1 requirement | AstroITProfile status |
|----------------|----------------------|
| All 10 transit body positions | Only Mercury + Uranus longitude used |
| All 12 natal cusps in output | Only 6th/10th cusp signs |
| Transit house placements (all bodies) | Only Uranus |
| Transit → natal planet aspects | Not implemented |
| Transit → cusp aspects | Not implemented |
| Orb calculation / filtering | Not implemented |
| House-ruler mapping | Not implemented |
| Analysis datetime (non-birth) | Not implemented |
| Facts JSON contract | IT profile response only |
| `verification_notes` in output | UTC computed but not exposed |
| Regression test suite | No `tests/` directory |
| Configurable orb / house system / zodiac mode | Hardcoded or implicit |
| `natal.py` / `rulership.py` / `aspects.py` / `tags.py` | Do not exist |

---

## 10. Recommended first extraction order

Aligns with `docs/astro_engine_v1_spec.md` §12 and minimizes risk.

| Step | Extract / build | Source | Validation |
|------|-----------------|--------|------------|
| **1** | `time_utils.py` | `timezones.py` | EXP-004: 11 Jun 2026 Moon UTC; Hollywood EDT/EST |
| **2** | `houses.py` — `_house_index_from_lon` + cusp normalize + `compute_natal_cusps` | `astro_calc.py` | EXP-005: Saturn/Neptune Aries → H1; Pluto → H12 |
| **3** | `ephemeris.py` — `_julian_day_utc`, `_planet_lon_ut`, all bodies | `astro_calc.py` | Moon vs external astroprocessor |
| **4** | `aspects.py` | New | Known angle pairs; orb ≠ separation |
| **5** | `schemas.py`, `natal.py`, `rulership.py` | New + `rules/house_ruler_mapping.yaml` | Ruler resolution matches case natal |
| **6** | `engine.py` | Pattern from `profile.py` | Full facts JSON smoke test |
| **7** | `POST /api/v1/astro/engine/facts` | FastAPI from AstroITProfile shell | End-to-end |
| **8** | Regression CI | New | Golden files in `data/exports/` |

**Do not start with** `it_profile.py`, `astro.py`, or API response shaping — those are outside Layer 1.

---

## 11. Dependency diagram (target)

```
EngineInput
    │
    ├─► time_utils ──► birth UTC, analysis UTC, verification_notes
    │
    ├─► ephemeris ───► transit_positions, natal planets
    │
    ├─► houses ──────► natal_cusps, house_placements  ◄── swe.houses_ex
    │                      ▲
    │                      └── _house_index_from_lon (from AstroITProfile)
    │
    ├─► aspects ─────► planet_aspects, cusp_aspects
    │
    ├─► rulership ───► house_rulers
    │
    └─► tags (optional) ► derived_tags

engine.py ──► FactsOutput JSON
```

---

## 12. Related documents

| Document | Purpose |
|----------|---------|
| `docs/astro_engine_v1_spec.md` | Target engine contract |
| `docs/money_compass_agent_architecture.md` | Layer 1 placement in product |
| `docs/operating_protocol.md` | Steps 1–3 the engine must satisfy |
| `training/hermes_training_summary_v1.md` | UTC + wrap-around lessons |
| `notes/experiments.md` | EXP-004, EXP-005 regression cases |

---

## Changelog

| Date | Change |
|------|--------|
| 2026-06-11 | Initial reuse map from AstroITProfile review |
