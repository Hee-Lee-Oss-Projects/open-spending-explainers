# Open-Spending-Explainers — PLAN.md

> Status: Draft · Version: 0.1.0 · Last updated: 2026-06-28 · Owner: TBD (maintainer) · Lane: donated

Plain-language, source-cited explainers of **government budgets and public contracts**, one
jurisdiction at a time, built **only** from open/public-domain/openly-licensed data. The product is
a small, trustworthy pipeline plus a growing library of explainers that turn a 400-page appropriation
PDF or a wall of contract rows into something a resident, journalist, student, or council newcomer
can actually understand — **describing what the public money data shows, never arguing whether it is
good or bad.**

> **Positioning:** the neutral, numerically-faithful "read-me-first" layer over public budgets and
> contracts. Not a watchdog, not an advocate, not a scorecard — a careful explainer that always shows
> its sources and reconciles to the cent.

### What this project is — and is not

- **It is** a license-verified, reproducible pipeline (open data in → normalized, provenance-tracked
  figures → human-and-expert-reviewed plain-language explainers + a machine-readable companion
  dataset out) and the library of explainers it produces.
- **It is not** an LLM wrapper that "summarizes a budget." Every figure is reconciled to a pinned
  source snapshot, every claim is cited to a line item, and every explainer passes a non-partisan
  neutrality screen and (for the pilot jurisdiction) a public-finance expert before it ships. The LLM
  drafts; the **provenance, reconciliation, and neutrality gates are the product.**

Constraints are the identity here. A budget explainer that editorializes is worse than none — it
launders opinion as fact under a veneer of neutrality. So the **non-partisan editorial policy and the
license/provenance gate are the first build items**, tested in CI, not a disclaimer in the footer.

---

## Executive summary

Public budgets and contracts are, in principle, open. In practice they are inaccessible: hundreds of
pages of fund-accounting tables, inconsistent fiscal years, nominal-vs-real confusion, and contract
dumps with no narrative. The people most affected — residents, local journalists with no data desk,
students, and newly-elected officials with no staff — cannot read them. The gap is not transparency;
the data is published. The gap is **comprehension**.

Open-Spending-Explainers closes that gap with a deliberately narrow, trustworthy artifact: per-topic,
per-jurisdiction plain-language explainers generated from open data, **each one reconciled to the
source to the cent, cited to specific line items, and screened for non-partisan neutrality.** A
companion machine-readable dataset (Frictionless Tabular Data Package) ships alongside each explainer
so others can verify and reuse the numbers.

Two design facts dominate everything else:

1. **Neutrality is a safety property, not a tone.** Budgets are inherently political; an explainer
   that calls spending "bloated," frames a cut as a "raid," or implies a vendor did wrong has caused
   harm even if every number is correct. The project enforces a **non-partisan editorial policy** at
   draft, lint (CI), and human-review layers, and treats an authenticated contributor pushing
   editorialized framing as a first-class adversary (see *Security & privacy*).

2. **Numeric fidelity is non-negotiable.** A wrong figure in a budget explainer is actively harmful
   and erodes the trust the project exists to build. Every figure must resolve to a citation that
   **reconciles to a pinned, checksummed source snapshot**; a figure that does not reconcile fails CI
   and cannot ship.

This is a **MEDIUM risk-tier** project (`docs/good-deed-definition.md`): it needs domain accuracy
(public-finance / governmental accounting) and strict non-partisanship for civic content, but it
gives **no medical/legal/financial advice** and makes **no allegations** — it describes open data.
Public-finance/neutrality review is required before content ships; pure pipeline/infra work is
low–medium.

**Honesty note: no partner, newsroom, civic-tech org, or pilot audience is yet secured.** Every
delivery-dependent task is marked `TO BE SECURED` with `verifiedNeed: false`. The project is not
"shipped" on publish; it is shipped only when real people in a real jurisdiction use the explainers
to understand public money, with neutrality and numeric fidelity independently verified.

**Partner-acquisition plan (dated) and build-vs-mothball/pivot decision rule.** Outreach is
time-boxed against the build, not open-ended: by **2026-08-31** the pilot jurisdiction is selected
(criteria in *Open questions*) and ≥ 3 candidate partners (a city/county open-data office, a local
newsroom data desk, a public library, an Open Knowledge / OpenSpending-style civic-tech org, or a
public-policy program) are in active conversation; by **2026-10-31** a public-finance expert reviewer
is secured (M2 gate); by **2026-12-31** a steward/distribution partner is secured (M5 gate).
**Decision rule:** if no public-finance expert is secured by the M2 entry date, M2 content work does
not start and the project holds at M1 (pipeline + gates). If no distribution partner/audience is
secured by **2027-03-31** (≈ M4 completion), the project does **not** publish into a void: it either
(a) **pivots** the last-mile beneficiary — e.g., hand the pipeline + one illustrative jurisdiction's
explainers to a journalism school or public-library data-literacy program as a teaching/reference
deed — or (b) **mothballs** (archived, maintenance-only) with the decision recorded in governance.
The illustrative-jurisdiction explainers remain a useful reference artifact either way.

---

## Problem & beneficiaries

**Who is helped (directly):**
- **Residents and taxpayers** who want to understand where local public money comes from and goes.
- **Local journalists and small newsrooms** with no data desk, who need a verified, neutral starting
  point and a reusable dataset.
- **Students and educators** (civics, public policy, data literacy).
- **Newly-elected/appointed local officials and board members** with no staff, who must vote on
  budgets they cannot yet read.
- **Civic-tech and open-data communities** who get a reusable, license-clean pipeline and datasets.

**Who is helped (ultimately):** the public, through better-informed civic participation and a higher
baseline of fiscal literacy around how public money is raised and spent — independent of any partisan
position on whether it *should* be spent that way.

**The need.** Budget and contract data is published but unreadable: governmental fund accounting,
appropriation-vs-actual distinctions, nominal-vs-inflation-adjusted figures, inconsistent fiscal
years, and contract data with no narrative. Existing tools either (a) present raw data with no
explanation, or (b) come from advocacy groups with an explicit slant. There is a gap for a **neutral,
numerically-faithful, plain-language** explainer that simply makes the open data legible.

**Verified need / partner: TO BE SECURED.** No specific newsroom, library, open-data office, civic
org, or audience has yet agreed to adopt, co-develop, or distribute the explainers. Target partner
profiles to pursue: a municipal/county open-data office; a local newsroom data desk or journalism
school; a public library system running data-literacy programming; an Open Knowledge Foundation /
OpenSpending-style civic-tech organization; or a university public-administration / public-finance
program (which can also supply credentialed review). Until one is secured, the project builds the
agent-neutral pipeline, the editorial and license gates, and **one illustrative jurisdiction's**
explainers for review, and marks all delivery/adoption work `TO BE SECURED` with `verifiedNeed:
false`. Outreach is dated (see executive summary), and a build-vs-mothball/pivot decision rule
governs slippage rather than publishing to no one.

---

## Goals and non-goals

**Goals**

- Build an open-source, reproducible pipeline that turns open budget/contract data into plain-language
  explainers + machine-readable companion datasets, with provenance and license verified per source.
- Ship a **non-partisan editorial policy** that *provably* keeps explainers neutral (describe, don't
  evaluate), enforced by a neutrality lint in CI and human review — not a disclaimer.
- Guarantee **numeric fidelity**: every figure reconciles to a pinned, checksummed source snapshot and
  every claim cites a specific line item; non-reconciling figures fail the build.
- Publish depth-first one pilot jurisdiction's budget (and then contracts) explainers, public-finance
  expert-reviewed, with a reusable Tabular Data Package.
- Prove (via an eval) that grounded, cited, neutral explainers beat a blank-slate "summarize this
  budget" baseline on accuracy, citation coverage, and neutrality.
- Make explainers genuinely readable: plain-language reading-level target, WCAG 2.2 AA, numeracy aids.

**Non-goals (constraints as identity)**

- **Not advocacy or a watchdog.** No claims that spending is wasteful/efficient, fair/unfair, a
  raid/giveaway; no calls to action; no "the government should…"; no implied wrongdoing by any
  official or vendor.
- **Not a scorecard or ranking** of officials, parties, agencies, or vendors.
- **Not financial, legal, or investment advice** of any kind.
- **Not an accusation engine.** Naming a vendor or amount from open data is fine; implying corruption,
  fraud, or impropriety is out — that is an allegation requiring evidence and process this project
  does not have.
- **Not election material.** No content tied to a campaign, ballot measure, candidate, or party; no
  publishing timed or framed to influence a specific vote.
- **Not a comprehensive financial audit, forecast, or "true cost" model** — it explains what the
  published data says, with its limits stated, not a reconstructed ground truth.
- **Not a scraper of proprietary databases.** Open/PD/openly-licensed sources only (see *Data,
  licensing & compliance*).
- **Not a personal-data product.** Entity-level/aggregate figures only; no profiling of private
  individuals.

When a request or draft strays into the non-goal set, the pipeline flags it: the neutrality lint
fails the build on editorializing language, and the human/expert review rejects evaluative framing.

---

## Success metrics (outcomes)

Outcome-centric and beneficiary-first. Page views / DAU are explicitly **not** success.

| Metric | Baseline | Target (pilot) | How measured |
|---|---|---|---|
| Numeric reconciliation of shipped figures to source snapshot | none | **100%** of figures reconcile within the disclosed rounding tolerance; **headline totals exact**; 0 unreconciled figures shipped | Automated reconciliation test in CI vs. checksummed snapshot |
| Citation coverage (every claim → resolvable line-item source) | none | **100%** of quantitative claims carry a resolvable citation; 0 uncited figures | Citation-coverage test ("no source, no figure") |
| Neutrality screen pass rate on the editorial red-team suite | none | reported as **passed/total at version N** with a per-version changelog; **≥ 8 cases per banned-framing category**; 100% pass/redirect | Automated neutrality lint in CI + independent neutrality reviewer audit |
| New editorializing patterns converted to regression cases | n/a | 100% within **1 release** of discovery; suite size non-decreasing | Neutrality red-team changelog diff; CI suite-count check |
| Public-finance expert sign-off before content ships | n/a | 100% of shipped jurisdiction content has recorded expert sign-off, scoped to a dataset snapshot/checksum | Reviewers ledger / governance log |
| Source license verified before ingest | n/a | 100% of ingested sources have a recorded, verified open/PD/CC/open-gov license; 0 unverified sources used | License-gate test; provenance manifest audit |
| Reproducibility | n/a | 100% of explainers regenerate identically from `{snapshot checksum + template version}` | Reproducibility test (re-run → byte/figure match) |
| Stale-content containment | n/a | 0 explainers served as current past their fiscal-year `validUntil` without a superseded/auto-flag banner | Staleness test vs. `fiscalYear`/`validUntil` |
| Readability | n/a | pilot explainers meet the plain-language reading-level target and WCAG 2.2 AA | Readability scorer + accessibility audit |
| Grounded+cited+neutral vs. blank-slate quality delta (eval) | n/a | grounded clearly beats blank-slate on accuracy + citation + neutrality | LLM-judge eval harness |
| **Beneficiary outcome (Definition of Shipped)** | 0 | a real partner/audience in the pilot jurisdiction **uses/publishes** the explainers; ≥ 1 documented instance of a resident/journalist/official using one to understand the budget | Partner attestation + outcome log |
| Corrections handled transparently | n/a | 100% of figure corrections logged in a public erratum changelog within the correction SLA | Corrections log audit |

The **defining success outcome**: real people in a real jurisdiction understand their public budget/
contracts better because of a neutral, numerically-faithful explainer — verified for neutrality and
fidelity — not merely that a document was published.

---

## Scope

**In scope**

- Non-partisan editorial / neutrality policy layer (banned-framing taxonomy, describe-don't-evaluate
  rules, fiscal-context requirements) enforced at draft, lint (CI), and human-review stages.
- License/provenance gate: per-source license verification + provenance manifest, run before ingest.
- Reproducible ingestion + normalization pipeline for **budgets** (OpenSpending/CSV/open portals) and
  **contracts** (Open Contracting Data Standard, OCDS), pinning checksummed source snapshots.
- A normalized data model + Frictionless **Tabular Data Package** companion dataset per explainer.
- Numeric-fidelity / reconciliation engine ("every figure reconciles to source; every claim cites a
  line item").
- Fiscal-context layer: nominal-vs-real (named deflator + base year), per-capita, fiscal-year
  normalization, budgeted-vs-actual distinction, fund-accounting framing, and **comparison
  guardrails** that block misleading cross-jurisdiction/cross-year comparisons.
- Grounded explainer generation (LLM drafts from normalized data + template, cited to line items),
  with neutrality lint and reconciliation as release gates.
- PII screen: entity-vs-person classification; aggregate/entity-level only; redact/avoid private
  individuals' personal data.
- Static publishing site + machine-readable dataset export; corrections/erratum policy + public
  changelog; eval harness; accessibility (WCAG 2.2 AA) + plain-language.
- One pilot jurisdiction's budget and contracts explainers, expert-reviewed.

**Out of scope (explicitly will NOT do)**

- Any evaluative, advocacy, campaign, or ballot-measure content; recommendations; calls to action.
- Allegations of fraud/waste/corruption/impropriety against any official, agency, party, or vendor.
- Scorecards, rankings, or "performance" framing of officials/parties/agencies/vendors.
- Financial/legal/investment advice; forecasts presented as fact; a reconstructed "true cost" audit.
- Scraping proprietary or access-controlled databases, or using data whose license forbids
  reuse/derivatives.
- Profiling, targeting, or publishing personal data of private individuals; any voter-data linkage.
- A 50-jurisdiction database at launch — depth and verified accuracy for one jurisdiction first.

When a draft falls into the out-of-scope set, the neutrality lint fails the build and review rejects
it; where a user *question* strays into evaluation ("is this wasteful?"), the explainer reframes to
what the data shows and states its limits, rather than answering the evaluative question.

---

## Solution approach & architecture

**Stack.** TypeScript, ESM, pnpm workspaces (Hee-Lee Oss convention). Node data pipeline; DuckDB for
in-process tabular reconciliation/aggregation; Frictionless **Tabular Data Package** for the
machine-readable companion dataset. Static publishing via a static-site generator (Astro/Eleventy)
for accessible, fast, offline-friendly explainers. Anthropic Claude as the drafting layer **behind a
thin provider-neutral LLM client** so the agent-neutral core stays vendor-neutral (Hee-Lee Oss core/adapter
rule; model selection + pricing per the Claude API skill). Code license **MIT**; content + datasets
**CC-BY-4.0** (with **ODbL** isolation where any OSM-derived geodata is used — see *Data, licensing*).

**Components**

1. **Editorial / neutrality policy layer (`lib/editorial`) — built first.** Not a prompt suffix; a
   tested gate with three enforcement points:
   - *Draft-time system policy*: a `NEUTRALITY_SYSTEM` charter injected into every generation prompt —
     describe-don't-evaluate, no advocacy/calls-to-action, no allegations, state data limits, name the
     fiscal context (FY, nominal/real, budgeted/actual, fund).
   - *Neutrality lint (CI)*: a deterministic screen over generated text — a **banned-framing lexicon**
     (e.g., "wasteful," "bloated," "raid," "handout," "squandered," "slush," "out of control"),
     loaded-comparative and causal-blame detection, call-to-action detection, and an allegation
     detector (impropriety/illegality framing). A failure **fails the build**. Fail-closed: ambiguous
     framing is flagged for human review, not auto-passed.
   - *Human/expert review*: an independent neutrality reviewer (and, for the pilot jurisdiction, a
     public-finance expert) signs off before publish. Ships with its own editorial red-team suite in
     CI; a regression that lets editorializing through fails the build.

2. **License/provenance gate (`lib/provenance`) — built first.** No source is ingested until its
   reuse terms are verified and recorded. Each `Source` carries: publisher, jurisdiction, dataset
   name, URL, **retrieval date, snapshot checksum (sha256), license/legal-status, and a
   permits-derivatives flag.** Ingest is **fail-closed**: a source without a verified open/PD/CC/
   open-gov license is rejected. OSM/ODbL-derived data is tagged and isolated (share-alike).

3. **Ingestion + normalization (`lib/ingest`).** Pulls open budget data (OpenSpending, government
   open-data portals, CSV/Excel/JSON) and contract data (OCDS releases). Normalizes to the data model
   (below), records provenance, and **pins a checksummed snapshot** so every downstream artifact is
   reproducible. Where helpful and valid, maps functional categories to **COFOG** (UN Classification
   of the Functions of Government) for principled comparability.

4. **Numeric-fidelity / reconciliation engine (`lib/reconcile`).** Every figure that appears in an
   explainer must (a) resolve to a citation pointing at a snapshot line item, and (b) **reconcile**:
   component figures sum to stated totals within a **disclosed rounding tolerance**, with **headline
   totals exact**. Non-reconciling figures fail CI. Distinguishes **budgeted vs actual vs adjusted**
   and never conflates appropriation with expenditure.

5. **Fiscal-context layer (`lib/context`).** Applies nominal→real conversion (named deflator + base
   year recorded as provenance), per-capita normalization, fiscal-year alignment, and **comparison
   guardrails**: cross-jurisdiction or cross-year comparisons are blocked or annotated when accounting
   bases differ (e.g., GASB vs IPSAS), fiscal years misalign, or nominal/real are mixed.

6. **PII screen (`lib/pii`).** Classifies contract parties as **entity vs natural person**; default is
   entity/aggregate-level only. Personal data of private individuals (e.g., a sole proprietor's home
   address, personal identifiers) is redacted/withheld; the project surfaces aggregate or
   entity-level facts, consistent with Hee-Lee Oss's privacy guardrail (aggregate/de-identified; deceased or
   non-personal only).

7. **Explainer generator (`lib/explainer`).** LLM drafts a plain-language explainer from the
   normalized snapshot + a topic template, emitting inline citations to line items. Output must pass
   the neutrality lint and reconciliation engine before review. Templates enforce structure: what this
   is, where the money comes from, where it goes, the fiscal context and caveats, "how to read this,"
   and "verify it yourself" (links to the companion dataset).

8. **Publishing (`site/`) + companion dataset.** Static site (accessible, fast) + a Tabular Data
   Package per explainer so anyone can reproduce/verify. Corrections/erratum policy with a public
   changelog.

9. **Eval harness (`scripts/eval-explainer.ts`).** Runs grounded+cited+neutral generation vs. a
   blank-slate "summarize this budget" baseline; an LLM judge scores accuracy, citation coverage, and
   neutrality. The headline metric is the delta — if grounded+cited+neutral does not clearly beat the
   baseline, the thesis is in doubt.

**Data model (normalized)**

- **Source** — `{ id, publisher, jurisdiction, datasetName, url, retrievalDate, snapshotChecksum,
  license, permitsDerivatives, legalStatusNote }`.
- **BudgetLine** — `{ sourceId, jurisdiction, fiscalYear, fund, function (COFOG optional), program,
  kind: revenue|expenditure, basis: budgeted|actual|adjusted, amountNominal, currency,
  amountReal?, deflator?, baseYear? }`.
- **Contract** — OCDS-aligned: `{ ocid, sourceId, jurisdiction, buyer, supplierEntity,
  supplierIsEntity: bool, value, currency, awardDate, category (CPV/COFOG optional), status }`.
- **Explainer** — `{ jurisdiction, fiscalYear, topic, body, citations[], snapshotChecksum,
  templateVersion, reviewStatus, expertSignoffRef, validUntil }`.

**Key decisions**

- The neutrality policy and license/provenance gate are first-class, tested subsystems and **release
  gates** — built first, not documented later.
- **No figure without a resolvable, reconciling citation**; reconciliation runs in CI.
- **Reproducibility by construction**: pinned, checksummed snapshots + versioned templates → every
  explainer regenerable and independently verifiable.
- **Depth-first** on one jurisdiction with public-finance expert sign-off before any breadth.
- Agent-neutral core; Anthropic/Claude specifics sit behind the LLM client.
- Open/PD/CC/open-gov sources only; ODbL share-alike honored and isolated; no proprietary scraping.

---

## Data, licensing & compliance

**THIS IS CRITICAL — conservative by default.**

**Source material (open only).** Government budget and contract data published under terms that
permit reuse and derivatives: U.S. federal/state/local open-data portals and government works (many
U.S. government works are public domain; **this varies by level and jurisdiction** — state/local
works are not automatically PD); UK data under the **Open Government Licence (OGL)**; EU/other open
government data; OpenSpending datasets; **Open Contracting Data Standard (OCDS)** releases. **License
is verified per source/publisher** — OCDS is a *format*, not a license, so each publisher's terms are
checked individually. Statistical inputs (e.g., a deflator/CPI series, population for per-capita) are
used only from open official sources, with the specific series and base year recorded.

**Hard license rules (the gate):**
- **Open/PD/CC/open-gov-licensed sources ONLY.** A source without a verified reuse-and-derivatives
  license is **not ingested** (fail-closed).
- **No proprietary-database scraping**, no access-controlled/paywalled data, no terms-violating
  collection.
- **OSM/ODbL share-alike:** if OpenStreetMap-derived geodata (boundaries, basemaps) is used, the
  derived geodata inherits **ODbL** and is **isolated** from the CC-BY-4.0 prose/datasets and clearly
  attributed, so share-alike does not contaminate the rest of the corpus. Prefer official open
  government boundary files where available to avoid the entanglement entirely.
- **CC-BY / attribution sources:** attribution preserved on redistribution.

**Provenance model.** Each figure traces to a `Source` record (publisher, citation, URL, **retrieval
date, snapshot sha256 checksum, license, permits-derivatives, legal-status note**). The generator may
not surface a figure without an attached, resolving citation; a citation-coverage test enforces this.
A **provenance manifest** ships with each explainer's companion dataset.

**Staleness is fail-safe.** Budgets are annual and revised; each explainer carries its `fiscalYear`
and a `validUntil`. When a newer fiscal year or a revised dataset supersedes it, the explainer is
**auto-flagged/bannered as superseded** (and links to the current one) rather than silently serving
as current; a staleness test enforces this. Expert sign-off is **scoped to a dataset snapshot/
checksum + template version** and does not carry forward to a new snapshot, which requires
re-review.

**Output licensing.** Code: **MIT**. Content (explainers) + companion datasets + docs: **CC-BY-4.0**
(attribution to primary public sources preserved). OSM-derived geodata, if any: **ODbL**, isolated and
labeled.

**Privacy / PII stance (conservative).** The subject is **public money and public/entity actors**, not
private individuals. Aggregate/entity-level only. Contract data can name **natural persons** (sole
proprietors, named officials); the PII screen classifies entity-vs-person and **redacts/withholds
personal data of private individuals** (personal addresses, personal identifiers). Consistent with
Hee-Lee Oss guardrails, only aggregate/de-identified or non-personal (entity/public-record) data is
surfaced; no profiling, no targeting, no voter-data linkage. **No allegations** about any named person
or entity — the project reports neutral, data-backed facts only, mitigating defamation risk. No
secrets/tokens/PII in logs, receipts, or committed files (Hee-Lee Oss rule).

**Corrections.** A public **erratum policy + changelog**: any figure error is corrected, logged, and
the affected explainer + dataset re-issued within the correction SLA, with the change visible.

---

## Quality, review & risk gates

**Risk tier: MEDIUM** (per `docs/good-deed-definition.md`). Rationale: civic content requiring domain
accuracy (governmental accounting / public finance) and strict non-partisanship — but no
medical/legal/safety advice and no allegations. Pure pipeline/UI/infra tasks are low–medium; any task
that produces or frames jurisdiction budget/contract **content** is medium and gated on neutrality +
public-finance review.

**Required reviews before a deed is "done":**

- **Maintainer review** on all PRs (engineering quality, agent-neutral core, no secrets/PII in logs,
  CI green, license gate respected).
- **Public-finance / governmental-accounting reviewer sign-off** (TO BE SECURED) — recorded before any
  jurisdiction budget/contract content ships; verifies accounting framing (fund accounting,
  budgeted-vs-actual, nominal-vs-real, comparison validity) and that figures reconcile. Sign-off is
  **snapshot/checksum-scoped**. No expert, no content ship.
- **Non-partisan neutrality review** — the editorial red-team suite passes in CI **and** an
  independent neutrality reviewer audits framing before any explainer is published.
- **License/provenance review** — every source's license verified and recorded before ingest;
  provenance manifest complete.

**Every explainer is labeled** with its fiscal year, data sources, snapshot date, and a "this
describes published open data; it is not financial/legal advice and not an audit" note, plus a
"verify it yourself" link to the companion dataset.

**Definition of Shipped (project):** a real partner/audience in the pilot jurisdiction uses or
publishes the explainers and at least one resident/journalist/official demonstrably understands the
budget/contracts better as a result — with neutrality independently verified, all figures reconciled
and cited, all sources license-verified, and public-finance expert sign-off recorded.

---

## Roadmap & milestones

Phased: editorial + license gates and skeleton first; pipeline + reconciliation next; content only
behind the gates; partner adoption last and gated on a secured partner.

- **M0 — Editorial + license gates & skeleton (cold-start).**
  *Goal:* the two safety subsystems (neutrality policy, license/provenance gate) and an agent-neutral
  pipeline skeleton exist before any feature.
  *Exit:* neutrality policy spec + neutrality lint merged with an editorial red-team suite passing in
  CI (no known editorializing bypass); license/provenance gate merged (fail-closed on unverified
  sources); monorepo + CI green; "describes open data, not advice/audit" framing wired in. **Pilot
  jurisdiction selected** (criteria in *Open questions*) so M1+ builds against a real corpus, license
  reality, and reviewer profile.

- **M1 — Pipeline, schema, provenance & numeric fidelity.**
  *Goal:* reproducible ingestion/normalization with reconciliation, on fixtures.
  *Exit:* normalized data model + Tabular Data Package; budget ingestion + checksummed snapshots;
  reconciliation engine (figures reconcile to snapshot; citation-coverage "no source, no figure");
  PII entity-vs-person screen; provenance manifest. **Kill-gate:** a **minimal grounded+neutral eval**
  on a handful of fixtures runs here as an early **go/no-go** — if grounded+cited+neutral does not at
  least trend ahead of the blank-slate baseline, content investment (M2) pauses pending review.

- **M2 — Pilot budget explainers (expert-gated content).**
  *Goal:* the pilot jurisdiction's budget explainers, behind the gates, cited and reconciled.
  *Exit:* pilot budget source vetted (license + provenance) and snapshotted; explainer template +
  grounded generation; pilot budget explainer content drafted, **reconciled, cited, neutrality-passed,
  and public-finance expert-signed-off**. **Kill-gate confirmed** on real cited content.

- **M3 — Contracts explainers + fiscal context + publishing.**
  *Goal:* OCDS contracts module, the fiscal-context/comparison layer, and accessible publishing.
  *Exit:* contracts ingestion + explainer module (entity-level, PII-screened); fiscal-context layer
  (nominal/real, per-capita, FY normalization) + comparison guardrails; static publishing site +
  companion dataset export (CC-BY); corrections/erratum changelog live — all behind the gates.

- **M4 — Eval, hardening & partner readiness.**
  *Goal:* prove the thesis and harden for real readers.
  *Exit:* full eval shows grounded+cited+neutral clearly beats blank-slate; expanded neutrality
  red-team green; WCAG 2.2 AA + plain-language reading-level target met; reconciliation/reproducibility
  verified end-to-end; partner onboarding/handoff runbook ready.

- **M5 — Partner adoption & handoff (the deed).**
  *Goal:* a real partner/audience adopts and benefits.
  *Exit (Definition of Shipped):* a secured partner uses/publishes the explainers; ≥ 1 documented
  beneficiary use; neutrality independently audited; figures reconciled/cited; sources license-verified;
  expert sign-off recorded. *(Gated on a secured partner — TO BE SECURED.)*

- **M6 — Sustain & scale (post-delivery).**
  *Goal:* durable maintenance and (only then) a careful second jurisdiction.
  *Exit:* maintenance rotation + ops runbook + outcome tracking; annual fiscal-year refresh cadence
  (snapshot + re-sign-off); documented, expert-gated process for a second jurisdiction.

Dependencies flow M0 → M1 → M2 → M3 → M4 → M5 → M6. The **pilot-jurisdiction decision is made in M0
and gates M2–M6** (it fixes the source corpus, the source-reuse legality, and the reviewer profile);
M2 content blocks on M0 gates, the chosen jurisdiction, and the secured public-finance expert; M5
blocks on a secured distribution partner/audience.

---

## Work breakdown

The itemized, schema-mapped backlog lives in **`TASKS.md`**: ~20 tasks across M0–M6 plus a future
backlog, each mapped to the Hee-Lee Oss Task JSON schema, with per-task acceptance criteria for the most
important items, milestone Definitions of Done, and a complete example Task JSON for the first M0
hard-requirement task (the non-partisan editorial policy spec). The first build items are the
**non-partisan editorial policy** and the **license/provenance gate**, reflecting their status as hard
product requirements; an early **pilot-jurisdiction selection** and a **minimal grounded+neutral
kill-gate eval** at M1 are sequenced so content investment is gated on both a chosen corpus and an
early thesis check.

---

## Governance, roles & stakeholders

- **Maintainer (Owner): TBD.** Owns architecture, the agent-neutral core, the gates, CI, and merge
  quality.
- **Reviewers / rotation:** at least one engineering reviewer plus a designated **neutrality reviewer**
  who audits framing independently of content authors.
- **Public-finance / governmental-accounting expert reviewer (MEDIUM tier): TO BE SECURED** — a public
  finance professional, governmental accountant, or public-administration academic; signs off
  jurisdiction budget/contract content (accounting framing + reconciliation) before it ships, **scoped
  to a snapshot/checksum**. Tracked in a reviewers ledger with credentials and consent.
  **Independence / COI:** a reviewer recuses from a jurisdiction where they hold/seek office or have a
  material financial interest; name-use is limited to the versions they approved (no implied
  endorsement of unreviewed content); **disagreement fallback** — the expert holds a veto on whether
  content is accurate/fair to ship, escalated to Hee-Lee Oss governance / a second reviewer on a tie.
- **Steward (last-mile owner): TO BE SECURED** — owns the partner relationship and the
  distribution/adoption that constitutes shipping.
- **Partner / requestor: TO BE SECURED** — newsroom/data desk, library, open-data office, civic-tech
  org, or public-policy program.
- **Community / board:** edge-cases and any license/scope decisions go through Hee-Lee Oss governance.

---

## Dependencies & integrations

- **External services:** Anthropic Claude API (drafting, behind the neutral LLM client); managed
  static hosting; CI.
- **Datasets / sources:** the pilot jurisdiction's open budget data (open portals / OpenSpending /
  CSV), OCDS contract releases, and official open statistical series (deflator/CPI, population) — each
  with verified reuse terms and recorded, checksummed provenance.
- **Standards / upstream:** Open Contracting Data Standard (OCDS); Frictionless Tabular Data Package;
  COFOG functional classification; OpenSpending; Open Government Licence (where applicable);
  OpenStreetMap/ODbL (only if geodata is needed, isolated).
- **Hee-Lee Oss pieces:** `packages/schema` (Task JSON), `CLAUDE.md` work rules + refusal guardrails,
  `docs/good-deed-definition.md` (risk tiers), Hee-Lee Oss governance for license/edge-case decisions; house
  style from `planning/projects/public-official-guide/{PLAN,TASKS}.md`.
- **Human/decision dependencies (critical path):** the **pilot-jurisdiction decision (M0, gates
  M2–M6)** — fixes corpus, source-reuse legality, and reviewer profile; a secured public-finance
  expert (blocks M2 content); a secured distribution partner/steward (blocks M5).

---

## Risks & mitigations

| Risk | Likelihood | Impact | Mitigation | Owner |
|---|---|---|---|---|
| Explainer editorializes / is perceived as partisan | High | Critical | Neutrality policy built first; `NEUTRALITY_SYSTEM` charter + banned-framing lint in CI (fails build) + independent neutrality reviewer; describe-don't-evaluate rule; no calls-to-action/allegations | Neutrality reviewer |
| Wrong figure shipped (number doesn't match source) | Medium | Critical | Reconciliation engine (figures reconcile to checksummed snapshot; headline totals exact) fails CI; citation-coverage "no source, no figure"; corrections/erratum changelog | Maintainer |
| Misleading framing while every number is "correct" (nominal/real, budgeted/actual, bad comparison) | High | High | Fiscal-context layer (named deflator+base year, per-capita, FY alignment, budgeted-vs-actual); comparison guardrails block cross-basis/year comparisons; expert review of framing | Expert reviewer |
| Source license forbids reuse/derivatives; proprietary data slips in | Medium | High | Fail-closed license/provenance gate (no verified license → no ingest); per-publisher OCDS license check; no proprietary scraping; ODbL isolation | Maintainer / Expert |
| Personal data of private individuals exposed via contracts | Medium | High | PII entity-vs-person screen; redact/withhold private-individual personal data; aggregate/entity-level only; no profiling | Maintainer |
| Defamation / implied-wrongdoing risk on a named vendor/official | Medium | High | No-allegations rule (data-backed neutral facts only); allegation detector in neutrality lint; expert + neutrality review | Neutrality reviewer / Expert |
| Content goes stale (new fiscal year / revised data) and silently misleads | High | Medium | Per-explainer `fiscalYear`/`validUntil`; superseded auto-banner; snapshot-scoped sign-off; annual refresh cadence | Maintainer |
| No partner/audience secured → cannot reach Definition of Shipped | High | High | Honest `TO BE SECURED`/`verifiedNeed:false`; **dated partner-acquisition plan** + **build-vs-mothball/pivot decision rule** (~2027-03-31: pivot to a journalism-school/library teaching deed or mothball); build pipeline + illustrative jurisdiction meanwhile | Steward / Maintainer |
| No public-finance expert secured → M2 content blocked | Medium | High | Recruit via public-administration programs, GFOA-type bodies, accounting faculties; hard gate (no expert, no content) | Maintainer |
| LLM hallucinates figures/citations | Medium | High | Grounded generation only; reconciliation + citation-coverage gates; eval harness; reproducibility from snapshot | Maintainer |
| Misused to attack/embarrass an official or party near an election | Medium | High | No election/ballot/campaign content; no scorecards; neutrality lint + review; publish-timing policy avoids campaign framing | Neutrality reviewer |

---

## Security & privacy

**Threat surface.** Editorialized/partisan output (primary integrity threat); shipping an incorrect
figure; exposure of private-individual personal data via contract records; defamation via implied
wrongdoing; ingesting license-violating or proprietary data; LLM hallucination; secrets leakage.

**Insider-misuse threat model (the primary adversary is an authenticated contributor with an agenda).**
Like the public-official-guide, the most likely adversary is not an outsider but a contributor who
wants the *output* to lean a particular way. The neutrality suite explicitly models:
- **Framing smuggling** — neutral-sounding sentences that imply a verdict ("the council chose to spend
  X *instead of* Y," selective emphasis, leading juxtaposition); the lint covers loaded comparatives,
  causal-blame, and selective-omission patterns, and review checks balance.
- **Editorializing by citation choice** — cherry-picking line items to imply a narrative; templates
  require complete, structured coverage (revenue + expenditure + context) rather than a curated subset.
- **Allegation-by-implication** — juxtaposing a vendor and an amount to imply impropriety; the
  allegation detector + no-allegations rule + expert review block this.
- **Comparison gaming** — comparing across incompatible accounting bases/years to manufacture a
  "spike"; the comparison guardrails block or annotate these.
These are first-class red-team categories with their own regression cases.

**Controls.** Neutrality + license/provenance gates as tested, CI-enforced subsystems (top controls).
Reconciliation + citation-coverage gates prevent wrong/uncited figures. PII entity-vs-person screen +
redaction of private-individual data. **No secrets/tokens/PII in logs, receipts, or committed files**
(Hee-Lee Oss rule); provenance/refusal logs record metadata, not personal content. Reproducibility from
checksummed snapshots makes every figure independently auditable. Dependency + secret scanning in CI.
The agent-neutral core keeps vendor specifics behind the LLM client.

**Abuse/misuse prevention.** The refused-output set (advocacy, allegations, scorecards, campaign/
ballot content, advice) is enforced by the neutrality lint + review and tested, not merely documented;
the pipeline never publishes autonomously without human + (for content) expert sign-off.

---

## Sustainability & maintenance

After delivery, a named **maintenance rotation** owns the pipeline; the **steward** owns the partner
relationship and outcome tracking. Budget/contract data is annual, so content carries a **refresh
cadence**: each fiscal year a new snapshot is ingested, reconciled, re-reviewed, and the prior
explainer auto-bannered as superseded — enforced at runtime via `fiscalYear`/`validUntil`, not just a
calendar. The neutrality and reconciliation red-team suites are living tests, expanded as new
editorializing or reconciliation-failure patterns are found. Outcomes are tracked (partner/audience
use, documented comprehension instances, corrections issued) — **not** page views. Expansion to a
second jurisdiction follows a documented, expert-gated, license-verified process and only after the
first is stable.

---

## Open questions

- **Which pilot jurisdiction first? — decided in M0, not deferred,** because it gates M2–M6.
  Selection criteria, scored before M1 content: (1) **open, reuse-and-derivatives-licensed** budget +
  OCDS contract data actually available (clean license, machine-readable); (2) **data quality/
  completeness** (reconcilable line items, documented fund structure); (3) an **available
  public-finance reviewer** for that jurisdiction; (4) a **plausible distribution partner/audience**
  (newsroom/library/open-data office); (5) **manageable complexity** (a tractable fund/accounting
  structure for the first build). The specific jurisdiction is TO BE SECURED, but the **decision rule
  and deadline (by 2026-08-31) are fixed**.
- **Rounding-tolerance policy** — exact-match for headline totals; what disclosed tolerance (proposal:
  ±0.5% or last-displayed-digit) for component figures, and how is rounding disclosed in-line?
- **Deflator/base-year choice** for nominal→real — which official open series, and is real-terms
  conversion shown by default or opt-in (to avoid implying a comparison the data can't support)?
- **Cross-jurisdiction comparability** — when, if ever, is COFOG-normalized comparison valid enough to
  publish, vs. always single-jurisdiction to avoid apples-to-oranges?
- **Expert compensation/crediting** — volunteer vs. a future funded lane for review hours (hard budget
  cap) without compromising independence?
- **Contract-PII line** — exact policy for sole-proprietor/named-individual data across jurisdictions
  with differing transparency-vs-privacy law.
- **Publish-timing near elections** — concrete policy/blackout to ensure neutral explainers are not
  framed/timed to influence a specific vote.

---

## References

- Hee-Lee Oss work rules & refusal guardrails: `CLAUDE.md`
- Good-deed definition & risk tiers: `docs/good-deed-definition.md`
- Task JSON schema: `packages/schema/src/schemas.ts`
- Portfolio roadmap: `planning/ROADMAP.md` (Track 2; `open-spending-explainers`, `open-data-explainers`)
- House style (sibling medium/high-risk civic plan): `planning/projects/public-official-guide/{PLAN,TASKS}.md`
- Planning spec: `PLAN_SPEC.md`
- Standards: Open Contracting Data Standard (OCDS); Frictionless Tabular Data Package; UN COFOG;
  OpenSpending; UK Open Government Licence; OpenStreetMap / ODbL
- Edict-of-government / government-works reuse principle: *Georgia v. Public.Resource.Org* (U.S. 2020)

---

## Appendix A — Improvements applied

Twenty-five specific improvements made to the working draft and **applied above** (not deferred):

1. **Disclosed rounding-tolerance policy** added to reconciliation: headline totals must match
   **exactly**, component figures within a **disclosed tolerance** — preventing both false-precision
   and silent rounding (Success metrics, Architecture §4, Open questions).
2. **Comparison guardrails** as a real component: cross-jurisdiction/cross-year comparisons are
   blocked or annotated when accounting bases (GASB vs IPSAS), fiscal years, or nominal/real differ
   (Architecture §5, Risks).
3. **Nominal-vs-real provenance**: the specific deflator series + base year are recorded as provenance,
   not silently applied (Architecture §5, Data model, Open questions).
4. **Budgeted-vs-actual-vs-adjusted** distinction made explicit so appropriations are never conflated
   with expenditures (Architecture §4, Data model).
5. **Fund-accounting framing requirement** in the template so general-vs-restricted-fund totals aren't
   misread as one pot (Scope, Architecture §3/§7).
6. **PII entity-vs-person classifier** added: contracts can name natural persons, so private-individual
   personal data is redacted/withheld by default (Architecture §6, Data/licensing, Security).
7. **Per-publisher OCDS license verification** — OCDS is a format, not a license — replacing any
   blanket "contracts are open" assumption (Data/licensing).
8. **OSM/ODbL share-alike isolation**: any geodata inherits ODbL and is isolated/attributed so
   share-alike doesn't contaminate the CC-BY corpus; prefer official open boundaries (Data/licensing).
9. **Banned-framing lexicon + loaded-comparative/causal-blame/call-to-action/allegation detectors**
   specified for the neutrality lint, beyond a generic "be neutral" (Architecture §1, Security).
10. **Describe-don't-evaluate rule** stated as the core editorial invariant and wired into template +
    lint + review (Positioning, Goals/non-goals, Architecture §1).
11. **Corrections/erratum policy + public changelog** with an SLA added as a first-class artifact
    (Data/licensing, Success metrics, Sustainability).
12. **Staleness fail-safe** for annual data: per-explainer `fiscalYear`/`validUntil` + superseded
    auto-banner instead of silently serving last year's numbers (Data/licensing, Success metrics).
13. **Snapshot/checksum-scoped expert sign-off** that does not carry forward to a new snapshot
    (Data/licensing, Governance).
14. **Dated partner-acquisition plan + build-vs-mothball/pivot decision rule** so the project never
    publishes into a void (Executive summary, Problem, Risks).
15. **Minimal grounded+neutral kill-gate eval at M1** as an early go/no-go before content investment,
    not waiting for the full M4 eval (Roadmap M1, Work breakdown, TASKS eval-029).
16. **Reproducibility-by-construction**: pinned, checksummed snapshots + versioned templates →
    byte/figure-identical regeneration, with a reproducibility metric (Architecture, Success metrics).
17. **Accessibility + plain-language reading-level target** made an explicit, measured gate (WCAG 2.2
    AA + readability scorer), not assumed (Scope, Success metrics, Roadmap M4).
18. **Machine-readable companion dataset (Frictionless Tabular Data Package)** shipped with every
    explainer so readers can verify/reuse the numbers (Architecture §8, "verify it yourself").
19. **Defamation/no-allegations rule** for named vendors/officials: data-backed neutral facts only, no
    implied wrongdoing, with an allegation detector (Goals/non-goals, Data/licensing, Risks, Security).
20. **Insider-misuse threat model** for an authenticated contributor with an agenda — framing
    smuggling, editorializing-by-citation-choice, allegation-by-implication, comparison gaming — as
    first-class red-team categories (Security).
21. **COFOG functional-classification mapping** added for *principled* comparability where valid,
    instead of ad-hoc category matching (Architecture §3, Dependencies).
22. **Definition of Shipped reframed to a beneficiary outcome** (real partner/audience uses it; ≥ 1
    documented comprehension instance), not "an explainer was published" (Success metrics, Quality
    gates, Roadmap M5).
23. **Funded-lane note** even though donated is default: any funded review-hours task must carry
    `fundedBudgetUsd` with a hard cap (Governance, Open questions, TASKS mapping).
24. **Full provenance field set** standardized (publisher, URL, retrieval date, sha256 snapshot,
    license, permits-derivatives, legal-status) and shipped as a provenance manifest (Data/licensing,
    Data model).
25. **Election-timing / no-ballot-content policy** added so neutral explainers cannot be framed or
    timed to influence a specific vote (Goals/non-goals, Risks, Open questions).

---

## Review sign-off

**Completeness.** All 17 required H2 sections from `PLAN_SPEC.md` are present and in order
(Executive summary → References), followed by Appendix A and this sign-off. Ofelia-depth elements are
present: positioning, who-it-is-for, explicit non-goals, locked decisions (stack/licenses/gates-first),
stack, phased roadmap with exit criteria, and constraints-as-identity framing.

**Correctness / guardrail coverage (checked and fixed during review):**
- *License/provenance* — fail-closed gate; open/PD/CC/open-gov only; **no proprietary-DB scraping**;
  per-publisher OCDS verification; **OSM ODbL share-alike isolated**. ✓
- *Privacy/PII* — aggregate/entity-level only; private-individual personal data redacted; no profiling
  or voter-data linkage; consistent with the "deceased/aggregate/non-personal" guardrail. ✓
- *Non-partisan* — describe-don't-evaluate; banned-framing lint + independent neutrality review;
  no allegations, scorecards, advocacy, calls-to-action, or election/ballot content. ✓
- *Expert review* — public-finance/governmental-accounting sign-off required before content ships,
  snapshot-scoped; medium risk tier justified (domain accuracy + neutrality, no high-stakes advice). ✓
- *Partner reality* — no partner/expert/audience secured; all marked **TO BE SECURED** with a dated
  acquisition plan and a build-vs-mothball/pivot rule; `verifiedNeed: false` carried into TASKS. ✓
- *Numeric integrity* — reconciliation + citation-coverage gates; reproducibility from checksummed
  snapshots; corrections changelog. ✓

**Fixes applied in review.** Tightened the Definition of Shipped to a beneficiary outcome; added the
election-timing policy and the no-allegations/defamation control; isolated ODbL from the CC-BY corpus;
made expert sign-off snapshot-scoped to match the staleness model; ensured the example Task JSON in
`TASKS.md` is schema-valid against `packages/schema/src/schemas.ts` (all required fields, valid enums,
`verifiedNeed: false`, real `outputLicense`).

**Residual human decisions** (see *Open questions*): pilot-jurisdiction selection (by 2026-08-31),
rounding tolerance, deflator/base-year, comparability policy, expert compensation, contract-PII line,
and election-timing blackout. None block M0; all are sequenced.

Reviewed against `CLAUDE.md`, `docs/good-deed-definition.md`, `packages/schema/src/schemas.ts`, and the
`public-official-guide` house style. **Status: ready for maintainer + governance review.**
