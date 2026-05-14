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
  client_type: [first_generation_homeowner | relocating_family | anxious_first_timer | equity_moveup | analytical_optimizer | investor_buyer | luxury_buyer | life_stage_seller | distressed_seller | equity_rich_seller | reluctant_seller | investor_seller | estate_seller | upgrading_seller]
  money_psychology_type: [reasonable | rational | loss_averse | tail_event_sensitive | safety_seeking | time_horizon_mismatch | compounding_oriented]
  financial_history_notes: [what do we know about their financial background]
  primary_fear: [losing_the_deal | overpaying | inspection_surprise | financing_failure | making_wrong_decision | market_timing | other]
  primary_motivation: [stability | investment_return | lifestyle | family_milestone | financial_freedom | necessity | identity]
  tail_event_history: [yes | no | unknown]
  tail_event_notes: [if yes, describe briefly]
  confidence: [high | medium | low]
  anxiety_level: [high | medium | low]
  responsiveness: [fast | normal | slow | inconsistent]
  decision_style: [fast_mover | deliberate | needs_consensus | impulsive]
  reassurance_required: [high | medium | low]
  time_horizon: [immediate | near | medium | long]
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

bd_state:
  status: [active_bd | dormant | sphere | past_client | referral_source]
  scheduled_touch: [YYYY-MM-DD]
  touch_cadence: [weekly | biweekly | monthly | quarterly | annual]
  last_bd_touch: [YYYY-MM-DD]
  last_bd_touch_summary: [one sentence]
  graduation_signals:
    - [signal if any]
  graduation_threshold: [what moves this contact to active lead status]
  relationship_depth: [introduction | acquaintance | trusted | advocate]
  bd_notes:

operational:
  next_human_action: [specific action]
  next_responsible_human: [full name]
  next_due_date: [YYYY-MM-DD]
  specialist_last_touched: [specialist name]
  notes:
    - "[YYYY-MM-DD HH:MM] [who]: [note]"
```
