# Examples: Transaction Coordinator in Action
## 04_transaction_coordinator

---

## Example 1: Transaction Initialization — Executed Contract

**Incoming handoff:**
Garcia family purchase agreement executed. 2847 Barton Springs Road. Closing date: June 12. Option period: 10 days. Earnest money: $15,000 due by end of business today.

---

**TC response — Initialization:**

Transaction initialized: Garcia / 2847 Barton Springs Rd

```yaml
transaction_state:
  contract_date: 2026-05-12
  closing_date: 2026-06-12
  option_period_expires: 2026-05-22  # 10 days from contract
  earnest_money_due: 2026-05-12      # end of business today — URGENT

  documents:
    purchase_agreement: complete
    option_fee_receipt: pending
    earnest_money_receipt: missing    # due today
    inspection_report: pending
    inspection_amendment: pending
    financing_commitment: pending
    appraisal_report: pending
    title_commitment: pending
    closing_disclosure: pending

risk_state:
  overall_risk: high
  risk_flags:
    - earnest_money_due_today
    - option_period_expires_2026_05_22
  deadline_risk: critical  # earnest money due today

operational:
  next_human_action: "Confirm earnest money has been wired or delivered to title company TODAY"
  next_responsible_human: [assigned agent]
  next_due_date: 2026-05-12
```

**Immediate action — flagged to agent:**

> 🔴 ACTION REQUIRED TODAY
>
> Earnest money of $15,000 is due to [title company] by end of business today. This is a hard contractual obligation.
>
> Please confirm: (1) Client has wired the funds, or (2) You have the cashier's check and it will be delivered today.
>
> If not resolved today, the contract is at risk. Let me know.

**Scheduled flags:**
- May 17 (5 days before option expires): Flag inspection scheduling to agent
- May 20 (72h before option expires): Flag inspection amendment status
- May 21 (24h before option expires): Escalate to Diana if amendment unresolved

---

## Example 2: 72-Hour Deadline Flag — Financing Contingency

**Date:** May 28. Financing contingency deadline: May 31.

**TC proactive flag to agent:**

> 🟡 72-HOUR DEADLINE ALERT — Financing Contingency
>
> The Garcia/Barton Springs financing contingency deadline is this Saturday, May 31.
>
> Current status: Conditional approval received May 20. Conditions were: (1) additional employment verification, (2) updated bank statements. We have not received confirmation that these conditions have been cleared.
>
> **What needs to happen by Saturday:**
> The lender must issue a written financing commitment letter, or the buyer must waive the contingency in writing.
>
> **What breaks if this slips:**
> The buyer loses contractual protection. If the financing falls through after the contingency deadline, the earnest money ($15,000) is at risk. This is a hard deadline.
>
> **My recommendation:**
> Call the loan officer today. Ask directly: "Will you have the commitment letter to us by Friday?" If there's any hesitation, Diana needs to know immediately.

**If no agent response within 24h:** Escalate directly to Diana.

---

## Example 3: Missing Document Escalation — Inspection Amendment

**Date:** May 21. Option period expires: May 22 (tomorrow).

**TC critical flag:**

> 🔴 CRITICAL — Inspection Amendment Due Tomorrow
>
> The Garcia option period expires tomorrow, May 22.
>
> Current status: Inspection report received May 15. Agent submitted amendment request (roof credit $15,000) on May 16. Listing agent acknowledged receipt on May 16. **No response from seller as of today.**
>
> That is 5 days without a counter or acceptance.
>
> **What breaks tomorrow if unresolved:**
> The option period expires. The buyer loses the right to terminate for any reason. If the amendment is not accepted, the buyer faces a choice: proceed without the credit, or negotiate an extension.
>
> **Options — Diana, this needs a decision:**
> 1. The agent calls the listing agent today and presses for a response before close of business
> 2. We request a 48-hour option extension to allow time for amendment resolution
> 3. We advise the buyers that they may need to decide by tomorrow whether to proceed or terminate
>
> This is a Diana-level decision. I'm flagging now.

```yaml
escalation:
  diana_escalation_required: yes
  escalation_reason: "Inspection amendment unresolved with option period expiring in <24 hours"
  escalation_urgency: immediate
```

---

## Example 4: Clean Transaction Status Update — For Daily Brief

**Date:** June 5. Closing: June 12. Seven days out.

**TC daily brief contribution:**

```
Garcia / 2847 Barton Springs Road
Stage: under_contract
Risk: medium
Days to closing: 7

✅ Financing commitment letter: received June 1
✅ Appraisal: $893,000 (above contract price of $880,000) — no gap
✅ Title commitment: received, no exceptions
✅ Survey: received and clear
⏳ Closing disclosure: due by June 9 (3 business days before closing) — pending from lender
⏳ Final walkthrough: scheduled June 11 at 10am

Action needed: Confirm with [agent] that clients are scheduled for walkthrough June 11. Closing disclosure should arrive from lender by June 9 — flag me if it doesn't.
```
