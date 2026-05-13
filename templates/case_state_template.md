# Case State Template
## AgencyOS — Blank Template

Copy this template for every new case. Replace all bracketed values.

```yaml
case_id: [YYMMDD-INITIALS-NNN]
case_type: [buyer | seller | dual_representation]
stage: [lead | qualifying | active_search | under_contract | closed | dead | on_hold]
owner: [Full Name]
created_date: [YYYY-MM-DD]
last_updated: [YYYY-MM-DD HH:MM]
last_updated_by: [specialist or agent name]

client:
  name: [Full Name(s)]
  contact_email: [email]
  contact_phone: [phone]
  preferred_contact: [email | text | call]
  type: [buyer | seller]
  referral_source: [how they found us]

lead_profile:
  urgency: [hot | warm | cold]
  budget_min: [dollar amount]
  budget_max: [dollar amount]
  financing_status: [pre_approved | in_process | cash | needs_lender | unknown]
  financing_lender: [name or unknown]
  down_payment_ready: [yes | no | unknown]
  timeline: [immediate | 30_days | 60_days | 90_days | 6_months | unclear]
  timeline_driver: [what creates the timeline]
  preferred_areas:
    - [neighborhood/zip]
  must_haves:
    - [requirement]
  nice_to_haves:
    - [preference]
  deal_breakers:
    - [disqualifier]
  constraints:
    - [constraint]

emotional_profile:
  client_type: [see CASE_STATE_SCHEMA.md for types]
  confidence: [high | medium | low]
  anxiety_level: [high | medium | low]
  responsiveness: [fast | normal | slow | inconsistent]
  decision_style: [fast_mover | deliberate | needs_consensus | impulsive]
  reassurance_required: [high | medium | low]
  communication_preference: [detailed | concise | visual | verbal | mixed]
  communication_notes: [any specific notes]
  red_flags:
    - [concerning pattern if any]

research_state:
  properties_reviewed: []
  current_focus_address: [address or blank]
  current_focus_mls: [MLS or blank]
  market_conditions: [buyers_market | balanced | sellers_market]
  competition_level: [high | medium | low]
  pricing_leverage: [buyer | neutral | seller]
  resale_risk: [high | medium | low]
  negotiation_leverage: [buyer advantage or blank]
  recommended_strategy: [one sentence or blank]

communication_log:
  last_touch_date: [YYYY-MM-DD or blank]
  last_touch_time: [HH:MM or blank]
  last_touch_type: [email | text | call | showing | in_person | voicemail]
  last_touch_summary: [one sentence]
  last_touch_by: [agent name]
  next_touch_due: [YYYY-MM-DD]
  next_touch_type: [what kind of follow-up]
  communication_health: [green | yellow | red]
  sentiment: [positive | neutral | concerned | frustrated | disengaged]

risk_state:
  overall_risk: [critical | high | medium | low | none]
  risk_flags: []
  deadline_risk: [none | approaching | critical | overdue]
  financing_risk: [none | low | medium | high | critical]
  communication_risk: [none | low | medium | high]
  deal_fall_risk: [none | low | medium | high]

escalation:
  diana_escalation_required: [yes | no]
  escalation_reason: [blank if no]
  escalation_urgency: [blank if no]
  escalation_resolved: [yes | no]

operational:
  next_human_action: [specific action]
  next_responsible_human: [full name]
  next_due_date: [YYYY-MM-DD]
  specialist_last_touched: [specialist name]
  notes:
    - "[YYYY-MM-DD HH:MM] [who]: [note]"
```
