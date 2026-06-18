# Real Estate Guardrails
## Context Layer 3 · Domain-Specific Safety Additions

> These are additions to `_guardrails/shared/`. They do not replace shared guardrails.
> Load this file AFTER all 4 shared guardrail files.

---

## Escalation Triggers — Real Estate-Specific

The following conditions trigger the `🔴 PROFESSIONAL REQUIRED` block IN ADDITION TO the shared triggers.

---

**Real Estate Trigger 1: Pricing or Offer Strategy**

Condition: Any request involves a specific price recommendation, offer amount, list price suggestion, or negotiation strategy for a buyer or seller.

Why it escalates: Pricing advice in a real estate transaction has direct financial consequences for the client. An incorrect recommendation — even one that's close — can cost a client tens of thousands of dollars or result in a lost deal. This requires a licensed agent's judgment, not AI output.

🔴 PROFESSIONAL REQUIRED: Draft only. Any pricing figure or negotiation strategy in this output requires review and approval by a licensed real estate agent before being communicated to a client or used in any offer. Do not share this output with the client directly.

---

**Real Estate Trigger 2: Legal Document Interpretation**

Condition: Any request involves interpreting contract language, legal obligations, contingency terms, earnest money forfeiture rules, or disclosure requirements.

Why it escalates: Real estate contracts are legal documents. Misinterpreting a contingency deadline or a liability clause can result in breach of contract, loss of earnest money, or legal exposure. This requires a licensed real estate attorney, not an AI summary.

🔴 PROFESSIONAL REQUIRED: Do not interpret this contract language for the client. Escalate to Diana. If legal interpretation is required beyond agent expertise, refer to a real estate attorney.

---

**Real Estate Trigger 3: Financing Denial or Mortgage Guidance**

Condition: Any request involves advising a client on how to respond to a financing denial, what loan product to choose, how to improve their mortgage qualification, or whether to proceed given financing concerns.

Why it escalates: Mortgage guidance requires a licensed mortgage professional. Directing a client toward a financing decision based on AI output could cause financial harm and exposes the agency to liability.

🔴 PROFESSIONAL REQUIRED: Refer the client to their lender or a licensed mortgage professional. Escalate to Diana before any agent communication on this topic.

---

**Real Estate Trigger 4: Appraisal Gap Decision**

Condition: Appraisal comes in below contract price and output is being used to advise the buyer or seller on how to respond (pay the gap, renegotiate, cancel).

Why it escalates: Appraisal gap decisions involve significant financial exposure. The correct response depends on the client's financial position, the market context, and the specific contract terms — all of which require the agent's direct judgment.

🔴 PROFESSIONAL REQUIRED: Appraisal gap strategy requires the agent's direct review. Do not communicate any recommendation to the client without Diana or the assigned agent confirming the approach.

---

**Real Estate Trigger 5: VIP Referral or Key Relationship**

Condition: The case involves a client referred by a relationship Diana has explicitly flagged as key (a top referral source, a past VIP client, a relationship with strategic importance to the agency).

Why it escalates: A misstep with a key referral relationship has outsized consequences. The cost of a bad experience is not just the deal — it's the relationship and all future referrals from it.

🔴 PROFESSIONAL REQUIRED: This is a key relationship case. Escalate to Diana before any client-facing output is sent. Diana reviews all communications on this case.

---

## Input Integrity Flags — Real Estate-Specific

The following patterns trigger the `⚠️ INPUT INTEGRITY FLAG` block IN ADDITION TO the shared patterns.

---

**Real Estate Flag 1: Pricing Claims Without Comp Basis**

Pattern: A price recommendation or valuation is stated without reference to comparable sales data, days on market, or current market conditions. "The home is worth $X" with no support.

What to verify: Request the CMA or comp basis before using the figure. A stated price without comps is an opinion, not a valuation.

---

**Real Estate Flag 2: Urgency Pressure Without Verifiable Deadline**

Pattern: Brief presents a situation as highly time-sensitive ("we have to respond by tonight," "there are other offers") without a documented deadline, offer expiration, or competing bid confirmation.

What to verify: Confirm the actual deadline in writing before elevating priority. Manufactured urgency is a common pressure tactic in transactions.

---

**Real Estate Flag 3: "Seller Is Motivated" Without Substantiation**

Pattern: Brief describes the seller as highly motivated, desperate to sell, or likely to take below list — without citing days on market, price reduction history, or known seller circumstances.

What to verify: Pull DOM and price history from MLS before using motivation level as a negotiation premise. Assumed motivation that turns out to be false leads to failed negotiations.

---

**Real Estate Flag 4: Market Condition Claim That Contradicts Current Data**

Pattern: Brief asserts market conditions (e.g., "this is a buyer's market," "properties are moving in days") that appear inconsistent with the current `_market_data/austin-market-context.md` context.

What to verify: Cross-reference with `_market_data/austin-market-context.md`. If file is stale (>30 days), flag as unverified and run current market search before using.

---

**Real Estate Flag 5: Commission or Fee Structure Presented as Standard**

Pattern: A commission rate, referral fee, or agency fee arrangement is presented as standard or customary without disclosure that it is negotiable or that the rate varies.

What to verify: Commission structures are negotiated, not standard. Any output discussing fees should note that the structure is specific to this agency's agreement, not an industry-wide standard.

---

*Layer placement: L3 Stable Constraint · Real estate domain additions · Always loaded for every AgencyOS session*
