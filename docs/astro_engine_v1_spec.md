# Astro Engine v1 Specification

Practical specification for the first deterministic astrology engine supporting Money Compass and the Hermes-style reasoning layer.

**Status:** Draft v1  
**Related:** `docs/money_compass_agent_architecture.md`, `docs/operating_protocol.md`, `training/hermes_training_summary_v1.md`

---

## 1. Purpose of Astro Engine v1

Astro Engine v1 exists to make **core astrology math deterministic, auditable, and reusable**.

It implements **Layer 1** from `docs/money_compass_agent_architecture.md`: the compute path only.

| Responsibility | Owner |
|----------------|-------|
| Strict calculation | **Astro Engine v1** |
| AI reasoning / interpretation | Hermes-style agent (separate layer) |

The engine produces **structured facts** — positions, cusps, house placements, aspects, orbs, rulers — in a fixed JSON contract. It does not interpret meaning, score poker windows, or generate user guidance.

**Primary method:** transit-to-natal.

---

## 2. Why the Engine Is Needed

Validated lessons from Hermes training and this repository:

| Problem | Consequence | Reference |
|---------|-------------|-----------|
| **UTC conversion errors** | Wrong Moon and fast-moving body positions | EXP-004 (`notes/experiments.md`) — 11 June 2026 Moon mismatch, Hollywood FL |
| **Wrap-around house placement errors** | Transits assigned to wrong natal house | EXP-005 — Saturn/Neptune in Aries → house 12 (wrong) vs. house 1 (correct) |
| **LLM as math source** | Hallucinated or approximate degrees, aspects, houses | `docs/money_compass_agent_architecture.md` — engine vs. agent boundary |
| **Non-reproducible output** | Cannot regression-test, debug, or ship to product | Money Compass requires auditable facts for API and UI |

Deterministic engine output is a **prerequisite** for:

- Pattern/rule layer scoring (`rules/*.yaml`)
- Hermes agent interpretation (`prompts/`, `training/`)
- Product responses (`docs/output_templates.md`)

---

## 3. Scope of v1

Intentionally **small and focused** — enough to power transit-to-natal facts for poker/casino timing work in this repo.

### v1 supports

| Capability | Detail |
|------------|--------|
| Local time → UTC normalization | Explicit timezone and DST handling |
| Transit positions | Sun, Moon, Mercury, Venus, Mars, Jupiter, Saturn, Uranus, Neptune, Pluto |
| Natal house cusps | For configured house system |
| Natal house placement of transits | Including wrap-around spans |
| Exact transit-to-natal aspects | Planet → natal planet |
| Exact transit-to-cusp aspects | Planet → natal house cusp |
| Orb calculation | Separate from exact angle; configurable thresholds |
| Wrap-around house placement | Two-part interval logic at 360°/0° boundary |
| House-ruler mapping | Resolve ruler per house from natal chart |
| Structured output only | JSON contract; no interpretation |

### Bodies in v1

```
Sun, Moon, Mercury, Venus, Mars, Jupiter, Saturn, Uranus, Neptune, Pluto
```

---

## 4. Non-Goals for v1

v1 must **not** implement:

| Non-goal | Belongs to |
|----------|------------|
| Full AI interpretation | Layer 3 — Hermes agent |
| Narrative explanations | Layer 3 / Layer 4 |
| Case comparison reasoning | Pattern layer + agent |
| Emotional coaching / practical blocks | Agent + product (`training/practical_instruction_block.md`) |
| Full product scoring UI logic | Layer 2 + Layer 4 |
| Window tier decisions (strong / moderate / weak / avoid) | Pattern layer (`rules/scoring_dimensions.yaml`) |
| “Best life decision” conclusions | Out of scope |
| Event chart overlay | Optional future extension; not v1 core |
| Progressions, returns, synastry | Out of scope for v1 |

---

## 5. Required Inputs

Minimum input contract for a single **facts** computation (natal chart + transit moment).

### Natal (birth) inputs

| Field | Required | Notes |
|-------|----------|-------|
| `birth_date` | Yes | ISO date |
| `birth_time` | Yes | Local time at birthplace |
| `birthplace` | Yes | Human-readable label |
| `birth_latitude` | Yes | Decimal degrees — **preferred over loose city name** |
| `birth_longitude` | Yes | Decimal degrees |
| `birth_timezone` | Yes | IANA tz database ID (e.g., `Europe/Minsk`) |
| `house_system` | Yes | e.g., `placidus`, `whole_sign` — must be explicit in output |
| `zodiac_mode` | Yes | e.g., `tropical` — must be explicit in output |

### Transit (analysis) inputs

| Field | Required | Notes |
|-------|----------|-------|
| `analysis_local_datetime` | Yes | Local datetime for event/window |
| `analysis_timezone` | Yes | IANA ID for analysis location (may differ from birth) |
| `analysis_latitude` | Optional | For event-chart extensions later; store if provided |
| `analysis_longitude` | Optional | Store if provided |

### Configuration inputs

| Field | Required | Default | Notes |
|-------|----------|---------|-------|
| `aspect_orb_max_deg` | No | `4.0` | Max orb for aspect listing in core output |
| `aspect_types` | No | major | Conjunction, sextile, square, trine, opposition |
| `cusp_aspect_orb_max_deg` | No | `4.0` | Max orb for transit → cusp aspects |
| `poker_houses` | No | `[2,3,5,6,8,11]` | Houses for cusp aspect targeting and ruler mapping |

### Input rules

- **Exact birthplace coordinates** are preferred over geocoding a loose city name at runtime (geocoding introduces ambiguity).
- **Timezone and DST** must be resolved explicitly — never assume a fixed offset without checking date.
- **Local timestamp and UTC timestamp** for the analysis moment must both appear in output (`timestamp_local`, `utc_timestamp`).
- All settings that affect cusps or positions must be echoed in output so results are reproducible.

---

## 6. Required Outputs

### JSON contract (v1)

Top-level object returned by the engine. No interpretation fields.

```json
{
  "schema_version": "1.0",
  "engine_version": "0.1.0",
  "method": "transit_to_natal",

  "timestamp_local": "2026-11-21T14:00:00-05:00",
  "timezone": "America/New_York",
  "utc_timestamp": "2026-11-21T19:00:00Z",
  "dst_active": false,

  "house_system": "placidus",
  "zodiac_mode": "tropical",
  "coordinates_used": {
    "birth": { "latitude": 27.3364, "longitude": -82.5307, "label": "Sarasota, FL" },
    "analysis": { "latitude": 26.0112, "longitude": -80.1495, "label": "Hollywood, FL" }
  },

  "natal_cusps": [
    { "house": 1,  "sign": "Aquarius", "degree": 10.0,  "longitude_deg": 310.0 },
    { "house": 2,  "sign": "Aries",    "degree": 16.0,  "longitude_deg": 16.0 },
    { "house": 12, "sign": "Aquarius", "degree": 0.0,   "longitude_deg": 300.0 }
  ],

  "transit_positions": [
    {
      "body": "Moon",
      "sign": "Gemini",
      "degree": 12.45,
      "longitude_deg": 72.45,
      "retrograde": false,
      "speed_deg_per_day": 13.2
    },
    {
      "body": "Saturn",
      "sign": "Aries",
      "degree": 8.10,
      "longitude_deg": 8.10,
      "retrograde": false
    }
  ],

  "house_placements": [
    {
      "body": "Saturn",
      "natal_house": 1,
      "longitude_deg": 8.10,
      "wrap_segment": "aries_0_to_cusp2"
    },
    {
      "body": "Pluto",
      "natal_house": 12,
      "longitude_deg": 303.38,
      "wrap_segment": null
    }
  ],

  "planet_aspects": [
    {
      "transit_body": "Jupiter",
      "natal_body": "Jupiter",
      "natal_target": "11th_ruler",
      "aspect": "trine",
      "exact_angle_deg": 120,
      "separation_deg": 118.5,
      "orb_deg": 1.5,
      "applying": true
    }
  ],

  "cusp_aspects": [
    {
      "transit_body": "Mars",
      "natal_house": 11,
      "cusp_longitude_deg": 285.0,
      "aspect": "square",
      "exact_angle_deg": 90,
      "separation_deg": 91.2,
      "orb_deg": 1.2
    }
  ],

  "house_rulers": {
    "2":  { "sign": "Aries",     "ruler": "Mars",    "ruler_longitude_deg": 142.5 },
    "3":  { "sign": "Taurus",    "ruler": "Venus",   "ruler_longitude_deg": 38.2 },
    "5":  { "sign": "Cancer",    "ruler": "Moon",    "ruler_longitude_deg": 98.7 },
    "6":  { "sign": "Leo",       "ruler": "Sun",     "ruler_longitude_deg": 128.1 },
    "8":  { "sign": "Libra",     "ruler": "Venus",   "ruler_longitude_deg": 38.2 },
    "11": { "sign": "Sagittarius", "ruler": "Jupiter", "ruler_longitude_deg": 245.3 }
  },

  "derived_tags": [],

  "warnings": [],
  "verification_notes": [
    "UTC conversion: America/New_York EST (UTC-5) applied for 2026-11-21",
    "House 1 wrap-around span active: 310.0°–360.0° and 0.0°–16.0°"
  ]
}
```

**Notes on example:**

- `house_rulers` values are illustrative structure — actual degrees come from natal chart computation.
- `derived_tags` is optional in v1 (e.g., `BANKROLL`, `WIN`) — may be populated by `tags.py` from ruler mapping only, not interpretation.
- `natal_target` on aspects (e.g., `11th_ruler`) is a **structural label** for poker-domain targeting, not interpretation.

### Field requirements

| Field | Required | Description |
|-------|----------|-------------|
| `timestamp_local` | Yes | Analysis moment in local timezone (ISO 8601) |
| `timezone` | Yes | IANA timezone used for analysis |
| `utc_timestamp` | Yes | Analysis moment in UTC |
| `house_system` | Yes | Echo input setting |
| `zodiac_mode` | Yes | Echo input setting |
| `coordinates_used` | Yes | Birth (and analysis if provided) |
| `natal_cusps` | Yes | All 12 houses with longitude |
| `transit_positions` | Yes | All v1 bodies |
| `house_placements` | Yes | Transit body → natal house |
| `planet_aspects` | Yes | Transit → natal planet (within orb threshold) |
| `cusp_aspects` | Yes | Transit → natal cusp (poker houses minimum) |
| `house_rulers` | Yes | Rulers for poker houses 2,3,5,6,8,11 at minimum |
| `derived_tags` | Optional | Structural domain tags only |
| `warnings` | Yes | Array (empty if none) |
| `verification_notes` | Yes | UTC, DST, wrap-around flags |

Sort `planet_aspects` and `cusp_aspects` by `orb_deg` ascending (tightest first) — matches `docs/operating_protocol.md`.

---

## 7. Proposed Module Structure

Python package layout for v1 implementation.

```
astro_engine/
├── schemas.py      # Pydantic/dataclass models for inputs and outputs
├── time_utils.py   # Local ↔ UTC, DST, IANA timezone resolution
├── ephemeris.py    # Body positions from Swiss Ephemeris (or equivalent)
├── natal.py        # Natal chart computation orchestration
├── houses.py       # Cusp calculation, placement, wrap-around logic
├── aspects.py      # Angle, aspect type, orb, applying/separating
├── rulership.py    # House sign → ruler planet; poker house set
├── tags.py         # Optional structural tags from rulers/houses
└── engine.py       # Top-level orchestration: input → facts JSON
```

### Module responsibilities

| Module | Owns |
|--------|------|
| `schemas.py` | `EngineInput`, `FactsOutput`, validation; schema version |
| `time_utils.py` | Parse local datetime + IANA tz → UTC; DST detection; emit verification notes |
| `ephemeris.py` | Longitude, sign, degree, retrograde, speed for each body at UTC instant |
| `natal.py` | Combine birth inputs + ephemeris → natal planet longitudes |
| `houses.py` | Compute 12 cusps; assign transit to house; **wrap-around two-part intervals** |
| `aspects.py` | Separation angle; map to aspect type; compute orb; filter by threshold |
| `rulership.py` | Traditional ruler per house sign; resolve ruler planet longitude |
| `tags.py` | Map house/ruler hits to structural tags (`BANKROLL`, `WIN`, etc.) — no scoring |
| `engine.py` | `compute_facts(input) -> FactsOutput`; single public entry point |

---

## 8. Core Logic Requirements

### Time handling

| Rule | Implementation |
|------|----------------|
| Local → UTC | Use IANA timezone database; apply DST rules for the **analysis date** |
| Both timestamps in output | `timestamp_local` and `utc_timestamp` always present |
| Verification notes | Log offset used (e.g., `EDT UTC-4` or `EST UTC-5`) |

**Regression cases (Hollywood, FL):**

| Season | Zone | Offset |
|--------|------|--------|
| Summer (DST) | EDT | UTC−4 |
| Winter | EST | UTC−5 |

11 June 2026 Moon mismatch (EXP-004) must pass after fix. Fast movers are the first signal of timestamp errors.

---

### Natal house placement

| Rule | Implementation |
|------|----------------|
| Wrap-around spans | When house N cusp longitude > house N+1 cusp longitude (mod 360), house N occupies **two segments** |
| Assignment | Test transit longitude against each segment |

**Validated regression case (Dzmitry natal chart):**

| Cusp | Longitude |
|------|-----------|
| House 1 | Aquarius 10°0′ → **310.0°** |
| House 2 | Aries 16°0′ → **16.0°** |

**House 1 spans:**

1. **310.0° → 360.0°** (Aquarius 10° through end of Pisces)
2. **0.0° → 16.0°** (Aries 0° through Aries 16°)

| Transit | Correct house |
|---------|---------------|
| Saturn in Aries (~8°) | **House 1** |
| Neptune in Aries | **House 1** |
| Pluto in Aquarius 3°23′ (303.38°) | **House 12** |

21 November 2026 regression (EXP-005) must pass.

`houses.py` must expose `wrap_segment` in output when a placement uses the wrapped interval.

---

### Aspect math

| Term | Definition |
|------|------------|
| `separation_deg` | Shortest arc between two longitudes (0–180°) |
| `exact_angle_deg` | Target aspect angle: 0, 60, 90, 120, 180 |
| `aspect` | Name of closest major aspect within orb threshold |
| `orb_deg` | `abs(separation_deg - exact_angle_deg)` — **not** the same as separation |

**Rules:**

- Orb is distance from exact aspect angle, not raw separation.
- Configurable `aspect_orb_max_deg` — repo preference for core listing: **max 4°** (stricter than loose 8–10° defaults).
- Pattern-layer thresholds in `rules/poker_window_rules.yaml` (1°, 2°, 2.5°, 3°) apply **downstream** — engine exports facts up to configured max; pattern layer filters tighter.
- Optionally compute `applying` / `separating` from body speeds.

---

### Natal cusps — why deterministic calculation matters

Cusps are the **boundaries** for house placement and cusp aspects. If cusps drift, every house assignment and every transit→cusp aspect changes. A single degree error at the Ascendant can misassign multiple transits.

Cusps must be computed by the engine from:

- Birth datetime in UTC (after correct birth-timezone conversion)
- Birth latitude and longitude
- House system algorithm
- Zodiac mode

**They must never be supplied by the LLM or hand-entered in production** (manual override only for debugging with explicit `warnings`).

#### Why “Placidus” alone is not enough

Labeling output as “Placidus” does **not** guarantee agreement with another astro tool. Cusps match only when **all** technical inputs and settings align:

| Must match | Mismatch effect |
|------------|-----------------|
| Birth datetime (local + correct UTC) | Wrong Ascendant → all cusps shift |
| Birth latitude / longitude | Wrong angles |
| House system (Placidus vs Whole Sign vs others) | Different house boundaries |
| Zodiac mode (tropical vs sidereal) | Different sign positions |
| Ephemeris / ayanamsa (if sidereal) | Different longitudes |
| Rounding and precision | Edge-case disagreements at cusp boundaries |

**Cross-validation policy:** Compare cusp and Moon positions against trusted external astroprocessor samples using **identical** inputs. Document all settings in `verification_notes`. See `notes/open_questions.md` — external astroprocessor cross-check.

---

## 9. First API Endpoints

Future HTTP API (product code). v1 implementation priority indicated.

### `POST /api/v1/astro/engine/facts` — **v1 priority**

**Purpose:** Core engine output — full JSON contract from Section 6.

**Input:** Natal inputs + analysis datetime/timezone + config.

**Returns:** `FactsOutput` — positions, cusps, placements, aspects, rulers, verification.

**Used by:** Pattern layer, Hermes agent, debugging, regression tests.

---

### `POST /api/v1/astro/engine/poker-signals` — **post-v1 / thin wrapper**

**Purpose:** `facts` output filtered/enriched for poker domain.

**Returns:** Subset of facts plus:

- Aspects to poker house rulers (2,3,5,6,8,11)
- Cusp aspects for poker houses
- Structural tags (`BANKROLL`, `WIN`, etc.)

**Does not return:** Window tier, practical block, or interpretation.

**Depends on:** `facts` stable + `rules/house_ruler_mapping.yaml` mapping.

---

### `POST /api/v1/astro/engine/window-scan` — **post-v1**

**Purpose:** Batch `facts` over multiple datetimes (e.g., 12 Jun and 19 Jun 2026).

**Returns:** Array of facts objects with datetime keys; no ranking (ranking = pattern layer).

**Depends on:** `facts` endpoint + job orchestration.

---

## 10. Minimal Test Plan

### Unit tests

| Suite | Cases |
|-------|-------|
| **timezone conversion** | Known local → UTC pairs; explicit offset in verification_notes |
| **DST** | Hollywood FL summer (EDT UTC−4) vs winter (EST UTC−5); spring forward / fall back edge dates |
| **wrap-around houses** | Dzmitry: H1 Aquarius 10° / H2 Aries 16°; Saturn/Neptune in Aries → house 1; Pluto Aquarius 3°23′ → house 12 |
| **aspect / orb math** | Known longitude pairs; verify `orb_deg` ≠ `separation_deg`; 0°, 60°, 90°, 120°, 180° cases |
| **orb filtering** | Aspects at 3.9° included at max 4°; 4.1° excluded |

### Integration tests

| Suite | Cases |
|-------|-------|
| **smoke: complete output** | Valid input → all required JSON fields present; sorted aspects |
| **cusp consistency** | Same birth data vs external astroprocessor — Ascendant and house cusps within agreed tolerance (document tolerance, e.g., 0.1°) |

### Validation policy

- **Moon** and other **fast movers** must be cross-checked against external astroprocessor samples during validation (EXP-004 lesson).
- Store golden samples in `data/exports/` (no secrets).
- Regression tests run in CI for UTC and wrap-around cases.

Reference: `notes/experiments.md` (EXP-004, EXP-005), `notes/open_questions.md` (Technical section).

---

## 11. How This Engine Connects to Hermes

```
Astro Engine v1     →  facts JSON
Pattern / rule layer →  scores, flags, pattern matches  (rules/*.yaml)
Hermes-style agent   →  interpretation, practical block   (prompts/, training/)
Product              →  API / UI                          (Layer 4)
```

| Layer | Reads | Writes |
|-------|-------|--------|
| Engine | Birth + analysis inputs | `facts` JSON |
| Pattern layer | `facts` + `rules/*.yaml` + `cases/` | Scored signals |
| Hermes agent | `facts` + scores + `training/` + `prompts/` | Explanation, practical block |

**Hermes must not be the source of truth for:**

- Transit positions
- Natal cusps
- House placements
- Aspect existence, exact angle, or orb

If engine output is missing, the agent states that explicitly — per `docs/operating_protocol.md` and skill **strict-technical-astrological-verification**.

Engine implements Steps 1–3 of the operating protocol. Agent implements Step 4 only after facts are confirmed.

---

## 12. Near-Term Implementation Plan

| Order | Module / deliverable | Outcome |
|-------|----------------------|---------|
| **1** | `time_utils.py` + tests | Correct local/UTC/DST; EXP-004 regression |
| **2** | `houses.py` + tests | Cusps + wrap-around placement; EXP-005 regression |
| **3** | `ephemeris.py` | Body longitudes at UTC instant |
| **4** | `aspects.py` + tests | Aspect type, orb, filtering |
| **5** | `natal.py`, `rulership.py`, `schemas.py` | Natal planets, rulers, typed I/O |
| **6** | `engine.py` | Orchestration → full facts JSON |
| **7** | `POST .../facts` endpoint | HTTP access to engine |
| **8** | Regression suite + golden samples | CI + external astroprocessor cross-check |

`tags.py` and `poker-signals` endpoint can follow after `facts` is stable.

Export path for golden outputs: `data/exports/`, `data/windows/`.

---

## Document Map

| Topic | Location |
|-------|----------|
| Agent architecture | `docs/money_compass_agent_architecture.md` |
| Compute workflow | `docs/operating_protocol.md` |
| Technical lessons | `training/hermes_training_summary_v1.md` |
| Regression evidence | `notes/experiments.md` |
| Poker ruler mapping | `rules/house_ruler_mapping.yaml` |
| Orb thresholds (pattern layer) | `rules/poker_window_rules.yaml` |

---

## Changelog

| Date | Change |
|------|--------|
| 2026-06-11 | Initial v1 specification |
