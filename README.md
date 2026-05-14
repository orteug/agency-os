# AgencyOS
### The operational layer for Diana's team.

> Run a high-touch boutique agency without operational chaos.

---

## What This Is

AgencyOS is a multi-folder AI operating system for a 4-person real estate team. Each folder is a specialist. Together, they form a shared operational brain that replaces scattered Google Docs, missed follow-ups, and the constant question: *"What needs attention right now?"*

This is not software. It is interpretable context methodology — folders as architecture, each file doing one job. Drop the folders into a Claude project. The system runs.

**Diana's test:** A new agent should be operational in one day. Start with `START_HERE.md`.

---

## The Architecture

```
agency-os/
├── README.md                    ← you are here
├── START_HERE.md               ← new agent onboarding
├── OPERATING_RHYTHM.md         ← daily and weekly cadence
├── HANDOFF_PROTOCOL.md         ← how work moves between specialists
├── CASE_STATE_SCHEMA.md        ← how operational reality is preserved
├── TEAM_ROLES.md               ← human ownership and escalation paths
│
├── 00_orchestrator/            ← front door; routes every request
├── 01_lead_qualifier/          ← turns inquiries into lead profiles
├── 02_property_research/       ← interpreted property and market briefs
├── 03_client_communication/    ← voice-matched drafts for agent review
├── 04_transaction_coordinator/ ← deadline tracking and risk management
├── 05_daily_deal_desk/         ← morning brief and operational awareness
│
├── examples/                   ← four complete end-to-end flows
└── templates/                  ← blank templates for case state, briefs, escalations
```

---

## The Core Architecture Decision

Most multi-agent systems pass work through handoffs. We use two distinct contracts:

**HANDOFF_PROTOCOL.md** — moves work between specialists. Tells the next specialist what to do.

**CASE_STATE_SCHEMA.md** — preserves operational reality. Tells every specialist what is true.

These are different things. The handoff says "here's the task." The case state says "here's everything we know about this client and this deal."

> **Handoffs move work. Case state preserves reality.**

This separation is what prevents context drift — the failure mode where specialists operate from conflicting or stale assumptions. Every specialist reads the same case state before acting. Every specialist updates the case state after acting.

---

## How a Typical Request Flows

### New Buyer Lead

```
Zillow inquiry arrives
        ↓
00_orchestrator
  → Initializes case (case_id, stage: lead)
  → Routes to 01_lead_qualifier [urgent]
        ↓
01_lead_qualifier
  → Qualification conversation (1-2 questions)
  → Updates lead_profile + emotional_profile in case state
  → Routes to 03_client_communication
        ↓
03_client_communication
  → Reads emotional_profile (client type determines tone)
  → Drafts first follow-up for agent review
  → Logs touch in communication_log
        ↓
Agent reviews draft → sends
        ↓
05_daily_deal_desk (next morning)
  → Case appears in pipeline snapshot
  → Flags if next touch is approaching stale
```

### Accepted Offer → Under Contract

```
Purchase agreement executed
        ↓
00_orchestrator
  → Updates stage: under_contract
  → Routes to 04_transaction_coordinator [urgent]
        ↓
04_transaction_coordinator
  → Maps all contractual deadlines
  → Documents all required documents
  → Sets 72h and 24h deadline flags
  → Initializes risk state
        ↓
05_daily_deal_desk (every morning)
  → Monitors all deadlines
  → Flags critical items in URGENT tier
  → Routes Diana escalations when needed
```

---

## The Six Specialists

| Specialist | Job | Talks To |
|------------|-----|----------|
| 00 Orchestrator | Routes every request; initializes cases | All specialists |
| 01 Lead Qualifier | Turns inquiries into lead + emotional profiles | 03 Communication |
| 02 Property Research | Interpreted briefs on properties and markets | 03 Communication |
| 03 Client Communication | Voice-matched drafts adapted to client type | Agent (for review) |
| 04 Transaction Coordinator | Deadline tracking, document status, risk escalation | 03 Communication, 05 Daily |
| 05 Daily Deal Desk | Morning brief, stale detection, Diana's queue | All specialists |

---

## The Daily Brief

Every morning, the daily deal desk produces a brief that tells the team:

- 🔴 **URGENT** — What breaks today if not acted on
- 🟡 **ATTENTION** — What to monitor
- 🟢 **IN PROGRESS** — What's healthy (visibility only)
- 📋 **DIANA'S QUEUE** — Decisions only Diana can make
- 📊 **PIPELINE SNAPSHOT** — Numbers
- 🔔 **STALE ALERTS** — Clients not contacted in too long

See `examples/daily_brief_example.md` for a real-looking brief across five active cases.

The brief replaced the morning standup.

---

## The Handoff Protocol

Every specialist-to-specialist handoff uses a structured envelope (see `HANDOFF_PROTOCOL.md`):
- Who is sending, who is receiving, and why
- The specific task
- Three critical context items the receiving specialist cannot miss
- What success looks like
- Escalation conditions

No vague summaries. No forwarded emails. Structured contracts.

---

## Client Emotional State — A First-Class Field

The case state includes `emotional_profile.client_type` with documented types:

- First-time buyer (needs reassurance, education)
- Relocating family (needs timeline management)
- Investor (needs data, not emotion)
- Emotionally attached seller (needs acknowledgment before practicalities)
- Luxury client (needs polish and detail)
- Aggressive negotiator (needs directness)

This field travels through every specialist. The communication specialist uses it to calibrate tone. The transaction coordinator uses it in escalation decisions. The daily deal desk uses it to assess communication risk.

The same deal update produces different emails depending on who is receiving it. That is not a feature. That is the product.

---

## Failure Modes Are Documented

Every `rules.md` includes an **Escalate When** section and a **Failure Modes** section. The system knows what it cannot handle and routes accordingly.

What gets escalated to Diana:
- Pricing strategy and negotiation position
- Legal ambiguity
- High-risk client situations
- VIP referral relationships
- Any `diana_escalation_required: yes` in case state

What does not reach Diana:
- Routine follow-up scheduling
- Standard property research
- Communication drafts for review
- Normal deadline management

Diana makes decisions. The system handles everything else.

---


---

## The Integration Layer

AgencyOS connects to Claude.ai native integrations when they are active on your team's account. Specialists detect available connectors automatically and adjust their behavior — no configuration required.

| Connector | Specialists That Use It | What Changes |
|-----------|------------------------|--------------|
| **Gmail** | Client Communication, TC, Daily Deal Desk, Orchestrator | Reads actual email threads before drafting. Detects stale leads from real gaps, not logged dates. Surfaces unanswered lender and listing agent threads as risk signals. |
| **Google Calendar** | TC, Daily Deal Desk, Orchestrator | Cross-references contractual deadlines against actual calendar. Includes today's showings and closings in the brief with exact times. Adjusts routing priority based on what's scheduled. |
| **Google Drive** | Property Research, TC | Checks for existing research briefs and transaction documents before rebuilding work. Updates document status from Drive contents. |
| **Slack** | Daily Deal Desk, Orchestrator | Scans active deal threads for unresolved questions. Surfaces unanswered decision requests to Diana's queue. |

**Connectors are additive — AgencyOS works fully without them.** When connected, specialists use live data. When not connected, specialists use manually maintained case state. Both paths produce complete, accurate output.

**Not yet available as Claude connectors:** Follow Up Boss, kvCORE, Dotloop, SkySlope, DocuSign, MLS feeds. These are the integrations the web application will connect natively. The Claude Project handles what's in the connector directory today. The app handles the rest.

**To connect:** Claude.ai Settings → Integrations → connect Gmail and Google Calendar with your team's Google account.

## Setting Up in Claude

1. Create one Claude project per specialist (six total), or use one project with all folders loaded
2. Upload the relevant folder(s) into each project's knowledge base
3. Include `CASE_STATE_SCHEMA.md` and `HANDOFF_PROTOCOL.md` in every project — they are shared contracts
4. Start every session by giving the orchestrator a case ID and a request

The case state lives in your conversation history and in a shared document (Google Doc, Notion, or any shared doc). Update it after each session.

---

## Onboarding a New Agent

1. Send them to `START_HERE.md` — everything they need to be operational today
2. Have Diana walk them through one active case using the system
3. Assign them as owner on their first two cases
4. Review their first two drafts before they send

Diana's test: operational in one day. The system makes that possible.

---

## Design Decisions

**Why a sixth folder (05_daily_deal_desk)?**
The five required folders handle work. The sixth folder creates operational awareness. Without it, the team has to reconstruct pipeline status every morning from memory. The daily deal desk does that reconstruction automatically and surfaces the one thing most multi-agent systems miss: *the things that aren't happening but should be.*

**Why separate CASE_STATE_SCHEMA from HANDOFF_PROTOCOL?**
Because context drift is the primary failure mode in multi-specialist systems. When state is embedded only in handoff summaries, it degrades with every pass. When state is canonical and always-current, specialists always start from truth.

**Why document failure modes?**
Because the system is more trustworthy when it knows its limits. A specialist that refuses to interpret a contract and routes to Diana is more useful than one that attempts the interpretation and gets it wrong. Every rules.md is explicit about the edge cases that require human judgment.

---

## What We Didn't Build and Why

**No inbox.md front door.**
Several implementations route everything through a single intake file before the orchestrator touches it. We skipped it. The orchestrator handles classification and routing in the same operation. An inbox adds a step without adding intelligence.

**No shared artifact files per case.**
An alternative pattern writes specialist outputs (lead cards, research briefs, deal files) as separate files in a shared folder. We use one canonical Agency File per case instead. Distributed artifacts create synchronization problems — when specialists write to different files, consistency depends on update discipline that breaks under pressure. One state object is always coherent.

**No dedicated market intelligence specialist.**
We considered a 06_market_intelligence/ folder. Built a freshness check and reference file in 02_property_research instead. Market intelligence isn't a separate workflow — it's context for research. A reference file with a 30-day staleness protocol serves the same function with less routing overhead.

**No autonomous agent runtime.**
No LangGraph, no CrewAI, no self-orchestrating loops. Diana's team is the coordination layer. Claude is the specialist. Human-in-the-loop is the architecture, not a workaround. This holds for the web app too — the system drafts and surfaces, humans decide and send.

**No CRM replacement.**
Some approaches try to replicate contact management inside the ICM system. AgencyOS sits alongside whatever Diana already uses. The Agency File holds operational transaction state, not contact history. The web app will connect to Follow Up Boss and Dotloop via API — not replace them.

**No JSON envelopes on handoffs.**
Structured JSON handoff contracts are precise but brittle. Agents read and write handoffs under time pressure. YAML for the Agency File, plain English for handoff instructions. Human-readable formats reduce errors at the points where errors are most costly.

---

## What Would Be Added Next

With another week: the web application. Persistent Agency Files in Supabase, the daily brief as a live dashboard, and native connectors to Dotloop, Follow Up Boss, DocuSign, and MLS feeds — the real estate tools that aren't in the Claude connector directory yet. The Agency File schema maps directly to a Supabase table. The specialist files become the system instructions layer of the application. The repo is the doctrine. The app is the building.

---

## Built For

Diana's team. And any 4-10 person boutique real estate agency that has outgrown scattered Google Docs and hasn't yet needed a full CRM.

The same architecture adapts: property management teams, boutique mortgage brokerages, small law firms, creative agencies. Anywhere that operational chaos comes from fragmented context and missed handoffs.

---

## Founding Cohort

AgencyOS is being built as a web application. The founding cohort (10 teams) gets the configured Claude Project now and the full web app when it launches Q3 2026. Founding rate: $199/month, locked for life.

The same architecture adapts to any 3–10 person team running on context overload — property management, boutique mortgage, small law firms. The operational problem is the same.

**hello@arielortiz.me** to join the founding cohort.
**Landing page:** https://landing-agencyos.vercel.app/

---

*AgencyOS v1.0 — Built for Skool Weekly Comp #4: The Agency*
*Architecture: Interpretable Context Methodology (ICM)*
*MIT License — May 2026*
