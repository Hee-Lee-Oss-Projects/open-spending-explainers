# Open-Spending-Explainers — TASKS.md

> Status: Draft · Version: 0.1.0 · Last updated: 2026-06-28 · Owner: TBD (maintainer) · Lane: donated

Backlog for **Open-Spending-Explainers** (slug: `open-spending-explainers`): plain-language,
source-cited explainers of government **budgets and public contracts**, one jurisdiction at a time,
built only from open/PD/openly-licensed data — **describing what the public-money data shows, never
arguing whether it is good or bad.** See `PLAN.md` for full context.

The **non-partisan editorial policy** and the **license/provenance gate** are the first build items
and hard product requirements. This is a **MEDIUM risk-tier** project: any task producing or framing
jurisdiction budget/contract content requires **non-partisan neutrality review** and **public-finance/
governmental-accounting expert sign-off** (snapshot-scoped) before it ships. No partner, expert, or
audience is yet secured, so delivery-dependent tasks carry `requestor: TO BE SECURED` and
`verifiedNeed: false`.

## How these tasks map to Elyos

Each task below becomes an Elyos **Task JSON** validated against `packages/schema/src/schemas.ts`.
Field mapping:

- **id** — stable slug `open-spending-explainers-<area>-NNN` (e.g.
  `open-spending-explainers-editorial-001`).
- **title** — the task title in the milestone table.
- **project** — `open-spending-explainers`.
- **type** — one of `code | research | writing | data | design-spec | maintenance`.
- **lane** — `donated` (default; no funded tasks planned. Any `funded` task must add
  `fundedBudgetUsd` with a hard cap — e.g. a future expert-review-hours task).
- **priority** — `high | medium | low`.
- **domain** — array, e.g. `["civic","open-data","public-finance","transparency","software"]`.
- **riskTier** — `low | medium | high`. Jurisdiction budget/contract **content** and its framing are
  `medium` (domain accuracy + neutrality); pure pipeline/infra/UI is `low`. Nothing here is `high`
  (no medical/legal/safety advice; no allegations).
- **urgent** — boolean (no urgent tasks at cold-start).
- **deliverable** — `pr | dataset | document | translation`.
- **tokenEstimate** — `small | medium | large` (maps to the Size column).
- **status** — `open | in-progress | review | delivered | done` (all start `open`).
- **context / objective / acceptanceCriteria[] / resources[] / output** — per task.
- **requestor** — partner/steward/expert; `TO BE SECURED` where unknown (currently everywhere).
- **verifiedNeed** — `true` only once a specific partner/need is confirmed; otherwise `false`
  (currently `false` everywhere — no partner secured).
- **outputLicense** — code: `MIT`; content/datasets/docs: `CC-BY-4.0`; any OSM-derived geodata:
  `ODbL-1.0` (isolated).

Size legend: small ≈ `small`, med ≈ `medium`, large ≈ `large`.
Reviewer "Expert (public-finance)" = credentialed public-finance / governmental-accounting reviewer
(MEDIUM-tier, snapshot-scoped sign-off, **TO BE SECURED**). "Neutrality reviewer" = independent
non-partisan framing reviewer.

---

## Milestone M0 — Editorial + license gates & skeleton (cold-start)

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| open-spending-explainers-jurisdiction-000 | Pilot jurisdiction selection (scored against explicit criteria) — gates M2–M6 | research | small | medium | document | — | Maintainer + Expert (public-finance) |
| open-spending-explainers-editorial-001 | Non-partisan editorial / neutrality policy specification (describe-don't-evaluate) | design-spec | medium | medium | document | — | Neutrality reviewer + Expert (public-finance) |
| open-spending-explainers-repo-002 | Monorepo + pnpm + TS/ESM + CI (build/test/lint) skeleton | code | small | low | pr | — | Maintainer |
| open-spending-explainers-neutrality-003 | Neutrality lint (banned-framing lexicon + comparative/blame/CTA/allegation detectors) wired into CI | code | medium | medium | pr | 001, 002 | Neutrality reviewer |
| open-spending-explainers-license-004 | License/provenance gate (fail-closed; per-source verification + provenance manifest) | code | medium | medium | pr | 002 | Maintainer + Expert (public-finance) |

**Acceptance criteria — key tasks**

- **open-spending-explainers-jurisdiction-000** (pilot jurisdiction selection)
  - Scores candidates against explicit criteria: (1) open, reuse-and-derivatives-licensed budget +
    OCDS contract data actually available and machine-readable; (2) data quality/completeness
    (reconcilable line items, documented fund structure); (3) an available public-finance reviewer;
    (4) a plausible distribution partner/audience; (5) manageable accounting complexity.
  - Records the decision (or a shortlist + the fixed decision deadline of 2026-08-31) with rationale;
    the chosen jurisdiction becomes a dependency for M2 source/content tasks.
  - Note: the *specific* jurisdiction remains `TO BE SECURED` until confirmed, but the decision rule
    and deadline are fixed; this task gates M2–M6 sequencing.

- **open-spending-explainers-editorial-001** (neutrality policy spec)
  - States the core invariant: **describe what the data shows, never evaluate it** (no waste/efficiency,
    fair/unfair, raid/giveaway language; no advocacy, calls-to-action, or "should").
  - Enumerates the banned-framing taxonomy in concrete, testable terms: loaded lexicon, loaded
    comparatives, causal-blame, selective-omission/cherry-picking, allegation-by-implication, and
    election/ballot/campaign framing.
  - Defines the required fiscal context every explainer must state (fiscal year; nominal vs real with
    named deflator + base year; budgeted vs actual; fund accounting; data limits/caveats).
  - Specifies three enforcement points — draft-time `NEUTRALITY_SYSTEM` charter, CI neutrality lint
    (fail-closed: ambiguous framing → human review), and independent human/expert review — and the
    "no-allegations" rule for named officials/vendors.
  - Defines the editorial red-team taxonomy the suite must cover, with **≥ 8 cases per banned-framing
    category**, including insider-misuse vectors (framing smuggling, editorializing-by-citation-choice,
    comparison gaming).
  - Mandates the per-explainer labeling ("describes published open data; not advice; not an audit") and
    the public-finance expert-review gate.
  - Reviewed and signed off by the neutrality reviewer (and noted for public-finance review).

- **open-spending-explainers-neutrality-003** (neutrality lint)
  - Implements deterministic checks for every banned-framing category from the spec; a failure **fails
    the build**; fail-closed (ambiguous → flag for human review, never auto-pass).
  - Ships an editorial red-team suite (≥ 8 cases/category + insider-misuse vectors) run in CI; suite
    size is **non-decreasing**; any newly-found editorializing pattern is added as a regression case
    within one release.
  - Reports the metric as **passed/total at version N** with a per-version changelog; target 100%
    pass/redirect.

- **open-spending-explainers-license-004** (license/provenance gate)
  - **Fail-closed:** a source without a verified open/PD/CC/open-gov reuse-and-derivatives license is
    rejected at ingest; no proprietary/access-controlled/paywalled sources.
  - Records full provenance per source: publisher, jurisdiction, dataset name, URL, retrieval date,
    **sha256 snapshot checksum**, license, permits-derivatives flag, legal-status note → emitted as a
    provenance manifest.
  - **OSM/ODbL handling:** any OSM-derived geodata is tagged `ODbL`, isolated from the CC-BY corpus,
    and attributed; per-publisher OCDS license is verified individually (OCDS is a format, not a
    license). Tested.

**M0 Definition of Done:** neutrality policy spec (reviewed) + neutrality lint merged with the
editorial red-team suite passing in CI (no known editorializing bypass, reported as passed/total at
version N); license/provenance gate merged and fail-closed with a provenance manifest; TS/ESM
skeleton + green CI; "describes open data, not advice/audit" framing wired in; **pilot jurisdiction
selected (or shortlisted with a fixed decision)** so M1+ builds against the right corpus, license
reality, and reviewer profile.

---

## Milestone M1 — Pipeline, schema, provenance & numeric fidelity

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| open-spending-explainers-schema-005 | Normalized data model (Source/BudgetLine/Contract) + Frictionless Tabular Data Package | design-spec | medium | low | document | 002 | Maintainer |
| open-spending-explainers-ingest-006 | Budget ingestion + normalization pipeline with checksummed source snapshots | code | large | medium | pr | 004, 005 | Maintainer + Expert (public-finance) |
| open-spending-explainers-reconcile-007 | Numeric-fidelity / reconciliation engine + citation-coverage ("no source, no figure") | code | medium | medium | pr | 006 | Maintainer |
| open-spending-explainers-pii-008 | PII entity-vs-person screen (aggregate/entity-level only; redact private individuals) | code | small | medium | pr | 005 | Maintainer |
| open-spending-explainers-eval-029 | Minimal grounded+neutral kill-gate eval (handful of fixtures, early go/no-go) | code | small | medium | pr | 006, 007 | Maintainer + Neutrality reviewer |

**Acceptance criteria — key tasks**

- **open-spending-explainers-ingest-006** (budget ingestion + snapshots)
  - Pulls open budget data (open portal / OpenSpending / CSV-Excel-JSON) and normalizes to the data
    model; distinguishes `revenue|expenditure` and `budgeted|actual|adjusted`.
  - **Pins a checksummed (sha256) snapshot** of each source so all downstream artifacts are
    reproducible; provenance recorded via the M0 gate.
  - Optional COFOG functional mapping recorded where applied; tested on fixtures.

- **open-spending-explainers-reconcile-007** (reconciliation + citation coverage)
  - Every figure resolves to a citation pointing at a snapshot line item; **no figure ships without a
    resolving citation** (citation-coverage test).
  - Component figures **reconcile** to stated totals within the disclosed rounding tolerance;
    **headline totals match exactly**; a non-reconciling figure **fails CI**.
  - Never conflates budgeted with actual; mismatches surface as explicit failures, not silent coercion.

- **open-spending-explainers-pii-008** (PII screen)
  - Classifies contract parties entity-vs-natural-person; default surfaces entity/aggregate-level only.
  - Personal data of private individuals (personal address, personal identifiers) is redacted/withheld;
    no profiling, no targeting, no voter-data linkage. Tested on fixtures including sole-proprietor rows.

- **open-spending-explainers-eval-029** (minimal kill-gate)
  - Runs grounded+cited+neutral generation vs. a blank-slate "summarize this budget" baseline on a
    small fixture set; reports the delta as an early **go/no-go**.
  - If grounded+cited+neutral does not at least trend ahead on accuracy/citation/neutrality, M2 content
    investment **pauses** for review rather than waiting for the full M4 eval (eval-017).

**M1 Definition of Done:** reproducible budget ingestion with checksummed snapshots; normalized data
model + Tabular Data Package; reconciliation + citation-coverage gates in place (non-reconciling/uncited
figures fail CI); PII entity-vs-person screen working; provenance manifest emitted; the **minimal
grounded+neutral kill-gate passes** (trends ahead of baseline) before M2 content investment.

---

## Milestone M2 — Pilot budget explainers (expert-gated content)

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| open-spending-explainers-sources-009 | Pilot jurisdiction budget source vetting (license verified) + snapshot + provenance | data | medium | medium | dataset | 000, 004, 006 | Expert (public-finance) + Maintainer |
| open-spending-explainers-template-010 | Explainer template + grounded generation (cited to line items, neutrality-passing) | code | large | medium | pr | 003, 007 | Neutrality reviewer + Maintainer |
| open-spending-explainers-content-011 | Pilot jurisdiction budget explainer content (reconciled, cited, expert-reviewed) | writing | medium | medium | document | 009, 010 | Expert (public-finance) + Neutrality reviewer |

**Acceptance criteria — key tasks**

- **open-spending-explainers-sources-009** (source vetting)
  - Reuse terms verified and recorded per source (open/PD/CC/open-gov; permits-derivatives); only
    license-clean sources enabled; no proprietary/paywalled data.
  - Snapshot pinned (sha256) and full provenance recorded (publisher, citation, URL, retrieval date,
    license, legal-status); expert sign-off recorded before sources are enabled, snapshot-scoped.

- **open-spending-explainers-template-010** (template + grounded generation)
  - Template enforces complete, structured coverage (what this is; revenue; expenditure; fiscal
    context incl. FY + nominal/real + budgeted/actual + fund; data limits; "how to read this"; "verify
    it yourself" → companion dataset) — preventing editorializing-by-citation-choice.
  - Generation emits inline citations to snapshot line items; output must pass the neutrality lint and
    reconciliation engine before review.

- **open-spending-explainers-content-011** (pilot budget explainer content)
  - All figures reconcile to the pinned snapshot (headline totals exact); 100% citation coverage;
    neutrality lint passes; reading-level + accessibility targets met.
  - **Public-finance expert sign-off recorded** (snapshot/checksum-scoped) and independent neutrality
    review passed before publish; labeled "describes published open data; not advice; not an audit."

**M2 Definition of Done:** the pilot jurisdiction's budget explainer(s) drafted from a license-verified,
checksummed snapshot — fully reconciled, 100% cited, neutrality-passed, and **public-finance
expert-signed-off** behind the gates; the **minimal grounded+neutral kill-gate (eval-029), now run on
real cited content, still shows grounded ahead** — otherwise stop and reassess before M3.

---

## Milestone M3 — Contracts explainers + fiscal context + publishing

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| open-spending-explainers-contracts-012 | OCDS contracts ingestion + explainer module (entity-level, PII-screened) | code | large | medium | pr | 008, 010 | Expert (public-finance) + Neutrality reviewer |
| open-spending-explainers-context-013 | Fiscal-context layer (nominal/real, per-capita, FY normalization) + comparison guardrails | code | medium | medium | pr | 007 | Expert (public-finance) |
| open-spending-explainers-publish-014 | Static publishing site + companion dataset export (CC-BY) + corrections/erratum changelog | code | medium | low | pr | 011 | Maintainer |

**Acceptance criteria — key tasks**

- **open-spending-explainers-contracts-012** (contracts module)
  - Ingests OCDS releases (per-publisher license verified), normalizes to the Contract model, and
    generates explainers describing buyers, supplier **entities**, values, and categories — entity-level
    only, PII screen applied, **no allegations** of impropriety.
  - Figures reconcile; citations resolve; neutrality lint passes; expert sign-off recorded.

- **open-spending-explainers-context-013** (fiscal-context + comparison guardrails)
  - Applies nominal→real (named deflator + base year recorded), per-capita, and fiscal-year alignment.
  - **Comparison guardrails:** cross-jurisdiction/cross-year comparisons are **blocked or annotated**
    when accounting bases (e.g., GASB vs IPSAS), fiscal years, or nominal/real differ; tested.

**M3 Definition of Done:** OCDS contracts explainers (entity-level, PII-screened, reconciled, cited,
neutral, expert-signed-off); the fiscal-context layer + comparison guardrails live; accessible static
publishing site + machine-readable companion dataset (CC-BY) + corrections/erratum changelog — all
behind the gates.

---

## Milestone M4 — Eval, hardening & partner readiness

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| open-spending-explainers-eval-015 | Full eval: grounded+cited+neutral vs. blank-slate (accuracy/citation/neutrality) | code | medium | medium | pr | 010, 012, 013, 029 | Maintainer + Neutrality reviewer |
| open-spending-explainers-hardening-016 | Hardening: expanded neutrality red-team, WCAG 2.2 AA + plain-language, reconciliation/reproducibility verification + partner runbook | code | medium | medium | pr | 003, 007, 014 | Neutrality reviewer + Maintainer |

**Acceptance criteria — key tasks**

- **open-spending-explainers-eval-015** (full eval)
  - Runs explainers grounded+cited+neutral vs. blank-slate on fixture scenarios; an LLM judge scores
    accuracy, citation coverage, and neutrality; reports the delta.
  - Grounded+cited+neutral must **clearly beat** blank-slate, or the thesis is flagged failing.

- **open-spending-explainers-hardening-016** (hardening + partner readiness)
  - Expanded neutrality red-team still 100% pass; insider-misuse vectors covered.
  - Core explainers meet **WCAG 2.2 AA** and the plain-language reading-level target; reconciliation +
    **reproducibility** (regenerate identically from snapshot checksum + template version) verified
    end-to-end; partner onboarding/handoff runbook written.

**M4 Definition of Done:** eval proves grounded+cited+neutral beats blank-slate; expanded neutrality
red-team green; accessibility + plain-language + reconciliation + reproducibility verified; the project
is partner-ready with an onboarding runbook.

---

## Milestone M5 — Partner adoption & handoff (the deed)

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| open-spending-explainers-partner-017 | Secure distribution partner/steward + independent neutrality audit + expert sign-off of shipped content | research | medium | medium | document | 011, 012, 015, 016 | Steward + Expert (public-finance) + Neutrality reviewer |
| open-spending-explainers-handoff-018 | Partner adoption: explainers used/published; ≥1 documented beneficiary comprehension instance | maintenance | medium | medium | document | 017 | Steward + Neutrality reviewer |

**Acceptance criteria — key tasks**

- **open-spending-explainers-partner-017** (secure partner + audits)
  - A real partner/audience (newsroom/library/open-data office/civic-tech/policy program) is secured;
    steward assigned; `verifiedNeed` flips to `true`.
  - Independent neutrality reviewer audits framing; all shipped content has snapshot-scoped
    public-finance expert sign-off; all figures reconciled/cited; all sources license-verified.
  - Driven by the **dated partner-acquisition plan** (jurisdiction by 2026-08-31, expert by 2026-10-31,
    partner by 2026-12-31). If no partner is secured by **~2027-03-31**, apply the **build-vs-mothball/
    pivot decision rule** (PLAN exec summary): pivot the last-mile beneficiary (e.g., hand the pipeline
    + illustrative jurisdiction to a journalism-school/library data-literacy program as a reference
    deed) or mothball to maintenance-only — recorded in governance — rather than publish to no one.

- **open-spending-explainers-handoff-018** (closed loop — the deed)
  - The partner/audience demonstrably uses or publishes the explainers; ≥ 1 documented instance of a
    resident/journalist/official understanding the budget/contracts better (recorded with attestation).
  - Neutrality independently verified; numeric fidelity upheld; "not advice / not an audit" framing
    intact throughout.

**M5 Definition of Done:** project-level **Definition of Shipped** met — a real partner/audience in the
pilot jurisdiction uses/publishes the explainers with ≥ 1 documented comprehension outcome; neutrality
independently audited; figures reconciled/cited; sources license-verified; expert sign-off recorded.
*(Gated on a secured partner — TO BE SECURED.)*

---

## Milestone M6 — Sustain & scale (post-delivery)

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| open-spending-explainers-ops-019 | Ops runbook + outcome tracking + maintenance rotation + annual fiscal-year refresh cadence | maintenance | medium | low | document | 018 | Maintainer + Steward |

**Acceptance criteria — open-spending-explainers-ops-019**
- Runbook covers deploy, source re-ingest, snapshot re-verification, and partner support.
- Outcome tracking records partner/audience use, documented comprehension instances, and corrections
  issued — **not** page views.
- Named maintenance rotation; **annual fiscal-year refresh cadence** (new snapshot → reconcile →
  re-review → prior explainer auto-bannered superseded); documented, expert-gated, license-verified
  process for adding a second jurisdiction.

**M6 Definition of Done:** project sustainably maintained with beneficiary outcomes tracked, a rotation
owning it, an annual refresh + re-sign-off cadence, and a gated expansion process.

---

## Backlog / future

| ID | Title | Type | Size | Risk | Deliverable | Notes |
|---|---|---|---|---|---|---|
| open-spending-explainers-jurisdiction-020 | Second jurisdiction content pack | data | large | medium | dataset | Only after M6; full license-vetting + snapshot + expert sign-off |
| open-spending-explainers-i18n-021 | Internationalization + community translations of explainers | writing | medium | medium | translation | Review per language; preserve neutrality + figures |
| open-spending-explainers-widgets-022 | Embeddable, accessible figure widgets for partners | code | medium | low | pr | Reuses companion dataset; no editorializing in captions |
| open-spending-explainers-literacy-023 | "How to read a public budget" companion (neutral fiscal literacy) | writing | small | low | document | Educational; product-neutral; no advice |
| open-spending-explainers-expert-review-024 | Funded expert-review-hours engagement (hard budget cap) | research | small | medium | document | `lane: funded` → must set `fundedBudgetUsd`; preserves reviewer independence |
| open-spending-explainers-comparability-025 | COFOG-normalized cross-jurisdiction comparison (only where valid) | code | medium | medium | pr | Behind comparison guardrails; publish only when bases align |

---

## Example task JSON

Complete, schema-valid Task JSON for the first M0 hard-requirement task
(`open-spending-explainers-editorial-001`):

```json
{
  "id": "open-spending-explainers-editorial-001",
  "title": "Non-partisan editorial / neutrality policy specification (describe-don't-evaluate)",
  "project": "open-spending-explainers",
  "type": "design-spec",
  "lane": "donated",
  "priority": "high",
  "domain": ["civic", "open-data", "public-finance", "transparency", "governance", "software"],
  "riskTier": "medium",
  "urgent": false,
  "deliverable": "document",
  "tokenEstimate": "medium",
  "status": "open",
  "context": "Open-Spending-Explainers produces plain-language, source-cited explainers of government budgets and public contracts, one jurisdiction at a time, built only from open/PD/openly-licensed data. Budgets are inherently political, so neutrality is a safety property, not a tone: an explainer that calls spending 'wasteful', frames a cut as a 'raid', or implies a vendor did wrong has caused harm even if every number is correct. The non-partisan editorial policy is therefore a hard product requirement built FIRST and enforced by a CI lint plus independent review - not a disclaimer footer. This is the cold-start task that specifies that policy before any explainer is generated. No partner, public-finance expert reviewer, or audience is yet secured.",
  "objective": "Write the authoritative non-partisan editorial / neutrality policy specification that all later templates, generation, lint, and review must implement and be tested against: the describe-don't-evaluate invariant, the concrete banned-framing taxonomy, the required fiscal-context elements, the three enforcement points (draft-time NEUTRALITY_SYSTEM charter, CI neutrality lint, independent human/expert review), the no-allegations rule, the editorial red-team taxonomy, and the labeling and expert-review gates.",
  "acceptanceCriteria": [
    "States the core invariant - describe what the open data shows, never evaluate it - and bans advocacy, calls-to-action, 'should' statements, and value judgments (waste/efficiency, fair/unfair, raid/giveaway)",
    "Enumerates the banned-framing taxonomy in concrete, testable terms: loaded lexicon, loaded comparatives, causal-blame, selective-omission/cherry-picking, allegation-by-implication, and election/ballot/campaign framing",
    "Defines the fiscal context every explainer must state: fiscal year; nominal vs real with named deflator and base year; budgeted vs actual; fund accounting; and explicit data limits/caveats",
    "Specifies three enforcement points - draft-time NEUTRALITY_SYSTEM charter, CI neutrality lint (fail-closed: ambiguous framing routes to human review, never auto-passes), and independent human/expert review - plus the no-allegations rule for named officials and vendors",
    "Defines the editorial red-team taxonomy the suite must cover with at least 8 cases per banned-framing category, including insider-misuse vectors (framing smuggling, editorializing-by-citation-choice, comparison gaming)",
    "Mandates per-explainer labeling ('describes published open data; not financial/legal advice; not an audit') and the snapshot-scoped public-finance expert-review gate before content ships",
    "Reviewed and signed off by an independent neutrality reviewer (recorded in the reviewers ledger), and noted for public-finance review"
  ],
  "resources": [
    "planning/projects/open-spending-explainers/PLAN.md",
    "CLAUDE.md",
    "docs/good-deed-definition.md",
    "packages/schema/src/schemas.ts",
    "planning/projects/public-official-guide/PLAN.md"
  ],
  "output": "A reviewed editorial-policy specification (the non-partisan neutrality charter) defining the describe-don't-evaluate invariant, the banned-framing taxonomy, required fiscal-context elements, the three enforcement points, the no-allegations rule, the editorial red-team taxonomy, and the labeling/expert-review gates - the contract that open-spending-explainers-neutrality-003 (neutrality lint) and open-spending-explainers-template-010 (explainer template + generation) implement and verify.",
  "requestor": "TO BE SECURED",
  "verifiedNeed": false,
  "outputLicense": "CC-BY-4.0"
}
```

---

## Generated task index

> Auto-generated by the Elyos task-decomposition agent on 2026-06-29.
> All 27 files in `tasks/` are schema-valid (validated against `packages/schema/src/schemas.ts`).
> Seed task `open-spending-explainers-editorial-001` was pre-existing; 26 tasks generated from this backlog.
> Fan-out dimensions: none (no explicitly enumerated fan-out axes found in TASKS.md).

| File | Milestone | Type | Lane | Priority | Risk | Deliverable | tokenEstimate |
|---|---|---|---|---|---|---|---|
| [open-spending-explainers-jurisdiction-000.json](tasks/open-spending-explainers-jurisdiction-000.json) | M0 | research | donated | high | medium | document | small |
| [open-spending-explainers-editorial-001.json](tasks/open-spending-explainers-editorial-001.json) | M0 (seed) | design-spec | donated | high | medium | document | medium |
| [open-spending-explainers-repo-002.json](tasks/open-spending-explainers-repo-002.json) | M0 | code | donated | high | low | pr | small |
| [open-spending-explainers-neutrality-003.json](tasks/open-spending-explainers-neutrality-003.json) | M0 | code | donated | high | medium | pr | medium |
| [open-spending-explainers-license-004.json](tasks/open-spending-explainers-license-004.json) | M0 | code | donated | high | medium | pr | medium |
| [open-spending-explainers-schema-005.json](tasks/open-spending-explainers-schema-005.json) | M1 | design-spec | donated | high | low | document | medium |
| [open-spending-explainers-ingest-006.json](tasks/open-spending-explainers-ingest-006.json) | M1 | code | donated | high | medium | pr | large |
| [open-spending-explainers-reconcile-007.json](tasks/open-spending-explainers-reconcile-007.json) | M1 | code | donated | high | medium | pr | medium |
| [open-spending-explainers-pii-008.json](tasks/open-spending-explainers-pii-008.json) | M1 | code | donated | medium | medium | pr | small |
| [open-spending-explainers-eval-029.json](tasks/open-spending-explainers-eval-029.json) | M1 | code | donated | high | medium | pr | small |
| [open-spending-explainers-sources-009.json](tasks/open-spending-explainers-sources-009.json) | M2 | data | donated | medium | medium | dataset | medium |
| [open-spending-explainers-template-010.json](tasks/open-spending-explainers-template-010.json) | M2 | code | donated | medium | medium | pr | large |
| [open-spending-explainers-content-011.json](tasks/open-spending-explainers-content-011.json) | M2 | writing | donated | medium | medium | document | medium |
| [open-spending-explainers-contracts-012.json](tasks/open-spending-explainers-contracts-012.json) | M3 | code | donated | medium | medium | pr | large |
| [open-spending-explainers-context-013.json](tasks/open-spending-explainers-context-013.json) | M3 | code | donated | medium | medium | pr | medium |
| [open-spending-explainers-publish-014.json](tasks/open-spending-explainers-publish-014.json) | M3 | code | donated | medium | low | pr | medium |
| [open-spending-explainers-eval-015.json](tasks/open-spending-explainers-eval-015.json) | M4 | code | donated | medium | medium | pr | medium |
| [open-spending-explainers-hardening-016.json](tasks/open-spending-explainers-hardening-016.json) | M4 | code | donated | medium | medium | pr | medium |
| [open-spending-explainers-partner-017.json](tasks/open-spending-explainers-partner-017.json) | M5 | research | donated | medium | medium | document | medium |
| [open-spending-explainers-handoff-018.json](tasks/open-spending-explainers-handoff-018.json) | M5 | maintenance | donated | medium | medium | document | medium |
| [open-spending-explainers-ops-019.json](tasks/open-spending-explainers-ops-019.json) | M6 | maintenance | donated | low | low | document | medium |
| [open-spending-explainers-jurisdiction-020.json](tasks/open-spending-explainers-jurisdiction-020.json) | Backlog | data | donated | low | medium | dataset | large |
| [open-spending-explainers-i18n-021.json](tasks/open-spending-explainers-i18n-021.json) | Backlog | writing | donated | low | medium | translation | medium |
| [open-spending-explainers-widgets-022.json](tasks/open-spending-explainers-widgets-022.json) | Backlog | code | donated | low | low | pr | medium |
| [open-spending-explainers-literacy-023.json](tasks/open-spending-explainers-literacy-023.json) | Backlog | writing | donated | low | low | document | small |
| [open-spending-explainers-expert-review-024.json](tasks/open-spending-explainers-expert-review-024.json) | Backlog | research | **funded** | low | medium | document | small |
| [open-spending-explainers-comparability-025.json](tasks/open-spending-explainers-comparability-025.json) | Backlog | code | donated | low | medium | pr | medium |
