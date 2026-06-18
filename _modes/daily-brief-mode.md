# Daily Brief Mode — Task Contract
## Context Layer 2 · Morning Pipeline Brief Execution

---

## Trigger

Fires when: "brief" is typed alone, "What needs attention today?" is asked, or morning pipeline review is requested.
Handled by: `05_daily_deal_desk`

---

## Pre-Brief Checklist (Run Before Output)

- [ ] Guardrails loaded? (`_guardrails/shared/` × 4 + domain file)
- [ ] Adversarial input flags scanned? (especially: urgency pressure, one-sided deal framing)
- [ ] Confidence level assessed? Brief cannot be 🟢 HIGH without current case state
- [ ] Market data current? Check `_market_data/austin-market-context.md` Last Updated (flag if >30 days)
- [ ] Gmail connector active? If yes: check for unread client threads before flagging communication health
- [ ] Calendar connector active? If yes: pull today's showings, closings, and deadlines
- [ ] Case states provided or accessible? If none: output LOW confidence brief with explicit gaps listed

---

## Output Structure

Produce in this order.

### 0. Input Integrity Flag (if triggered)
If any deal data presented appears one-sided or unverified: prepend `⚠️ INPUT INTEGRITY FLAG` block. If none: omit.

### 1. Brief Header
```
📋 AgencyOS — Daily Brief
[Day, Date]
Generated: [time if known]
Confidence: [🟢 / 🟡 / 🔴] — [one-line reason]
```

### 2. Verdict-Level Summary
One sentence: what is the state of the pipeline today? What's the single most important thing?

Add immediately after:
- Confidence level block from `confidence-floor.md`

### 3. 🔴 URGENT — Act Before Anything Else
Items where something breaks if ignored today. Include:
- Case ID + client name
- What breaks + when
- Assigned agent
- One-sentence recommended action

If none: "No urgent items today."

### 4. 🟡 ATTENTION — Act Today
Items that need same-day attention but aren't time-critical by the hour. Same format as urgent.

### 5. Diana's Queue
Items that require Diana's decision or awareness. Format:
- Situation (one sentence)
- What's already been tried
- Specific decision or action Diana needs to make

### 6. 📈 BD Touches Due Today
Contacts in the BD pipeline with touches due. Include:
- Contact name + case ID
- Touch type (call / text / email)
- Last contact date
- One-sentence context on where the relationship stands

### 7. Pipeline Health
Full pipeline snapshot — every active case with:
- Case ID + client name + stage
- Next milestone + date
- Communication health (🟢 / 🟡 / 🔴)
- Agent owner

### 8. Stale Alerts
Cases with no logged update in >5 business days. Flag: case ID, last update date, owner.

### 9. Professional Required Block (if triggered)
Check `escalation-triggers.md` + `real-estate-guardrails.md`.
If any trigger fires: insert `🔴 PROFESSIONAL REQUIRED` block.
If none: omit.

### 10. Today's First Action
One specific sentence for the team. The single highest-leverage action to take before 10 AM.

### 11. Disclaimer Block (always)
Append full disclaimer from `_guardrails/shared/output-disclaimers.md`. No exceptions.

---

## Calibration Log Entry (After Every Brief)

Append to `_working/_calibration_log.md`:

```
## [YYYY-MM-DD] Morning Brief
- Active cases: [count]
- Urgent items: [count]
- Connectors active: [Gmail Y/N · Calendar Y/N]
- Confidence level: [HIGH / MEDIUM / LOW]
- Escalated to Diana: [Y/N — count if Y]
- Data gaps: [list or "none"]
- Brief accuracy note (fill on follow-up): [did urgent items resolve as flagged?]
```

---

*Layer placement: L2 Task Contract · Daily brief mode · Load only when brief is requested*
