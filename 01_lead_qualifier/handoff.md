# Handoff: Lead Qualifier
## 01_lead_qualifier

---

## What I Receive

**From:** 00_orchestrator

**Required in every incoming handoff:**
- Case ID (or direction to create a new case)
- Lead's name and contact method
- Source of the lead
- Verbatim or summarized initial message
- Any urgency signals observed by the orchestrator

**Minimum to begin qualification:** Name + contact method + intent (buying/selling). If less than this arrives, I ask the orchestrator for clarification before proceeding.

---

## What I Produce

A fully populated `lead_profile` and `emotional_profile` in the case state, plus:

**For hot and warm leads:**
A structured handoff to 03_client_communication with:
- Client type and communication preference
- The specific communication task (first follow-up, re-engagement, etc.)
- The most important context the communication specialist needs
- Any timing constraints

**For Diana escalations:**
A structured escalation to 00_orchestrator with:
- Three-sentence escalation summary (situation, complexity, what Diana needs to decide)
- All populated case state fields
- Recommended next action pending Diana's response

**For cold leads:**
A handoff to 03_client_communication with:
- Low priority designation
- Single-touch nurture communication request
- Follow-up cadence recommendation

---

## Output Quality Checklist

Before routing, all of these must be populated:

**lead_profile:**
- [ ] `urgency` (hot / warm / cold — not blank)
- [ ] `financing_status` (a value, even if `unknown`)
- [ ] `timeline` (a value, even if `unclear`)
- [ ] `preferred_areas` or `property_address` (for sellers)
- [ ] `budget_max` or `desired_list_price` (even if range or estimate)

**emotional_profile:**
- [ ] `client_type` (must use a documented type, not a custom label)
- [ ] `anxiety_level`
- [ ] `responsiveness`
- [ ] `communication_preference`

**operational:**
- [ ] `next_human_action` (specific — not "follow up")
- [ ] `next_responsible_human` (named person)
- [ ] `next_due_date`

**risk_state:**
- [ ] `risk_flags` (populated list or confirmed empty list)

---

## Back-Handoff Protocol

I return work to the orchestrator when:

1. **Lead contact is invalid.** Phone disconnected, email bounced, form fill was spam.
   - Document: what was attempted, what failed
   - Orchestrator: routes to agent to find alternative contact

2. **Lead requires Diana first.** Referral from key relationship, legal complexity, emotionally sensitive situation requiring Diana's personal touch.
   - Document: why Diana must be first contact
   - Orchestrator: escalates to Diana

3. **Lead is a duplicate.** Same person in the system under a different name or contact.
   - Document: which existing case this matches
   - Orchestrator: merges with existing case

---

## Handoff to 03_client_communication — Template

```yaml
handoff:
  from_specialist: 01_lead_qualifier
  to_specialist: 03_client_communication
  trigger: lead_qualified

  task:
    type: draft_communication
    priority: [urgent | normal | low]
    description: "Draft [first_follow_up | re_engagement | educational_touch] for [client name]"

  critical_context:
    - "Client type: [from emotional_profile.client_type] — adapt tone accordingly"
    - "Key fact: [the single most important thing about this lead]"
    - "Timing: [any deadline or urgency for the communication]"

  expected_output:
    deliverable: "Drafted [email | text] for agent review and send"
    success_criteria: "Tone matches client type, addresses their core concern, includes a clear next step"

  escalate_if:
    - "Client responds with frustration or concern — return to orchestrator before drafting further"
    - "Client's response changes the urgency assessment — re-route through orchestrator"
```

---

## Confidence and Verification Guidance

**Typical confidence level for this specialist:** medium

**Typical confidence reason:** Lead profile built from initial contact only. Financial history and tail event data are inferred, not confirmed.

**verification_required:** false

**ei_summary guidance:** Required. Lead qualifier is the primary setter of the EI profile. ei_summary must be populated on every outbound handoff.
