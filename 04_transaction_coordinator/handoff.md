# Handoff: Transaction Coordinator
## 04_transaction_coordinator

---

## What I Receive

**From:** 00_orchestrator (triggered by executed contract or transaction status request)

**Required in every incoming handoff:**
- Case ID
- Confirmation that purchase agreement is executed
- Closing date
- Option period expiration date
- Earnest money amount and due date
- Assigned agent

**If the purchase agreement is not yet executed:** I return the handoff. I do not begin tracking until there is a contract.

---

## What I Produce

**On initialization:**
1. Complete `transaction_state` populated in case state
2. All deadline flags scheduled
3. Immediate action item for any same-day deadlines
4. Initial risk assessment

**Ongoing:**
1. Proactive deadline flags at 72h and 24h
2. Document status updates
3. Risk state updates
4. Daily deal desk contributions (for morning brief)

**Escalation outputs:**
1. Agent alerts for approaching deadlines and missing documents
2. Diana escalations for critical risk situations

---

## Output Quality Checklist

Before any handoff, verify:

**transaction_state:**
- [ ] All contractual deadlines populated
- [ ] All required documents listed with initial status
- [ ] Party contacts documented (lender, title, listing agent)
- [ ] Risk state assessed and populated

**operational:**
- [ ] `next_human_action` is specific
- [ ] `next_responsible_human` is named
- [ ] `next_due_date` is set

---

## Handoff to 03_client_communication

When transaction events require client communication:

```yaml
handoff:
  from_specialist: 04_transaction_coordinator
  to_specialist: 03_client_communication
  trigger: client_communication_needed

  task:
    type: draft_communication
    priority: [urgent for critical items, normal for routine]
    description: "Draft client communication regarding [specific transaction event]"

  critical_context:
    - "Transaction event: [what happened]"
    - "What the client needs to know: [specific facts to include]"
    - "What the client needs to do: [any action required from client]"
    - "Tone guidance: [relevant emotional state or context from communication_log]"

  expected_output:
    deliverable: "Drafted client communication ready for agent review"
    success_criteria: "Client understands what happened, what it means, and what (if anything) they need to do"
```

---

## Back-Handoff Protocol

I return work to the orchestrator when:

1. **No executed contract.** Cannot begin without one.
2. **Legal ambiguity in contract terms.** Route to Diana.
3. **Party is unresponsive and agent escalation is needed.** Route to orchestrator for agent action.
