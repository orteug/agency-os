# Examples: Lead Qualifier in Action
## 01_lead_qualifier

---

## Example 1: Hot Buyer Lead — Pre-Approved, Clear Timeline

**Incoming from orchestrator:**

New buyer lead, Marcus Webb. Source: Zillow inquiry.
Initial message: "Looking for 4/3 in Barton Hills or Zilker, budget around $900k. Want to move in 60 days."
Contact: phone number in form.

---

**Lead qualifier assessment:**

Initial signals: 60-day timeline + specific areas + specific budget = potentially hot. Need to confirm financing.

**Qualification via one-touch (text or email):**

> "Hi Marcus, thanks for reaching out! I'm with Diana's team and saw your inquiry. A couple quick questions so I can point you to the right properties: Are you currently pre-approved with a lender, or still in that process? And is the 60-day timeline tied to something specific like a lease ending?"

**Marcus responds:** "Yes, pre-approved through First Texas Bank, $950k max. Relocating from Chicago for a new job starting July 1. Need to be settled before then."

---

**Updated case state:**

```yaml
lead_profile:
  urgency: hot
  budget_max: 950000
  financing_status: pre_approved
  financing_lender: First Texas Bank
  timeline: immediate
  timeline_driver: job_start_july_1
  preferred_areas:
    - Barton Hills
    - Zilker
  must_haves:
    - 4 bedrooms
    - 3 bathrooms
  constraints:
    - relocation_from_out_of_state

emotional_profile:
  client_type: relocating_family
  confidence: medium
  anxiety_level: medium
  responsiveness: fast
  reassurance_required: medium
  communication_preference: concise
  communication_notes: "Job-start deadline creates real urgency. Chicago-based means limited ability to tour. Likely needs video tours or trusted agent guidance on properties they can't visit."

risk_state:
  risk_flags:
    - relocation_timeline_hard_deadline
    - limited_in_person_touring_capacity

operational:
  next_human_action: "Call Marcus today to introduce Diana's team and schedule initial video consultation"
  next_responsible_human: "[assigned agent]"
  next_due_date: 2026-05-12
```

**Routing:**
Handoff to **03_client_communication** — Urgent. Draft first follow-up for Marcus acknowledging his relocation timeline and explaining how the team will support remote buyers.

---

## Example 2: Emotionally Attached Seller — Pricing Gap Risk

**Incoming from orchestrator:**

New seller lead, Patricia Morales. Referral from Diana's former client.
Initial message: "My mother passed away in February and we're ready to sell her home at 1847 Travis Heights Blvd. We've lived there as a family for 40 years. Looking for guidance."

---

**Lead qualifier assessment:**

Estate or family legacy sale. High emotional complexity. Referral from Diana's client base — notify Diana immediately. Pricing expectations likely to be above market.

**Diana notification (immediate):**

> Diana — new referral from the Garcia family regarding the sale of Patricia Morales's late mother's home at 1847 Travis Heights Blvd. Travis Heights property, family of 40 years. This is likely an emotionally sensitive sale. Wanted you aware before we engage further.

**Qualification (warm, not interrogative):**

> "Patricia, thank you for trusting us with something so significant. I want to make sure we approach this the right way for your family. A few questions to help us understand the situation: Is there a timeline you're working toward for the sale, or are you still figuring that out? And are there other family members involved in the decision?"

**Patricia responds:** "My two brothers and I are all involved. We'd like to list in about 3 months once we've cleared out the house. We're hoping to get around $950k — the neighborhood has changed so much."

---

**Quick market check (note from qualifier):**

Travis Heights comps in the last 90 days suggest a ceiling of $780k-$820k for this address size. A $950k expectation is approximately 15-22% above market. This is a Diana pricing conversation, not an agent conversation.

**Updated case state:**

```yaml
lead_profile:
  urgency: warm
  timeline: 90_days
  motivation: estate_sale
  desired_list_price: 950000
  constraints:
    - multiple_decision_makers_three_siblings
    - property_clearance_required_before_listing

emotional_profile:
  client_type: emotionally_attached_seller
  confidence: low
  anxiety_level: medium
  reassurance_required: high
  communication_preference: detailed
  communication_notes: "Three siblings — consensus required. Legacy property with 40-year family history. Pricing expectation 15-22% above market — Diana must set expectations carefully. Do NOT discuss pricing until Diana has spoken with Patricia."

risk_state:
  risk_flags:
    - pricing_expectation_significantly_above_market
    - multiple_decision_makers
    - emotional_sensitivity_high

escalation:
  diana_escalation_required: yes
  escalation_reason: "Referral from Garcia family + pricing expectation 15-22% above market + emotionally complex estate sale"
  escalation_urgency: today

operational:
  next_human_action: "Diana to call Patricia personally before any pricing discussion"
  next_responsible_human: Diana
  next_due_date: 2026-05-13
```

**Routing:** Escalation to Diana (not to communication specialist) — Diana must make first contact on this one.

---

## Example 3: Qualification Failure — Lead Not Ready

**Incoming from orchestrator:**

New buyer lead, Kevin T. Source: Website contact form.
Initial message: "Interested in buying in Austin. What's the market like?"

---

**Lead qualifier assessment:**

No timeline, no budget, no criteria. Market question suggests early research phase. Qualification conversation needed before routing.

**Initial contact:**

> "Hi Kevin! Happy to give you the lay of the land. Are you actively looking to buy, or more in the research phase right now?"

**Kevin responds:** "Just looking for now. Probably 2-3 years out."

---

**Qualifier decision:**

2-3 year timeline = cold lead. Not an active qualification opportunity. Routing to communication for a single educational touch and setting a 6-month follow-up cadence.

```yaml
lead_profile:
  urgency: cold
  timeline: unclear
  financing_status: unknown

emotional_profile:
  client_type: browsing_buyer
  reassurance_required: low
  communication_preference: concise

operational:
  next_human_action: "Send one market overview email and set 6-month follow-up reminder"
  next_responsible_human: "[assigned agent]"
  next_due_date: 2026-05-13
```

**Routing:** Handoff to **03_client_communication** — Low priority. Draft a single educational market overview. No aggressive follow-up cadence.
