# Open-Spending-Explainers — Competitive & Improvement Analysis

> Scope: rigorous review of `PLAN.md` (v0.1.0, 2026-06-28) for the Hee-Lee Oss good-deed project
> `open-spending-explainers` — plain-language, source-cited explainers of government budgets and
> public contracts, built only from open fiscal data. Guardrails: strictly non-partisan, sourced to
> official open data, accurate, no editorializing, transparent methodology, per-source license.
> Web-researched; sources cited inline.

---

## Executive summary (250–350 words)

**Top 3 competitors.** (1) **USAFacts** — the closest non-partisan analogue: a Ballmer-funded
non-profit that re-presents government data "free from spin," explicitly refraining from
interpretation ([about](https://usafacts.org/about-usafacts/)). It is national/federal-first,
not local, and its "organize spending by the Preamble" choice is itself an editorial frame we can
avoid. (2) **USAspending.gov** — the authoritative U.S. federal spending portal, but GAO documents
serious data-quality gaps (billions tagged "Unknown/Other," 49 of 152 agencies not reporting, $40B+
unreported OTAs) ([GAO-22-104702](https://www.gao.gov/products/gao-22-104702)) — raw data, no
narrative. (3) **OpenSpending / Where Does My Money Go (OKFN)** — the canonical open-budget platform
and the origin of the Fiscal/Tabular Data Package, but largely in maintenance/"Next" limbo
([OpenSpending](https://www.openspending.org/about); [OKFN blog](https://blog.okfn.org/category/okfn-projects/open-spending/)).
Adjacent: **Open Contracting Partnership/OCDS** (procurement standard, 50+ governments,
[OCP](https://www.open-contracting.org/data-standard/)), OECD/IMF COFOG fiscal data, Ballotpedia
(non-partisan claim contested by funding;
[Wikipedia](https://en.wikipedia.org/wiki/Ballotpedia)).

**Strongest differentiator.** Nobody combines **neutrality-as-a-tested-CI-gate** with
**cent-level reconciliation to a checksummed snapshot** and a **machine-readable companion dataset
per explainer**. Competitors give you data without comprehension (USAspending), comprehension with an
unaudited editorial frame (USAFacts), or platforms without explainers (OpenSpending). The wedge is
the *plain-language layer that provably cannot editorialize and provably reconciles*.

**Top 3 Claude-API ideas.** (a) Grounded explainer drafting from normalized line items with inline
citations and a `NEUTRALITY_SYSTEM` charter; (b) normalization narration — Claude *explains* the
nominal→real / per-capita / budgeted-vs-actual choices the deterministic layer made; (c)
per-jurisdiction scaling: regenerate templated explainers across fund structures and auto-draft
methodology docs and red-team neutrality cases.

**Two most important plan-correctness findings.** (1) The neutrality lint is specified as a
**lexicon + pattern detector**, which catches slurs but not the harder, plan-acknowledged threat
(framing-by-omission, juxtaposition) — the plan must not let a green lint imply "neutral"; human
review must remain load-bearing and the metric must avoid false assurance. (2) The plan leans on
**OpenSpending as an ingest source** without noting it is effectively unmaintained — source-currency
and platform-liveness risk should be an explicit license/provenance criterion, with primary
government portals preferred over OKFN re-hosts.

---

## 1. Correctness & completeness review of PLAN.md

The plan is unusually rigorous for a v0.1 draft: it treats neutrality and license/provenance as
*first-build, CI-tested subsystems* rather than disclaimers, mandates cent-level reconciliation to a
checksummed snapshot, ships a Frictionless companion dataset, and is honest that no partner/expert is
secured (`verifiedNeed: false` everywhere). The constraints-as-identity framing is correct for civic
content. Findings below are gaps and risks, ordered by importance.

**1.1 Non-partisan / no-editorializing boundary — strong but over-trusting the lint.**
The plan's deepest risk is correctly named (framing smuggling, editorializing-by-citation-choice,
allegation-by-implication, comparison gaming — Security §) but the *enforcement* is a deterministic
**banned-framing lexicon + comparative/blame/CTA/allegation detectors**. A lexicon reliably catches
"wasteful/bloated/raid" but **cannot catch neutral-sounding bias**: selective inclusion of line
items, leading juxtaposition, true-but-cherry-picked emphasis, or a technically-correct comparison
that misleads. The plan half-acknowledges this (templates require "complete structured coverage";
human review checks balance), but the **success-metric framing risks false assurance**: a "neutrality
screen pass rate" reported as passed/total can read as "this explainer is neutral" when the lint only
proves "no banned tokens." Recommendation: rename the metric to something like *banned-framing-screen
pass rate*, make **human/expert neutrality sign-off the binding gate** (lint is necessary-not-
sufficient), and add **omission/coverage tests** (does the explainer cover the full revenue+expenditure
structure, not a curated subset?) since omission is the dominant residual bias vector. Consider an
**LLM-judge neutrality critic** as a second screen (it can flag implication/juxtaposition a lexicon
cannot) — used to *flag for human review*, never to auto-pass.

**1.2 Fiscal-data standards — well chosen, with gaps.**
COFOG, Fiscal/Tabular Data Package, and OCDS are the right standards and are correctly characterized
(OCDS is a *format not a license* — per-publisher verification; COFOG "optional mapping where valid").
Gaps: (a) the plan names **COFOG** but not the **IMF GFSM 2014** accounting framework that underlies
internationally comparable COFOG data ([IMF GFSM](https://www.imf.org/en/data/statistics/government-finance-statistics-manual));
for U.S. local data the relevant frameworks are **GASB** (state/local) and the **ACFR/CAFR** fund
structure — the plan mentions GASB vs IPSAS once but should make the **US-local accounting model
(funds, modified accrual, ACFR, appropriation→obligation→outlay)** an explicit normalization concern,
since the pilot is likely U.S. local. (b) **Fiscal Data Package vs generic Tabular Data Package**: the
plan ships a *Tabular* Data Package but OpenSpending's domain-specific **Fiscal Data Package** profile
exists for exactly this data ([spec history](https://github.com/openspending/fiscal-data-package));
adopting/aligning to it (even as a profile over Tabular DP) buys interoperability with existing tools.
(c) **Data Package v2.0** shipped June 2024 ([Frictionless](https://frictionlessdata.io/blog/2024/06/26/datapackage-v2-release/))
— pin the version.

**1.3 Inflation / per-capita normalization — correct stance, needs guardrails sharpened.**
Recording the named deflator + base year as provenance is exactly right and ahead of most
competitors. Two refinements: (a) the plan should specify *which* deflator class (CPI vs GDP deflator
vs a government-specific price index) since the choice materially changes "real" figures and is itself
a contestable editorial decision — default to the most neutral official series and **state the
limitation** rather than pick a "best." (b) Per-capita and real-terms conversions **imply a comparison**;
the plan's instinct to make them opt-in/annotated (Open questions) is correct — codify "no derived
comparison without an explicit caveat block" as a template rule, not just an open question.

**1.4 Misleading-comparison guardrails — present and good.**
Comparison guardrails that block/annotate cross-basis (GASB vs IPSAS), cross-year, and nominal/real
mixing are a genuine strength few competitors implement. Strengthen by enumerating the **specific
invalid-comparison classes** as test fixtures (different fund scopes; gross vs net; consolidated vs
entity; FY misalignment; reorganized agencies year-over-year).

**1.5 Data currency / staleness — strong.**
Per-explainer `fiscalYear`/`validUntil` + superseded auto-banner + snapshot-scoped expert sign-off is
better than most public portals. Missing: a check on **upstream source liveness** (see §1.7) and a
defined cadence for detecting that a portal *revised* a prior-year figure (governments restate).

**1.6 Source-license — rigorous, one real-world gap.**
Fail-closed gate, per-source verification, ODbL isolation, "US gov works PD varies by level" caveat,
and the *Georgia v. Public.Resource.Org* edict-of-government cite are all correct and unusually
careful. Gap: **state/local open-data portal Terms of Use** often add attribution/no-warranty/no-
endorsement clauses on top of the data being "public record" — the gate should capture **ToU text and
an explicit `endorsementRestriction` flag**, not just a license name, because misuse-of-endorsement is
a live risk for a neutral civic product.

**1.7 OpenSpending as a source — currency risk unflagged.**
The plan repeatedly lists **OpenSpending** as an ingest source. Research indicates OpenSpending is
effectively in maintenance/"Open Spending Next" limbo with sparse recent activity
([OKFN forum](https://discuss.okfn.org/t/open-spending-next-update-and-teaser/2663);
[about](https://www.openspending.org/about)). Treat OKFN re-hosts as **secondary** and prefer
**primary government portals** for currency and authority; add platform-liveness to the source-vetting
criteria.

**1.8 Accessibility — specified, under-operationalized.**
WCAG 2.2 AA + plain-language reading-level target + readability scorer is the right bar. Add concrete
items: **data-table accessibility** (charts need accessible tables/alt text — critical for budget viz),
**numeracy aids** for low-numeracy readers (the "Daily Bread / your tax contribution" device from
WDMMG is a proven pattern), and **plain-language at a stated grade level** with the scorer named.

**1.9 Minor completeness notes.** Currency/locale handling (multi-currency, thousands/lakh
separators) for international jurisdictions is implied but unspecified. The eval's **LLM-judge** for
neutrality/accuracy needs its own bias controls (judge ≠ drafter model; rubric pinned) to avoid
circularity. "Reproducible byte-identical regeneration" is ambitious given LLM nondeterminism — clarify
that *figures/citations* must be byte-identical (deterministic layer) while prose may be pinned via
temperature 0 + cached template, or the metric should be figure-level not byte-level.

---

## 2. Competitive landscape

| Competitor | What it is | Strengths | Weaknesses (our opening) |
|---|---|---|---|
| **USAFacts** ([about](https://usafacts.org/about-usafacts/)) | Non-profit, non-partisan re-presentation of US gov data, founded 2017 (Ballmer) | Genuinely non-partisan reputation (Ad Fontes rates it low-bias); polished UX; free, no paywall; clear data sourcing | Federal/national-first, **not local**; "organize by Preamble" is an unacknowledged editorial frame; summaries not reconciled-to-cent or line-item-cited publicly; no per-explainer machine-readable companion |
| **USAspending.gov** ([GAO-22-104702](https://www.gao.gov/products/gao-22-104702), [GAO-24-106214](https://www.gao.gov/products/gao-24-106214)) | Authoritative US federal spending portal (DATA Act 2014) | Authoritative, bulk-downloadable, API; legally mandated | **Documented data-quality gaps** ("Unknown/Other" obligations, 49/152 agencies missing, $40B+ unreported OTAs, 90-day DoD lag, misleading geo-coding); **raw data, zero narrative**; federal only |
| **OpenSpending / WDMMG (OKFN)** ([about](https://www.openspending.org/about), [WDMMG](https://blog.okfn.org/category/okf-projects/wdmmg/)) | Global open-budget platform; origin of Fiscal Data Package + BubbleTree viz | Pioneered the field; standards (Fiscal/Tabular DP); embeddable viz; global datasets | **Maintenance/"Next" limbo**, stale datasets; platform not explainers; uploads not reconciled/expert-reviewed; comprehension gap remains |
| **Open Contracting Partnership / OCDS** ([data-standard](https://www.open-contracting.org/data-standard/), [standard.open-contracting.org](https://standard.open-contracting.org/)) | The international procurement-transparency standard (50+ govts, G7/G20-endorsed) | De-facto contracts standard; validation/flattening tools; strong governance | A **standard + advocacy org**, not a plain-language explainer; OCDS is a format not a license; analysis left to others — exactly our contracts wedge |
| **OECD / IMF fiscal data (COFOG, GFSM 2014)** ([OECD GaaG 2025](https://www.oecd.org/en/publications/2025/06/government-at-a-glance-2025_70e14c6c/full-report/classification-of-the-functions-of-government-cofog_16aa2337.html), [IMF GFSM](https://www.imf.org/en/data/statistics/government-finance-statistics-manual)) | Internationally comparable functional spending classification & framework | Authoritative comparability framework; cross-country; open access | National/aggregate, **not local or resident-facing**; expert audience; no comprehension layer |
| **National/sub-national budget portals** (e.g. data.gov, UK OGL data, openfiscaldata.go.kr) | Government open-data publishing | Primary, authoritative, often well-licensed (OGL) | Inconsistent quality; portal ToU traps; raw; no narrative or normalization |
| **Frictionless / Tabular Data Package** ([v2 release](https://frictionlessdata.io/blog/2024/06/26/datapackage-v2-release/)) | Open data-packaging spec (our companion-dataset format) | Mature (v2.0, 2024), tool ecosystem, FAIR | Infra not product; complements rather than competes |
| **Ballotpedia** ([Wikipedia](https://en.wikipedia.org/wiki/Ballotpedia), [funding critique](https://www.chaoticera.news/p/this-popular-election-resource-is-funded-by-far-right-donors-does-it-matter)) | "Non-partisan" political/election/policy encyclopedia incl. budget ballot measures | Broad local coverage; encyclopedic | **Non-partisan claim contested by right-leaning funder concentration** — a cautionary tale: *neutrality is judged by perception + funding transparency, not just content*; budget coverage is election/measure-centric, not budget-comprehension |

**Read-across:** the field bifurcates into (a) authoritative-but-unreadable raw portals/standards and
(b) readable-but-editorially-unaudited summaries. The plan targets the empty quadrant:
**readable + reconciled + provably neutral + machine-verifiable**, depth-first at the **local** level
where comprehension need is highest and competition thinnest.

---

## 3. Gaps we can fill

1. **Local-government comprehension.** USAFacts/USAspending/OECD are federal/national; the acute
   unmet need is city/county budgets and ACFR fund accounting that residents, new council members, and
   no-data-desk local journalists genuinely cannot read.
2. **Plain-language + reconciliation together.** No incumbent ships explainers where *every figure
   reconciles to the cent against a checksummed snapshot and cites a line item.* That is the trust
   artifact.
3. **Auditable neutrality.** Nobody exposes a *tested* editorial-neutrality gate + red-team suite;
   USAFacts asks for trust, Ballotpedia's is contested. We can make neutrality inspectable.
4. **Per-explainer machine-readable companion + provenance manifest.** "Verify it yourself" with a
   Frictionless package is rare; it converts skeptics into auditors.
5. **Budgets + contracts in one neutral frame.** OCDS-based contract comprehension (entity-level,
   PII-screened, no-allegation) alongside budgets — OCP standardizes, doesn't explain.
6. **Currency/staleness discipline.** Auto-superseding stale explainers fixes a chronic failure of
   static gov dashboards and abandoned civic-tech sites (e.g. OpenSpending).
7. **Reproducibility-by-construction** as a public good: pinned snapshots + versioned templates so any
   third party regenerates identical figures.

---

## 4. Differentiators to win

1. **Neutrality as a safety property, enforced in CI + binding human/expert sign-off** — not a tone, a
   tested gate with a non-decreasing red-team suite. This is the single strongest, hardest-to-copy
   differentiator.
2. **Cent-level reconciliation + "no source, no figure" citation coverage** — a wrong number fails the
   build; this is a level of numeric integrity no competitor advertises.
3. **License/provenance fail-closed gate with full provenance manifest + ODbL isolation** — license-
   clean by construction, defensible for reuse.
4. **Depth-first, local-first** with snapshot-scoped public-finance expert sign-off — credibility per
   jurisdiction over breadth.
5. **Machine-readable companion + reproducibility** — turns readers into verifiers; structurally
   immune to "trust us."
6. **Funding/governance transparency** — explicitly learn from Ballotpedia: publish funder/governance
   and COI/recusal rules so neutrality is credible, not just asserted.

---

## 5. Claude API leverage — and the hard boundaries

**Where Claude adds leverage (drafting/explanation/scaling, never deciding facts):**

1. **Grounded explainer drafting.** Claude turns normalized, reconciled line items + a topic template
   into plain-language prose with **inline citations to line-item IDs**, under the `NEUTRALITY_SYSTEM`
   charter. Numbers are injected from the deterministic layer, never generated.
2. **Normalization narration.** Claude *explains* the choices the deterministic pipeline made —
   "this is nominal, not inflation-adjusted; FY runs July–June; this is the General Fund only" — making
   the fiscal context legible. High-value, low-risk (it narrates recorded provenance).
3. **Per-jurisdiction scaling + methodology docs.** Claude drafts methodology pages, "how to read
   this," glossary entries, and adapts templates across fund structures — accelerating breadth once the
   gates exist. Also drafts **candidate neutrality red-team cases** for human curation.
4. **Neutrality critic (second screen, advisory).** A separate Claude judge flags implication/
   juxtaposition/omission the lexicon lint cannot — output **flags for human review, never auto-passes**.
5. **Plain-language + reading-level rewriting** to hit the accessibility target, and accessible
   alt-text/table summaries for charts.
6. **Eval LLM-judge** (distinct model/rubric) scoring grounded-vs-baseline accuracy/citation/neutrality.

**Where Claude must NOT decide (human/expert + deterministic systems own these):**

- **Numbers come from data, not the LLM.** Every figure originates in the reconciled snapshot;
  reconciliation + citation-coverage gates fail any figure Claude could have invented.
- **Factual accuracy and accounting framing** (fund scope, budgeted vs actual, comparison validity)
  are verified by the **public-finance expert**, snapshot-scoped — not by model confidence.
- **Non-partisan framing is human-verified.** The lint is necessary-not-sufficient; the **independent
  neutrality reviewer's sign-off is binding.** No model judgment ships unaudited.
- **No political editorializing / no allegations / no calls-to-action** — refused by policy + lint +
  review, never a model "judgment call."
- **License/reuse legality is human-verified.** Claude may *summarize* a license; a human records the
  verified `permits-derivatives` + endorsement flags. (Model selection/pricing per the Claude API
  skill; Claude sits behind the agent-neutral LLM client per Hee-Lee Oss core/adapter rule.)

---

## 6. Ten concrete optimizations

1. **Rename the neutrality metric** to "banned-framing-screen pass rate" and make **human/expert
   neutrality sign-off the binding gate**; the lint is necessary-not-sufficient. Prevents false "this
   is neutral" assurance from a green CI.
2. **Add omission/coverage tests** (revenue + expenditure + fund structure fully represented) — omission
   is the dominant residual bias the lexicon cannot catch.
3. **Add an advisory LLM-judge neutrality critic** that flags implication/juxtaposition for human
   review (never auto-pass); distinct model from the drafter.
4. **Adopt/align the Fiscal Data Package profile** (over Tabular DP) and **pin Data Package v2.0** for
   interoperability with the existing OpenSpending/Frictionless tool ecosystem.
5. **Make US-local accounting explicit** (GASB/ACFR funds, modified accrual, appropriation→obligation→
   outlay) in the normalization model — not just GASB-vs-IPSAS in passing.
6. **Capture portal Terms-of-Use + an `endorsementRestriction` flag** in the license gate, not just a
   license name — gov "public record" data often carries no-endorsement/attribution clauses.
7. **Flag source-platform liveness/currency** in source vetting; prefer **primary government portals**
   over OpenSpending/OKFN re-hosts (which are stale).
8. **Specify the deflator class** (default a neutral official series, e.g. GDP deflator/CPI) and make
   real-terms/per-capita **opt-in with a mandatory caveat block** — a derived comparison without a
   caveat fails the template check.
9. **Publish funder/governance + COI/recusal transparency** (the Ballotpedia lesson): credibility of
   neutrality depends on perceived independence and funding transparency.
10. **Harden the eval**: judge model ≠ drafter model, pinned rubric; redefine "reproducibility" as
    **figure/citation byte-identical** (deterministic) with prose pinned via temp-0 + cached template —
    avoids over-claiming byte-identical LLM output.

---

## 7. Parallel & perpendicular spin-offs

**Parallel (same pipeline, new corpus/jurisdiction):**
- **open-data-explainers** — the general sibling: the same grounded-draft + reconciliation + neutrality
  + provenance pattern over *any* open dataset (health, environment, education stats). Spending is the
  hardest case; success generalizes the engine.
- **open-data-datasheets** — emit a **"datasheet for the dataset"** (provenance, license, currency,
  known-gaps, accounting basis) per source; reuse the provenance manifest. Directly complements §1.6/1.7.

**Perpendicular (different deed, shared components):**
- **public-official-guide** — the named house-style sibling; reuse the neutrality gate + insider-misuse
  threat model. New council members are a *shared beneficiary* (they must vote on budgets they can't
  read) — bundle a "your first budget vote" explainer.
- **fact-check-toolkit** — reuse the reconciliation + citation-coverage engine and the advisory
  LLM-judge as a general **claim→source verification** harness; budget figures are a high-value
  claim class.

**Productized artifacts:**
- **Embeddable budget widgets** — accessible, reconciled mini-explainers/charts (the WDMMG BubbleTree /
  "your tax contribution" pattern, modernized + WCAG 2.2 AA) that newsrooms/libraries embed; each ships
  its companion data package.
- **MCP server** — expose the normalized snapshots, reconciliation engine, and provenance as an **MCP
  tool** so any agent can query "what does jurisdiction X spend on function Y, FY Z, cited and
  reconciled" without re-implementing the pipeline. A natural distribution + reuse channel and a clean
  Claude-API integration story.

---

## 8. Open questions

1. **Pilot jurisdiction** (plan's own M0 gate): which level/locale balances clean reuse-licensed data,
   reconcilable line items, an available public-finance reviewer, and a plausible distribution partner?
   US-local maximizes comprehension need but raises ACFR/fund complexity.
2. **How much weight can the lint carry publicly** before "passed the neutrality screen" misleads? What
   is the exact division of labor between deterministic lint, advisory LLM critic, and binding human
   review — and how is it communicated to readers?
3. **Fiscal Data Package vs plain Tabular DP** — adopt the domain profile (interoperability) or stay
   generic (simplicity)? Affects ecosystem fit.
4. **Deflator/base-year and "real terms" default** — show real-terms by default (comprehension) or
   opt-in (avoid implying unsupported comparisons)?
5. **Cross-jurisdiction comparison** — is COFOG-normalized comparison ever publishable, or always
   single-jurisdiction? (Plan leans single-jurisdiction; competitors over-compare.)
6. **Contract-PII line** across jurisdictions with differing transparency-vs-privacy law (sole
   proprietors, named officials).
7. **Funding/governance transparency model** to pre-empt the Ballotpedia-style "non-partisan but
   captured" critique — what is published, and how is reviewer independence demonstrated?
8. **Source liveness/currency** — how to detect upstream restatements and abandoned portals, and what
   SLA triggers re-snapshot vs supersede-banner?
9. **MCP/widget distribution** — is an embeddable/MCP surface in scope for the pilot, or a post-M5
   spin-off (risks scope creep vs. accelerates adoption)?

---

### Sources

- USAFacts — about / methodology: https://usafacts.org/about-usafacts/
- USAspending.gov data quality — GAO-22-104702: https://www.gao.gov/products/gao-22-104702 ; GAO-24-106214: https://www.gao.gov/products/gao-24-106214
- OpenSpending: https://www.openspending.org/about ; Open Spending Next: https://discuss.okfn.org/t/open-spending-next-update-and-teaser/2663 ; OKFN blog: https://blog.okfn.org/category/okfn-projects/open-spending/
- Where Does My Money Go (OKFN): https://blog.okfn.org/category/okf-projects/wdmmg/
- Fiscal Data Package (moved): https://github.com/openspending/fiscal-data-package
- Open Contracting / OCDS: https://www.open-contracting.org/data-standard/ ; https://standard.open-contracting.org/
- COFOG / OECD Government at a Glance 2025: https://www.oecd.org/en/publications/2025/06/government-at-a-glance-2025_70e14c6c/full-report/classification-of-the-functions-of-government-cofog_16aa2337.html
- IMF GFSM 2014: https://www.imf.org/en/data/statistics/government-finance-statistics-manual
- Frictionless Data Package v2.0 (2024): https://frictionlessdata.io/blog/2024/06/26/datapackage-v2-release/ ; Tabular Data Package: https://specs.frictionlessdata.io/tabular-data-package/
- Ballotpedia: https://en.wikipedia.org/wiki/Ballotpedia ; funding critique: https://www.chaoticera.news/p/this-popular-election-resource-is-funded-by-far-right-donors-does-it-matter
