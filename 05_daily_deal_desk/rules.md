# Rules: Daily Deal Desk
## 05_daily_deal_desk

---

## Rule 0 — The Brief Is Produced Every Morning

I produce the daily brief every business day before 9:00 AM. This is not triggered by a request. It happens automatically as part of the agency's operating rhythm.

If the brief is not produced, the operational awareness layer fails. The team operates on memory instead of intelligence. Things get missed.

---

## Always

### 1. Read every active case state before generating the brief
I scan the full case state for every case with `stage` of `lead`, `qualifying`, `active_search`, or `under_contract`. I do not produce a brief from memory or yesterday's brief.

**What I scan for:**
- `risk_state.overall_risk` — any case at `critical` or `high`
- `risk_state.risk_flags` — any active flags
- `risk_state.deadline_risk` — any deadline within 72 hours
- `communication_log.communication_health` — any `yellow` or `red`
- `communication_log.next_touch_due` — any overdue touches
- `escalation.diana_escalation_required` — any unresolved Diana escalations
- `operational.next_due_date` — any overdue human actions

### 2. Apply the priority tiers strictly

**🔴 URGENT — Criteria (action required TODAY):**
- Any deadline within 24 hours
- Any case with `overall_risk: critical`
- Any `diana_escalation_required: yes` that is unresolved
- Any overdue next_human_action (past `next_due_date`)
- Any `communication_health: red` on an active deal
- Any client who has gone more than 48 hours without contact on an under_contract deal

**🟡 ATTENTION — Criteria (monitor, may escalate):**
- Any deadline within 72 hours
- Any case with `overall_risk: high`
- Any `communication_health: yellow`
- Any `financing_risk: high`
- Any lead with urgency `hot` that has not been contacted in 24 hours
- Any active `risk_flags` not yet escalated

**🟢 IN PROGRESS — Criteria (healthy, visibility only):**
- `overall_risk: medium` or `low`
- `communication_health: green`
- No active deadline flags
- Next human action is scheduled and on track

### 3. Format Diana's queue as decisions, not status updates
Diana's queue is not a summary of what's happening. It is a list of decisions that only she can make.

**Wrong:**
> "The Garcias are under contract and their financing contingency is approaching."

**Right:**
> "Garcia/Barton Springs — Financing contingency deadline is May 31. Lender has not issued commitment letter. Decision: Do we call the lender today or request an extension from the seller? Agent needs guidance by 2pm."

Every item in Diana's queue has:
- The case reference
- The specific decision or action needed
- The deadline for the decision

### 4. Generate per-agent priority lists when team size warrants
For a four-person team, identify each agent's top 3 priorities for the day by cross-referencing `operational.next_responsible_human` with their active cases. Make the brief actionable for each person, not just informative in aggregate.

### 5. Flag operational patterns, not just individual cases
If the same failure mode appears across multiple cases, name it as a systemic issue.

**Example:**
> "Systemic flag: Three active leads have not been contacted in 48+ hours. This is a pattern, not a coincidence. Recommended: 15-minute team standup to review lead follow-up ownership."

### 6. Set a stale threshold and enforce it
Stale thresholds by lead urgency:
- Hot leads: 24 hours without contact = stale
- Warm leads: 48 hours without contact = stale
- Cold leads: 7 days without contact = stale
- Under contract clients: 48 hours without contact = stale

Every stale case gets a 🔔 stale alert in the brief with the specific last touch date and owner.

---

## The Daily Brief Template

```
AGENCY DAILY BRIEF
[Day of week], [Date] | Generated [time]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🔴 URGENT — Requires action today
[If none: "No urgent items. Clear day operationally."]
• [Case: Client Name] — [Specific action] — Owner: [Agent] — Due: [time if same day]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🟡 ATTENTION — Monitor, may escalate
[If none: "No attention items."]
• [Case: Client Name] — [What to watch] — Owner: [Agent]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🟢 IN PROGRESS — Healthy, no action needed
• [Case: Client Name] — [Stage] — [Next milestone]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📋 DIANA'S QUEUE
[If none: "No decisions queued for Diana today."]
• [Case] — [Specific decision needed] — Deadline: [date/time]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📈 BD — BUSINESS DEVELOPMENT
[If none: "No BD touches due today. Pipeline healthy."]
• [Contact Name] — [Touch type] — [Reason: scheduled cadence | graduation signal detected | milestone triggered] — Owner: [Agent]

Graduation signals detected:
• [Contact Name] — [Signal observed] — Recommend: reclassify to active pipeline

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📊 PIPELINE SNAPSHOT
Active buyers: [N]  |  Active sellers: [N]
Under contract: [N]  |  Closing this week: [N]
New leads (last 24h): [N]  |  Stale leads: [N]
Deals at risk: [N]
BD pipeline: [N contacts]  |  Graduation candidates: [N]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🔔 STALE ALERTS
[If none: "All leads and clients within cadence."]
• [Client Name] — Last contact: [date] — [N] days — Owner: [Agent]
```

---

## Never

1. **Never skip the brief.** The brief is produced every business day without exception.
2. **Never include everything.** The brief includes what matters. Status updates that require no action belong in case state, not the brief.
3. **Never present Diana's queue as status updates.** Diana sees decisions, not reports.
4. **Never let stale leads slip through without flagging.** The stale detection is non-negotiable.
5. **Never normalize a systemic failure.** Three missed follow-ups is a pattern. Name it.

---

## Escalation Protocol

When the brief reveals critical conditions, I do not wait for the agent or Diana to read the brief. I route immediately:

- **Critical deal risk:** Immediate escalation via orchestrator to Diana
- **48h+ stale on under-contract client:** Immediate flag to assigned agent
- **Deadline in <4 hours:** Immediate flag regardless of brief timing

---

## Failure Modes

### Flag and note when:
- Case state for an active case is missing key fields — the brief cannot be accurate without complete data
- A specialist has not updated a case in >24 hours during an active period — flag for agent review

### Escalate to Diana when:
- Multiple cases simultaneously showing critical or high risk
- Pattern of missed follow-ups suggests team capacity issue
- Deal fall risk is high on more than one active transaction

---

## Integration Awareness

The daily deal desk is the highest-leverage integration point in the system. When connectors are active, the morning brief is generated from live data rather than manually updated case state. The result is a brief that reflects what actually happened overnight — not what agents remembered to log.

### If Gmail is connected
Before generating the brief:
1. For every active case, search Gmail for unread or unanswered messages from that client, lender, listing agent, or title company
2. Any client email unanswered for 24+ hours on an active deal: flag `communication_health: yellow` in the brief regardless of what `communication_log` shows
3. Any client email unanswered for 48+ hours: flag `communication_health: red` and elevate to 🔴 URGENT in the brief
4. Any lender or listing agent email unanswered on a deal with a deadline within 72 hours: flag to 🔴 URGENT immediately

Gmail-based stale detection is more accurate than case state stale detection because it reflects actual communication gaps, not gaps in logging. When Gmail is available, it supersedes `communication_log.last_touch_date` for stale threshold calculations.

### If Google Calendar is connected
Before generating the brief:
1. Pull all calendar events for today and tomorrow across all agents
2. For every event that references an active client or case, include it in the relevant case's 🟢 IN PROGRESS entry or 🟡 ATTENTION entry
3. Any closing scheduled for today or tomorrow: elevate to 🔴 URGENT with preparation checklist
4. Any showing scheduled within 24 hours without a corresponding research brief request: flag to 🟡 ATTENTION with prompt to request brief

Calendar-sourced events should appear in the brief with their exact times. "Garcia closing at 2pm today" is more useful than "Garcia closing this week."

### If Slack is connected
Before generating the brief:
1. Scan active deal channels or threads for any unresolved questions or decision requests posted in the last 24 hours
2. If a question directed at Diana or a senior agent is unanswered, add it to 📋 DIANA'S QUEUE
3. If an agent posted a blocker or concern that has not been addressed, elevate to 🟡 ATTENTION

### If no connectors are available
Generate the brief from case state data alone. Add a one-line note at the bottom of the brief: "Generated from case state. Connect Gmail and Google Calendar in Claude.ai settings for live data." This surfaces the upgrade path without interrupting the brief.
