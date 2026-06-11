# Roadmap

Development path for Money Compass Astro Lab and downstream product integration.

---

## Phase 1 — Knowledge Foundation (Current)

- [x] Repository structure and canonical docs
- [x] Operating protocol and notation standards
- [x] Poker house-ruler model (2/3/5/6/8/11)
- [x] Dzmitry poker case scaffold (3 tournaments)
- [x] Dimasik/Shay casino case scaffold
- [x] Prompt library and rules YAML
- [ ] Populate `data/natal/` with structured chart JSON from engine
- [ ] Export first window analysis to `data/windows/`

---

## Phase 2 — Case Validation

- [ ] Complete tournament_1/2/3 with full Step 1–3 data from engine
- [ ] Finalize `repeated_patterns.md` with engine-verified aspects
- [ ] Score 12 Jun and 19 Jun 2026 windows; record in `future_windows.md`
- [ ] Validate or refute patterns against non-winning sessions (negative cases)
- [ ] Casino case: complete winning/risk signature lists

---

## Phase 3 — Engine Integration

- [ ] Define chart JSON schema in `data/natal/README` (or schema file)
- [ ] Pipeline: engine → window export → agent interpretation
- [ ] Automated orb-sorted aspect listing (no manual degree entry)
- [ ] House-ruler resolution from chart metadata

**Principle:** Engine owns math. Repo owns rules and prompts. Agent owns synthesis.

---

## Phase 4 — Hermes Product Hooks

- [ ] Map `rules/scoring_dimensions.yaml` to API response shape
- [ ] Practical instruction block as structured JSON field
- [ ] Pattern match API: case ID + confidence tier
- [ ] Versioned prompt bundles for agent deployment

---

## Phase 5 — Expansion

- [ ] Additional gambling contexts (trading windows, sports — if in scope)
- [ ] `hermes_training_summary_v2.md` after next training cycle
- [ ] Multi-native comparative patterns
- [ ] Negative case library (sessions to avoid)

---

## Open Decisions

Track in `notes/open_questions.md`:

- Default house system for Money Compass product
- Orb thresholds for "exact repeat" classification
- Minimum window score for user-facing "play" recommendation

---

## Changelog

| Date | Change |
|------|--------|
| 2026-06-11 | Initial repository scaffold and training v1 content |
