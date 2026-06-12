# Money Compass Agent Architecture

How trained Hermes-style astrology reasoning fits into the future Money Compass application.

**Status:** Architecture reference  
**Audience:** Implementation, product, and agent-training decisions  
**Related:** `README.md`, `docs/operating_protocol.md`, `docs/roadmap.md`

---

## 1. Overview

### Money Compass as an application

**Money Compass** is the user-facing product: timing windows, risk signals, and practical guidance for money-related decisions — starting with **poker** and **casino** contexts developed in this repository.

Users ask questions like:

- When is a favorable poker window for me?
- Should I play this session or wait?
- What are the main risks and edges in this window?

The application must answer with **structured, actionable output** — not unstructured astrology prose.

### Astrology as an internal intelligence layer

Astrology is not the product surface. It is an **internal intelligence layer** that produces structured signals about timing, support, and risk relative to a native's chart.

The primary method is **transit-to-natal**: compare the current sky to the native's birth chart. Event charts are optional and must remain a separate overlay when used.

### Hermes-style agent as reasoning brain

**Hermes** (the trained astrology agent) is the **reasoning brain** above structured signals. It does not replace the math engine. It:

- Follows the strict compute-first workflow
- Applies domain models (poker house-ruler logic, casino session logic)
- Matches patterns against validated cases
- Synthesizes support vs. risk
- Produces user-facing explanation and the **Practical Instruction Block**

Training artifacts live in this repo (`training/`, `cases/`, `prompts/`, `rules/`).

### Why this architecture exists

Early Hermes training showed two failure modes when layers are mixed:

1. **LLM-only astrology** — positions, aspects, and house placements can be wrong or invented; errors propagate into confident-sounding interpretation.
2. **Engine-only output** — correct math without domain logic, case memory, or practical guidance is not useful as a product.

The architecture **separates deterministic computation from reasoning** so each layer does what it is good at. This repo (`money-compass-astro-lab`) preserves the reasoning layer, pattern knowledge, and verification rules so training is not lost when chats, tools, or local state change.

---

## 2. Core Architectural Principle

Three responsibilities. One direction of dependency.

```
Engine computes  →  Pattern layer structures  →  Agent reasons and explains
```

| Responsibility | Owner | Nature |
|----------------|-------|--------|
| **Compute** | Astro math engine | Deterministic, auditable, testable |
| **Structure** | Pattern / rule layer | Validated mappings, scoring, flags |
| **Reason** | AI agent | Interpretation, synthesis, explanation |

**Simple rule:** Math flows up; meaning is built on top — never the reverse.

The agent receives **confirmed facts** (positions, aspects, house placements, pattern scores) and produces **meaning and guidance**. It must not be the source of ephemeris truth.

Technical verification (UTC conversion, wrap-around house assignment) belongs in the compute path, codified after validated training fixes. See `training/hermes_training_summary_v1.md` — Technical Lessons Learned.

---

## 3. Layered Architecture

Four layers from ephemeris to user-facing output.

```
┌─────────────────────────────────────────────────────────────┐
│  Layer 4 — Product Output                                    │
│  API responses, UI summaries, decision packs, scored windows │
├─────────────────────────────────────────────────────────────┤
│  Layer 3 — AI Reasoning Agent (Hermes-style)                 │
│  Interpretation, synthesis, practical block, explanation       │
├─────────────────────────────────────────────────────────────┤
│  Layer 2 — Pattern / Rule Layer                              │
│  Repeats, support/risk, scoring, domain mappings             │
├─────────────────────────────────────────────────────────────┤
│  Layer 1 — Astro Math Engine                                 │
│  Positions, aspects, houses, orbs, time conversion           │
└─────────────────────────────────────────────────────────────┘
```

---

### Layer 1 — Astro Math Engine

**Purpose:** Produce reproducible astronomical facts. No interpretation.

**Owns:**

| Function | Detail |
|----------|--------|
| Transit positions | Sign, degree, retrograde for all bodies at event datetime |
| Time conversion | Local timestamp → UTC with correct DST (e.g., Hollywood FL: EDT UTC−4 / EST UTC−5) |
| Natal house cusps | Cusp degrees for house system in use |
| House placement | Which natal house each transiting planet occupies — including **wrap-around spans** crossing 360°/0° |
| Exact aspects | Transit-to-natal aspects to planets and cusps |
| Orb calculation | Exact angle and orb for every aspect |
| House-ruler mapping | Resolve ruler of each relevant house from natal chart (e.g., poker houses 2/3/5/6/8/11) |
| Cusp activation | Transits aspecting natal house cusps |
| Technical verification | Emit local + UTC timestamps; support regression against external astroprocessor samples |

**Output shape (conceptual):** Structured chart/window export — e.g., JSON in `data/natal/`, `data/windows/`.

**Repo references:** `docs/operating_protocol.md` (Steps 1–3), `notes/experiments.md` (EXP-004 UTC, EXP-005 wrap-around).

**Must not:** Interpret meaning, score windows, or generate user guidance.

---

### Layer 2 — Pattern / Rule Layer

**Purpose:** Turn engine facts into structured, comparable signals using validated domain knowledge.

**Owns:**

| Function | Source in this repo |
|----------|---------------------|
| Exact repeated patterns | `cases/dzmitry_poker/repeated_patterns.md`, `rules/poker_window_rules.yaml` |
| Functional repeats | `training/general_window_logic.md`, `docs/pattern_taxonomy.md` |
| Contextual support | Pattern taxonomy, case signatures |
| Contextual risks | `rules/risk_flags.yaml`, case risk signatures |
| Poker window logic | `training/poker_model.md` — house-ruler hierarchy, composite signature |
| Casino window logic | `training/casino_model.md` |
| Scoring dimensions | `rules/scoring_dimensions.yaml` |
| Risk flags | `rules/risk_flags.yaml` — severity, veto combinations |
| Domain mappings | `rules/house_ruler_mapping.yaml` |

**Pattern classes (established):**

| Class | Role |
|-------|------|
| Exact repeats | Same transit→natal geometry across winning events |
| Functional repeats | Different geometry, same house-ruler layer |
| Contextual support | Strengthens window without being a repeat |
| Contextual risks | Weakens window; may trigger avoid |

**Composite poker signature (validated):** `luck + cognition + aggression + discipline + timing`

**Output shape (conceptual):** Scored window object — tier, dimension breakdown, matched patterns, active risk flags, case references.

**Must not:** Invent aspects or house placements; override engine facts.

---

### Layer 3 — AI Reasoning Agent

**Purpose:** Interpret structured signals and produce coherent, user-appropriate guidance.

**Owns:**

| Function | Detail |
|----------|--------|
| Interpretation | Step 4 of operating protocol — after math and pattern scoring |
| Synthesis | Combine house-ruler layers, pattern matches, and risks into a window assessment |
| Comparative reasoning | Recall case logic (e.g., Dzmitry poker repeats vs. current window) |
| Practical guidance | Mandatory **Practical Instruction Block** for poker windows |
| User-facing explanation | Clear language tied to established signals — not vague luck prose |
| Context adaptation | Adjust emphasis for user question, domain (poker vs. casino), and window tier |

**Interpretation priority (poker, established):**

1. House-ruler layer (2, 3, 5, 6, 8, 11)
2. Key natal planets
3. Natal house placement of transits
4. Natal house cusp activation

**Practical Instruction Block sections (required for poker):**

1. How to play the window  
2. What to avoid  
3. Main edge  
4. Main danger  
5. Best style of play  
6. Bankroll instruction  
7. Emotional-control instruction  

**Repo references:** `prompts/strict_transit_to_natal.md`, `prompts/poker_window_search.md`, `training/practical_instruction_block.md`, skill: **strict-technical-astrological-verification**.

**Must not:** Compute or guess positions, aspects, orbs, or house placements. Must not skip compute-first order.

---

### Layer 4 — Product Output Layer

**Purpose:** Deliver agent output in forms the application and user can consume.

**Owns:**

| Output type | Description |
|-------------|-------------|
| API responses | Structured JSON — window score, tier, pattern matches, risk flags, practical block fields |
| UI-friendly summaries | Short headline + expandable detail |
| Decision packs | Play / moderate / weak / avoid recommendation with rationale |
| Practical action blocks | Mapped from Practical Instruction Block (`play_guidance`, `avoid`, `bankroll`, etc.) |
| Scored windows / ranked options | Multi-date scan results (e.g., compare 12 Jun vs. 19 Jun 2026 windows) |

**Conceptual response fields (from repo training, not final API spec):**

```
window_tier          strong | moderate | weak | avoid
window_score         0–100 (from scoring_dimensions.yaml)
pattern_matches      exact / functional / support / risk
composite_signature  luck, cognition, aggression, discipline, timing — full | partial | absent
practical_block      seven required sections
verification         local_timestamp, utc_timestamp
method               transit_to_natal
```

**Repo references:** `docs/output_templates.md`, `docs/roadmap.md` Phase 4.

---

## 4. Engine vs. Agent Boundary

### What must never be left to the agent alone

| Item | Why |
|------|-----|
| Transit positions | Requires ephemeris; LLMs hallucinate degrees |
| Local → UTC conversion | Validated bug source (Moon mismatch, 11 Jun 2026) |
| Natal house placement | Requires cusp math; wrap-around spans need deterministic logic |
| Aspect identification and orbs | Requires exact angle calculation |
| House-ruler resolution | Derived from natal chart structure |
| Pattern score arithmetic | Should be reproducible from rules YAML |

If the engine is unavailable, the agent **states that explicitly**. It does not estimate planetary positions.

### What must be deterministic and auditable

- All Layer 1 outputs
- Pattern classification inputs (which aspects matched which catalog entries)
- Window tier/score from `rules/scoring_dimensions.yaml` and `rules/poker_window_rules.yaml`
- Risk flag activation from `rules/risk_flags.yaml`
- Timestamp verification block (local + UTC)

These must be loggable, replayable, and regression-testable. See `notes/open_questions.md` — Technical section.

### What the agent is good at

- Applying interpretation priority in correct order **after** facts are confirmed
- Explaining why a window is strong or weak in practical poker/casino terms
- Mapping pattern matches to case narratives (Dzmitry wins, casino signatures)
- Generating the Practical Instruction Block with appropriate tone and specificity
- Adapting structured output to the user's question without changing underlying math
- Detecting protocol drift and recovering (technical reset)

### Why exact astro math must not rely on LLM reasoning alone

Training validated that **small math errors invalidate entire analyses**:

- Wrong UTC → wrong Moon → wrong house placement → wrong interpretation
- Wrong wrap-around logic → Saturn/Neptune in Aries assigned to house 12 instead of house 1

LLMs are not ephemeris engines. They cannot be the system of record for degrees, orbs, or cusp boundaries. The agent's value is **reasoning over verified signals**, not replacing the engine.

---

## 5. How Hermes Training Feeds Money Compass

Hermes training produces **durable product knowledge** stored in this repo. Each artifact maps to an implementation layer.

| Training artifact | Repo location | Product layer |
|-------------------|---------------|---------------|
| Operating protocol | `docs/operating_protocol.md` | Engine output spec + agent workflow |
| Technical verification rules | `docs/operating_protocol.md`, `training/hermes_training_summary_v1.md` | Engine + agent pre-interpretation checks |
| Strict notation | `docs/operating_protocol.md` | Engine output format; agent listing standard |
| Poker model | `training/poker_model.md` | Pattern layer + agent interpretation priority |
| Casino model | `training/casino_model.md` | Pattern layer (casino context) |
| Window logic | `training/general_window_logic.md` | Pattern layer classification |
| Practical instruction block | `training/practical_instruction_block.md` | Agent output + Layer 4 product fields |
| Case learning | `cases/dzmitry_poker/`, `cases/dimasik_shay_casino/` | Pattern layer case memory |
| Repeated patterns | `cases/dzmitry_poker/repeated_patterns.md` | Pattern layer exact/functional catalog |
| Prompts | `prompts/*.md` | Agent behavior contracts (deployable prompt bundles) |
| Output templates | `docs/output_templates.md` | Layer 4 response shapes |
| Structured rules | `rules/*.yaml` | Pattern layer (machine-readable scoring and flags) |
| Experiments / lessons | `notes/experiments.md` | Engine regression requirements |

**Promotion path (established):**

```
Hermes training session
  → notes/experiments.md (hypothesis)
  → case or training doc (validated)
  → rules/*.yaml (canonical structured rule)
  → engine/agent implementation
  → product API field
```

Example: UTC and wrap-around fixes went from experiments → operating protocol → permanent verification rules.

---

## 6. Future Pattern System

Case-based learning in this repo is the seed of a **two-tier pattern memory** for Money Compass.

### Personal pattern layer

Patterns validated for **one native** from their own events.

| Example | Location |
|---------|----------|
| Dzmitry poker winning signature | `cases/dzmitry_poker/repeated_patterns.md` |
| Dzmitry house rulers (2→Mars, 3→Venus, etc.) | `training/poker_model.md`, `cases/dzmitry_poker/natal_reference.md` |
| Dimasik/Shay casino signatures | `cases/dimasik_shay_casino/winning_signatures.md`, `risk_signatures.md` |

**Characteristics:**

- Requires native natal chart and event history
- Highest confidence for that specific user
- Exact repeats are native-specific (ruler targets depend on chart)

### Generalized pattern layer

Patterns abstracted to **functional layers** that can apply across natives with similar chart structures.

| Example | Abstraction |
|---------|-------------|
| Exact repeat: transiting Jupiter → natal 11th ruler | Functional: WIN layer support |
| Dzmitry: luck + cognition + aggression + discipline + timing | Functional composite signature |
| RISK: FALSE_LUCK (11th bright without 6th discipline) | Generalized risk flag in `rules/risk_flags.yaml` |

**Characteristics:**

- Expressed in `rules/*.yaml` and `docs/pattern_taxonomy.md`
- Domain tags: BANKROLL, READ, PLAY, DISCIPLINE, POT, WIN
- Future users with similar functional signatures get comparable scoring — after their rulers are resolved from Layer 1

### Case memory vs. generalized rule memory

| Type | Storage | Use |
|------|---------|-----|
| **Case memory** | `cases/*/`, `data/windows/` | "This happened for this native on this date" — evidence, audit trail |
| **Rule memory** | `rules/*.yaml`, `training/*.md` | "When WIN layer + DISCIPLINE clear, score uplifts" — reusable logic |

**Evolution path:**

1. Event recorded in case file with engine-verified Steps 1–3
2. Pattern classified (exact / functional / support / risk)
3. Repeated across events → promoted to `repeated_patterns.md`
4. Stable enough → encoded in `rules/poker_window_rules.yaml` or `risk_flags.yaml`
5. Product scores new windows against both personal case catalog and generalized rules

Negative cases (non-winning sessions) are planned but not yet populated — they will constrain generalized rules the same way.

---

## 7. Example Flow

**User request:** "Is 12 June 2026 a good poker window for me?"  
**Context:** Dzmitry natal chart; Hollywood, FL venue; transit-to-natal method.

### Step A — Request intake (Product)

- Parse user, date range, domain (poker), location
- Load natal chart from `data/natal/` (or engine on demand)
- Resolve query datetime(s) with local timezone

### Step B — Layer 1: Engine compute

Engine produces window export:

```
verification:
  local:  2026-06-12 14:00 EDT (Hollywood, FL)
  utc:    2026-06-12 18:00 UTC

transit_positions: [...]

aspects:
  - transiting Jupiter → natal Jupiter (11th ruler): trine, orb 1.2°
  - transiting Saturn → natal Mars (2nd ruler): square, orb 0.8°
  [...]

natal_house_placement:
  - transiting Saturn: natal house 1  # wrap-around logic applied
  [...]

house_rulers:
  2: Mars, 3: Venus, 5: Mercury, 6: Moon, 8: Venus, 11: Jupiter
```

No interpretation at this stage.

### Step C — Layer 2: Pattern / rule scoring

Pattern layer processes engine export against:

- `cases/dzmitry_poker/repeated_patterns.md` — exact/functional match check
- `rules/scoring_dimensions.yaml` — dimension weights
- `rules/risk_flags.yaml` — e.g., BANKROLL_STRESS if 2nd ruler afflicted
- `rules/poker_window_rules.yaml` — tier thresholds

Example structured result:

```
window_score: 62
window_tier: moderate
exact_repeats: []          # pending engine-populated tournament catalog
functional_repeats: [F_WIN]
contextual_support: [11th layer active]
contextual_risks: [BANKROLL_STRESS — major]
composite_signature: partial  # luck + cognition present; discipline unclear
case_reference: dzmitry_poker
```

### Step D — Layer 3: Agent reasoning

Agent receives engine export + pattern score. Applies:

- `docs/operating_protocol.md` Step 4 order
- `training/poker_model.md` hierarchy
- Does **not** recompute math

Agent produces:

- Interpretation narrative tied to listed aspects and layers
- Window assessment explaining moderate tier
- **Practical Instruction Block** (seven sections) — e.g., reduced buy-in due to 2nd-layer risk, controlled aggression per 5th layer, emotional-control emphasis

### Step E — Layer 4: Product output

API/UI returns structured decision pack:

```json
{
  "window_tier": "moderate",
  "window_score": 62,
  "recommendation": " playable with strict bankroll cap ",
  "composite_signature": "partial",
  "top_support": ["11th/Jupiter WIN layer"],
  "top_risks": ["2nd/Mars bankroll stress"],
  "practical_block": {
    "play_guidance": "...",
    "avoid": "...",
    "main_edge": "...",
    "main_danger": "...",
    "style": "...",
    "bankroll": "...",
    "emotional_control": "..."
  },
  "verification": {
    "local": "2026-06-12 14:00 EDT",
    "utc": "2026-06-12 18:00 UTC"
  },
  "method": "transit_to_natal"
}
```

User sees summary + expandable detail. Audit log retains full engine export and pattern match record.

---

## 8. Why This Matters

| Benefit | How the architecture delivers |
|---------|-------------------------------|
| **Speed** | Engine computes once; pattern layer scores mechanically; agent focuses on explanation |
| **Reliability** | Math is deterministic; UTC and wrap-around bugs are fixed in code, not re-discovered per chat |
| **Explainability** | Every claim traces to engine facts → pattern class → interpretation; timestamps auditable |
| **Easier debugging** | Failures isolate to layer: bad Moon → engine/UTC; wrong tier → rules; vague advice → agent/prompts |
| **Scalability** | New users add natal chart + case memory; generalized rules apply without retraining from scratch |
| **Reusability** | This repo is the knowledge base; prompts and YAML migrate to production without rewriting logic |

Without layer separation, Money Compass would either ship unreliable astrology or brittle, unreadable engine dumps. The architecture targets **verified math + validated patterns + practical guidance**.

---

## 9. Near-Term Implementation Recommendations

Based on `docs/roadmap.md` and current repo state.

### Code first (Layer 1 — highest priority)

| Item | Rationale |
|------|-----------|
| Astro math engine with ephemeris | Foundation for everything else |
| Local → UTC conversion with DST | Validated failure mode (EXP-004) |
| Natal house placement including wrap-around | Validated failure mode (EXP-005) |
| Aspect + orb calculation | Step 2 of protocol |
| House-ruler resolution from natal chart | Required for poker model |
| Structured window export JSON | `data/natal/`, `data/windows/` schema |
| Regression tests for UTC and wrap-around | Open questions in `notes/open_questions.md` |

### Pattern layer next (Layer 2)

| Item | Rationale |
|------|-----------|
| Implement scoring from `rules/scoring_dimensions.yaml` | Reproducible window tiers |
| Risk flag evaluator from `rules/risk_flags.yaml` | Veto and downgrade logic |
| Pattern matcher against case catalogs | Exact/functional repeat detection |
| Populate Dzmitry tournament aspect data | Unblocks real exact-repeat matching |

### Prompts / knowledge docs for now (Layer 3 — partial)

| Item | Can stay as docs/prompts initially |
|------|-------------------------------------|
| Full interpretation synthesis | `prompts/poker_window_search.md`, agent with protocol prompts |
| Practical Instruction Block generation | `prompts/practical_block_prompt.md` |
| Case narrative recall | Agent reads `cases/` until RAG/index built |
| strict-technical-astrological-verification | Agent skill / prompt bundle |

Agent can operate against engine JSON + YAML scores **before** a custom reasoning service exists — using prompt bundles from `prompts/` as the behavior contract.

### Product layer (Layer 4 — after engine + scoring stable)

| Item | Timing |
|------|--------|
| API response schema from `docs/output_templates.md` | After first end-to-end window export |
| Practical block as structured JSON fields | Roadmap Phase 4 |
| Multi-date window ranking | After single-window pipeline works |
| UI decision packs | After API shape validated |

### Later: production reasoning layer

| Evolution | Trigger |
|-----------|---------|
| Versioned prompt bundles in deployment | Agent behavior stabilizes across test windows |
| RAG over `cases/` and `training/` | Case library grows beyond manual prompt context |
| Personal vs. generalized pattern stores | Multiple natives with validated case histories |
| External astroprocessor cross-check in CI | Moon/fast-body verification policy decided |

---

## Document Map

| Need | Read |
|------|------|
| Compute workflow | `docs/operating_protocol.md` |
| Technical bugs and fixes | `training/hermes_training_summary_v1.md` |
| Poker domain model | `training/poker_model.md` |
| Pattern taxonomy | `docs/pattern_taxonomy.md` |
| Scoring and flags | `rules/*.yaml` |
| Implementation phases | `docs/roadmap.md` |
| Regression evidence | `notes/experiments.md` |

---

## Changelog

| Date | Change |
|------|--------|
| 2026-06-11 | Initial architecture document |
