# Reconciliation: Brief (2026-07-10) → PRD (2026-07-23)

Source: `briefs/brief-Diagnostic_structurel_IA_v2-2026-07-10/brief.md` + `addendum.md`
Target: `prds/prd-Diagnostic_structurel_IA_v2-2026-07-23/prd.md` + `addendum.md`

Known intentional divergences excluded per instruction: buildings-only scope; AI-drafted editable causes/recommendations; signature outside the tool; RGPD deferred.

Findings ordered by importance.

---

## F-1. AI cold-start plan (founder's seed corpus) silently dropped — SM-3 has no stated data source

**Severity: High (a — missing requirement/constraint)**

- **Source:** Brief Executive Summary ("he holds the crown-jewel inputs (his own site photos, past reports, worked examples, and reference norms) needed to seed the AI, and he is his own first user and first labeler"); Brief "What Makes This Different" ("his own labeled data to seed the model and beat the cold start that stops most entrants"); Brief addendum §2 ("Crown-jewel inputs held in-house: own site photos, past diagnostic reports, worked examples, and all required norms/reference documents — softens the AI cold-start").
- **PRD:** Absent. FR-21/FR-22 cover data captured *by the product going forward* (validations, corrections), and FR-11 demands >75% precision on fissures/corrosion at v1 (SM-3) — but nothing in the PRD or its addendum says where the initial training data comes from. The brief's answer (ingest/label the founder's existing corpus before or alongside v1) is the load-bearing feasibility argument for SM-3 and it has vanished. Downstream architecture/epics will have no FR or open question prompting a "seed dataset ingestion + labeling" workstream.
- **Suggested fix:** add an FR (or at minimum an Open Question / architecture note) covering initial dataset assembly from the founder's existing photos/reports, and reference the norms/reference documents as inputs.

## F-2. User documentation is measured (SM-6) but never required by any FR

**Severity: Medium-High (a — missing requirement)**

- **Source:** Brief addendum §3, acceptance §9.3 verbatim: "La documentation utilisateur permet à un expert non formé de prendre en main l'outil en moins d'une heure." Brief Success Criteria: "an untrained expert productive in <1 hour."
- **PRD:** SM-6 measures "productive in under one hour **using only the documentation**", but no FR in §5 requires user documentation to exist as a deliverable. Every other acceptance criterion in §9.1–9.3 traces to an FR; this one traces only to a metric. Epics generated from §5 will not contain a documentation story.
- **Suggested fix:** add a small FR (e.g. under §5.8 or a new §5.9) for French-language user documentation sufficient for unassisted onboarding.

## F-3. Regulatory qualification thresholds dropped (Bac+5, RC Pro €1M/€1.5M, DTG Bac+3)

**Severity: Medium (a — dropped constraint detail; also feeds an internal FR-17 gap)**

- **Source:** Brief Executive Summary ("Bac+5 engineer to sign every diagnosis and carry ≥€1M professional liability"); Brief addendum §4 ("signed by a Bac+5 engineer (≥2 yrs, independent) with RC Pro ≥ €1M/claim, €1.5M/year; DTG (copropriété) requires a Bac+3 qualified diagnostician with RC Pro").
- **PRD:** Reduced to "a named human engineer bears liability" (§1) and, in the PRD addendum, "professional credentials + insurance" as décret report content. The concrete thresholds are gone from both PRD files. They matter twice: (1) they define who a future commercial user legally is (SM-7 gate); (2) the PRD's own addendum says the décret report must contain "professional credentials + insurance", yet FR-17's Rapport content list (building info, checklists, values, observations, photos, Coupe, Schéma, cover page with logo/title/date/Expert) includes no credentials/insurance block — so the "compliant PDF" FR omits a compliance element both addenda point at.
- **Suggested fix:** restore the thresholds to the PRD addendum's regulatory section; add credentials/insurance (and ideally the other décret content items) to FR-17 or to the FR-19 template scope.

## F-4. Open question "Timeline & budget" dropped from §10

**Severity: Medium (a — dropped scope statement)**

- **Source:** Brief "Open Questions": "Timeline & budget — not yet determined; set once the personal-use v1 scope is locked."
- **PRD:** §10 Open Questions carries pricing, RGPD, devices, retraining cadence, arrêté template, missing Cahier sections — but not timeline/budget. The PRD is exactly the moment "the personal-use v1 scope is locked", i.e. the trigger the brief set for answering it; dropping the question silently loses that trigger.
- **Suggested fix:** add it to §10 with the brief's trigger condition.

## F-5. Brief's competitive digest replaced wholesale, not merged — and §2 slightly overclaims

**Severity: Medium-Low (b/c — dropped context + mild contradiction)**

- **Source:** Brief addendum §4: PlanRadar ("dominant EU incumbent, 150k+ users"), Dalux, Fieldwire/Hilti, Procore, **Inspectly360 ("offline + on-device AI — closest UX overlap")**, STRUCINSPECT, Niricson, Trendspek+Datature, betonexpertise, AQC "Batist"/"Hiiro", plus market sizing (SHM ~$4–8B; building-inspection software ~$0.8–1.3B → ~$2.7B/2033).
- **PRD:** The PRD addendum's competitive section (2026-07-23 web research: T2D2, Spectora, Hosta, SpaceCapture, TÜV SÜD×Contilio) is entirely new and does not carry forward any of the brief's named competitors or the market-sizing figures. Two consequences:
  - **(c) Contradiction risk:** PRD §2 states "the serious players are enterprise, drone, or 3D-scan oriented" — the brief's own research disputes this: PlanRadar is a mass-market mobile field SaaS and Inspectly360 was flagged as the *closest UX overlap* (offline + on-device AI). The wedge claim survives ("none produce the compliant French structural deliverable"), but the categorical framing in §2 is weaker than the brief's more careful version.
  - **(b) Lost knowledge:** the AQC "Batist"/Sycodés signal (institutional French appetite, ~60k-claim dataset — relevant both as validation and as a data-advantage caveat) and the brief's explicit "incumbents hold more and better-structured data" caveat on the learning loop are gone.
- **Suggested fix:** merge (not replace) the two digests in the PRD addendum; soften §2's "serious players are enterprise/drone/3D-scan" to acknowledge PlanRadar-class field SaaS and Inspectly360.

## F-6. Offline Mission *creation* is an acceptance criterion but only implicit in the PRD

**Severity: Low (a — weakened traceability)**

- **Source:** Brief addendum §3, acceptance §9.1: "Un expert terrain peut **créer une mission**, saisir l'intégralité d'un relevé et prendre des photos **sans connexion réseau**." Brief Success Criteria: "Offline: create a mission, complete a full survey, capture photos with no network."
- **PRD:** FR-7 covers offline Relevé capture; FR-1 (Mission creation) does not state offline; UJ-1 even frames the Mission as "created beforehand". Only NFR §9 ("full Mission + Relevé + photos with no network") implies it. Because FR-1's testable consequences omit offline, a test suite generated from FRs would miss the "create a mission with no network" acceptance case.
- **Suggested fix:** add an offline-creation consequence to FR-1 (or fold Mission creation into FR-7's consequences).

## F-7. Minor tonal/qualitative drops (no action strictly required)

**Severity: Low (b)**

- The founder's **dual profile** (practicing expert *and* data engineer, "his own first user and first labeler" — brief Exec Summary, addendum §2) appears nowhere in the PRD. Mostly fine for a PRD, but it silently underpins FR-22's "retraining performed by the technical team" (the technical team *is* the founder) and F-1 above.
- The brief's 2–3-year Vision paragraph ("reference platform for francophone diagnosticians", corpus flywheel "earned over time, not claimed") has no echo in the PRD beyond Non-Goals. Acceptable for a v1 scope contract; noted for completeness.
- Brief's "Honest beats impressive" voice is otherwise well preserved (PRD §1, §6 Non-Goals, SM-C1 rubber-stamp counter-metric — the last being a genuine improvement over the brief).

---

## Verified as covered (spot-checks, no finding)

- 30s sync / zero loss / force-close → FR-7, SM-5. 30% faster entry → SM-2. <5s validation → FR-13, SM-4. >75% fissures/corrosion → FR-11, SM-3. Six Désordre categories, Sévérité, Score de confiance → Glossary, FR-11. Coupe numbering consistency → FR-15. PDF layout stability → FR-17. Templates ×3 → FR-19. Corrections exportable → FR-14/FR-22. Dashboard precision/recall by category → FR-23. Three roles → FR-24. iOS+Android, 5 devices, French-only → §7.1, NFR §9. Pricing open → §10.6. Commercial gate → SM-7 / Non-Goals. ~1.5-day / 2× ROI → SM-1. Décret framing and "AI proposes, Expert disposes" → §1–§2.
