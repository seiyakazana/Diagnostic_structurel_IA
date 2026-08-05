---
title: "PRD: Diagnostic Structurel IA"
status: final
created: 2026-07-23
updated: 2026-08-05
---

# PRD: Diagnostic Structurel IA
*Working title — confirm.*

## 0. Document Purpose

This PRD defines v1 of Diagnostic Structurel IA, a POC whose first users are its two founding experts. It is written for the downstream BMad workflows (UX design, architecture, epics and stories) and for the founders themselves as the scope contract. It builds on the product brief of 2026-07-10 (scoping authority) and the Cahier des Charges v1.1 of May 2026 (feature-detail authority); it does not duplicate them.

Vocabulary is anchored in §4 Glossary; features are grouped in §5 with globally numbered FRs; every unconfirmed inference carries an inline `[ASSUMPTION]` tag, indexed in §11. The PRD is written in English; **the product itself ships in French** (hard requirement). Technical depth, competitive research, and regulatory detail live in the companion `addendum.md`. References of the form "(cahier acceptance §9.x)" cite the Cahier des Charges acceptance criteria, whose text survives via the brief's addendum (the cahier file itself is truncated); each cited criterion is restated as a testable consequence where it appears.

## 1. Vision

A full structural diagnostic today takes ~3.5 days, of which only half a day is spent at the building. The remaining ~85% is deskwork: writing the report, placing annotated photos against disorders, and drawing Coupes and Schémas — the most dreaded, least reusable part of the job. Diagnostic Structurel IA compresses that deskwork into a **review-and-sign step**: the same diagnostic, minus two days at a desk.

The product is a mobile-first field companion plus a web back-office. On site, the Expert captures a guided, offline-capable Relevé — structured observations and geolocated photos pinned to structural elements. The AI then proposes: it detects and classifies Désordres on the photos, grades severity, and drafts cause hypotheses and preliminary recommendations as editable text. The Expert disposes: validates, corrects, or rejects every AI output, positions Désordres on Coupes, and exports a compliant PDF Rapport. Every correction feeds a learning loop that makes the next mission better.

The positioning principle is non-negotiable: **the AI proposes, the Expert disposes and signs.** This is expert-validated assist and triage, never automated diagnosis. The product sits at mission level R0/R1 (observations and hypotheses) — it performs no stability calculation, and a named human engineer bears liability for every Rapport.

## 2. Why Now

Décret n°2025-814 (12 August 2025, loi Habitat dégradé) makes structural diagnosis of collective housing mandatory in commune-designated zones, performed by qualified engineers whose capacity is capped by the manual deskwork ceiling. Demand is rising against unchanged throughput. Meanwhile, CV defect detection is mature enough to assist (binary crack detection is commoditized). Adjacent tools exist — T2D2 does phone-photos-in / condition-report-out and Spectora drafts inspection comments (see addendum) — so the defensible gap is narrower and specific: **nothing is francophone-native (French pathology vocabulary, French report conventions) and nothing maps to the coming official décret report template.** Generic field-inspection SaaS (PlanRadar and peers) manages photos and snag lists, and the AI-detection players are enterprise, drone, or 3D-scan oriented — none produce the compliant French structural deliverable. That sharpest differentiator depends on the arrêté publishing the official template (§10 Q2); FR-19 carries the contingency.

## 3. Target User

### 3.1 Jobs To Be Done

- **Functional:** complete a compliant structural diagnostic in ~1.5 days instead of ~3.5 — capture the Relevé once, on site, and leave with the Rapport nearly done.
- **Functional:** never lose field data — surveys, photos, and sketches survive dead zones, force-closed apps, and end-of-day syncs.
- **Emotional:** eliminate the dreaded report-writing evenings; end a site day with the sense that the work is essentially done.
- **Social/professional:** deliver homogeneous, compliant Rapports regardless of which Expert did the survey — consistency the profession currently lacks.
- **Builder's framing:** this is v1 for the founders' own missions; it must prove itself on their real work before anyone else touches it.

### 3.2 Non-Users (v1)

- Other diagnosticians, cabinets d'expertise, bureaux d'études — commercial rollout is gated behind personal validation on the founders' real missions.
- Inspectors of ouvrages d'art (bridges, tunnels) — deferred.
- Insurers, syndics, building owners — they receive the PDF Rapport, they never touch the product.

### 3.3 Key User Journeys

- **UJ-1. The Expert surveys a building with no signal and loses nothing.**
  - **Persona + context:** Marc `[ASSUMPTION: persona name — the associate/founding Expert]`, structural engineer, diagnosing a 1960s collective-housing building under the new décret. Basement and stairwells have no network. He wears gloves; one hand often holds a lamp.
  - **Entry state:** authenticated on the mobile app from a previous session; the Mission was created beforehand (by him or the Chef de projet) with building info and checklist.
  - **Path:** opens the Mission → walks Zone by Zone through the guided form adapted to the structure type → for each Élément structurel, records observations via checkboxes, 1–5 intensity sliders, and free text → photographs each Désordre from within the form; the photo auto-attaches to the Élément and Zone with timestamp and geolocation → annotates directly on the image (arrows, circles, text).
  - **Climax:** he finishes the survey entirely offline; back in signal, the app syncs everything within 30 seconds — nothing to re-enter, nothing lost.
  - **Resolution:** the Relevé is complete and queued for AI analysis; Marc drives home instead of starting a photo-sorting evening.
  - **Edge case:** the app is force-closed mid-survey (battery, OS kill); on relaunch every entry and photo is intact.

- **UJ-2. The Expert turns AI proposals into a signed-ready Rapport in an hour.**
  - **Persona + context:** same Marc, next morning at the office, web or mobile `[ASSUMPTION: validation happens on either surface]`.
  - **Entry state:** the synced Relevé has been processed; each photo carries AI Détections.
  - **Path:** reviews Détections one by one — nature, localization overlay, severity, confidence score — and validates, corrects, or rejects each detection in seconds → reviews the AI-drafted cause hypotheses and preliminary recommendations at his own pace, editing or deleting the draft text (no speed target applies to this step) → adds two Désordres the AI missed → selects the relevant Coupe types from the library and positions the numbered survey points on them; the system keeps every point cross-referenced to its Fiche de désordre → sketches one annotated Schéma of the worst Zone.
  - **Climax:** generates the PDF Rapport — cover page with cabinet logo, TOC, per-Zone synthesis, annotated photos, Coupes and Schéma, pagination — and reads a document that is essentially finished.
  - **Resolution:** he signs via his existing external process and sends the Rapport by email or secure link. His corrections are already stored as training data.

- **UJ-3. The Chef de projet runs the pipeline without touching a wall.**
  - **Persona + context:** Nadia `[ASSUMPTION: persona name]`, the second founding Expert, acting as Chef de projet `[ASSUMPTION: in v1 the two founders cover all three roles]`.
  - **Path (one line):** creates the Mission in the back-office (building info, structure type, usage, past-mission history) → assigns the Expert → after UJ-2, reviews the generated Rapport → consults the AI performance dashboard to see whether last month's corrections improved the model's agreement with Expert Validations and its scores on the frozen test set.

## 4. Glossary

*Downstream workflows must use these terms exactly. French domain terms are kept in French — they are the product's native vocabulary.*

- **Ouvrage** — the built structure a Mission targets. In v1, buildings only: collective housing, commercial/ERP, industrial.
- **Mission** — one diagnostic engagement on one Ouvrage. Carries Ouvrage info (address, structure type, estimated construction date, usage), assigned Experts, configurable checklist, and history of past Missions on the same Ouvrage. Contains one or more Relevés.
- **Relevé** — the structured field survey of a Mission: observations per Zone and Élément structurel, photos, values. Captured offline-first.
- **Zone** — a spatial subdivision of the building under survey (e.g. basement, façade nord).
- **Élément structurel** — a structural member category within a Zone: fondations, planchers, poteaux, poutres, voiles, façades, toiture.
- **Désordre** — an observed structural disorder. Classified into six evolving categories, each with named sub-disorders (liste évolutive, full taxonomy in the addendum): fissuration (fissures de retrait, fissures structurelles, lézardes, microfissures); corrosion/carbonatation (épaufrures, armatures apparentes, taches de rouille, bullage); humidité/infiltrations (remontées capillaires, moisissures, efflorescences, salpêtres); déformations (flambement, désaffleurements, tassements différentiels, dévers); désordres maçonnerie (déliaisonnements, décollements, joints dégradés, faïençage); dégradations diverses (éclatements, défauts d'enrobage, cavités, impacts).
- **Détection** — one AI-proposed Désordre on one photo, produced by the CV analysis: nature (category + sub-category), localization overlay, proposed Sévérité, Score de confiance. Always subject to Validation. A Détection is complete without drafted text; the AI-drafted cause hypotheses and preliminary recommendations (FR-12) are a **separate, later-bound artifact** attached to the Détection/Fiche de désordre when generated.
- **Sévérité** — the four-level grading of a Désordre: léger / modéré / sévère / critique. The AI's Sévérité is a proposal with **no accuracy commitment in v1** and no confidence percentage of its own; the Expert's Validation sets the grade that counts.
- **Score de confiance** — the AI's confidence in the **detection** (nature + localization), displayed as a percentage. It does not apply to Sévérité, cause hypotheses, or recommendations, and the UI must not present it as if it did.
- **Validation** — the Expert's decision on a Détection: validate, correct (any field), or reject. Every correction is stored as training data.
- **Fiche de désordre** — the unit record of one validated Désordre: description, photos, localization, Sévérité, cause hypotheses, recommendations. Numbered consistently with the Relevé.
- **Coupe** — a technical cross-section drawing selected from a preconfigured library of types (mur, plancher, fondation, toiture, poteau-poutre, …) on which Désordres are positioned.
- **Schéma** — a simplified annotated sketch drawn by the Expert (freehand, geometric shapes, legends, dimensions).
- **Rapport** — the exported PDF deliverable assembled from a Mission. Template-driven: rapport de visite, rapport de diagnostic complet, or fiche de désordre unitaire. Signed outside the product.
- **Expert terrain (Expert)** — role: performs Relevés, Validations, Coupes/Schémas, Rapport export.
- **Chef de projet** — role: creates Missions, assigns Experts, reviews Rapports.
- **Administrateur** — role: user management, settings, AI-model monitoring, Rapport-template management.

## 5. Features

### 5.1 Mission Management
**Description:** Missions are created and managed from the back-office or the mobile app. A Mission carries the building's identity and drives everything downstream: which guided form the Relevé uses, which checklist applies, which Experts may work on it. Past Missions on the same building are visible for context. Realizes UJ-3.

#### FR-1: Mission creation
A Chef de projet or Expert can create a Mission with Ouvrage info: address, structure type, estimated construction date, usage (résidentiel / ERP / industriel), and **recent-works history** (travaux récents on the Ouvrage and their suspected impact on stability — a décret-mandated Rapport content block; editable during the Mission as facts emerge). Realizes UJ-3.
**Consequences (testable):**
- A Mission cannot be created without address and structure type.
- The usage list contains exactly the three v1 values; no infrastructure option is present.
- A Mission can be created on the mobile app with no network (cahier acceptance §9.1).
- Recent-works history entered on the Mission appears in the rapport de diagnostic complet (FR-17); an empty history renders as an explicit "aucuns travaux récents signalés" statement, never as a silent omission.

#### FR-2: Expert assignment
A Chef de projet can assign one or more Experts to a Mission; only assigned Experts can edit its Relevés.
**Consequences (testable):**
- A Relevé is editable on exactly one assigned device at a time; a second Expert (or the back-office) cannot edit the same Relevé concurrently — ownership transfers only after the current owner's changes have synced. Concurrent-edit conflicts are therefore prevented, not merged.

#### FR-3: Ouvrage history
When a Mission concerns an Ouvrage with prior Missions `[ASSUMPTION: matched by address]`, the app surfaces those past Missions to the assigned Experts and the Chef de projet.

### 5.2 Guided Field Survey (Relevé)
**Description:** The heart of the field experience. The survey form is dynamic — its structure adapts to the Mission's structure type — and organized by Zone and Élément structurel. Entry is optimized for gloves, poor light, and one hand: checkboxes, 1–5 intensity sliders, dropdowns, free text. Everything works offline and geolocates automatically. Realizes UJ-1.

#### FR-4: Dynamic guided forms
An Expert can complete a Relevé through a form adapted to the Mission's structure type, organized by Zone and Élément structurel (fondations, planchers, poteaux, poutres, voiles, façades, toiture).
**Consequences (testable):**
- Changing the Mission's structure type changes the form sections presented.
- Every key survey action is reachable in ≤3 taps.

#### FR-5: Rapid field entry
An Expert can record observations via checkboxes, intensity sliders (1–5), dropdowns, and free text, with no perceptible wait on local entry.
**Consequences (testable):**
- Local entry never blocks on network; the UI responds to any field input within 200 ms `[ASSUMPTION: threshold — "no waiting on local entry" made testable]`.

#### FR-6: Configurable checklists
A Chef de projet or Administrateur can configure the checklist attached to a Mission before or during the engagement. `[ASSUMPTION: checklist configuration is back-office-side; the field app consumes it]`

#### FR-7: Offline capture with deferred sync
An Expert can create and complete an entire Relevé — forms, photos, sketches — with no network. Realizes UJ-1.
**Consequences (testable):**
- A full Relevé captured offline syncs with zero data loss within 30 seconds of reconnection.
- Force-closing the app loses no data (cahier acceptance §9.3).

#### FR-8: Automatic geolocation
Each survey entry and photo is automatically stamped with geolocation and timestamp.
**Consequences (testable):**
- When no GPS fix is available (basements, stairwells — the UJ-1 setting), capture proceeds without blocking; the entry is stamped with the last-known location flagged as approximate, or with no location if none exists.

### 5.3 Photo Capture and Annotation
**Description:** Photos are captured from within the form so context is never lost: each photo auto-associates to its Élément structurel and Zone. The Expert annotates on-image in the field or later. Realizes UJ-1.

#### FR-9: In-form photo capture
An Expert can open the camera from within the survey form; the photo auto-associates to the current Élément structurel and Zone, with timestamp and geolocation in metadata. Gallery import is also supported.

#### FR-10: On-image annotation
An Expert can annotate a photo with arrows, circles, and text, on-device, including offline.
**Consequences (testable):**
- Annotations round-trip: an annotated photo appears in the exported Rapport with its annotations legible and positioned as drawn.

### 5.4 AI Disorder Analysis (Détection and Validation)
**Description:** Synced photos are analyzed automatically. The AI proposes Détections across the six Désordre categories; the Expert disposes of each one. Cause hypotheses and preliminary recommendations ship in v1 as **AI-drafted editable text**, not CV claims — the Expert edits or deletes them like any draft. Nothing enters a Rapport unvalidated. Realizes UJ-2.

**Pipeline ordering (binding for architecture):** detection (FR-11) runs per photo at sync-time analysis and produces complete Détections; drafted text (FR-12) is generated per Désordre afterwards, over the Détection + Relevé context, and binds to the Fiche de désordre when it arrives. A Détection can exist, and be validated on its detection fields, while its drafted text is still pending; drafted-text review is a separate, unhurried step (see FR-13).

#### FR-11: Automatic Désordre detection
The system analyzes each synced photo and proposes Détections classified into the six categories and their sub-disorders (per the Glossary taxonomy; full evolving list in the addendum), each with nature (category + sub-category), localization overlay on the image `[ASSUMPTION: bounding box in v1; segmentation masks deferred]`, proposed Sévérité, and Score de confiance displayed as a percentage. **Acceptance scope:** fissuration and corrosion/carbonatation carry the accuracy commitments below; the other four categories are **best-effort in v1 with no acceptance threshold** — they ship if useful, and v1 detection scope narrows to fissuration + corrosion if their corpus falls short (see §5.7 corpus gate).
**Consequences (testable):**
- For fissures and corrosion, model precision exceeds 75% (cahier acceptance §9.2) **and recall is at or above 60%** `[ASSUMPTION: recall floor — no cahier criterion exists; set so the metric structure cannot reward a model that misses most désordres]`, both on the frozen test set.
- The test set is frozen and versioned before model tuning, includes held-out pilot-mission data, and includes a slice blind-labeled by the second founder (not the model's trainer).
- Every Détection displays its Score de confiance, scoped to the detection only (never to Sévérité or drafted text).
- Every photo shows its analysis status: pending / done / failed / no findings — "no findings" is always distinguishable from "not yet analyzed".
- Analysis of a standard synced Relevé completes within 1 hour `[ASSUMPTION: SLA target — UJ-2's "next morning" made testable]`; if the analysis service is unavailable, capture, manual Désordre entry, and Validation of already-analyzed photos are unaffected.

#### FR-12: AI-drafted causes and recommendations
For each Détection, the system drafts cause hypotheses ("hypothèses d'origine probable") and preliminary recommendations as editable text clearly marked as AI-proposed. `[ASSUMPTION: generated via language-model assist over the Détection + Relevé context, not via CV — mechanism belongs to architecture]` Recommendations are structured into the décret's three content blocks so the Rapport can assemble them per Zone and for the Ouvrage: **investigations complémentaires recommandées**, **mesures conservatoires**, and **travaux hiérarchisés** (prioritized works). The Expert edits, reorders, or deletes each block like any draft.
**Consequences (testable):**
- Drafted text is reviewed through its own affordance with **no speed target** — it is excluded from the <5s Validation budget (FR-13) and from SM-4.
**Out of Scope:** any form of stability calculation or definitive causal diagnosis.

#### FR-13: Expert Validation
An Expert can validate, correct (any field, including Sévérité, nature, causes, recommendations), or reject each Détection, and can manually add Désordres the AI missed. Realizes UJ-2.
**Consequences (testable):**
- A single Détection's **detection fields** (nature, localization, Sévérité) can be processed (validate/correct/reject) in under 5 seconds (cahier acceptance §9.2). The <5s budget explicitly excludes drafted cause/recommendation text, which has its own unhurried review affordance (FR-12).
- No unvalidated Détection can appear in a Rapport.
- Drafted text (causes, recommendations) appears in a Rapport only after the Expert has explicitly accepted or edited it — accepting a Détection's detection fields does not silently accept its drafted text.

#### FR-14: Correction capture for learning
Every Validation outcome (including corrections and manual additions) is stored as structured training data with full traceability: Expert, date, Mission, building. Training capture serves the learning loop only; the **legal** record of who wrote what in a Rapport is the separate provenance record (FR-25) — the two are distinct requirements, not one store worn two ways.
**Consequences (testable):**
- Corrections are exportable as a retraining dataset (cahier acceptance §9.2), passing the §7.2 personal-data and geolocation filters.

### 5.5 Coupes and Schémas
**Description:** The Rapport's visual backbone. The Expert selects Coupe types from a preconfigured library and **positions Désordres on them manually**; the system's job is consistency — numbering and cross-references to each Fiche de désordre are guaranteed, never re-entered. (v1 makes no automatic-placement claim: no FR captures the positional data auto-placement would need.) A simplified sketch tool covers what the library cannot. Realizes UJ-2.

#### FR-15: Coupe library and Désordre positioning
An Expert can select Coupe types from a preconfigured library (mur, plancher, fondation, toiture, poteau-poutre, …) and position Désordres on them — directly in the mobile app, in the field, as well as from the back-office. The system assigns each positioned point its survey number and maintains the cross-reference to the corresponding Fiche de désordre automatically; the Expert never types a number.
**Consequences (testable):**
- Désordre numbering on Coupe/Schéma is consistent with the Relevé and the associated Fiche de désordre (cahier acceptance §9.1).
- Inserting or removing a Désordre renumbers consistently across Relevé, Coupes, and Fiches with no manual re-entry.

#### FR-16: Sketch tool
An Expert can draw a Schéma with freehand strokes, geometric shapes, legends, and dimensions (cotations). Finger drawing is the required input; a Bluetooth stylus works as generic touch input, and dedicated stylus features (pressure, palm rejection) are deferred.
**Consequences (testable):**
- A Schéma round-trips: it appears in the exported Rapport legibly, with its legends and cotations intact.

### 5.6 Rapport Generation and Export
**Description:** The payoff feature: the compliant PDF assembled from everything upstream. Template-driven, offline-initiable, and layout-stable regardless of content volume. The Rapport leaves the product unsigned; the Expert signs through their existing external process. Realizes UJ-2.

#### FR-17: PDF Rapport generation
An Expert can generate a structured PDF Rapport with customizable cover page (cabinet logo, mission title, date, Expert), automatic TOC, pagination, per-Zone synthesis, and photo annexes. Content depends on the selected template (FR-19): the **rapport de diagnostic complet** contains all Ouvrage info, checklists, values, observations, annotated photos, at least one relevant Coupe, and an annotated Schéma of the diagnosed Zone; the rapport de visite and fiche de désordre unitaire carry the content floors defined in the addendum ("Rapport template content floors") `[ASSUMPTION: floors drafted by PM from template purpose — founders to confirm]`.
**Consequences (testable):**
- The rapport de diagnostic complet includes every piece of entered data, the Expert's identity, and the credentials/insurance block the décret requires on the deliverable (cahier acceptance §9.1; décret 2025-814 — see addendum).
- The rapport de diagnostic complet contains every décret-mandated content block: building description; structural elements examined and observed désordres; recent-works history and stability impact (FR-1); recommended further investigations, mesures conservatoires, and prioritized works (FR-12, Expert-validated). A block with no content renders as an explicit statement, never as a silent omission.
- Layout (page de garde, sommaire, pagination) remains readable regardless of photo/Schéma volume (cahier acceptance §9.1).

#### FR-18: Offline generation, finalized at sync
An Expert can request Rapport generation from the field while offline, including mid-Mission on an in-progress Relevé; the on-device render assembles all locally captured data, and AI-derived content (validated Détections, accepted drafted text) integrates when the PDF is finalized at sync.
**Consequences (testable):**
- The offline render is visibly watermarked **"PROVISOIRE — non finalisé"** on every page and cannot be exported or shared through FR-20.
- The finalized PDF supersedes any provisional render; finalization happens automatically at sync once analysis and Validation state are known, and the Expert can see unambiguously whether a Rapport is provisional or final.
- Both render outcomes obey FR-13: no unvalidated Détection and no unaccepted drafted text appears in either artifact.

#### FR-19: Rapport templates
An Administrateur can manage configurable Rapport templates; v1 ships three: rapport de visite, rapport de diagnostic complet, fiche de désordre unitaire.
**Notes:** `[NOTE FOR PM]` The décret's official report template arrives by arrêté; the template system must absorb it when published (see §10 Open Questions). Contingency: if the arrêté's structure diverges materially from the v1 Rapport model, template absorption becomes a scoped change request, not a silent rework — surface it via `bmad-correct-course`.

#### FR-20: Export and sharing
An Expert can export a Rapport locally, send it by email, or share it via secure link. `[ASSUMPTION: "secure link" = expiring authenticated URL; mechanism belongs to architecture]`
**Consequences (testable):**
- Only finalized Rapports (FR-18) can be exported, emailed, or shared; provisional renders are blocked from every FR-20 path.
- Share links expire, are revocable, and every share event is recorded (see §9 Security).

#### FR-25: Rapport provenance record
For every generated Rapport, the system retains an immutable provenance record with the Mission: which content was AI-drafted, what the Expert corrected or authored, and the Validation timestamps and acting Expert for each Détection and drafted-text block. This is the liability trail décret 2025-814 makes necessary — it answers, years later, "who wrote this sentence: the AI or the engineer?"
**Consequences (testable):**
- For any finalized Rapport, the provenance record reconstructs, per Désordre, the AI's original proposal and the Expert's final validated version, with actor and timestamp.
- The record is append-only: corrections after finalization create new entries, never rewrites.

### 5.7 Data Management and Learning Loop
**Description:** Every validated Désordre becomes an asset. The structured database feeds periodic retraining; the dashboard tells the Administrateur whether the model is actually getting better. The AI cold start is mitigated by the founders' in-house corpus — their own site photos, past reports, and worked examples serve as the seed labeled dataset, annotated by the founding Expert himself (see addendum); SM-3 is measured against a frozen, versioned test set drawn from this corpus plus pilot-mission data, with a blind-labeled slice from the second founder. Realizes UJ-3.

**Corpus gate (feasibility, binding before FR-11 acceptance testing):** FR-11 acceptance testing begins only once the labeled corpus reaches a per-category minimum of 150 usable examples spanning at least two capture devices `[ASSUMPTION: gate values — no source criterion exists; set to make the cold-start bet falsifiable]`. Categories that fall short ship best-effort or not at all; the fallback v1 scope is fissuration + corrosion only — which is all SM-3 commits to anyway. The corpus count per category is reviewed before detection work is scheduled, so the biggest feasibility dependency in the product is measured, not assumed.

#### FR-21: Structured Désordre database
The system stores all images and annotations classified by structure type, Désordre nature, Sévérité, and geographic location, with full traceability (Expert, date, Mission, building).

#### FR-22: Dataset export for retraining
An Administrateur can export the validated dataset for retraining. Retraining itself is performed offline by the technical team, validated on a dedicated test set before deployment. `[ASSUMPTION: retraining cadence unspecified — "periodic"; no in-product training pipeline in v1]`

#### FR-23: AI performance dashboard
An Administrateur can view, per Désordre category: **validation-agreement statistics** (validate/correct/reject rates from Expert Validations — labeled as agreement, not accuracy: the Expert's Validations are not ground truth and can themselves drift, see SM-C1) and **true precision/recall on the frozen test set per model version** (FR-11), plus model version history.
**Descoped to v2:** model-version comparison views and performance-drift alerts — statistically meaningless at pilot volume (two users, a handful of Missions per month) and undefined without a baseline. `[NON-GOAL for MVP]`

### 5.8 Roles and Access
**Description:** Three roles — Expert terrain, Chef de projet, Administrateur — as defined in the Glossary. In v1 the two founders cover all roles; the model exists so the pilot mirrors real cabinet structure.

#### FR-24: Role-based access
The system enforces the three roles' capabilities as defined in the Glossary (Expert: Relevés/Validation/Rapports; Chef de projet: Missions/assignment/review; Administrateur: users/settings/AI monitoring/templates).

### 5.9 Documentation
**Description:** User documentation is an MVP deliverable in its own right (§7.1, SM-6) — an FR so the epics workflow cannot drop it.

#### FR-26: User documentation
The product ships with French-language user documentation covering the Expert field flow (Mission → Relevé → photos → sync), Validation, Coupes/Schémas, and Rapport generation, sufficient for a new Expert to be productive in under one hour (cahier acceptance §9.3; measured at the SM-7 gate — see SM-6).

## 6. Non-Goals (Explicit)

- **No automated diagnosis.** The AI never produces a conclusion that stands without Expert Validation. The product performs no stability calculation and stays at mission level R0/R1 (observations + hypotheses).
- **No signature.** The Rapport is signed outside the product through the Expert's existing process. No in-app signature, drawn or certificate-based.
- **No ouvrages d'art.** Bridges, tunnels, and infrastructure are out of v1 entirely — including as a form option.
- **No commercial platform.** No multi-tenant subscription, billing, mission accounting, or pricing machinery. Commercialization is gated behind personal validation.
- **No automated legal-expertise reports.** Rapports d'expertise judiciaire are out of scope.
- **No claimed AI moat.** The CV model and learning loop are enablers, not the product's defensible asset; the PRD makes no accuracy promise beyond the acceptance thresholds.

## 7. MVP Scope

### 7.1 In Scope
- Mobile app (iOS + Android), French-language, offline-first, tested on the pilot Experts' actual devices plus representative iOS/Android models chosen at architecture time (§10 Q4).
- Web back-office: Mission management, supervision, administration, AI dashboard.
- All features §5.1–§5.9 at the depth stated in their FRs.
- Buildings only: collective housing, commercial/ERP, industrial.
- Two pilot users (the founders) on their real Missions.
- User documentation as a deliverable (FR-26) — sufficient for a new Expert to be productive in under one hour (SM-6, cahier acceptance §9.3).

### 7.2 Out of Scope for MVP
- Segmentation masks for localization (bounding boxes suffice) — deferred to v2.
- In-product retraining pipeline — retraining stays a manual technical-team process.
- RGPD/data-protection **machinery** (DPIA, consent flows, subject-access tooling) — deferred: the POC runs exclusively on the founders' own Missions. **But the data is not the founders':** the photos capture third parties' occupied buildings, GPS-stamped and reused as training data from v1, so a v1 data minimum applies now: (a) the founders' mission engagement letters carry a data-basis note covering photo capture and training-data reuse; (b) geolocation is stripped or coarsened in training-dataset exports (FR-22); (c) the FR-22 export filters out personal data — faces, license plates, interiors identifying occupants. `[NOTE FOR PM]` Full machinery remains a hard gate before any external user or commercial pilot: hosting, retention, consent for training-data reuse, and photo ownership must be resolved first.
- Multi-language product UI — French only.
- Third-party integrations (insurers, syndics, cadastre) — none.

## 8. Success Metrics

**Primary**
- **SM-1: Deskwork compression.** Deskwork hours per Mission fall to **≤50% of a documented pre-tool baseline**, sustained over at least 5 real Missions `[ASSUMPTION: N=5 and the time-logging method — founders log deskwork hours per Mission against a baseline documented before first pilot use]`. Target horizon end of 2026, **provisional pending the Q7 timeline decision**. This is the proof gate: does it cut the founders' own deskwork in half, on real missions? Validates FR-11–FR-20.
- **SM-2: Field entry speed.** Time to capture a standard Relevé `[ASSUMPTION: benchmarked on the pilot Experts' typical collective-housing Mission]` is ≥30% below a documented pre-tool baseline, measured with the same time-logging method as SM-1 (cahier acceptance §9.1). Validates FR-4–FR-10.
- **SM-3: Detection quality.** On the frozen, versioned test set — seed corpus plus held-out pilot-mission data, including a slice blind-labeled by the second founder — fissures and corrosion reach **>75% precision and ≥60% recall** `[ASSUMPTION: recall floor — see FR-11]` (cahier acceptance §9.2 for precision). Validates FR-11.

**Secondary**
- **SM-4: Validation ergonomics.** Median Détection's **detection fields** processed in <5 seconds (cahier acceptance §9.2). Drafted cause/recommendation text is excluded from this metric by design (FR-12/FR-13) — speed on that step is not a goal. Validates FR-13.
- **SM-5: Data durability.** Zero data loss across offline capture, sync (≤30s after reconnection), and force-close, over the entire pilot (cahier acceptance §9.1/§9.3). Validates FR-7.
- **SM-6: Time-to-productive (measured at the SM-7 gate).** A recruited non-founder Expert `[ASSUMPTION: a colleague under NDA performing a scripted cold-start session]` is productive in under one hour using only the documentation (cahier acceptance §9.3). No "new Expert" exists inside v1's user universe (§3.2), so this metric gates v2 readiness, not v1. Validates FR-26.
- **SM-7: Commercial signal (post-pilot gate).** At least 3 target-segment experts commit behaviorally — signed pilot agreements, prepaid pilots, or LOIs with pricing attached `[ASSUMPTION: N=3; expressed willingness to pay does not count]`. Gates commercialization; validates nothing in v1 — it decides v2.

**Counter-metrics (do not optimize)**
- **SM-C1: Rubber-stamp rate.** If the Expert correction+rejection rate falls below 5% over a rolling window of 50 Détections `[ASSUMPTION: floor and window — set so the counter-metric has a trigger, not just a sentiment]`, the Administrateur (owner) reviews a sample of recent Validations against the photos before the next Rapport ships. A silent counter-metric is decoration; this one has a floor, an owner, and an action. Counterbalances SM-3 and SM-4.
- **SM-C2: Rapport compliance.** Speed gains (SM-1, SM-2) must never degrade the Rapport's regulatory completeness — the §9.1 layout criteria and the décret content blocks (FR-17) are a floor, not a trade-off.

## 9. Cross-Cutting NFRs

- **Offline-first (hard constraint):** full Mission + Relevé + photos with no network; sync with zero loss within 30s of reconnection; durability across force-close.
- **Field ergonomics:** any key feature in ≤3 taps; high contrast; ≥14pt type; one-handed operation for common actions; no waiting on local entry. The following are **UX-spec-owned qualities** — the downstream UX workflow defines their done-ness: explicit icons, dark mode, haptic feedback on important actions, contextual guidance via in-app help (aide en ligne), tooltips, and clear error messages.
- **Language:** product UI, forms, AI-drafted text, and Rapports entirely in French.
- **Platform:** iOS + Android mobile; web back-office; Bluetooth stylus usable as generic touch input (FR-16).
- **Trust surface:** every AI output visibly marked as proposed-not-validated until Validation; Score de confiance always displayed and visibly scoped to the detection — never presented as confidence in Sévérité, causes, or recommendations.
- **Security (POC floor — the cahier's missing security sections are formally superseded by this block plus the architecture spine):** all surfaces require authentication; on-device data (survey, photos) is encrypted at rest — a phone carries a complete building diagnostic; FR-20 share links are expiring, revocable, single-purpose, and every share event is logged; pilot data is retained for the duration of the pilot and its legal retention obligations, with disposition decided at the RGPD gate (§10 Q3). Everything beyond this floor is deferred with that same gate.

## 10. Open Questions

1. **Cahier des Charges §4–8 are missing** from the file on disk (presumed to cover technical requirements, security, and planning). They are now **formally superseded**: this PRD (including the §9 security floor) plus the architecture spine are authoritative; if a complete cahier ever surfaces, differences are reconciled via change control, not silently absorbed.
2. **Official report template by arrêté** (décret 2025-814): publication date unknown; the Rapport template system (FR-19) must absorb it. Contingency for material structural divergence is stated at FR-19. Owner: PM watch.
3. **RGPD & data governance** — machinery deferred, with the v1 data minimum now in scope (§7.2: engagement-letter basis note, geo-stripped training exports, personal-data filter). Full machinery remains a phase-blocker before any external pilot. Owner: PM; revisit at SM-7 gate.
4. **The device test list** — the pilot Experts' actual devices plus representative iOS/Android models, named at architecture time.
5. **Retraining cadence** — "periodic" is unquantified; set after first dataset export.
6. **Pricing model** (per-month vs per-report) — deliberately open until SM-7; belongs to the commercialization decision, not v1.
7. **Timeline & budget** — per the brief, to be set once the v1 scope is locked; this PRD is that trigger. Owner: founders, at PRD sign-off.

## 11. Assumptions Index

- §3.3 UJ-1 — persona name "Marc" stands in for the associate/founding Expert.
- §3.3 UJ-2 — Détection Validation available on both mobile and web surfaces.
- §3.3 UJ-3 — persona name "Nadia" stands in for the second founding Expert; the two founders cover all three roles in v1.
- §5.1 FR-3 — Ouvrage history is matched by address; same Ouvrage under variant addresses is out of v1.
- §5.2 FR-5 — 200 ms local-entry response threshold makes "no waiting on local entry" testable.
- §5.2 FR-6 — checklist configuration is back-office-side; the field app consumes it.
- §5.4 FR-11 — localization uses bounding boxes in v1; segmentation deferred.
- §5.4 FR-11 — recall floor 60% for fissures/corrosion: no cahier criterion exists; set so the metric cannot reward a model that misses most désordres.
- §5.4 FR-11 — analysis SLA: a standard synced Relevé is analyzed within 1 hour (UJ-2's "next morning" made testable).
- §5.4 FR-12 — causes/recommendations generated via language-model assist, not CV; mechanism is an architecture decision.
- §5.6 FR-17 — content floors for rapport de visite and fiche de désordre unitaire drafted by PM in the addendum; founders to confirm.
- §5.6 FR-20 — "secure link" means an expiring authenticated URL.
- §5.7 corpus gate — 150 labeled examples per category across ≥2 capture devices before FR-11 acceptance testing; values set to make the cold-start bet falsifiable.
- §5.7 FR-22 — no in-product training pipeline; retraining is a manual technical-team process at unspecified cadence.
- §8 SM-1 — N=5 Missions and founder time-logging against a pre-tool baseline as the measurement method.
- §8 SM-2 — "standard Relevé" benchmarked on the pilot Experts' typical collective-housing Mission.
- §8 SM-6 — measured via a recruited non-founder colleague under NDA in a scripted cold-start session, at the SM-7 gate.
- §8 SM-7 — behavioral gate of ≥3 signed pilot agreements / prepaid pilots / priced LOIs.
- §8 SM-C1 — rubber-stamp floor: correction+rejection rate <5% over a rolling 50 Détections triggers Administrateur review.
