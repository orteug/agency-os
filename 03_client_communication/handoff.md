# Handoff: Client Communication Specialist
## 03_client_communication

---

## What I Receive

**From:** 00_orchestrator, 01_lead_qualifier, or 02_property_research

**Required in every incoming handoff:**
- Case ID linking to the active case state
- `emotional_profile.client_type` (must be set — I cannot adapt tone without it)
- `emotional_profile.communication_preference`
- Current deal stage
- Specific communication task: what needs to be communicated and why

**If emotional_profile is missing:** I return the handoff to the orchestrator requesting that 01_lead_qualifier completes qualification first.

**For property-research-based communications:** I need the research brief (or key findings) in the handoff context. I do not pull from case state alone — the agent or research specialist must include the relevant content.

---

## What I Produce

**Standard output:**
1. A complete drafted communication (email or text) ready for agent review
2. Updated `communication_log` fields in the case state:
   - `last_touch_date` and `last_touch_type`
   - `last_touch_summary`
   - `next_touch_due`
   - `communication_health` status
3. A flag if the communication requires a decision from the client (so the agent knows to follow up)

**What a complete draft includes:**
- Subject line (for emails)
- Greeting addressed to the client by first name
- Body text calibrated to client type
- Clear next step or ask
- Agent sign-off (formatted for the assigned agent)

---

## Output Quality Checklist

Before routing, verify:

- [ ] Draft passes the "could the agent have written this?" test
- [ ] Tone matches `emotional_profile.client_type`
- [ ] Any research findings are translated into client language (not copy-pasted from the brief)
- [ ] Subject line is specific (not "Update" or "FYI")
- [ ] A next step or call to action is included
- [ ] `communication_log` fields updated in case state
- [ ] `next_touch_due` is set
- [ ] `communication_health` is updated

---

## Handoff to Agent (Standard Routing)

Most communications route to the agent for review and send. The handoff to the agent includes:

```yaml
handoff:
  from_specialist: 03_client_communication
  to_specialist: agent_review
  trigger: draft_complete

  task:
    type: review_and_send
    priority: [matches incoming priority]
    description: "Review and send drafted [email | text] to [client name]"

  critical_context:
    - "Client type: [from emotional_profile] — tone calibrated accordingly"
    - "Key point of the communication: [what this message accomplishes]"
    - "Decision required from client: [yes/no — and if yes, what the decision is]"

  expected_output:
    deliverable: "Communication sent to client"
    next_step: "Log send in case state and set next_touch_due"
```

---

## Handoff to 04_transaction_coordinator

When communication reveals a transaction-critical issue (client concerns about timeline, financing, inspection resolution), flag to TC:

```yaml
handoff:
  from_specialist: 03_client_communication
  to_specialist: 04_transaction_coordinator
  trigger: transaction_risk_detected

  task:
    type: coordinate_transaction
    priority: urgent
    description: "Client communication revealed [specific risk] — transaction state needs review"

  critical_context:
    - "What the client said: [verbatim or summary]"
    - "What this may affect: [which deadline or document]"
    - "Communication sent: [what the agent communicated — so TC has full context]"
```

---

## Back-Handoff Protocol

I return work to the orchestrator when:

1. **Emotional profile is missing** — I cannot calibrate tone without client type.
2. **The situation requires a strategic decision** before communicating (pricing dispute, legal concern) — Diana must decide before I draft.
3. **The communication involves information I do not have** — agent must provide specific details (offer terms, inspection specifics, lender details) before I can draft accurately.

---

## Confidence and Verification Guidance

**Typical confidence level for this specialist:** high

**Typical confidence reason:** Leave blank — drafts are clearly labeled for agent review before sending.

**verification_required:** false — agent review before send is the verification step

**ei_summary guidance:** Required on all outbound handoffs. Communication specialist carries EI summary to TC and deal desk so they understand client state.
