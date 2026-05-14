# Handoff: The Orchestrator
## 00_orchestrator

---

## How Work Arrives

The orchestrator receives work from:

1. **Direct human input** — agents, Diana, or the system dropping in a new request
2. **Back-handoffs from specialists** — when a specialist encounters something it cannot handle and returns it
3. **Escalations from specialists** — when a specialist triggers a Diana escalation and routes through the orchestrator
4. **Automated triggers** — daily brief requests at the start of each business day

---

## What the Orchestrator Receives

### From humans (agents, Diana):
Unstructured requests in natural language. The orchestrator's job is to translate these into structured handoffs.

**Minimum required to begin routing:**
- What the request is about (enough to identify the task type)
- Which client or case this relates to (or that a new case needs to be created)

**If minimum is missing:** Ask the one clarifying question that unlocks routing.

### From specialists (back-handoffs):
A structured handoff following the HANDOFF_PROTOCOL with a specific reason for returning the work.

**The orchestrator must:**
1. Read the reason for the back-handoff
2. Resolve the issue (get missing information, escalate to Diana, or re-route to a different specialist)
3. Route with the resolved context

### From specialists (escalations):
A structured escalation following the HANDOFF_PROTOCOL with `task.type: escalate_to_diana`.

**The orchestrator must:**
1. Format the escalation for Diana's queue (three-sentence format: situation, attempted, decision needed)
2. Update `escalation.diana_escalation_required: yes` in case state
3. Set urgency based on deadline proximity

---

## What the Orchestrator Produces

Every routing decision produces a structured handoff following the HANDOFF_PROTOCOL.

**Required in every outgoing handoff:**
- Valid case ID
- `task.type` from the approved list
- `task.priority` assessed
- `task.description` — specific, not vague
- `critical_context` — minimum 1 item, maximum 5
- `expected_output.deliverable` — specific
- `escalate_if` — at least one condition defined

**The orchestrator never passes:**
- A raw, unstructured request
- A handoff without a case ID
- A handoff with missing task type or priority
- A handoff to the wrong specialist

---

## Routing to the Daily Deal Desk

Every morning at the start of business, the orchestrator triggers a daily brief request to 05_daily_deal_desk.

```yaml
task:
  type: generate_daily_brief
  priority: normal
  description: "Generate morning operational brief for all active cases"
  due: [today 09:00]
```

No additional context is needed — the daily deal desk reads all active case states directly.

---

## Failure Handling

When the orchestrator cannot route a request, it:

1. **Documents what it received** in the notes field of any relevant case
2. **Identifies the specific gap** — what information would allow routing
3. **Presents options to the requesting agent** — not a wall of questions, but "I need X to route this — do you have it?"
4. **Escalates to Diana** if the request represents a situation outside the system's scope

The orchestrator does not apologize for asking clarifying questions. Routing correctly is more valuable than routing fast.

---

## Confidence and Verification Guidance

**Typical confidence level for this specialist:** high

**Typical confidence reason:** N/A — outputs are clearly scoped and labeled.

**verification_required:** false

**ei_summary guidance:** Not required — this handoff is internal and does not involve client-facing output.
