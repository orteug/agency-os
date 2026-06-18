# Changelog — AgencyOS

## v3.0 — 2026-06-17 — Guardrails Layer

Adds a structural safety layer for deployment to any real estate team. Safety posture is now load-order enforced, not embedded in prose rules.

### Added

- `_guardrails/shared/output-disclaimers.md` — required disclaimer blocks on all output; plain English; role-specific additions (pricing / legal / financing)
- `_guardrails/shared/confidence-floor.md` — input threshold rules; 🟢 HIGH / 🟡 MEDIUM / 🔴 LOW confidence levels; hard STOP when case ID and request type are both absent
- `_guardrails/shared/escalation-triggers.md` — professional escalation gates
- `_guardrails/shared/adversarial-input-flags.md` — one-sided input detection patterns
- `_guardrails/domain/real-estate-guardrails.md` — 5 real estate escalation triggers (pricing advice, legal document interpretation, financing denial, appraisal gap, VIP referral) + 5 real estate input flags (pricing without comps, urgency without deadline, motivated seller without substantiation, market claim vs. current data, commission as standard)
- `routing.md` updated with Step 0: load all guardrails before any routing decision

### Changed

- `_modes/orchestrator-mode.md` — output structure updated: Input Integrity Flag (section 0), confidence level after routing decision, Professional Required block before Next Step, Disclaimer Block (always)
- `_modes/daily-brief-mode.md` — same guardrail hooks added to brief output structure
- `DESIGN_DECISIONS.md` — Decision 3 added: guardrails layer rationale

---

## v2.0 — 2026-06-17 — ICM Architecture Upgrade

Applies the Interpretable Context Methodology (ICM) framework from Van Clief & McDermott (arXiv:2603.16021).

### Added

- `routing.md` — L1 routing extracted from `00_orchestrator/rules.md`. Routing table, BD pipeline decision rules, integration awareness, data currency check.
- `_modes/orchestrator-mode.md` — explicit L2 task contract for routing sessions. Output structure, pre-routing checklist, session log requirement.
- `_modes/daily-brief-mode.md` — explicit L2 task contract for daily brief generation. 11-section output structure, confidence level, pipeline health snapshot.
- `_market_data/austin-market-context.md` — moved from `02_property_research/reference/`. Time-sensitive market data separated from timeless specialist rules.
- `_working/_calibration_log.md` — L4 session outcome tracker.
- `_working/_patterns.md` — cross-session pattern tracker.

### Changed

- `00_orchestrator/rules.md` — trimmed to behavioral rules only. Routing table, BD routing rules, and integration awareness extracted to `routing.md`. Pointer references added.

### Structural Changes Summary

| v1 location | v2 location | Reason |
|-------------|-------------|--------|
| Routing table in `00_orchestrator/rules.md` | `routing.md` | L1 routing separated from L3 behavioral constraints |
| BD routing rules in `00_orchestrator/rules.md` | `routing.md` | Same — L1/L3 mixing resolved |
| Integration awareness in `00_orchestrator/rules.md` | `routing.md` | Same |
| `02_property_research/reference/austin-market-context.md` | `_market_data/austin-market-context.md` | Time-sensitive (L4), not stable constraint (L3) |

---

## v1.0 — 2026-05 — Initial Release

Built as a portfolio piece for the Jeff van Clief Skool community.

**Structure:**
```
agency-os/
├── START_HERE.md
├── OPERATING_RHYTHM.md
├── HANDOFF_PROTOCOL.md
├── TEAM_ROLES.md
├── EMOTIONAL_INTELLIGENCE.md
├── CASE_STATE_SCHEMA.md
├── 00_orchestrator/ (identity, rules, examples, handoff)
├── 01_lead_qualifier/ (identity, rules, examples, handoff)
├── 02_property_research/ (identity, rules, examples, handoff, reference/)
├── 03_client_communication/ (identity, rules, examples, handoff)
├── 04_transaction_coordinator/ (identity, rules, examples, handoff)
├── 05_daily_deal_desk/ (identity, rules, examples, handoff)
├── 06_bd_coordinator/ (identity, rules, examples, handoff)
├── profiles/
├── templates/
└── examples/
```

**What it did well:**
- Multi-specialist routing with handoff contracts — proto-L2 patterns in each specialist folder
- Case ID system — clean L4 state tracking concept
- Escalation rules to Diana — correct instinct on professional escalation gates
- 30-day refresh cadence on austin-market-context.md — correct data currency instinct
- Integration awareness section — connector-aware routing

**What v2 fixes:**
- Routing logic embedded in 00_orchestrator/rules.md (L1/L3 mixing)
- No explicit task contracts at root level (each specialist had proto-contracts but no orchestrator-level contract)
- Time-sensitive market data in a specialist's reference/ folder (L3/L4 mixing)
- No L4 working layer — no calibration log, no cross-session learning
- No safety guardrails layer
