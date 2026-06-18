# Design Decisions — v2 & v3 Architecture
## AgencyOS

> Maps every structural change from v1 to v3 to its source concept in:
> **Van Clief & McDermott, "ICM-Folder-Structure-as-Agentic-Architecture" (arXiv:2603.16021)**

---

## The Core Insight from the ICM Paper

The paper argues that Claude Projects are not just knowledge bases — they are **Interpretable Context Methodologies (ICMs)**. The folder structure *is* the architecture. Files at different layers serve different cognitive roles. Mixing them degrades output.

| Layer | Role | Should contain |
|-------|------|----------------|
| L0 — Identity | Who the agent is | Fixed character, expertise, limits |
| L1 — Routing | Where inputs go | Mode triggers, input classification |
| L2 — Task contract | How each task executes | Per-mode execution specs |
| L3 — Reference (stable) | Stable constraints | Frameworks that don't change per-session |
| L4 — Working (per-run) | Per-session data | Active case data, session logs |

v1 mixed these layers. v2+v3 make each layer explicit.

---

## Decision 1: Extract routing from `00_orchestrator/rules.md` into `routing.md`

**v1 behavior:** The orchestrator's rules.md contained routing logic (the routing table, BD pipeline decision rules, integration awareness) AND behavioral rules (never route without a case ID, accuracy review protocol, escalation format). A single file served two distinct cognitive roles.

**v2 change:**
- `routing.md` — L1 only: routing table, BD routing rules, integration awareness, data currency check
- `00_orchestrator/rules.md` — behavioral rules only: how to route with context, accuracy review, new case initialization, escalation format, failure modes

**ICM source:** L1 (routing) and L3 (behavioral constraints) serve different cognitive functions. The paper argues mixing routing logic with constraint definitions forces the model to hold two context types simultaneously — increasing context collapse risk where routing behavior bleeds into reasoning behavior or vice versa.

**What this fixes:** In v1, reading `00_orchestrator/rules.md` required parsing which sections were "do this for this input type" (routing) vs. "always behave this way" (constraint). In v2, routing decisions happen in `routing.md` before any specialist reasoning begins.

---

## Decision 2: Create L2 task contracts for orchestrator and daily brief

**v1 behavior:** The orchestrator's execution was implied by reading rules.md. Each specialist had a `handoff.md` which was a proto-L2 contract for what to pass forward, but there was no explicit contract for what the orchestrator itself produces in a routing session. The daily brief was described in OPERATING_RHYTHM.md but had no formal execution contract.

**v2 change:**
- `_modes/orchestrator-mode.md` — explicit execution contract for routing sessions
- `_modes/daily-brief-mode.md` — explicit execution contract for daily brief generation

**ICM source:** Context Layer 2 (task contract). The paper distinguishes L2 from L1 (routing) and L3 (constraints): L2 is mode-specific, loaded only when that mode is active. The handoff.md files in each specialist folder are proto-L2 contracts — v2 adds the missing orchestrator-level contracts.

**What this fixes:** Cold-start sessions had no formal output structure for the orchestrator itself. The routing session output varied in format and completeness. A formal task contract makes the execution unambiguous and auditable.

**Note:** The per-specialist `handoff.md` files are preserved as-is. They function correctly as specialist-level L2 contracts. This decision adds orchestrator-level contracts without modifying the specialist layer.

---

## Decision 3: Move `austin-market-context.md` to `_market_data/`

**v1 behavior:** `02_property_research/reference/austin-market-context.md` had a correct 30-day refresh cadence noted, but lived inside a specialist's reference folder — mixed with its operational rules and stable frameworks.

**v2 change:** Moved to `_market_data/austin-market-context.md` — root-level time-sensitive data folder, visible to any specialist that needs it.

**ICM source:** L3 vs. L4 distinction. The paper states L3 files should be "stable per invocation cycle." Market conditions data that refreshes monthly is not a stable constraint — it is working data that changes slowly. Treating it as L3 allows stale data to contaminate sessions silently. The `_market_data/` folder signals: this file requires currency checks.

**What this fixes:** Market data was buried inside a specialist folder, making it inaccessible to the orchestrator and daily deal desk without loading specialist context. Root-level placement makes it available to all modes.

---

## Decision 4: Add L4 working layer (`_working/`)

**v1 behavior:** No persistent state at the system level. Individual cases had state (via CASE_STATE_SCHEMA.md), but there was no mechanism for the system itself to learn from sessions — which routing decisions were accurate, which urgency flags resolved as predicted, which brief assessments were off.

**v2 change:**
- `_working/_calibration_log.md` — session outcomes logged after every brief and significant routing session
- `_working/_patterns.md` — cross-session pattern tracker

**ICM source:** Context Layer 4 (working/per-run data). The paper argues the absence of L4 is the most common structural gap in Claude Projects deployments. A calibration log converts isolated sessions into a compounding intelligence system.

**What this fixes:** In v1, the system had no feedback mechanism. A routing rule that was systematically wrong (misclassifying BD leads as active, under-flagging urgency) would fire incorrectly indefinitely. The calibration log is the feedback mechanism.

---

## Decision 5: Add `_guardrails/` as a structural safety layer (v3)

**v1+v2 behavior:** Safety guidance existed as escalation rules in `00_orchestrator/rules.md` — escalate to Diana for pricing, legal, financing denial. Correct instinct, but embedded in prose alongside routing rules. Under context pressure, prose rules are the first things deprioritized.

**v3 change:**
- `_guardrails/shared/` — four files always loaded, applying to every mode, every session
- `_guardrails/domain/real-estate-guardrails.md` — 5 escalation triggers + 5 input flags specific to real estate transactions
- `routing.md` Step 0: load guardrails before any routing begins
- Both mode contracts updated with structural slots: Input Integrity Flag, confidence level, Professional Required block, Disclaimer Block

**ICM source:** Layer 3 stability principle. Safety posture that lives embedded in prose is subject to context pressure. Structural placement in `_guardrails/` makes guardrails load-order enforced — they cannot be forgotten, softened, or overridden by user instruction.

**Real estate-specific rationale:** Real estate transactions involve clients making decisions worth hundreds of thousands of dollars based partly on AI-assisted analysis. Pricing strategy, legal interpretation, and financing guidance all require licensed professionals. The guardrails make this explicit and structural, not advisory.

**Shared vs. domain split:** `_guardrails/shared/` is portable — identical to the shared guardrails in the HVAC & Fire Safety Acquisition Analyst repo. Domain-specific triggers are in `_guardrails/domain/` and do not pollute the shared layer.

---

## What Didn't Change

- Per-specialist identity.md files — all clean L0. Character, expertise, limits. No structural changes needed.
- Per-specialist `handoff.md` files — functioning proto-L2 contracts. Preserved as-is.
- CASE_STATE_SCHEMA.md, HANDOFF_PROTOCOL.md, EMOTIONAL_INTELLIGENCE.md — root-level doctrine files. Stable. No changes.
- The core operational framework — lead qualification, transaction coordination, BD pipeline, daily brief logic — all preserved. v2+v3 are structural upgrades, not content rewrites.

---

*Reference: Van Clief & McDermott, "ICM-Folder-Structure-as-Agentic-Architecture" (arXiv:2603.16021)*
*v2+v3 built: 2026-06-17*
