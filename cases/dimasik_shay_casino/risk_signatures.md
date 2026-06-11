# Risk Signatures — Dimasik / Shay Casino

Transit-to-natal configurations correlated with **losses, chase behavior, or session overextension**.

---

## Signature Format

```markdown
### [RISK-CXX] Short name
- **Severity:** veto | major | minor
- **Aspect:** transiting X → natal Y (angle, orb)
- **Layers:** ...
- **Behavioral correlate:** ...
- **Mitigation:** ...
```

Cross-reference: `rules/risk_flags.yaml`

---

## Catalog

### [RISK-C01] Neptune chase fog

- **Severity:** major
- **Typical configs:**
  - transiting Neptune → natal 5th ruler (conjunction/square/opposition)
  - transiting Neptune in natal 2nd with afflicted 2nd ruler
- **Layers:** PLAY, BANKROLL
- **Behavioral correlate:** "One more spin"; denial of losses; session extension
- **Mitigation:** Hard session cap; avoid session entirely if orb < 1°
- **Sessions:** _pending_

---

### [RISK-C02] Saturn 11th grind

- **Severity:** major
- **Typical configs:**
  - transiting Saturn → natal 11th ruler (hard aspect)
  - Saturn through natal 11th without Jupiter mitigation
- **Layers:** WIN
- **Behavioral correlate:** Slow bleed; false hope; inability to accept loss
- **Mitigation:** Shorter session; lower bankroll; no progressive chase
- **Sessions:** _pending_

---

### [RISK-C03] Moon / 6th tilt extension

- **Severity:** minor → major if combined with RISK-C01
- **Typical configs:**
  - Moon afflicted (square Saturn, opposition Mars)
  - Transiting Mars → natal Moon (6th ruler in some charts)
- **Layers:** DISCIPLINE
- **Behavioral correlate:** Emotional session extension; revenge play against house
- **Mitigation:** Pre-set alarm; mandatory exit on first chase impulse
- **Sessions:** _pending_

---

### [RISK-C04] 5th fire without 11th support

- **Severity:** minor
- **Typical configs:**
  - Strong Mars/Mercury activation to 5th
  - No concurrent Jupiter/11th support
- **Layers:** PLAY, WIN
- **Behavioral correlate:** Entertainment spend mistaken for edge; high variance loss
- **Mitigation:** Treat as play budget only, not expected win session
- **Sessions:** _pending_

---

## Veto Combinations

Avoid session when **two or more major risks** active, especially:

- RISK-C01 (Neptune chase) + RISK-C03 (discipline break)
- RISK-C02 (Saturn 11th) + any 5th push without WIN support

---

## Negative Case Library

| Session | Active risks | Outcome |
|---------|--------------|---------|
| _pending_ | | |

---

## Maintenance

- [ ] Link each risk to at least one documented session
- [ ] Sync severity to `rules/risk_flags.yaml`
