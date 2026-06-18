# Orchestrator Mode — Task Contract
## Context Layer 2 · Routing Session Execution

---

## Trigger

Default mode. Fires when any request is submitted to the orchestrator without a mode modifier.

---

## Pre-Routing Checklist (Run Before Output)

- [ ] Guardrails loaded? (`_guardrails/shared/` × 4 + `_guardrails/domain/real-estate-guardrails.md`) — if not, load before proceeding
- [ ] Adversarial input flags scanned? Run `adversarial-input-flags.md` against input now
- [ ] Confidence level assessed? (HIGH / MEDIUM / LOW per `confidence-floor.md`)
- [ ] Case ID present? If not: create one or ask for context to create one
- [ ] Request type identified? (see routing.md Step 2 table)
- [ ] Integration connectors checked? (Gmail / Calendar / Slack status)
- [ ] Market data currency OK? If property research involved, check `_market_data/austin-market-context.md` Last Updated

---

## Output Structure

Produce in this order.

### 0. Input Integrity Flag (if triggered)
If adversarial input patterns detected per `adversarial-input-flags.md`: prepend `⚠️ INPUT INTEGRITY FLAG` block before everything else. If none detected: omit entirely.

### 1. Routing Decision
- **Case ID:** [existing or newly created]
- **Route to:** [specialist name]
- **Priority:** [Urgent / Normal]
- **Reason:** one sentence

### 2. Confidence Level
Per `confidence-floor.md`. State immediately after routing decision.
- 🟢 HIGH — all required context present
- 🟡 MEDIUM — routing is directionally correct; some context missing
- 🔴 LOW — critical context missing; routing is provisional

### 3. Context Package (passed to specialist)
What the receiving specialist needs:
- The three most critical facts about this case
- Specific task to complete
- Priority level
- Any escalation conditions or flags
- Connector status (Gmail / Calendar / Slack: active / not active)

### 4. Sequence Note (if multi-step)
If this request requires multiple specialists in sequence: document the full sequence here so the next routing decision is pre-planned.

### 5. Professional Required Block (if triggered)
Check all conditions in `_guardrails/shared/escalation-triggers.md` + `_guardrails/domain/real-estate-guardrails.md`.
If any trigger fires: insert `🔴 PROFESSIONAL REQUIRED` block here, before Next Step.
If none fire: omit entirely.

### 6. Next Step
One specific sentence. An action for the agent, not a category.

### 7. Disclaimer Block (always)
Append full disclaimer block from `_guardrails/shared/output-disclaimers.md`. No exceptions.

---

## Session Log Entry (After Each Routing Decision)

Append to `_working/_calibration_log.md`:

```
## [YYYY-MM-DD] [Case ID] — [Specialist Routed To]
- Request type: [category]
- Priority assigned: [Urgent / Normal]
- Confidence level: [HIGH / MEDIUM / LOW]
- Connectors active: [list or "none"]
- Escalated to Diana: [Y/N — reason if Y]
- Routing note: [one sentence on what made this case non-standard, or "standard routing"]
```

---

*Layer placement: L2 Task Contract · Orchestrator mode · Load when orchestrator receives any request*
