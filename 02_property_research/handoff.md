# Handoff: Property Research Specialist
## 02_property_research

---

## What I Receive

**From:** 00_orchestrator (routed from agent request or 01_lead_qualifier)

**Required in every incoming handoff:**
- Property address or neighborhood being researched
- Case ID linking to the active case state
- Client type and emotional profile (must be set in case state before research begins)
- Budget ceiling (from `lead_profile.budget_max` or `lead_profile.desired_list_price`)
- Purpose of research (pre-showing brief, CMA, neighborhood analysis, offer preparation)

**If case state is missing emotional profile:** I return the handoff to the orchestrator requesting that 01_lead_qualifier completes qualification first. Research without client context produces generic output.

**If deadline is present:** I prioritize accordingly. Pre-showing briefs needed within 24 hours are urgent.

---

## What I Produce

**Standard output:**
1. A complete research brief following the Research Brief Doctrine structure
2. Updated `research_state` fields in the case state
3. Risk flags populated in `risk_state` if research surfaces concerns

**Exception output (when flagging to orchestrator first):**
1. A flag with specific concern (litigation, structural, significant pricing gap)
2. The partial research completed to date
3. A recommendation for how to proceed

---

## Output Quality Checklist

Before routing, verify:

- [ ] Brief follows all 9 sections of the Research Brief Doctrine
- [ ] Section 1 (Bottom Line Up Front) is written — not skipped
- [ ] Section 5 (Hidden Risks) is populated — even if risk is low, document why
- [ ] Section 7 (Negotiation Leverage) makes a specific recommendation
- [ ] Section 8 (Emotional Fit) references the specific client type from case state
- [ ] Section 9 (Recommended Approach) is a single actionable sentence
- [ ] `research_state` fields updated in case state
- [ ] `risk_state.risk_flags` updated if applicable
- [ ] `last_updated` and `last_updated_by` set in case state

---

## Handoff to 03_client_communication — Template

When research is complete and communication is the next step:

```yaml
handoff:
  from_specialist: 02_property_research
  to_specialist: 03_client_communication
  trigger: research_complete

  task:
    type: draft_communication
    priority: [based on showing/timeline urgency]
    description: "Draft [pre-showing summary | post-showing follow-up | pricing conversation prep] for [client name]"

  critical_context:
    - "Key finding: [the most important thing from research — the agent must know this]"
    - "Client type: [from emotional_profile] — this affects how findings should be framed"
    - "Risk flag (if any): [specific risk the communication must address or avoid]"

  expected_output:
    deliverable: "Drafted communication that translates research findings into client-appropriate language"
    success_criteria: "Agent can send this without needing to explain the research"

  escalate_if:
    - "Research surfaced significant risk factors — agent must review before any client communication"
    - "Pricing expectation gap is large — Diana must be involved before pricing is discussed with client"
```

---

## Back-Handoff Protocol

I return work to the orchestrator when:

1. **Emotional profile is missing.** I need to know who I'm researching for.
   - Request: Lead qualifier to complete qualification before research proceeds

2. **Significant red flag discovered pre-brief.** Litigation, structural, or undisclosed disclosure risk.
   - Notify orchestrator immediately. Do not complete brief until agent or Diana confirms direction.

3. **Request is for legal analysis, not research.** Contract interpretation, zoning questions, easement disputes.
   - Route to: Diana or external counsel. This is outside my scope.
