# Example Flow: New Buyer Lead
## AgencyOS — End-to-End Walkthrough

This example traces Marcus Webb, a relocating buyer, from first inquiry through property research and communication. It shows how case state builds across specialists and how handoffs carry the intelligence forward.

---

## Step 1: Lead Arrives

**Channel:** Zillow inquiry, 9:07 AM
**Initial message:** "Looking for a 4/3 in Barton Hills or Zilker, budget around $900k. Want to move in 60 days."

**Orchestrator action:**
- Initializes case: `260512-MW-001`
- Stage: `lead`, Type: `buyer`
- Routes to 01_lead_qualifier — Priority: Urgent

---

## Step 2: Lead Qualification (01_lead_qualifier)

**Qualifier sends:**
> "Hi Marcus, thanks for reaching out! I'm with Diana's team and saw your inquiry. Two quick questions: Are you currently pre-approved with a lender? And is the 60-day timeline tied to something specific?"

**Marcus responds:** "Yes, pre-approved through First Texas Bank, $950k max. Relocating from Chicago for a job starting July 1."

**Case state updated:**
```yaml
lead_profile:
  urgency: hot
  budget_max: 950000
  financing_status: pre_approved
  financing_lender: First Texas Bank
  timeline: immediate
  timeline_driver: job_start_july_1

emotional_profile:
  client_type: relocating_family
  anxiety_level: medium
  responsiveness: fast
  communication_preference: concise
  communication_notes: "Hard July 1 deadline. Out-of-state — limited ability to tour in person. Will need video tour support."

risk_state:
  risk_flags:
    - relocation_timeline_hard_deadline
    - limited_in_person_touring_capacity
```

**Qualifier routes to:** 03_client_communication — Urgent
*"Draft first follow-up for Marcus. Relocating family type. Acknowledge July 1 deadline. Explain how we support remote buyers. Offer video call."*

---

## Step 3: First Communication (03_client_communication)

**Communication specialist reads:** `emotional_profile.client_type = relocating_family`

**Drafted communication (for agent review):**
> Subject: Great to connect — and I've already started thinking about your July 1 deadline
>
> Hi Marcus,
>
> Thanks for reaching out. A job move from Chicago with a July 1 start date — that's a real timeline, and I want to make sure we're working backward from it effectively.
>
> A few things I'd want to go over with you: which specific neighborhoods within Barton Hills and Zilker feel right for your family, what the must-haves are beyond the 4/3, and how we can make the remote touring process as smooth as possible since you're not here yet.
>
> Would a 20-minute video call this week work? I can pull what's currently on and off-market and walk you through the options so you're not flying blind when you visit.
>
> [Agent name]

**Case state updated:**
```yaml
communication_log:
  last_touch_date: 2026-05-12
  last_touch_type: email
  last_touch_summary: "First follow-up sent. Offered video call. Timeline acknowledged."
  next_touch_due: 2026-05-14
  communication_health: green
```

---

## Step 4: Video Call → Property Identified

**Two days later:** Marcus and Elena Webb join a video call. They identify 2847 Barton Springs Road as a strong candidate. Showing scheduled for Thursday, May 16 (Marcus flying in for the day).

**Agent submits research request via orchestrator.**

**Orchestrator routes to:** 02_property_research — Priority: Urgent (showing in 48h)
*"Full research brief on 2847 Barton Springs Road before Thursday. Clients: relocating family, $950k ceiling, July 1 deadline, two kids, schools and commute are top concerns."*

---

## Step 5: Research Brief (02_property_research)

**Research specialist reads:**
- `emotional_profile.client_type = relocating_family`
- `lead_profile.must_haves` — school quality, commute
- `lead_profile.budget_max = 950000`

**Brief produced** (full brief in examples/new_buyer_lead_flow_research.md)

**Key findings extracted:**
- Barton Hills Elementary: rated 8/10, stable trend ✅
- Downtown commute: 12-20 min ✅
- DOM: 22 days, price reduction from $925k → $895k (seller motivation signal)
- Hidden risk: roof likely near end of life, $15-25k
- Negotiation leverage: opening offer $880k is defensible

**Case state updated:**
```yaml
research_state:
  current_focus_address: 2847 Barton Springs Road
  competition_level: medium
  pricing_leverage: buyer
  resale_risk: low
  hidden_risks:
    - roof_near_end_of_life
  negotiation_leverage: "Price reduction after 22 DOM. Defensible offer at $880k."
  recommended_strategy: "Walk neighborhood first. Address roof as negotiating point, not deal-breaker."
```

**Research routes to:** 03_client_communication
*"Translate research brief into pre-showing communication for Marcus and Elena. Relocating family type. Highlight schools and commute. Mention roof naturally as context, not alarm."*

---

## Step 6: Pre-Showing Communication (03_client_communication)

**Drafted communication:**
> Subject: What to expect Thursday — and what I found on 2847 Barton Springs
>
> Hi Marcus and Elena,
>
> Really looking forward to Thursday. A few things to know about this property going in:
>
> The good stuff: Barton Hills Elementary is rated well and has been consistently strong — that checks the box you cared about most. The downtown commute is genuinely 12-15 minutes most of the time (I did it myself last week). And the neighborhood has a real community feel — families who've been there for years, walkable to Barton Springs Pool.
>
> The one thing I want you to walk the front of the property Thursday afternoon around 4pm: it fronts on Barton Springs Road, which is a main arterial. The noise is real during peak hours. I'd rather you experience it firsthand than discover it post-offer.
>
> The seller reduced the price from $925k to $895k about three weeks ago. That's a data point worth noting for our offer strategy conversation.
>
> See you Thursday. Text me when you're on your way.
>
> [Agent name]

---

## What the System Preserved

By the time the agent walks into Thursday's showing, the case state contains:
- A complete lead profile (urgency, financing, timeline, constraints)
- An emotional profile (client type, communication preference, anxiety level)
- A property research brief with negotiation leverage pre-calculated
- A full communication log showing two prior touches and client responsiveness
- A risk flag (relocation timeline) that is visible in the daily brief

No reconstruction. No "what do I know about these clients again?" The agent walks in prepared.
