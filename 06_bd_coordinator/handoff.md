# Handoff Protocol: Business Development Coordinator
## 06_bd_coordinator

---

## Inbound Handoffs (what the BD Coordinator receives)

### From 00_orchestrator — New BD Contact
**Trigger:** Lead scored below active threshold, or contact explicitly not ready to transact.

```yaml
from_specialist: 00_orchestrator
to_specialist: 06_bd_coordinator
trigger: lead_scored_below_active_threshold | contact_not_ready

case_ref:
  case_id:
  client_name:
  stage: lead
  owner:

task:
  type: initialize_bd_contact
  priority: normal
  description: "Add to BD pipeline at appropriate status. Set touch cadence based on EI profile and stated timeline."

critical_context:
  - "[Why this contact is not active — what did they say, what was their stated timeline]"
  - "[Money psychology type if established by lead qualifier]"
  - "[Any prior relationship context — referral source, past meeting, etc.]"

expected_output:
  deliverable: "Populated bd_state in Agency File. First touch scheduled."
  format: "Update CASE_STATE_SCHEMA bd_state fields. Confirm scheduled_touch date."
  success_criteria: "bd_state.status set correctly. touch_cadence appropriate to client type. graduation_threshold documented."

escalate_if:
  - "Contact is a referral from a VIP relationship — Diana should be notified of this contact personally"
  - "Contact expressed interest in a specific property — may belong in active pipeline, not BD"

confidence_level: medium
confidence_reason: "Lead qualifier established initial profile but full EI assessment may be incomplete at this stage."
verification_required: false
ei_summary: "[Copy from lead qualifier handoff if available]"
```

---

## Outbound Handoffs (what the BD Coordinator sends)

### To 03_client_communication — Touch Due
**Trigger:** Scheduled touch date reached, or opportunity signal detected.

```yaml
from_specialist: 06_bd_coordinator
to_specialist: 03_client_communication
trigger: bd_touch_due | opportunity_signal_detected

case_ref:
  case_id:
  client_name:
  stage: bd
  owner:

task:
  type: draft_bd_communication
  priority: normal
  description: "Draft a [touch type] communication for [client name] based on the BD brief below."

critical_context:
  - "[What the touch should accomplish — relationship maintenance | opportunity signal | milestone acknowledgment]"
  - "[Specific reference points to include — prior conversation detail, market event, life milestone]"
  - "[What to avoid — any topics that would feel sales-y or out of place given client type]"

expected_output:
  deliverable: "Drafted email or text for agent review"
  format: "Communication log entry + draft in agent's voice"
  success_criteria: "Draft is appropriate to money psychology type. No manufactured urgency. Achieves the stated touch objective."

escalate_if:
  - "Contact has expressed frustration or disengagement — Diana should review before sending"
  - "Draft touches on a sensitive personal topic (divorce, loss, financial difficulty) — agent review required"

already_done:
  - "EI profile reviewed"
  - "Touch cadence verified"
  - "Relevant context assembled"

confidence_level: high
verification_required: false
ei_summary: "[Money psychology type + primary motivation + one communication instruction specific to this touch]"
```

---

### To 00_orchestrator — Graduation
**Trigger:** Explicit or strong implicit graduation signals detected.

```yaml
from_specialist: 06_bd_coordinator
to_specialist: 00_orchestrator
trigger: graduation_signals_detected

case_ref:
  case_id:
  client_name:
  stage: bd → lead (recommended)
  owner:

task:
  type: reclassify_to_active
  priority: urgent
  description: "BD contact has shown graduation signals. Recommend reclassification to active pipeline and fresh qualification."

critical_context:
  - "[Specific signals observed — what changed, what was said or done]"
  - "[Time in BD pipeline — how long this relationship has been developed]"
  - "[Relationship depth — introduction | acquaintance | trusted | advocate]"

expected_output:
  deliverable: "Routing decision: reclassify to active and route to 01_lead_qualifier, or hold in BD with elevated monitoring"
  format: "Orchestrator confirms routing"
  success_criteria: "Fresh qualification preserves relationship continuity built during BD phase"

escalate_if:
  - "Contact is a high-value relationship — Diana should be personally briefed before any outreach"
  - "Competing agent mentioned — urgency elevated, Diana's direct involvement recommended"

already_done:
  - "Graduation signals documented in bd_state.graduation_signals"
  - "Graduation threshold evaluated against bd_state.graduation_threshold"

confidence_level: high
verification_required: false
```

---

## Back-Handoff Protocol

If 03_client_communication returns a draft that does not match the BD brief:
- Return the draft with specific notes on what needs adjusting (tone, reference points, length)
- Do not escalate to Diana for communication style mismatches — work through the 03_client_communication specialist

If 00_orchestrator disagrees with a graduation recommendation:
- Accept the routing decision and continue BD management
- Increase monitoring frequency for signals
- Note the disagreement in `bd_state.bd_notes`

---

## Confidence and Verification Guidance

**Typical confidence level:** medium

**Typical confidence reason:** Touch recommendations are based on the EI profile as populated by the lead qualifier and agent. If the profile is incomplete or outdated, touch recommendations may be misaligned. Graduation signal assessments are high-confidence when signals are explicit, medium-confidence when implicit.

**verification_required:** false for touch recommendations. true for any market-driven opportunity signal that contains specific pricing or cap rate claims — these must be flagged for agent verification before reaching the client.

**ei_summary guidance:** Required on all outbound handoffs to 03_client_communication. The communication specialist needs to know the money psychology type, what the touch is designed to accomplish, and what emotional register is appropriate. A BD communication that gets the psychology wrong undermines months of relationship building in one message.
