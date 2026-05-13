# Handoff Protocol
## AgencyOS — How Work Moves Between Specialists

> **Handoffs move work. Case state preserves reality.**
>
> The case state schema tells every specialist what is true.
> This protocol tells every specialist how to pass work along.
> These are different things. Both are required.

---

## Core Principle

A handoff is not a summary. A handoff is a structured contract.

It contains:
1. **What happened** — what the sending specialist did
2. **What's needed** — the specific task for the receiving specialist
3. **What's true** — a reference to the updated case state
4. **What matters most** — the 3 things the receiving specialist cannot miss

A handoff that says "here's the context, please help" is not a handoff. It is a forwarded email.

---

## The Handoff Envelope

Every specialist-to-specialist handoff uses this structure:

```yaml
handoff:

  # Who is sending, who is receiving, and why
  from_specialist:          # e.g., "01_lead_qualifier"
  to_specialist:            # e.g., "03_client_communication"
  timestamp:                # YYYY-MM-DD HH:MM
  trigger:                  # what caused this handoff
                            # e.g., "lead_qualified", "research_complete", "showing_completed"

  # The case this handoff belongs to
  case_ref:
    case_id:                # must match case state
    client_name:            # for human readability
    stage:                  # current stage from case state
    owner:                  # assigned agent

  # The task being handed off
  task:
    type:                   # qualify_lead | research_property | draft_communication |
                            # coordinate_transaction | generate_daily_brief | escalate_to_diana
    priority:               # urgent | normal | low
                            # urgent = needs response within 2 hours
                            # normal = needs response same business day
                            # low = next available
    description:            # one sentence: specifically what needs to be done
    due:                    # YYYY-MM-DD HH:MM if time-sensitive

  # The three things the receiving specialist must know
  critical_context:
    - "[Item 1 — the most important fact about this case right now]"
    - "[Item 2 — the second most important fact]"
    - "[Item 3 — the third most important fact]"

  # What the receiving specialist needs to produce
  expected_output:
    deliverable:            # what is the output? (e.g., "qualified lead profile", "drafted email")
    format:                 # where does it go? (e.g., "update case state + draft in communication log")
    success_criteria:       # how do we know it's done well?

  # Escalation rules for this specific handoff
  escalate_if:
    - "[condition that requires Diana or agent involvement]"
    - "[condition that requires returning work to previous specialist]"

  # What the sending specialist already tried or confirmed
  already_done:
    - "[anything the receiving specialist does NOT need to redo]"

  # Any special instructions
  notes:                    # optional, only if something doesn't fit above
```

---

## Standard Handoff Paths

These are the most common routing patterns. Each one has specific required fields.

---

### Path 1: New Lead → Orchestrator → Lead Qualifier

**Trigger:** New lead arrives via any channel (Zillow, referral, website, phone)

**Orchestrator passes to 01_lead_qualifier:**

```yaml
task:
  type: qualify_lead
  priority: urgent  # all new leads are urgent
  description: "New [buyer/seller] lead requires qualification"

critical_context:
  - "Source: [where the lead came from]"
  - "Initial message: [verbatim or summary of first contact]"
  - "Contact method available: [phone/email/text]"

expected_output:
  deliverable: "Complete lead_profile + emotional_profile in case state"
  format: "Updated case state + handoff to 03_client_communication or agent"
  success_criteria: "All required lead_profile fields populated, urgency assessed, next touch scheduled"

escalate_if:
  - "Lead mentions legal dispute, divorce, or estate situation — escalate to Diana"
  - "Lead is a known referral from a key relationship — notify Diana immediately"
```

---

### Path 2: Lead Qualifier → Client Communication

**Trigger:** Lead qualified, first follow-up communication needed

**01_lead_qualifier passes to 03_client_communication:**

```yaml
task:
  type: draft_communication
  description: "Draft initial follow-up to newly qualified lead"

critical_context:
  - "Client type: [from emotional_profile.client_type]"
  - "Urgency: [from lead_profile.urgency]"
  - "Key constraint or motivation: [most important fact from qualification]"

expected_output:
  deliverable: "Drafted email or text for agent review"
  format: "Draft in communication log + send for agent approval"
  success_criteria: "Tone matches client type, addresses their stated need, includes clear next step"

escalate_if:
  - "Lead expresses frustration or dissatisfaction during qualification — escalate before drafting"
  - "Lead has unrealistic expectations (budget vs. desired area) — flag before communication"
```

---

### Path 3: Active Search → Property Research

**Trigger:** Agent or client identifies a property to research

**Orchestrator passes to 02_property_research:**

```yaml
task:
  type: research_property
  description: "Full research brief on [address]"

critical_context:
  - "Client type and emotional profile: [key facts from emotional_profile]"
  - "Budget ceiling: [from lead_profile.budget_max]"
  - "Primary concern or motivation: [what matters most to this client]"

expected_output:
  deliverable: "Complete research brief formatted for agent-client conversation"
  format: "Saved to case state research_state + formatted brief document"
  success_criteria: "Includes neighborhood interpretation, negotiation leverage, hidden risks, and emotional fit analysis"

escalate_if:
  - "Property has active litigation, structural red flags, or unusual permit history — flag before brief"
  - "Property is significantly outside client's stated budget or criteria — flag before researching"
```

---

### Path 4: Property Research → Client Communication

**Trigger:** Research complete, agent needs to present findings to client

**02_property_research passes to 03_client_communication:**

```yaml
task:
  type: draft_communication
  description: "Draft research summary communication for client"

critical_context:
  - "Client emotional type: [from emotional_profile.client_type]"
  - "Key finding: [the most important thing from research]"
  - "Recommended framing: [how to position findings for this client]"

expected_output:
  deliverable: "Drafted client communication ready for agent review"
  format: "Draft in communication log"
  success_criteria: "Translates research into language appropriate for client type, builds toward a decision"

escalate_if:
  - "Research surfaced significant negative findings — agent must review before any client communication"
```

---

### Path 5: Accepted Offer → Transaction Coordinator

**Trigger:** Purchase agreement executed

**Orchestrator passes to 04_transaction_coordinator:**

```yaml
task:
  type: coordinate_transaction
  description: "Initialize transaction coordination for executed contract"

critical_context:
  - "Closing date: [from transaction_state.closing_date]"
  - "Option period expires: [from transaction_state.option_period_expires]"
  - "Client emotional profile: [key facts — especially anxiety level]"

expected_output:
  deliverable: "Complete transaction checklist with all deadlines mapped + missing document list"
  format: "Updated transaction_state in case state + deadline summary to agent"
  success_criteria: "All deadlines logged, all required documents identified, first risk flag issued if any deadline within 72h"

escalate_if:
  - "Contract has unusual terms or contingencies — escalate to Diana before initializing"
  - "Closing date is fewer than 21 days away — flag as critical immediately"
```

---

### Path 6: Any Specialist → Daily Deal Desk

**Trigger:** Automatic (morning review) or requested case summary

**Any specialist passes to 05_daily_deal_desk:**

```yaml
task:
  type: generate_daily_brief
  description: "Include this case in next daily brief"

critical_context:
  - "Current risk level: [from risk_state.overall_risk]"
  - "What changed since last brief: [key update]"
  - "What action is needed today: [specific item]"

expected_output:
  deliverable: "Case included in next morning brief with appropriate priority tier"
  format: "Integrated into daily brief output"
  success_criteria: "Risk correctly tiered, owner assigned, action clear"
```

---

### Path 7: Any Specialist → Diana Escalation

**Trigger:** Any specialist encounters a condition requiring Diana's judgment

**Any specialist escalates via orchestrator:**

```yaml
task:
  type: escalate_to_diana
  priority: urgent

critical_context:
  - "Escalation reason: [specific reason from escalation triggers]"
  - "What has already been tried: [what the specialist attempted]"
  - "What Diana needs to decide: [specific decision needed]"

expected_output:
  deliverable: "Diana is informed and the case is held pending her response"
  format: "Escalation logged in case state + notification to Diana"
  success_criteria: "Diana understands the situation and has a specific decision to make"

escalate_if:  # (Meta-escalation — when to escalate the escalation)
  - "Diana is unavailable and deadline is within 4 hours — agent handles with best judgment, log decision"
```

---

## Handoff Validation Rules

Before any specialist passes a handoff, it must verify:

**Hard requirements (handoff rejected if missing):**
- [ ] `case_id` present and matches an active case
- [ ] `task.type` is a recognized task type
- [ ] `task.priority` is set
- [ ] `task.description` is specific (not "please help with this")
- [ ] `critical_context` has at least one item
- [ ] `expected_output.deliverable` is specific
- [ ] Sending specialist has updated `last_updated` in case state

**Soft requirements (flag but don't reject):**
- [ ] `task.due` is set for urgent tasks
- [ ] `already_done` is populated to avoid duplicated work
- [ ] `escalate_if` conditions are checked before sending

---

## When to Reject a Handoff

A specialist must reject (return to sender) any handoff that:

1. Does not reference a valid case ID
2. Has a task description that is ambiguous ("help with client")
3. Is missing required context for the task type
4. Requests work the specialist cannot perform (wrong specialist)
5. Contains conflicting instructions from the case state

**When rejecting:** Return the handoff to the orchestrator with a specific note on what is missing or conflicting. Do not attempt to complete the work.

---

## Handoff Latency Standards

| Priority | Maximum response time | Escalate if exceeded |
|----------|----------------------|---------------------|
| Urgent | 2 hours | Yes — alert agent |
| Normal | Same business day | Yes — alert agent if end of day |
| Low | 48 hours | No — monitor in daily brief |

---

## The Golden Rule

> **If the next person can't pick this up cold and know exactly what to do, the handoff is incomplete.**

Read your handoff as if you had never seen this case before.
If you have questions, answer them in the handoff before sending it.
