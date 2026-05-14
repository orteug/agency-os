# Judge Guide

## AgencyOS — Test It in 5 Minutes

This file is for competition judges. Three steps, one prompt, live output.

---

## Step 1 — Create a Claude Project

1. Go to [claude.ai](https://claude.ai)
2. Click **Projects** in the left sidebar → **New Project**
3. Name it `AgencyOS Test`

---

## Step 2 — Upload the Folders

Clone or download from **https://github.com/orteug/agency-os**, then upload all files to the project knowledge base maintaining the folder structure.

Required for the system to work correctly:

- All seven specialist folders (`00_orchestrator` through `06_bd_coordinator`)
- `CASE_STATE_SCHEMA.md`
- `HANDOFF_PROTOCOL.md`
- `START_HERE.md`

Optional but recommended:

- `README.md`, `TEAM_ROLES.md`, `OPERATING_RHYTHM.md`
- `examples/` folder
- `templates/` folder

**Tip:** Upload the folders in order, 00 through 05, then the root files.

---

## Step 3 — Paste One of These Prompts

Pick the scenario you want to test. Paste it into the project conversation exactly as written.

---

### Test A — New Buyer Lead (start here)

```
New lead just came in. Zillow inquiry from Marcus Webb.

His message: "Looking for a 4/3 in Barton Hills or Zilker, budget around $900k. 
Want to move in 60 days. Phone number is 512-555-0142."

What do we do with this?
```

**What you should see:**
The orchestrator creates a new case, routes to the lead qualifier, qualification questions are identified, a drafted first follow-up is produced, and a complete lead profile and emotional profile are populated. The system identifies this as a hot buyer (pre-approved financing needed, hard July 1 deadline likely) and flags urgency accordingly.

---

### Test B — Daily Brief

```
Good morning. Generate the daily brief for the team.

Active cases:
- Marcus Webb: hot buyer, pre-approved, July 1 deadline. Last contact 2 days ago.
- Garcia family: under contract on 2847 Barton Springs Rd, closing June 12. 
  Financing contingency expires tomorrow. Lender hasn't responded in 3 days.
- Patricia Morales: seller, listing appointment next week. 
  Pricing conversation pending with Diana.
- Patel family: under contract 512 Dawson Rd, closing June 5. All clean.
```

**What you should see:**
The daily deal desk produces a full morning brief with 🔴 URGENT / 🟡 ATTENTION / 🟢 IN PROGRESS tiers, a populated Diana's queue with specific decision requests, a pipeline snapshot, and a stale alert for Marcus Webb. The Garcia financing item should be in URGENT — option expires tomorrow.

---

### Test C — Inspection Issue

```
The Garcia family inspection just came back.

Key findings:
- Roof needs full replacement. Inspector estimate: $18,000–$22,000.
- Foundation: two piers needed. Estimate: $4,500–$6,000.
- HVAC filter needs replacement. ~$200.
- Two ungrounded outlets. ~$150.

We're in the option period. It expires May 22 — that's 7 days from now. 
The Garcias are first-time buyers, medium anxiety, relocating from Chicago. 
How do we handle this?
```

**What you should see:**
The orchestrator identifies this as a multi-specialist situation: the transaction coordinator maps the amendment timeline and flags the option period deadline, the client communication specialist drafts an email to the Garcias in a tone calibrated for an anxious first-time buyer (not a clinical list of findings), and the escalation protocol surfaces a Diana decision point on amendment strategy.

---

### Test D — What Does the Handoff Look Like?

After running Test A or Test C, ask:

```
Show me the handoff envelope that would pass this case to the next specialist.
```

**What you should see:**
A structured YAML handoff following the HANDOFF_PROTOCOL — `from_specialist`, `to_specialist`, `trigger`, `case_ref`, `task`, `critical_context` (3 items), `expected_output`, and `escalate_if` conditions. This is the architecture that prevents context drift.

---

---

### Test E — Emotional Intelligence in Action

This test demonstrates the third root contract. Same property, same price, same market conditions — different client type produces a different research brief.

**Run this first (Anxious First-Timer):**

```
Research 2847 Barton Springs Rd for the Garcia family. Showing Thursday.

Client profile: First-time buyers, relocating from Chicago. Medium anxiety. 
Three kids, schools and commute are their top priorities. Budget ceiling $950k.
This is the largest financial decision of their lives.
```

Then run this (Analytical Investor):

```
Research 2847 Barton Springs Rd for Marcus Chen. Showing Thursday.

Client profile: Investor, owns four Austin properties. Analytical, data-forward. 
Evaluating as a rental or flip. Budget ceiling $950k. Needs cap rate context 
and exit scenario analysis.
```

**What you should see:**

Two completely different research briefs from the same property. The Garcia brief leads with school district quality, neighborhood feel, and what could go wrong with a manageable explanation of each risk. The Chen brief leads with pricing leverage, cap rate estimates, days on market as negotiating signal, and exit scenarios.

Same property. Same data. Different humans. Different briefs.

This is not prompt engineering — the client type is a field in the case state that the specialist reads before writing its first word. The `emotional_profile.money_psychology_type` field (introduced in v1.1) carries the behavioral finance context that determines the interpretive frame.

See `EMOTIONAL_INTELLIGENCE.md` for the full framework. See `02_property_research/rules.md` → "Tailor the emotional fit analysis to the client type" for the rules that produce this behavior.

## What to Look For

The four judging criteria, and where each shows up in the tests:

**1. Does the architecture make sense?**
Test A shows the routing logic. Test D shows the handoff structure. The specialists don't overlap — each one owns its domain and passes work with the context the next one needs.

**2. Are handoffs actually defined or hand-waved?**
Test D answers this directly. The handoff is a structured contract, not a summary paragraph. Required fields are validated. Back-handoff protocol is documented for every specialist.

**3. Is the README good enough to onboard a stranger?**
`START_HERE.md` is the onboarding document. A new agent should be operational from that file alone. The README explains the full architecture.

**4. Real design decisions or template filling?**
Three decisions worth examining:

*Decision 1 — Two distinct contracts.* `HANDOFF_PROTOCOL.md` moves work. `CASE_STATE_SCHEMA.md` preserves reality. These are kept separate intentionally. When state is embedded only in handoff summaries, it degrades with every pass.

*Decision 2 — Client emotional state as a first-class field.* See `CASE_STATE_SCHEMA.md` → `emotional_profile.client_type`. The same inspection result in Test C produces a different email for an anxious first-time buyer than it would for an analytical investor. That logic lives in the communication specialist's rules — not in the prompt.

*Decision 3 — Emotional intelligence as a root-level contract.* `EMOTIONAL_INTELLIGENCE.md` sits alongside the two contracts above as the third foundational document. Test E demonstrates this directly.

---

## If You Have Gmail or Google Calendar Connected

The specialists detect active Claude.ai connectors automatically. If you have Gmail connected, the lead qualifier will search for prior contact with Marcus Webb before qualifying. The daily deal desk will read actual email thread gaps instead of using logged dates. The transaction coordinator will check lender email threads for the Garcia case.

See `START_HERE.md` → "Connecting Your Tools" for details.

---

## Beyond the Competition

AgencyOS is being built as a web application — persistent Agency Files in Supabase, shared team dashboard, live daily brief interface, and native connectors to Dotloop, Follow Up Boss, DocuSign, and MLS feeds. The founding cohort (10 teams) is open now at $199/month. The Claude Project you're testing is the early access tier.

**hello@arielortiz.me** to join the founding cohort or discuss the product roadmap.
