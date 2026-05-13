# Case State Schema
## AgencyOS — Canonical Operational State Object

> **Naming note:** This document is referred to externally as **"The Agency File"** — the single source of truth for every active client and deal. "Case State Schema" is the internal architectural name used throughout this repo and the specialist files.

> **This is the source of truth.**
>
> Every specialist reads from this schema. Every specialist writes to it.
> No specialist invents state that isn't here. No specialist ignores state that is.
>
> When state conflicts with what a specialist was told verbally, the schema wins.
> Update the schema. Don't work around it.

---

## Core Principle

**Handoffs move work. Case state preserves reality.**

The handoff protocol tells a specialist what to do next.
The case state tells every specialist what is actually true about this client and deal.

These are different things. Both are required.

---

## The Canonical Case State Object

```yaml
# ============================================================
# CASE IDENTITY
# ============================================================
case_id:                    # Format: YYMMDD-INITIALS-NNN (e.g., 260512-JF-001)
case_type:                  # buyer | seller | dual_representation
stage:                      # lead | qualifying | active_search | under_contract | closed | dead | on_hold
owner:                      # Assigned agent full name
created_date:               # YYYY-MM-DD
last_updated:               # YYYY-MM-DD HH:MM
last_updated_by:            # specialist name or agent name


# ============================================================
# CLIENT
# ============================================================
client:
  name:                     # Full name(s)
  contact_email:
  contact_phone:
  preferred_contact:        # email | text | call
  type:                     # buyer | seller
  referral_source:          # how they found us


# ============================================================
# LEAD PROFILE
# (Set by 01_lead_qualifier. Read by all specialists.)
# ============================================================
lead_profile:
  urgency:                  # hot | warm | cold
                            # hot = needs to move in <30 days or highly motivated
                            # warm = 30-90 day timeline, engaged
                            # cold = 90+ days, browsing, or unclear

  budget_min:               # dollar amount
  budget_max:               # dollar amount
  financing_status:         # pre_approved | in_process | cash | needs_lender | unknown
  financing_lender:         # lender name if known
  down_payment_ready:       # yes | no | unknown

  timeline:                 # immediate | 30_days | 60_days | 90_days | 6_months | unclear
  timeline_driver:          # what is creating the timeline (lease end, job start, etc.)

  preferred_areas:          # list of neighborhoods/zip codes
    - 
  must_haves:               # non-negotiable requirements
    -
  nice_to_haves:            # preferred but flexible
    -
  deal_breakers:            # anything that ends the search
    -
  constraints:              # school district, commute, HOA, etc.
    -

  # For sellers
  property_address:         # if seller
  desired_list_price:       # seller's expectation
  motivation:               # why selling (job, downsize, upgrade, divorce, estate, etc.)
  timing_flexibility:       # must_close_by | flexible | undecided


# ============================================================
# EMOTIONAL PROFILE
# (Set by 01_lead_qualifier. Updated by 03_client_communication.)
# This field drives communication tone and escalation decisions.
# ============================================================
emotional_profile:
  client_type:              # first_time_buyer | repeat_buyer | investor | relocating_family |
                            # luxury_buyer | emotionally_attached_seller | luxury_seller |
                            # aggressive_negotiator | analytical_buyer | grieving_seller

  confidence:               # high | medium | low
                            # How confident are they in the process and their decisions?

  anxiety_level:            # high | medium | low
                            # How much reassurance do they need?

  responsiveness:           # fast (<2h) | normal (same day) | slow (1-2 days) | inconsistent
  decision_style:           # fast_mover | deliberate | needs_consensus | impulsive
  reassurance_required:     # high | medium | low

  communication_preference: # detailed | concise | visual | verbal | mixed
  communication_notes:      # any specific notes on how to communicate with this client

  red_flags:                # list any concerning behavioral patterns
    - 


# ============================================================
# PROPERTY RESEARCH STATE
# (Set by 02_property_research. Read by 03_client_communication.)
# ============================================================
research_state:
  properties_reviewed:      # list of addresses reviewed
    -
  current_focus_address:    # primary property under active consideration
  current_focus_mls:        # MLS number

  market_conditions:        # buyers_market | balanced | sellers_market
  competition_level:        # high | medium | low (buyer competition for target properties)
  pricing_leverage:         # buyer | neutral | seller
  days_on_market_context:   # interpretation of DOM for this property/area

  resale_risk:              # high | medium | low
  resale_notes:             # what specifically to watch

  hidden_risks:             # list of identified risks
    -

  negotiation_leverage:     # what gives the buyer/our seller an advantage
  recommended_strategy:     # brief statement of recommended approach

  research_notes:           # anything important not captured above


# ============================================================
# COMMUNICATION LOG
# (Updated by 03_client_communication after every touch.)
# ============================================================
communication_log:
  last_touch_date:          # YYYY-MM-DD
  last_touch_time:          # HH:MM
  last_touch_type:          # email | text | call | showing | in_person | voicemail
  last_touch_summary:       # one sentence: what was communicated, what was the client's response
  last_touch_by:            # agent name

  next_touch_due:           # YYYY-MM-DD
  next_touch_type:          # what kind of follow-up is planned
  next_touch_notes:         # what to address in the next touch

  communication_health:     # green | yellow | red
                            # green = within cadence, client responsive
                            # yellow = approaching stale or client slow to respond
                            # red = stale (48h+ no contact on active deal) or client unresponsive

  stale_threshold_hours:    # 48 for active leads, 72 for warm leads, 168 for cold leads
  sentiment:                # positive | neutral | concerned | frustrated | disengaged


# ============================================================
# TRANSACTION STATE
# (Set and maintained by 04_transaction_coordinator.)
# Only populated when stage = under_contract.
# ============================================================
transaction_state:
  contract_date:            # YYYY-MM-DD
  closing_date:             # YYYY-MM-DD
  closing_time:             # HH:MM if known

  # Key deadlines (all YYYY-MM-DD)
  option_period_expires:
  earnest_money_due:
  inspection_deadline:
  inspection_amendment_deadline:
  financing_contingency_deadline:
  appraisal_deadline:
  title_commitment_deadline:
  survey_deadline:
  final_walkthrough_date:

  # Document status
  documents:
    purchase_agreement:           # complete | pending | missing
    option_fee_receipt:           # complete | pending | missing
    earnest_money_receipt:        # complete | pending | missing
    seller_disclosure:            # complete | pending | missing | na
    inspection_report:            # complete | pending | missing | na
    inspection_amendment:         # complete | pending | missing | na
    appraisal_report:             # complete | pending | missing | na
    financing_commitment:         # complete | pending | missing | na
    title_commitment:             # complete | pending | missing
    survey:                       # complete | pending | missing | na
    hoa_documents:                # complete | pending | missing | na
    final_settlement_statement:   # complete | pending | missing
    closing_disclosure:           # complete | pending | missing

  missing_documents:        # list — populated automatically by 04
    -
  pending_actions:          # list of open items with owners
    -
  completed_actions:        # list of completed items
    -

  # Financing details
  financing_status:         # pending | approved | conditional_approval | denied | cash | na
  lender_name:
  loan_officer:
  loan_officer_phone:
  appraisal_status:         # not_ordered | ordered | complete | below_value | na
  appraisal_value:          # dollar amount if complete

  # Other parties
  title_company:
  title_officer:
  seller_agent:
  seller_agent_phone:


# ============================================================
# RISK STATE
# (Updated by all specialists. Primary maintainer: 04 and 05.)
# ============================================================
risk_state:
  overall_risk:             # critical | high | medium | low | none

  risk_flags:               # list of active risk flags
    -
                            # Examples:
                            # financing_unclear
                            # inspection_amendment_unresolved
                            # communication_stale_48h
                            # deadline_approaching_24h
                            # client_emotionally_distressed
                            # legal_ambiguity_present
                            # conflicting_client_instructions
                            # appraisal_gap_risk
                            # hoa_issues_unresolved

  deadline_risk:            # none | approaching (72h) | critical (24h) | overdue
  financing_risk:           # none | low | medium | high | critical
  communication_risk:       # none | low | medium | high
  deal_fall_risk:           # none | low | medium | high

  stuck_reason:             # if deal is stalled, what is blocking it
  stuck_since:              # YYYY-MM-DD when it got stuck


# ============================================================
# ESCALATION
# (Managed by orchestrator and 05_daily_deal_desk.)
# ============================================================
escalation:
  diana_escalation_required:    # yes | no
  escalation_reason:            # why Diana needs to be involved
  escalation_urgency:           # immediate | today | this_week
  escalation_date:              # when it was flagged
  escalation_resolved:          # yes | no


# ============================================================
# OPERATIONAL
# (Updated after every specialist action.)
# ============================================================
operational:
  next_human_action:            # specific action required from a human
  next_responsible_human:       # full name of who owns the next action
  next_due_date:                # YYYY-MM-DD
  next_due_time:                # HH:MM if time-sensitive
  specialist_last_touched:      # which specialist last worked this case
  
  notes:                        # running operational notes (newest first)
    - "[YYYY-MM-DD HH:MM] [who]: [note]"
```

---

## Rules for Using Case State

### Every specialist must:
1. **Read the full case state before starting any task.** Do not work from partial context.
2. **Update the relevant fields after completing a task.** Case state must reflect current reality.
3. **Set `last_updated` and `last_updated_by` on every write.** Stale state is dangerous.
4. **Populate `operational.next_human_action` and `operational.next_responsible_human` before completing any task.** Every handoff ends with a clear owner.

### No specialist may:
1. **Invent state that isn't in the schema.** Add to the schema instead.
2. **Leave `risk_state.overall_risk` blank.** If you touched the case, you assessed the risk.
3. **Remove fields without setting them to a documented value.** Use `unknown` or `na`, never delete.
4. **Set `diana_escalation_required: no` without verifying.** When in doubt, escalate.

### State update order:
1. Read current state
2. Complete the task
3. Update relevant state fields
4. Update `risk_state` to reflect current conditions
5. Set `operational.next_human_action` and `operational.next_responsible_human`
6. Update `last_updated` and `last_updated_by`
7. Pass the updated state in the handoff

---

## Stage Definitions

| Stage | Meaning |
|-------|---------|
| `lead` | Initial contact made, not yet qualified |
| `qualifying` | In active qualification conversation |
| `active_search` | Qualified buyer searching, or qualified seller preparing to list |
| `under_contract` | Purchase agreement executed |
| `closed` | Transaction complete |
| `dead` | Lead or deal ended (document why in notes) |
| `on_hold` | Temporarily paused with a known resume date |

---

## Risk Flag Reference

| Flag | Meaning | Default Owner |
|------|---------|--------------|
| `financing_unclear` | Financing status unknown or unstable | Lead qualifier or TC |
| `communication_stale_48h` | No contact with client in 48+ hours | Assigned agent |
| `deadline_approaching_24h` | A hard deadline is within 24 hours | TC |
| `deadline_overdue` | A deadline has passed without resolution | TC + Diana |
| `inspection_amendment_unresolved` | Amendment not returned within 24h of inspection | TC |
| `client_emotionally_distressed` | Client showing anxiety, frustration, or confusion | Assigned agent |
| `appraisal_gap_risk` | Appraisal may come in below contract price | Diana |
| `legal_ambiguity_present` | Something in the deal requires legal review | Diana |
| `missing_document_critical` | A document required for next deadline is missing | TC |
| `conflicting_client_instructions` | Client has given contradictory direction | Diana |
| `deal_fall_risk_high` | Deal shows multiple signs of being at risk | Diana |
