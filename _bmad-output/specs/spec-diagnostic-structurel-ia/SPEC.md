---
id: SPEC-diagnostic-structurel-ia
companions:
  - acceptance-criteria.md                                        # spec-authored: §9 acceptance criteria → CAP map (the test contract)
  - compliance-references.md                                      # spec-authored: regulatory + signer/liability specifics
  - ../../../docs/Cahier_des_Charges_DiagnosticStructurel_IA.md   # adopted: authoritative feature-level spec (§1–3: 3-module catalogs, roles, UX)
sources:
  - ../../planning-artifacts/briefs/brief-Diagnostic_structurel_IA_v2-2026-07-10/brief.md
  - ../../planning-artifacts/briefs/brief-Diagnostic_structurel_IA_v2-2026-07-10/addendum.md
---

> **Canonical contract.** This SPEC and the files in `companions:` are the complete, preservation-validated contract for what to build, test, and validate. Source documents listed in frontmatter are for traceability only — consult them only if you need narrative rationale or prose color this contract intentionally omits.
>
> *Contract prose is English; the product itself ships in French (hard requirement). French domain terms (désordre, coupe, schéma, relevé, ouvrage) are kept verbatim because they are the shipped vocabulary.*

# Diagnostic Structurel IA

## Why

For a practicing French structural-diagnostic expert, **the site visit is the fast part.** A full diagnostic runs ~3–3.5 days, yet only ~0.5 day is on-site — the other ~85% is deskwork: writing the report (~1.5 days), placing annotated photos against disorders (~0.5 day), and drawing technical *coupes* and *schémas* (~0.5–1 day). This work is dreaded, barely reusable, and caps how many diagnostics one expert can run. Three forces make now the moment: **a mandate** — Décret n°2025-814 (Aug 2025), under the *loi Habitat dégradé*, makes structural diagnosis of collective housing mandatory in designated zones, raising volume against an unchanged manual ceiling; **an opportunity** — computer-vision defect detection is finally mature enough to *assist* expert judgment; and **a pain** — the deskwork bottleneck itself. Critically, French law requires a **Bac+5 engineer to sign every diagnosis** and carry professional liability, so the AI can never diagnose alone: **the AI proposes, the expert disposes and signs.** It is built by a practicing diagnostic expert who is also a data engineer — for his own daily use first, and as a subscription product for other French diagnosticians second — which means he holds the labeled photos, past reports, and reference norms needed to seed the model and is his own first user and labeler.

## Capabilities

- **CAP-1** — Mission creation & management
  - **intent:** An expert or project lead can create a diagnostic mission from mobile or back-office — capturing *ouvrage* metadata (address, structure type, estimated construction date, usage), assign one or more experts, and consult prior missions on the same *ouvrage*.
  - **success:** A created mission is retrievable with its *ouvrage* metadata, assigned experts, and prior-mission history on that *ouvrage*.

- **CAP-2** — Guided offline field survey
  - **intent:** An expert can complete a standardized survey on site, entirely offline, using dynamic forms adapted to the structure type — organized by structural zone/element (*fondations, planchers, poteaux, poutres, voiles, façades, toiture*) with checklists, 1–5 intensity sliders, dropdowns, free text, and automatic geolocation.
  - **success:** On a device with no network, an expert completes an entire standard survey with zero data loss, and survey entry is ≥30% faster than the current manual process, validated by pilot experts.

- **CAP-3** — Photo capture, association & annotation
  - **intent:** An expert can capture or import photos directly from the survey form, each auto-associated to a structural element and zone, annotate them (arrows, circles, text), with timestamp and geolocation embedded in the metadata.
  - **success:** A photo captured from the form is bound to its structural element/zone, carries geo + timestamp metadata, and its annotations persist through offline sync.

- **CAP-4** — Offline-first sync
  - **intent:** Survey data and photos captured offline synchronize automatically on reconnection.
  - **success:** Sync completes with zero data loss within 30 seconds of reconnection, and no data is lost on a forced app close.

- **CAP-5** — Compliant PDF deliverable
  - **intent:** An expert can generate the compliant PDF report — assembling all survey data and annotated photos into a chosen configurable template (visit report / full diagnostic / unit disorder sheet) with the regulatory layout (customizable cover, auto table-of-contents, pagination, per-zone synthesis) and export/share it (local export, email, secure link), generable on-site offline and finalized at sync.
  - **success:** The generated PDF contains all captured survey data, annotated photos, ≥1 relevant *coupe*, ≥1 annotated *schéma*, disorder mapping consistent with survey numbering, and the correct regulatory layout — and stays readable regardless of photo/schema volume.

- **CAP-6** — Technical *coupes* with disorder positioning
  - **intent:** An expert can select from a library of preconfigured technical *coupe* types (wall, floor, foundation, roof, post-beam) and position surveyed disorders on it directly from the app.
  - **success:** At least one relevant *coupe* with positioned disorders is embedded in the PDF, and the positions match the survey numbering.

- **CAP-7** — Annotated *schémas*
  - **intent:** An expert can draw a principle *schéma* of the *ouvrage* or zone using a simplified sketch tool (freehand, geometric shapes, legends, *cotations*).
  - **success:** An annotated *schéma* is produced and embedded in the PDF.

- **CAP-8** — Automatic disorder mapping
  - **intent:** Numbered survey disorder points are automatically reported onto the selected *coupe*/*schéma*, each cross-referenced to its *fiche de désordre*.
  - **success:** The mapping is consistent with the survey numbering and each mapped point links to the correct disorder sheet.

- **CAP-9** — AI disorder detection & classification
  - **intent:** From submitted photos, the system pre-classifies visible disorders across six categories (*fissuration; corrosion/carbonatation; humidité/infiltrations; déformations; désordres maçonnerie; dégradations diverses*), each with image localization (bounding box or segmentation mask), a severity indication (*léger/modéré/sévère/critique*), and a confidence score.
  - **success:** The model reaches **>75% accuracy on cracks and corrosion** on the test set with a confidence score shown per detection; other categories are best-effort.

- **CAP-10** — Expert validation loop
  - **intent:** The expert validates, corrects, or rejects each AI detection and manually adds disorders the model missed — the *AI proposes, expert disposes* mechanism.
  - **success:** Each result can be validated/corrected/rejected in **<5 seconds**, and every validated correction is stored as training data.

- **CAP-11** — Structured disorder database
  - **intent:** The system stores all collected images and annotations, classified by structure type, disorder nature, severity, and geolocation, with full traceability (expert, date, mission, *ouvrage*), and exports datasets for model training.
  - **success:** Datasets are exportable for retraining with complete per-record traceability.

- **CAP-12** — Continuous learning loop
  - **intent:** Validated corrections flow through an iterative cycle — collect → annotate → retrain → validate → deploy — operated by the founder.
  - **success:** Validated corrections feed a training dataset the founder retrains from **manually, ad-hoc, once enough corrections accumulate**; a retrained model is gated on test-set performance before deployment. No automated retraining pipeline and no fixed cadence at POC stage.

- **CAP-13** — AI performance dashboard
  - **intent:** The system tracks model quality — precision, recall, and F1 per disorder category — with model version history and comparison, and alerts on performance drift.
  - **success:** Per-category metrics and cross-version comparison are visible, and an alert fires when performance drift is detected.

- **CAP-14** — Probable cause & recommendation inference *(extends CAP-9)*
  - **intent:** For each detected disorder, the system proposes probable-origin hypotheses (*hypothèses d'origine probable*) and preliminary monitoring or intervention recommendations, per Cahier §2.2.2.
  - **success:** Each detected disorder carries ≥1 proposed cause hypothesis and a preliminary recommendation; every such output is visually marked **non-binding** and cannot enter the generated PDF until the expert has validated or edited it; expert edits are captured.

## Constraints

- **AI never diagnoses or signs autonomously.** Every AI output must be expert-validated before it enters the deliverable; a named **Bac+5 engineer signs each diagnosis and bears the professional liability** (RC Pro ≥ €1M/claim). This bends the entire propose/dispose flow, the permission model, and the audit trail. *(See [compliance-references.md](compliance-references.md).)*
- **Offline-first, zero data loss.** Full mission creation, survey, and photo capture must work with no network; sync within 30s of reconnection; no loss on forced close. Forces a local-first storage architecture.
- **French UI and domain vocabulary** — a hard requirement. The product ships in French (*désordres, coupes, schémas, relevés*).
- **Mobile field ergonomics.** iOS + Android, tested on the 5 most common expert devices; any key function reachable in ≤3 taps; ≥14pt type, high contrast, dark mode, one-handed use, haptic feedback, Bluetooth stylus/keyboard support; usable while gloved and under variable light.
- **Regulatory-compliant deliverable layout.** The PDF must meet the defined report structure (*coupes*, *schémas*, disorder mapping, per-zone synthesis, cover/TOC/pagination) — a generic snag list is insufficient; the compliant deliverable is the product's whole point.
- **AI acceptance gate.** >75% accuracy on cracks and corrosion on the test set; per-detection confidence shown; validate/correct/reject in <5s. Sets both the model release gate and a UX latency budget.
- **Inferred causes and recommendations are non-binding and carry no accuracy commitment** (CAP-14). Cause inference and severity grading are not solved problems, so these outputs must be visually distinguished from detection results and gated behind explicit expert validation before they can enter a signed deliverable. Bends the AI review UI and the PDF assembly gate.
- **Onboarding.** An untrained expert must become productive in <1 hour, via in-app guidance/tooltips and user documentation.

## Non-goals

- **Ouvrages d'art** (bridges, tunnels, infrastructure) — out of v1; deferred to Vision.
- **Mission accounting / billing** — out of v1.
- **Automated authoring of legal-expertise reports** — out of v1.
- **Multi-tenant subscription / commercial rollout** — gated behind personal validation; out of v1.
- **Autonomous AI diagnosis** — the tool assists and documents; it never renders a diagnosis on its own.

## Success signal

On a real mission this year, the founding expert completes the on-site survey, the AI pre-classifies the disorder photos, he validates or corrects each in seconds, and the compliant PDF — *coupes*, *schémas*, disorder mapping, regulatory layout — is generated for a **review-and-sign** step. Per-diagnostic turnaround drops from ~3.5 days toward **~1.5 days** (roughly **2× throughput**) by compressing the deskwork into a review step, not by rushing the site visit. Measurable gates: turnaround measured on real missions, survey entry ≥30% faster, a compliant PDF produced end-to-end, AI >75% on cracks/corrosion, and zero data loss offline.

## Assumptions

- **The project is at POC stage.** RGPD/GDPR, pricing, and timeline/budget are consciously deferred (see Open Questions) rather than overlooked.
- **Cahier §2.2.2 is implemented literally** — the AI produces probable causes and preliminary recommendations (CAP-14). This decision was taken with the reliability concern (cause inference is not a solved CV problem; severity grading is under-researched) and the liability concern (output flows into a PDF signed by a Bac+5 engineer) explicitly on the record. The risk is managed by **mandatory expert validation** (CAP-10 and the non-binding constraint), not by narrowing what the AI outputs.
- Contract prose is written in English per config `document_output_language`; the product and UI ship in French (user-confirmed).
- v1 scope is taken from the brief's "In (v1)" list; capabilities are derived from Cahier des Charges §2 (functional) and §9 (acceptance, preserved via the brief addendum).
- The Cahier's §2.1.1 usage list includes "infrastructure," but the brief defers *ouvrages d'art* to Vision — **v1 is treated as buildings only** (collective housing, ERP/commercial, industrial). Flagged so downstream reading the adopted Cahier does not re-introduce infrastructure scope.
- "The 5 most common expert devices" is a pilot-defined test matrix, not yet enumerated.

## Open Questions

All remaining questions are **consciously deferred at POC stage** — parked, not overlooked.

- **Deferred (POC) — RGPD/GDPR.** The sources are silent on data residency, retention, and consent for stored site photos, geolocation, *ouvrage* addresses, expert PII, and exported training datasets. ⚠️ **Must be resolved before any real-mission data collection or productization** — POC data of this kind is already personal data under RGPD, so the deferral is safe only while the tool runs on the founder's own test material.
- **Deferred (POC) — Subscription pricing.** Model (per-month vs per-report) and price point — validate with a pilot before productizing. Already gated by the multi-tenant non-goal.
- **Deferred (POC) — Timeline & budget.** Not yet determined; set once POC scope is locked.

*Resolved since first derivation:* AI cause/recommendation output → implemented per Cahier §2.2.2 as **CAP-14** (see Assumptions). Retraining cadence & ownership → founder-operated, manual and ad-hoc (see **CAP-12**).
