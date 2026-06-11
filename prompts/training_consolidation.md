# Prompt: Training Consolidation

Run after a Hermes training or analysis session to capture durable knowledge.

---

## Instruction

Review the session and consolidate learnings into this repository.

**Do not** dump chat logs. Extract **reusable, validated** items only.

---

## Consolidation Checklist

### Protocol changes?
- [ ] Update `docs/operating_protocol.md` if workflow changed
- [ ] Update `training/hermes_training_summary_vN.md` or create new version

### New patterns?
- [ ] Add to relevant `repeated_patterns.md` or `winning_signatures.md` / `risk_signatures.md`
- [ ] Classify using `docs/pattern_taxonomy.md`
- [ ] Promote to `rules/*.yaml` if pattern is stable

### New case events?
- [ ] Add event file using `cases/templates/case_template.md`
- [ ] Update `case_summary.md`

### Prompt improvements?
- [ ] Update `prompts/*.md` if agent behavior should change

### Open questions?
- [ ] Log in `notes/open_questions.md`

### Experiments?
- [ ] Log in `notes/experiments.md`

---

## Output Format

```markdown
# Training Consolidation — [YYYY-MM-DD]

## Session summary
[2–3 sentences]

## Confirmed (add to repo)
- ...

## Experimental (notes only)
- ...

## Rejected / incorrect
- ...

## Files to update
| File | Change |
|------|--------|
| | |

## Next actions
- [ ] ...
```

---

## User Message Template

```
Consolidate this training session into money-compass-astro-lab:

[paste key conclusions, aspects, or session summary]

Mark confirmed vs. experimental. Propose specific file edits.
```

---

## Quality Bar

Promote to canonical docs only if:
- Aspect data is engine-verified OR explicitly marked pending
- Pattern appears in 2+ events OR is a protocol/rule change
- Wording is clean and reusable
