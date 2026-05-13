# Operating Rhythm
## AgencyOS — Daily & Weekly Cadence

This document defines how the agency uses AgencyOS in the flow of a normal week. The system works when the team uses it consistently. The system fails when the team works around it.

---

## Daily Rhythm

### Morning (Before Client Hours)

**8:45 AM — Daily Brief Available**
The daily deal desk produces the morning brief automatically. Every agent checks it before making their first client call.

Each agent reads:
- Their items in 🔴 URGENT and 🟡 ATTENTION
- Diana's queue (for awareness)
- Any stale alerts on cases they own

Time required: 5-7 minutes.

**9:00 AM — Action on Urgent Items**
Urgent items from the brief are actioned first. Before outbound prospecting. Before admin. Before anything.

The brief is only useful if it changes what the team does first.

---

### Active Hours (9 AM – 6 PM)

**Lead intake:** New leads route through the orchestrator. Agent or Diana assigns ownership. Lead qualifier activates within 2 hours for hot leads.

**Client communication:** Agents review drafted communications from 03_client_communication before sending. Edits are encouraged — agents know their clients. Drafts are a starting point, not a final product.

**Property research requests:** Agent or client identifies a property. Agent submits research request to orchestrator. Research brief returned before the next showing.

**Transaction updates:** TC specialist is updated after every significant touchpoint — lender call, inspection, amendment, document received. Do not let transaction state go stale.

---

### End of Day (Before 7 PM)

**One-touch case state hygiene:**
For any case that had significant activity today, verify:
- `communication_log.last_touch_date` is current
- `operational.next_human_action` and `next_due_date` are updated
- Any new risk flags are logged

This takes 2-3 minutes per active case. It is what makes the next morning's brief accurate.

---

## Weekly Rhythm

### Monday Morning
- Review the weekly pipeline (brief covers this automatically)
- Assign ownership for any unowned new leads from the weekend
- Diana reviews Diana's queue backlog — resolve or defer with explicit reasoning

### Wednesday
- Mid-week check: any case that has not had a touch since Monday gets a brief review
- Identify any under-contract deals approaching the weekly deadline window

### Friday Afternoon
- Pipeline cleanup: any lead that has been stale for 7+ days either gets an outreach attempt or gets moved to `on_hold` or `dead` with a documented reason
- Next week preview: flag anything with a Monday or Tuesday deadline
- Junior agent check-in: Diana or senior agent reviews junior agent's active cases for anything needing guidance

---

## New Case Cadence

When a new lead arrives:
1. Orchestrator initializes case and routes to lead qualifier (same day)
2. Lead qualifier completes qualification (within 24h for hot leads, 48h for warm)
3. First client communication drafted and sent (within 2h for hot leads, 24h for warm)
4. Next touch scheduled and logged

**The 2-hour rule for hot leads:** Hot buyers and motivated sellers wait for no one. A 2-hour response window is the agency's standard. If an agent cannot respond within 2 hours, the orchestrator flags it to Diana.

---

## Under Contract Cadence

Once a deal is under contract:
- Minimum every-other-day client touch (even if brief: "All on track, talk Thursday")
- TC specialist updated after every party interaction
- Daily brief monitors all deadline proximity
- Any deadline within 72 hours triggers a proactive agent notification

**The 48-hour rule:** No under-contract client goes 48 hours without a touch. This is the most common source of client anxiety and the easiest to prevent.

---

## System Health Maintenance

**Weekly (Friday):**
- Clear out dead leads (document why and close)
- Verify all active cases have current `next_due_date` set

**Monthly:**
- Review closed cases for patterns: what friction points recurred? What worked well?
- Update any reference material in specialist files that reflects new market conditions or team conventions

**Quarterly:**
- Full system review: are the specialist rules still accurate?
- Update `industry-benchmarks` or market-specific reference material in 02_property_research
