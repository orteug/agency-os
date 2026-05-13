# Handoff: Daily Deal Desk
## 05_daily_deal_desk

---

## What I Receive

**From:** 00_orchestrator (automatic morning trigger) or any specialist (case update for next brief)

**For daily brief generation:**
No handoff required. I read all active case states directly. The orchestrator triggers me at the start of each business day.

**For individual case updates (from other specialists):**
Specialists may flag a case for inclusion in the next brief with a specific item:

```yaml
task:
  type: generate_daily_brief
  description: "Include [specific risk or update] in next morning brief"

critical_context:
  - "What changed: [specific update]"
  - "Risk level: [updated overall_risk]"
  - "What action is needed: [specific item for brief]"
```

---

## What I Produce

**Standard output:**
The daily brief — formatted per the template in rules.md.

**Immediate escalation output (when critical conditions exist):**
Before waiting for the brief, I route critical items directly:
- Critical deal risk → Orchestrator → Diana
- 48h+ stale on under-contract client → Assigned agent
- Deadline in <4 hours → Orchestrator → Assigned agent + Diana

---

## Handoff to Other Specialists

The daily brief may trigger handoffs to other specialists:

**To 03_client_communication:**
When a stale lead or communication health flag requires outreach:
```yaml
task:
  type: draft_communication
  priority: normal
  description: "Re-engagement touch for [client] — last contact [date], [N] days"
  critical_context:
    - "Reason for outreach: [what triggered the stale flag]"
    - "Client type: [from case state]"
    - "What to accomplish: [reconnect, schedule showing, check-in on status]"
```

**To 04_transaction_coordinator:**
When a deadline flag in the brief requires TC review:
```yaml
task:
  type: coordinate_transaction
  priority: urgent
  description: "Deadline flag from daily brief: [specific deadline]"
```

**To 00_orchestrator (Diana escalation):**
When a brief item requires Diana's decision:
```yaml
task:
  type: escalate_to_diana
  priority: urgent
  description: "[Three-sentence format: situation, attempted, decision needed]"
```

---

## The Brief Is Not the End

The daily brief creates action items. Those action items route to specialists or agents. The brief closes the overnight gap between case state and operational awareness. It does not replace the specialists — it coordinates them.
