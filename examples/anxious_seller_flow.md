# Example Flow: Anxious Seller
## AgencyOS — End-to-End Walkthrough

Patricia Morales, emotionally attached seller. Family home of 40 years. Three siblings making decisions together. Price expectation 15-22% above market. This example shows how the system handles emotional complexity, consensus decision-making, and a sensitive pricing conversation.

---

## Step 1: Lead Arrives

**Source:** Referral from Diana's former client, the Garcia family.
**Initial message:** "My mother passed away in February. We're ready to sell her home at 1847 Travis Heights Blvd. Family home of 40 years. Looking for guidance."

**Orchestrator:**
- Initializes case: `260513-PM-001`
- Tags as referral from Garcia family — immediate Diana notification
- Routes to lead qualifier

**Orchestrator flags to Diana simultaneously:**
> Diana — referral from the Garcia family re: sale of Patricia Morales's late mother's home, 1847 Travis Heights. Estate/legacy sale, emotionally complex. Wanted you aware before lead qualifier engages.

---

## Step 2: Lead Qualification (01_lead_qualifier)

**Qualifier initial assessment:** Estate sale with emotional weight. Referral. Multiple decision-makers. Price expectation likely aspirational.

**Qualification approach — warm, not interrogative:**
> "Patricia, thank you for trusting us with something this significant. To make sure we approach this the right way for your family, a couple of questions: Is there a timeline you're working toward, and are your brothers involved in the decision?"

**Patricia responds:** "My two brothers and I are all deciding together. We'd like to list in about 3 months. We're hoping for around $950k — the neighborhood has changed so much."

**Quick market check by qualifier:** Travis Heights comps suggest $800k-$835k ceiling. $950k expectation is 15-22% above market.

**Case state updated:**
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
  communication_notes: "Legacy property. Three siblings must reach consensus. Do NOT discuss pricing until Diana has spoken with Patricia personally. Every communication should acknowledge the emotional significance before the practical."

risk_state:
  risk_flags:
    - pricing_expectation_significantly_above_market
    - multiple_decision_makers
    - emotional_sensitivity_high

escalation:
  diana_escalation_required: yes
  escalation_reason: "Referral from key client + estate sale + pricing gap 15-22% above market"
  escalation_urgency: today
```

**Qualifier routes to:** Orchestrator → Diana escalation (NOT to communication first)
*"Diana must make first contact. Do not route to communication specialist until Diana has spoken with Patricia."*

---

## Step 3: Diana's First Contact

**Diana calls Patricia the same day.**

The conversation is personal, unhurried. Diana acknowledges the loss. She doesn't mention price. She sets a listing appointment for the following week and promises to provide a thorough market analysis.

**Agent logs after call:**
```yaml
communication_log:
  last_touch_date: 2026-05-13
  last_touch_type: call
  last_touch_summary: "Diana called Patricia. Warm conversation. Listing appointment set May 20. No pricing discussed."
  last_touch_by: Diana
  next_touch_due: 2026-05-20
  communication_health: green
```

---

## Step 4: Pre-Listing Research Brief

**Diana requests research brief on 1847 Travis Heights for the listing appointment.**

**Research specialist produces:**
- Comp center: $815k-$835k for comparable properties
- Market condition: balanced, slight buyer lean in this price range
- Negotiation framing for Diana: lead with genuine appreciation gains, present comp ceiling, offer the "priced to move" vs. "priced to sit" framing
- Recommendation: Diana should present $825k as the "designed to generate competition" price vs. $950k as the "designed to wait and see" price. Let Patricia and her brothers choose which outcome they want.

---

## Step 5: Listing Appointment — Pricing Conversation

**Diana attends with the research brief. All three siblings present.**

The conversation acknowledges the family's years there, the mother's legacy, the significance of the decision. Then Diana presents the comp analysis — not as a ceiling imposed by the market, but as a reality the family can choose to work with or against.

**Outcome:** After 45 minutes of conversation, the siblings agree to list at $845k. Not quite the comp center — Diana honored the family's desire for something above market — but within a defensible range.

**Case state updated:**
```yaml
lead_profile:
  desired_list_price: 845000
  stage: active_search  # moving toward listing preparation

escalation:
  diana_escalation_required: no
  escalation_resolved: yes

communication_log:
  last_touch_summary: "Listing appointment completed. Pricing agreed at $845k. All three siblings aligned. Listing prep begins."
```

---

## What the System Preserved

Every subsequent agent interaction with the Morales family benefits from:
- The emotional profile flagging that every communication must lead with acknowledgment before practicalities
- The communication note that three siblings must be kept equally informed
- The pricing history showing the gap between expectation and resolution (useful if price reduction conversations arise later)
- The risk flags ensuring no agent accidentally treats this as a transactional relationship
