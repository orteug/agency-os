# Examples: Business Development Coordinator
## 06_bd_coordinator

---

## Example 1 — Quarterly Touch: Past Client

**Situation:**
Marcus and Elena Webb closed on their Barton Hills home 11 months ago. They are in `bd_state.status: past_client`. Touch cadence is quarterly. Last touch was 90 days ago. No new signals detected.

**Money psychology type:** Equity move-up. This was their second purchase. They built equity on their first home and made a deliberate financial move. They understand the asset class and trust Diana's judgment.

**BD Coordinator output:**

```
TOUCH RECOMMENDATION — Marcus & Elena Webb

Contact type: Past client
Status: bd_state.past_client
Last touch: 90 days ago
Touch due: Today

Recommended approach:
Light-touch check-in. Not sales. Relationship maintenance.

What to reference:
- The Barton Hills home — ask how they are settling in
- The neighborhood — anything notable happening locally
- One sentence noting market conditions without pressure
- Close with: "If you know anyone who is thinking about buying or selling, 
  I'd love to help them the way I helped you."

Tone calibration:
Equity move-up / reasonable money psychology.
Warm, peer-level, not transactional.
They are successful professionals who trust Diana.
Write to that level.

Confidence level: high
EI summary for 03_client_communication:
"Equity move-up. Closed 11 months ago, Barton Hills. 
Trust Diana's judgment. Peer-level tone. Warm but efficient.
Goal: maintain relationship and open referral channel."

Route to: 03_client_communication for draft
```

---

## Example 2 — Graduation Signal Detected

**Situation:**
The Chen family has been in `bd_state.active_bd` for 8 months. They said in October "we're thinking about upgrading but probably not until next year." Their money psychology type is Relocating Family — they moved from Chicago two years ago and love their current neighborhood but have outgrown the house with their third child arriving.

Today, during a check-in call, the agent noted in their Agency File: "Chen mentioned they toured the Mueller neighborhood last weekend on their own. Asked about school enrollment deadlines."

**BD Coordinator output:**

```
GRADUATION SIGNAL DETECTED — Chen Family

Previous status: bd_state.active_bd (8 months)
Signal type: IMPLICIT → approaching EXPLICIT
Signals observed:
  1. Independently toured a specific target neighborhood
  2. Asked about school enrollment deadlines (calendar-driven urgency signal)
  3. Third child arriving (life event = space constraint becoming acute)

Assessment:
These signals indicate the Chen family is transitioning from "someday" to 
"soon." The school enrollment question suggests they have a target academic 
year in mind. Independently touring Mueller without prompting indicates 
they are already in shopping mode.

RECOMMEND: Reclassify to active pipeline.

Graduation threshold met:
"States a target timeline within 90 days OR takes independent action 
toward a specific property or neighborhood."
Independent Mueller tour + enrollment deadline question = threshold met.

Handoff to orchestrator:
Route Chen family to 01_lead_qualifier for fresh active qualification.
Note: 8-month BD relationship. Diana has strong rapport. 
Fresh qualification should feel like a natural continuation, not a 
restart. The lead qualifier should reference the prior relationship 
depth in its communication brief.

Confidence level: high
verification_required: false
```

---

## Example 3 — Referral Source Cultivation

**Situation:**
Jordan Reeves is an attorney who referred two clients to Diana in the last 6 months. Both closed. Jordan is in `bd_state.status: referral_source`. Last touch was 35 days ago — a brief thank-you email after the second closing.

The BD Coordinator flags: referral source, two closings in 6 months, 35 days since last touch. Approaching the acknowledgment threshold.

**BD Coordinator output:**

```
TOUCH RECOMMENDATION — Jordan Reeves (Referral Source)

Contact type: Referral source
Referrals sent: 2 in 6 months (both closed)
Last touch: 35 days ago
Touch due: Now (approaching 30-day threshold)

Recommended approach:
Personal appreciation touch. Not generic. Reference both referrals.

What to include:
- Acknowledge the second closing specifically — Jordan should know 
  the client they referred closed successfully
- Express genuine appreciation — two referrals from one relationship 
  in 6 months is significant
- Look for reciprocity opportunity: does Diana know anyone who needs 
  an attorney? If yes, offer the referral back.
- Keep brief. Jordan is a professional. Respect their time.

ESCALATE TO DIANA:
Jordan Reeves has sent two referrals in 6 months. This is a 
high-value relationship. Diana should send this touch personally — 
not through an agent. A handwritten note or a personal call 
would be appropriate. This contact warrants Diana's direct attention.

Tone calibration:
Peer-level. Professional. Genuine appreciation without over-effusion.
Referral sources who feel over-thanked become uncomfortable.
Acknowledge, appreciate, reciprocate. That's the formula.

Confidence level: high
```

---

## Example 4 — Investor, Opportunity Signal

**Situation:**
Patricia Chen (no relation to the Chen family above) is an investor contact in `bd_state.sphere`. She owns three investment properties in Austin and has mentioned to Diana that she watches Mueller for the right deal. She has been in sphere for 14 months. Cadence is quarterly.

Today, the property research specialist produced a brief on a Mueller property showing: price reduction of 8.5% from original list, now at $615,000, sitting 68 days on market. Cap rate estimate of 5.2% at current list price — within the range Patricia mentioned when they last spoke.

**BD Coordinator output:**

```
OPPORTUNITY SIGNAL — Patricia Chen

Contact type: Sphere / Investor
Status: bd_state.sphere (14 months)
Signal: External market trigger — property matching stated criteria

Property: [address], Mueller
DOM: 68 days | Price reduction: 8.5% | Current: $615K
Cap rate estimate: 5.2% (Patricia stated minimum 5.0% in prior conversation)

RECOMMEND: Immediate opportunity touch, not scheduled cadence.

Rationale:
Investors do not respond to relationship warmth. They respond to 
deals that fit their criteria. A property matching Patricia's stated 
investment thesis is a high-value signal. Waiting for the next 
scheduled quarterly touch means losing the window on this property.

Communication approach:
Data-forward. Brief. One opportunity, clearly stated.
Do not build a narrative. Patricia wants the numbers.
Include: address, current price, DOM, cap rate estimate, one-sentence 
assessment of why it fits her criteria.
Close with: "Happy to pull more detail or arrange a showing if the 
numbers look interesting."

Tone calibration:
Rational money psychology. Investor. Peer-level. Numbers first, 
always. Emotional warmth is secondary to information value.

EI summary for 03_client_communication:
"Investor / rational. 14-month sphere relationship.
Data-forward. No narrative. Numbers, fit assessment, offer to act.
Do not manufacture urgency she hasn't signaled."

Confidence level: medium
Confidence reason: Cap rate estimate is based on list price and 
market context file, not a formal analysis. Patricia should verify 
with her own analysis before acting.
verification_required: true — cap rate claim should be flagged as 
estimate before reaching client communication.
```
