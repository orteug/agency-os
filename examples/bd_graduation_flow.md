# Business Development — Graduation Flow
## examples/bd_graduation_flow.md

**Scenario:** A six-month BD contact graduates to the active pipeline.

This example shows the full lifecycle: initial scoring below active threshold, BD management through six months, graduation signal detection, reclassification, and re-engagement as an active lead.

---

## The Contact

**Name:** David and Christine Lam
**Source:** Referral from past client (Jordan Reeves)
**First contact:** November 2025

**Initial context:**
David is a software engineer. Christine manages a boutique interior design firm. They have a daughter starting middle school in two years. They own a condo in Mueller — bought in 2021 at $490,000, current estimated value $620,000. They want a 4-bedroom house in Hyde Park or Barton Hills when the time is right. "We're thinking 2026 or 2027 — not in a rush."

**Money psychology type:** Equity move-up. They have compounded wealth in their current property and understand the asset. Their primary motivation is lifestyle — they want a home that fits their growing family and Christine's aesthetic. Their primary fear is overpaying in a market they don't fully understand.

**Tail event:** None documented.

---

## November 2025 — Lead Qualifier Assessment

**Input to orchestrator:**
> "New referral from Jordan Reeves. David and Christine Lam. Interested in upgrading from Mueller condo to 4BR in Hyde Park or Barton Hills. Timeline is 2026 or 2027. Jordan said Christine has very specific design standards — she's a designer herself."

**Lead qualifier output:**

```yaml
case_id: 251118-DL-004
case_type: buyer
stage: lead

lead_profile:
  urgency: cold
  timeline: 12-24 months
  budget_range: "$850,000–$1,100,000"
  must_haves:
    - 4 bedrooms
    - Hyde Park or Barton Hills
    - Architectural character (not generic new construction)
  deal_breakers:
    - Generic subdivision aesthetics
    - Traffic noise

emotional_profile:
  client_type: equity_moveup
  money_psychology_type: reasonable
  primary_fear: overpaying
  primary_motivation: lifestyle
  financial_history_notes: "Own Mueller condo since 2021, significant equity. Christine is a designer — aesthetic standards are high. David is analytical but defers to Christine on property feel."
  confidence: high
  time_horizon: medium (12-24 months)
  reassurance_required: low
  decision_style: deliberate

ROUTING DECISION:
Timeline is 12-24 months. No urgency signals. Route to 06_bd_coordinator.
Initiate active_bd status. Referral from VIP source — flag to Diana for personal acknowledgment.

confidence_level: high
ei_summary: "Equity move-up couple. Reasonable money psychology — they are optimizing for lifestyle and architectural quality, not financial metrics. Christine drives aesthetic decisions. David is analytical but supportive. Low anxiety. No urgency. Cultivate with architectural and neighborhood content."
```

**Orchestrator routes to BD Coordinator. Diana sends a personal thank-you to Jordan Reeves.**

---

## November 2025 – April 2026 — BD Management

### Touch cadence: Monthly

**Touch 1 (November 2025) — Introduction + setup**
The BD Coordinator surfaces the initial touch brief. 03_client_communication drafts a welcome message in Diana's voice. Acknowledges the Jordan referral, expresses genuine interest in their search, offers to share anything relevant in Hyde Park and Barton Hills when it comes up. Sets the tone: no pressure, no urgency, Diana is a resource.

**Touch 2 (December 2025) — Neighborhood content**
A Hyde Park property with exceptional architectural character came on and went under contract quickly. BD Coordinator recommends sharing it with the Lams — not as a missed opportunity but as a market signal. "This is the kind of property that moves quickly when it comes on. It gives you a sense of what Hyde Park looks like when it's done right."

Christine responds immediately: "This is exactly what we're looking for. The built-ins on the second floor are incredible."

BD Coordinator flags: increased responsiveness on architectural content. Notes in graduation_signals. Continues current cadence.

**Touch 3 (January 2026) — Market update**
Annual market check-in. Diana shares a brief summary of how the market closed 2025 in their target neighborhoods. No urgency framing. "Good to know as you're thinking through your timeline."

David responds: "Rates came down a bit — does that change the market much in those areas?"

BD Coordinator flags: rate question signals financial engagement, not just casual curiosity. Notes in graduation_signals. Suggests bumping cadence to biweekly. Orchestrator approves.

**Touch 4-6 (February–March 2026) — Biweekly cadence**
Two properties shared that match the Lam criteria. Christine engages deeply on both. One has "the kitchen we've been talking about for years." The Lams pass on acting — "not quite the timing." But the engagement level continues to rise.

BD Coordinator graduation_signals now contains:
- Increased response speed
- Deep engagement on specific properties
- Rate question indicating financial consideration
- "Not quite the timing" — using timing language, not "not right for us"

---

## May 2026 — Graduation

### The signal

May 7, 2026. The agent notes in the Agency File after a brief call:

> "Christine called unprompted. Said she and David have been thinking a lot about the move. Their daughter starts middle school in September 2027 — they want to be in the new house before that. David was promoted in January and their income picture has improved. Christine said: 'We want to start looking seriously. Is there anything good in Hyde Park right now?'"

**BD Coordinator assessment:**

```yaml
GRADUATION SIGNAL DETECTED — David & Christine Lam

Previous status: bd_state.active_bd (6 months)
Signal type: EXPLICIT

Signals observed:
  1. Contact initiated unprompted — Christine called Diana directly
  2. Named a specific timeline: "Before September 2027 school start"
  3. Income change confirmed: David promoted in January
  4. Language shift: "start looking seriously" vs. "thinking about 2026 or 2027"
  5. Asked about current inventory: "Is there anything good right now?"

Graduation threshold met:
"States a target timeline within 90 days OR takes independent action
toward a specific property or neighborhood."
Christine initiated the call and stated a specific timeline.
Threshold met on both conditions.

Assessment:
Six months of relationship building produced a warm, trusted connection.
Christine called Diana — not searched Zillow. That is the result of six
months of consistent, appropriate contact calibrated to their psychology.

RECOMMEND: Immediate reclassification to active pipeline.

Important context for 01_lead_qualifier:
- Fresh qualification needed (income change, clearer timeline)
- Relationship depth: trusted (6 months, multiple meaningful exchanges)
- Christine is the aesthetic decision-maker. David controls financial decisions.
  Both must be aligned before an offer.
- They have been shown 4 properties over 6 months. They know what they want.
  Do not over-qualify — they are not a cold lead.
- The school timeline (September 2027 school start) means they need to close
  by August 2027 at the latest. Working backward from that, offer by June 2027.
  Timeline is real but not compressed.

confidence_level: high
```

---

## May 2026 — Reclassification

**Orchestrator routing:**
The orchestrator receives the graduation handoff. Routes to 01_lead_qualifier with the BD context package.

**Lead qualifier update:**

```yaml
case_id: 251118-DL-004
stage: lead (reclassified from bd)
urgency: warm (elevated from cold)
timeline: 14-16 months (close by August 2027)

Updated financial profile:
  financing: pre_approval_needed (income changed, needs updated qualification)
  budget_range: "$900,000–$1,200,000" (updated from prior estimate based on promotion)

Updated emotional profile:
  urgency: warm
  time_horizon: near (14-16 months — concrete deadline now)
  confidence: high (they know what they want, 6 months of education)

Recommended first active outreach:
Diana calls Christine directly. This is a warm relationship of 6 months.
The first active outreach should feel like a natural continuation, not a
fresh sales call. Acknowledge that Christine reached out, confirm the timeline,
schedule a proper consultation.
```

**First active communication draft (03_client_communication):**

> Hi Christine,
>
> So glad you called. Six months of watching Hyde Park and Barton Hills together — I think we've built a pretty good picture of what you're looking for.
>
> With the September 2027 start date as the anchor, we have a realistic window to be deliberate rather than rushed. That's the right position to be in for the properties you want.
>
> Can we get together for a proper sit-down this week? I want to understand how your thinking has evolved since we last talked, and there's one property that came on last week that I'd love to show you — it has the kitchen Christine mentioned.
>
> Diana

---

## What This Example Demonstrates

**1. The BD Coordinator does not pressure.** Six months of contact, and the Lams were never made to feel urgency they did not feel themselves. The graduation happened on their timeline.

**2. The emotional intelligence framework drove every touch.** Reasonable money psychology + lifestyle motivation = architectural content, neighborhood intelligence, and peer-level conversation. Not rate sheets.

**3. Graduation was detected, not manufactured.** Christine called Diana because the relationship was built. The BD Coordinator recognized the signals and escalated promptly.

**4. Relationship depth carries forward.** The lead qualifier's fresh qualification preserved the 6 months of context. The first active communication sounds like a continuation, not a reset.

**5. Confidence fields flagged uncertainty honestly.** The cap rate estimate in Example 4 from the BD examples was flagged as unverified. The graduation assessment in this flow was high-confidence because the signals were explicit.
