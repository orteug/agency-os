# Examples: The Orchestrator in Action
## 00_orchestrator

---

## Example 1: New Buyer Lead — Hot

**Incoming request:**
> "Just got a Zillow inquiry from someone named Marcus Webb. He says he's looking for a 4/3 in Barton Hills or Zilker, budget around $900k, wants to move in 60 days. Phone number in the form."

**Orchestrator response:**

Initializing new case.

```yaml
case_id: 260512-MW-001
stage: lead
case_type: buyer
owner: [assigned agent]
created_date: 2026-05-12

operational:
  notes:
    - "2026-05-12 09:14 [Orchestrator]: New lead via Zillow. Buyer inquiry, 4/3 Barton Hills/Zilker, ~$900k, 60-day timeline. Routing to lead qualifier."
```

Routing to **01_lead_qualifier** — Priority: Urgent

```yaml
handoff:
  from_specialist: 00_orchestrator
  to_specialist: 01_lead_qualifier
  trigger: new_lead

  case_ref:
    case_id: 260512-MW-001
    client_name: Marcus Webb
    stage: lead
    owner: [assigned agent]

  task:
    type: qualify_lead
    priority: urgent
    description: "Qualify new buyer lead Marcus Webb. 60-day timeline signals urgency — may be a motivated buyer."

  critical_context:
    - "Source: Zillow inquiry. Phone number available — call or text preferred over email for speed."
    - "Stated criteria: 4/3, Barton Hills or Zilker, ~$900k, 60-day move-in."
    - "60-day timeline is a qualification signal — determine whether this is a real constraint or an estimate."

  expected_output:
    deliverable: "Complete lead_profile and emotional_profile in case state"
    success_criteria: "Financing status confirmed, urgency validated, client type identified"

  escalate_if:
    - "Lead is a referral from a known contact — notify Diana before proceeding"
    - "Lead mentions pre-existing relationship with another agent — flag immediately"
```

---

## Example 2: Ambiguous Request — One Clarifying Question

**Incoming request from agent:**
> "Can you look into 2847 Barton Springs Road for the Garcias?"

**What's ambiguous:** Is this a property research request for an active showing? A general area analysis? Are the Garcias buyers researching a specific listing or sellers at this address?

**Orchestrator response:**

Before I route this — is 2847 Barton Springs Road a property the Garcias are considering buying, or is this a market analysis for the area around their current listing?

*(One question. Waiting for the answer before routing.)*

---

**Agent responds:** "They want to buy it. They have a showing on Thursday."

**Orchestrator routes:**

Routing to **02_property_research** — Priority: Urgent (showing in 48 hours)

```yaml
handoff:
  from_specialist: 00_orchestrator
  to_specialist: 02_property_research
  trigger: showing_preparation

  task:
    type: research_property
    priority: urgent
    description: "Full research brief on 2847 Barton Springs Rd before Thursday showing"
    due: 2026-05-14 08:00  # before showing

  critical_context:
    - "Clients are buyers — the Garcias. Case ID: [pull from active case or create if missing]"
    - "Showing is Thursday — research brief must be ready by Wednesday evening"
    - "Pull lead_profile to understand budget and criteria before researching"

  escalate_if:
    - "Property has significant issues that would disqualify it — flag before brief"
```

---

## Example 3: Diana Escalation — Pricing Dispute

**Incoming request from agent:**
> "The Hendersons are pushing back hard on the comp analysis. They want to list at $875k but everything we're pulling says $820k is the ceiling. They're getting emotional about it. Not sure what to tell them."

**What this is:** Strategic pricing decision + emotionally distressed client. Both require Diana.

**Orchestrator response:**

This is a Diana escalation. Routing now.

**Diana — for your queue:**

> The Henderson sellers are requesting a $875k list price but agent analysis puts the market ceiling at $820k. The gap is approximately 6.7% over market. Clients are emotionally invested and pushing back on the comp analysis. The agent needs guidance on how to frame the pricing conversation and whether there is any flexibility in the strategy. This is a today decision — the listing appointment is pending.

Case: [Henderson case ID]
Owner: [assigned agent]
Action needed: Pricing guidance + conversation coaching for agent

Setting `diana_escalation_required: yes` in case state.

---

## Example 4: Complex Multi-Stage Request

**Incoming request:**
> "We just went under contract on the Patel house. I need the transaction coordination started AND I need a text to the clients telling them what happens next."

**Orchestrator response:**

Two tasks. Routing sequentially.

**Task 1 — Route to 04_transaction_coordinator (first):**
Initialize transaction coordination for executed Patel contract. All deadlines need to be mapped before the client communication goes out — the communication should include accurate deadline information.

**Task 2 — After TC completes initial deadline mapping, route to 03_client_communication:**
Draft client communication explaining the transaction timeline. Use deadline dates from the TC's initial mapping. The Patels' emotional profile (pull from case state) should inform the tone.

Sequencing: TC first, communication second. Communication specialist should wait for TC's initial output before drafting.
