# Example Flow: Inspection Issue
## AgencyOS — End-to-End Walkthrough

This example traces the Garcia family through an inspection that surfaces a significant issue, showing how the system coordinates the response across communication, transaction coordination, and escalation.

---

## Situation

Garcia / 2847 Barton Springs Road — Under contract, closing June 12.
Option period expires May 22 (10 days from contract date of May 12).
Inspection scheduled May 14. Report delivered May 15.

---

## Step 1: Inspection Report Received

**Agent logs in transaction coordinator:**
Inspection report received. Key items:
- Roof: Inspector recommends full replacement. Estimated $18,000-$22,000.
- Foundation: Two piers needed in the back-left quadrant. Estimated $4,500-$6,000.
- HVAC: Filter replacement, tune-up recommended. ~$200.
- Minor: Two outlets, crack in garage floor (cosmetic).

**TC specialist updates case state:**
```yaml
transaction_state:
  documents:
    inspection_report: complete
  pending_actions:
    - "Inspection amendment negotiation — due before May 22"

risk_state:
  overall_risk: high
  risk_flags:
    - inspection_amendment_outstanding
    - option_period_expires_may_22
    - significant_repair_items_roof_and_foundation
  deadline_risk: approaching
```

**TC routes to:** 03_client_communication + Orchestrator flag for agent
*"Inspection results require client communication. Two significant items: roof ($18-22k) and foundation ($4.5-6k). Total repair scope ~$22,500-$28,000. Agent needs to advise on amendment request before communicating."*

---

## Step 2: Agent + Diana Strategy Conversation

**Orchestrator flags to agent and Diana:**
> Garcia inspection returned two significant items — roof replacement (~$20k) and foundation repairs (~$5k). Total exposure ~$25k. We're in the option period with 7 days remaining. Amendment request needs to go out today or tomorrow to leave negotiation time before May 22. What's our ask?

**Diana and agent decide:** Request a $20,000 credit at closing. This accounts for roof and foundation, is documented, and is more likely to move than asking for repairs.

---

## Step 3: Client Communication (03_client_communication)

**Client type:** relocating_family, medium anxiety
**EI profile:** `money_psychology_type: time_horizon_mismatch`, `primary_fear: inspection_surprise`, `tail_event_history: unknown`
**Situation:** Inspection found significant but manageable issues. Client needs to understand what was found, what it means, and that the team has a plan. The time_horizon_mismatch type means they cannot afford to walk away — their July 1 deadline is fixed. Communication leads with reassurance and the plan, not the problem.

**Drafted communication:**
> Subject: Inspection results — here's what we found and what we're doing
>
> Hi Marcus and Elena,
>
> I have the inspection report and I want to walk you through it directly.
>
> Two items I want you to know about:
>
> **The roof** needs full replacement — the inspector estimates $18,000-$22,000. This isn't unusual for a home built in 1978, and we anticipated something like this based on what we saw during the showing.
>
> **The foundation** has two areas that need pier support — estimated $4,500-$6,000. This sounds alarming if you've never heard it before, but foundation pier repairs are extremely common in Austin's clay soil and they come with long warranties. This is a fixable, documented issue.
>
> **Here's what we're doing:** I'm submitting an amendment request today asking for a $20,000 credit at closing. This covers both items and gives you cash to handle the repairs on your own timeline after closing rather than depending on a seller to choose the contractor.
>
> You have a strong offer and a motivated seller who has already reduced their price once. We're in a good negotiating position.
>
> I'll be in touch as soon as I hear back. Call me if you have questions before then.
>
> [Agent name]

---

## Step 4: Amendment Submitted and Monitored

**TC specialist logs:**
- Amendment request submitted May 15 to listing agent
- 72-hour flag set for May 18 (if no response)
- 24-hour flag set for May 21 (option expires May 22)

**May 18 brief (🟡 ATTENTION):**
> Garcia / Barton Springs — Inspection amendment outstanding. Submitted May 15, no response from listing agent. 4 days until option expiry. Owner: [Agent] — Check in with listing agent today.

**May 20 brief (🔴 URGENT):**
> Garcia / Barton Springs — Inspection amendment STILL unresolved. Option expires tomorrow, May 22. Listing agent has not responded. Agent: Call listing agent NOW. Diana: if no response by 3pm today, you need to decide whether to request an option extension.

---

## Step 5: Resolution

**May 20, 2pm:** Listing agent responds. Seller counters at $15,000 credit.

**Agent and Diana confer (2 minutes):** Accept the counter. $15,000 covers the roof; buyers will handle foundation out of pocket. Deal is worth it.

**TC updates:**
```yaml
transaction_state:
  documents:
    inspection_amendment: complete
  completed_actions:
    - "Inspection amendment executed May 20 — $15,000 closing credit"

risk_state:
  overall_risk: medium
  risk_flags: []  # cleared
  deadline_risk: none
```

**Daily brief (May 21):**
> Garcia / Barton Springs — Inspection amendment resolved. $15k credit. Deal clean. Next milestone: financing commitment letter due May 31. On track.
