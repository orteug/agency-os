# Team Roles & Human Ownership
## AgencyOS — Who Owns What

This document defines the human ownership layer of AgencyOS. Every case has an assigned human owner. Every escalation has a clear destination. The system handles information; humans make decisions.

---

## The Team

**Diana** — Owner, Lead Agent
- Final authority on all pricing strategy and negotiation position
- Handles all legally ambiguous situations
- Makes all decisions involving commission exceptions
- Personal point of contact for VIP referrals and high-touch sellers
- Receives all critical escalations

**Senior Agent A** — [Name]
- Active buyer management
- Primary on buyer-side offer strategy
- Handles own lead qualification follow-up with system support

**Senior Agent B** — [Name]
- Active seller management and listing coordination
- Primary on seller-side pricing conversations (with Diana approval)
- Handles own listing marketing and showing coordination

**Junior Agent C (Ramping)** — [Name]
- Lead qualification follow-up (with system guidance)
- Showing coordination and scheduling
- Document tracking and transaction support
- Property research requests
- All significant client decisions escalate to Diana or senior agent

**06 BD Coordinator**
Owned by: Diana (primary) and assigned agent
Responsibilities: Long-pipeline relationship management, BD touch scheduling,
graduation signal detection, referral source cultivation
Escalation path: Orchestrator (graduation reclassification), Client Communication (touch due)

---

## Decision Ownership Matrix

| Decision Type | Owner | Escalate To |
|---------------|-------|-------------|
| Pricing strategy (list price or offer price) | Diana | — |
| Negotiation position | Diana (with agent recommendation) | — |
| Commission exception | Diana | — |
| Legal review request | Diana | External counsel |
| Contract interpretation | Diana | External counsel |
| New lead assignment | Diana or available senior agent | — |
| Showing scheduling | Assigned agent | — |
| Communication drafts | Assigned agent (reviews specialist draft) | — |
| Document collection | Assigned agent + TC specialist | Diana if blocked |
| Financing escalation | Assigned agent first | Diana if unresolved 24h |
| Inspection amendment strategy | Diana + assigned agent | — |
| Deal termination decision | Diana | — |
| Referral relationship management | Diana | — |

---

## Escalation Paths

### Level 1: Assigned Agent
Routine operational decisions. Agent handles with system support.
- Follow-up scheduling
- Showing coordination
- Communication drafts (review and send)
- Document collection (standard items)

### Level 2: Senior Agent
When junior agent needs guidance.
- Offer strategy context
- Client management approach
- Non-standard situations within normal transaction scope

### Level 3: Diana
Strategic and sensitive decisions. All escalations that cannot be resolved at Level 1 or 2 within 24 hours, plus:
- All pricing decisions
- All legally ambiguous situations
- All high-risk client situations
- All VIP referral relationships
- Any situation the system flags as `diana_escalation_required: yes`

### External: Counsel
Legal interpretation, contract disputes, undisclosed disclosure situations. Diana routes to counsel. Agents do not engage counsel directly.

---

## New Agent Onboarding

When a fifth agent joins:
1. Diana or senior agent assigns them as owner on a set of active or incoming cases
2. All their cases are set to `diana_escalation_required: yes` for the first 30 days as a monitoring layer
3. The daily brief includes a "New Agent Queue" section during the ramp period
4. See START_HERE.md for the full onboarding flow

---

## The Rule on Human Ownership

**Every case has a named human owner at all times.**

The system can route, draft, research, and track. Only humans can decide, commit, and build the relationships that close deals. The human owner is the person responsible for the client's experience from first contact through closing.

When ownership changes (agent vacation, reassignment, team departure), the orchestrator must update `case_state.owner` immediately. There is no such thing as an unowned active case.
