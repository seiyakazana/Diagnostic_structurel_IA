---
title: "PRD: Diagnostic Structurel IA"
status: final
created: 2026-07-23
updated: 2026-07-27
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

Décret n°2025-814 (12 August 2025, loi Habitat dégradé) makes structural diagnosis of collective housing mandatory in commune-designated zones, performed by qualified engineers whose capacity is capped by the manual deskwork ceiling. Demand is rising against unchanged throughput. Meanwhile, CV defect detection is mature enough to assist (binary crack detection is commoditized), and no francophone-native, phone-photos-in / draft-report-out tool exists. Generic field-inspection SaaS (PlanRadar and peers) manages photos and snag lists, and the AI-detection players are enterprise, drone, or 3D-scan oriented — but none produce the compliant structural deliverable, draft the diagnostic narrative, or map to the coming official French report template (see addendum).

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
  - **Path:** reviews Détections one by one — nature, localization overlay, severity, confidence score, plus AI-drafted cause hypotheses and preliminary recommendations as editable text → validates, corrects, or rejects each in under 5 seconds → adds two Désordres the AI missed → selects the relevant Coupe types from the library; numbered survey points map automatically onto the Coupe, cross-referenced to each Fiche de désordre → sketches one annotated Schéma of the worst Zone.
  - **Climax:** generates the PDF Rapport — cover page with cabinet logo, TOC, per-Zone synthesis, annotated photos, Coupes and Schéma, pagination — and reads a document that is essentially finished.
  - **Resolution:** he signs via his existing external process and sends the Rapport by email or secure link. His corrections are already stored as training data.

- **UJ-3. The Chef de projet runs the pipeline without touching a wall.**
  - **Persona + context:** Nadia `[ASSUMPTION: persona name]`, the second founding Expert, acting as Chef de projet `[ASSUMPTION: in v1 the two founders cover all three roles]`.
  - **Path (one line):** creates the Mission in the back-office (building info, structure type, usage, past-mission history) → assigns the Expert → after UJ-2, reviews the generated Rapport → consults the AI performance dashboard to see whether last month's corrections improved precision.

## 4. Glossary

*Downstream workflows must use these terms exactly. French domain terms are kept in French — they are the product's native vocabulary.*

- **Ouvrage** — the built structure a Mission targets. In v1, buildings only: collective housing, commercial/ERP, industrial.
- **Mission** — one diagnostic engagement on one Ouvrage. Carries Ouvrage info (address, structure type, estimated construction date, usage), assigned Experts, configurable checklist, and history of past Missions on the same Ouvrage. Contains one or more Relevés.
- **Relevé** — the structured field survey of a Mission: observations per Zone and Élément structurel, photos, values. Captured offline-first.
- **Zone** — a spatial subdivision of the building under survey (e.g. basement, façade nord).
- **Élément structurel** — a structural member category within a Zone: fondations, planchers, poteaux, poutres, voiles, façades, toiture.
- **Désordre** — an observed structural disorder. Classified into six evolving categories, each with named sub-disorders (liste évolutive, full taxonomy in the addendum): fissuration (fissures de retrait, fissures structurelles, lézardes, microfissures); corrosion/carbonatation (épaufrures, armatures apparentes, taches de rouille, bullage); humidité/infiltrations (remontées capillaires, moisissures, efflorescences, salpêtres); déformations (flambement, désaffleurements, tassements différentiels, dévers); désordres maçonnerie (déliaisonnements, décollements, joints dégradés, faïençage); dégradations diverses (éclatements, défauts d'enrobage, cavités, impacts).
- **Détection** — one AI-proposed Désordre on one photo: nature (category + sub-category), localization overlay, Sévérité, Score de confiance, plus AI-drafted cause hypotheses and preliminary recommendations. Always subject to Validation.
- **Sévérité** — the four-level grading of a Désordre: léger / modéré / sévère / critique.
- **Score de confiance** — the AI's confidence in a Détection, displayed as a percentage.
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
A Chef de projet or Expert can create a Mission with Ouvrage info: address, structure type, estimated construction date, and usage (résidentiel / ERP / industriel). Realizes UJ-3.
**Consequences (testable):**
- A Mission cannot be created without address and structure type.
- The usage list contains exactly the three v1 values; no infrastructure option is present.
- A Mission can be created on the mobile app with no network (cahier acceptance §9.1).

#### FR-2: Expert assignment
A Chef de projet can assign one or more Experts to a Mission; only assigned Experts can edit its Relevés.

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

### 5.3 Photo Capture and Annotation
**Description:** Photos are captured from within the form so context is never lost: each photo auto-associates to its Élément structurel and Zone. The Expert annotates on-image in the field or later. Realizes UJ-1.

#### FR-9: In-form photo capture
An Expert can open the camera from within the survey form; the photo auto-associates to the current Élément structurel and Zone, with timestamp and geolocation in metadata. Gallery import is also supported.

#### FR-10: On-image annotation
An Expert can annotate a photo with arrows, circles, and text, on-device, including offline.

### 5.4 AI Disorder Analysis (Détection and Validation)
**Description:** Synced photos are analyzed automatically. The AI proposes Détections across the six Désordre categories; the Expert disposes of each one. Cause hypotheses and preliminary recommendations ship in v1 as **AI-drafted editable text**, not CV claims — the Expert edits or deletes them like any draft. Nothing enters a Rapport unvalidated. Realizes UJ-2.

#### FR-11: Automatic Désordre detection
The system analyzes each synced photo and proposes Détections classified into the six categories and their sub-disorders (per the Glossary taxonomy; full evolving list in the addendum), each with nature (category + sub-category), localization overlay on the image `[ASSUMPTION: bounding box in v1; segmentation masks deferred]`, Sévérité, and Score de confiance displayed as a percentage.
**Consequences (testable):**
- For fissures and corrosion, model precision exceeds 75% on the dedicated test set (cahier acceptance §9.2).
- Every Détection displays its Score de confiance.

#### FR-12: AI-drafted causes and recommendations
For each Détection, the system drafts cause hypotheses ("hypothèses d'origine probable") and preliminary surveillance/intervention recommendations as editable text clearly marked as AI-proposed. `[ASSUMPTION: generated via language-model assist over the Détection + Relevé context, not via CV — mechanism belongs to architecture]`
**Out of Scope:** any form of stability calculation or definitive causal diagnosis.

#### FR-13: Expert Validation
An Expert can validate, correct (any field, including Sévérité, nature, causes, recommendations), or reject each Détection, and can manually add Désordres the AI missed. Realizes UJ-2.
**Consequences (testable):**
- A single Détection can be processed (validate/correct/reject) in under 5 seconds (cahier acceptance §9.2).
- No unvalidated Détection can appear in a Rapport.

#### FR-14: Correction capture for learning
Every Validation outcome (including corrections and manual additions) is stored as structured training data with full traceability: Expert, date, Mission, building.
**Consequences (testable):**
- Corrections are exportable as a retraining dataset (cahier acceptance §9.2).

### 5.5 Coupes and Schémas
**Description:** The Rapport's visual backbone. The Expert selects Coupe types from a preconfigured library and the system maps numbered survey points onto them automatically, cross-referenced to each Fiche de désordre. A simplified sketch tool covers what the library cannot. Realizes UJ-2.

#### FR-15: Coupe library and Désordre mapping
An Expert can select Coupe types from a preconfigured library (mur, plancher, fondation, toiture, poteau-poutre, …) and position Désordres on them — directly in the mobile app, in the field, as well as from the back-office; numbered survey points map automatically onto the selected Coupe with cross-reference to the corresponding Fiche de désordre.
**Consequences (testable):**
- Désordre numbering on Coupe/Schéma is consistent with the Relevé and the associated Fiche de désordre (cahier acceptance §9.1).

#### FR-16: Sketch tool
An Expert can draw a Schéma with freehand strokes, geometric shapes, legends, and dimensions (cotations), with Bluetooth stylus support.

### 5.6 Rapport Generation and Export
**Description:** The payoff feature: the compliant PDF assembled from everything upstream. Template-driven, offline-initiable, and layout-stable regardless of content volume. The Rapport leaves the product unsigned; the Expert signs through their existing external process. Realizes UJ-2.

#### FR-17: PDF Rapport generation
An Expert can generate a structured PDF Rapport with customizable cover page (cabinet logo, mission title, date, Expert), automatic TOC, pagination, per-Zone synthesis, and photo annexes. Content depends on the selected template (FR-19): the **rapport de diagnostic complet** contains all Ouvrage info, checklists, values, observations, annotated photos, at least one relevant Coupe, and an annotated Schéma of the diagnosed Zone; the rapport de visite and fiche de désordre unitaire carry their template's deliberately partial content floor.
**Consequences (testable):**
- The rapport de diagnostic complet includes every piece of entered data, the Expert's identity, and the credentials/insurance block the décret requires on the deliverable (cahier acceptance §9.1; décret 2025-814 — see addendum).
- Layout (page de garde, sommaire, pagination) remains readable regardless of photo/Schéma volume (cahier acceptance §9.1).

#### FR-18: Offline generation, finalized at sync
An Expert can request Rapport generation from the field while offline, including mid-Mission on an in-progress Relevé; the on-device render assembles all locally captured data, and AI-derived content (Détections, drafted text) integrates when the PDF is finalized at sync.

#### FR-19: Rapport templates
An Administrateur can manage configurable Rapport templates; v1 ships three: rapport de visite, rapport de diagnostic complet, fiche de désordre unitaire.
**Notes:** `[NOTE FOR PM]` The décret's official report template arrives by arrêté; the template system must absorb it when published (see §10 Open Questions).

#### FR-20: Export and sharing
An Expert can export a Rapport locally, send it by email, or share it via secure link. `[ASSUMPTION: "secure link" = expiring authenticated URL; mechanism belongs to architecture]`

### 5.7 Data Management and Learning Loop
**Description:** Every validated Désordre becomes an asset. The structured database feeds periodic retraining; the dashboard tells the Administrateur whether the model is actually getting better. The AI cold start is mitigated by the founders' in-house corpus — their own site photos, past reports, and worked examples serve as the seed labeled dataset, annotated by the founding Expert himself (see addendum); SM-3's precision target is measured against a test set drawn from this corpus plus pilot-mission data. Realizes UJ-3.

#### FR-21: Structured Désordre database
The system stores all images and annotations classified by structure type, Désordre nature, Sévérité, and geographic location, with full traceability (Expert, date, Mission, building).

#### FR-22: Dataset export for retraining
An Administrateur can export the validated dataset for retraining. Retraining itself is performed offline by the technical team, validated on a dedicated test set before deployment. `[ASSUMPTION: retraining cadence unspecified — "periodic"; no in-product training pipeline in v1]`

#### FR-23: AI performance dashboard
An Administrateur can view precision/recall/F1 per Désordre category, plus model version history and comparison, and receives performance-drift alerts.

### 5.8 Roles and Access
**Description:** Three roles — Expert terrain, Chef de projet, Administrateur — as defined in the Glossary. In v1 the two founders cover all roles; the model exists so the pilot mirrors real cabinet structure.

#### FR-24: Role-based access
The system enforces the three roles' capabilities as defined in the Glossary (Expert: Relevés/Validation/Rapports; Chef de projet: Missions/assignment/review; Administrateur: users/settings/AI monitoring/templates).

## 6. Non-Goals (Explicit)

- **No automated diagnosis.** The AI never produces a conclusion that stands without Expert Validation. The product performs no stability calculation and stays at mission level R0/R1 (observations + hypotheses).
- **No signature.** The Rapport is signed outside the product through the Expert's existing process. No in-app signature, drawn or certificate-based.
- **No ouvrages d'art.** Bridges, tunnels, and infrastructure are out of v1 entirely — including as a form option.
- **No commercial platform.** No multi-tenant subscription, billing, mission accounting, or pricing machinery. Commercialization is gated behind personal validation.
- **No automated legal-expertise reports.** Rapports d'expertise judiciaire are out of scope.
- **No claimed AI moat.** The CV model and learning loop are enablers, not the product's defensible asset; the PRD makes no accuracy promise beyond the acceptance thresholds.

## 7. MVP Scope

### 7.1 In Scope
- Mobile app (iOS + Android), French-language, offline-first, tested on the 5 most common smartphone models among the pilot Experts.
- Web back-office: Mission management, supervision, administration, AI dashboard.
- All features §5.1–§5.8 at the depth stated in their FRs.
- Buildings only: collective housing, commercial/ERP, industrial.
- Two pilot users (the founders) on their real Missions.
- User documentation as a deliverable — sufficient for a new Expert to be productive in under one hour (SM-6, cahier acceptance §9.3).

### 7.2 Out of Scope for MVP
- Segmentation masks for localization (bounding boxes suffice) — deferred to v2.
- In-product retraining pipeline — retraining stays a manual technical-team process.
- RGPD/data-protection machinery — deferred entirely: the POC runs exclusively on the founders' own Missions. `[NOTE FOR PM]` This is a hard gate before any external user or commercial pilot: hosting, retention, consent for training-data reuse, and photo ownership must be resolved first.
- Multi-language product UI — French only.
- Third-party integrations (insurers, syndics, cadastre) — none.

## 8. Success Metrics

**Primary**
- **SM-1: Deskwork compression.** End-to-end turnaround on the founders' real Missions drops from ~3.5 days toward ~1.5 days (~2× throughput) by end of 2026. The near-term proof gate: *does it cut the founder's own deskwork in half, on real missions?* Validates FR-11–FR-20.
- **SM-2: Field entry speed.** Time to capture a standard Relevé `[ASSUMPTION: benchmarked on the pilot Experts' typical collective-housing Mission]` is ≥30% below the current process, validated by the pilot Experts (cahier acceptance §9.1). Validates FR-4–FR-10.
- **SM-3: Detection quality.** >75% precision on fissures and corrosion on the dedicated test set, built from the founders' seed corpus plus pilot-mission data (cahier acceptance §9.2). Validates FR-11.

**Secondary**
- **SM-4: Validation ergonomics.** Median Détection processed in <5 seconds (cahier acceptance §9.2). Validates FR-13.
- **SM-5: Data durability.** Zero data loss across offline capture, sync (≤30s after reconnection), and force-close, over the entire pilot (cahier acceptance §9.1/§9.3). Validates FR-7.
- **SM-6: Time-to-productive.** A new Expert is productive in under one hour using only the documentation (cahier acceptance §9.3). Validates FR-4, FR-13.
- **SM-7: Commercial signal (post-pilot gate).** A handful of target-segment experts express willingness to pay. Gates commercialization; validates nothing in v1 — it decides v2.

**Counter-metrics (do not optimize)**
- **SM-C1: Rubber-stamp rate.** If the Expert correction/rejection rate trends toward zero, treat it as a red flag (automation complacency), not a success. Counterbalances SM-3 and SM-4.
- **SM-C2: Rapport compliance.** Speed gains (SM-1, SM-2) must never degrade the Rapport's regulatory completeness — the §9.1 layout and content criteria are a floor, not a trade-off.

## 9. Cross-Cutting NFRs

- **Offline-first (hard constraint):** full Mission + Relevé + photos with no network; sync with zero loss within 30s of reconnection; durability across force-close.
- **Field ergonomics:** any key feature in ≤3 taps; high contrast; ≥14pt type; explicit icons; one-handed operation for common actions; dark mode; haptic feedback on important actions; no waiting on local entry; contextual guidance via in-app help (aide en ligne), tooltips, and clear error messages.
- **Language:** product UI, forms, AI-drafted text, and Rapports entirely in French.
- **Platform:** iOS + Android mobile; web back-office; Bluetooth accessory compatibility (stylus, external keyboard).
- **Trust surface:** every AI output visibly marked as proposed-not-validated until Validation; Score de confiance always displayed.

## 10. Open Questions

1. **Cahier des Charges §4–8 are missing** from the file on disk (presumed to cover technical requirements, security, and planning). Does a complete version exist? Until then, this PRD + brief addendum are authoritative.
2. **Official report template by arrêté** (décret 2025-814): publication date unknown; the Rapport template system (FR-19) must absorb it. Owner: PM watch.
3. **RGPD & data governance** — deferred by decision (POC on founders' own missions only); becomes a phase-blocker before any external pilot. Owner: PM; revisit at SM-7 gate.
4. **The "5 most common smartphones"** test list is undefined — pilot Experts to name their devices.
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
- §5.4 FR-12 — causes/recommendations generated via language-model assist, not CV; mechanism is an architecture decision.
- §5.6 FR-20 — "secure link" means an expiring authenticated URL.
- §5.7 FR-22 — no in-product training pipeline; retraining is a manual technical-team process at unspecified cadence.
- §8 SM-2 — "standard Relevé" benchmarked on the pilot Experts' typical collective-housing Mission.
