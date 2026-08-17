---
stepsCompleted: ['step-01-validate-prerequisites', 'step-02-design-epics', 'step-03-create-stories', 'step-04-final-validation']
inputDocuments:
  - _bmad-output/planning-artifacts/prds/prd-Diagnostic_structurel_IA_v2-2026-07-23/prd.md
  - _bmad-output/planning-artifacts/prds/prd-Diagnostic_structurel_IA_v2-2026-07-23/addendum.md
  - _bmad-output/planning-artifacts/architecture/architecture-Diagnostic_structurel_IA_v2-2026-07-27/ARCHITECTURE-SPINE.md
  - _bmad-output/specs/spec-diagnostic-structurel-ia/SPEC.md
  - _bmad-output/specs/spec-diagnostic-structurel-ia/acceptance-criteria.md
---

# Diagnostic_structurel_IA_v2 - Epic Breakdown

## Overview

This document provides the complete epic and story breakdown for Diagnostic_structurel_IA_v2, decomposing the requirements from the PRD, UX Design if it exists, and Architecture requirements into implementable stories.

## Requirements Inventory

### Functional Requirements

FR-1: Mission creation — a Chef de projet or Expert can create a Mission with Ouvrage info (address, structure type, estimated construction date, usage résidentiel/ERP/industriel) and recent-works history (décret-mandated, editable during the Mission). Address and structure type are mandatory; creation works offline on mobile; empty recent-works history renders in the Rapport as an explicit "aucuns travaux récents signalés" statement.

FR-2: Expert assignment — a Chef de projet can assign one or more Experts to a Mission; only assigned Experts can edit its Relevés. A Relevé is editable on exactly one assigned device at a time; ownership transfers only after the current owner's changes have synced (conflicts prevented, never merged).

FR-3: Ouvrage history — when a Mission concerns an Ouvrage with prior Missions (matched by address), the app surfaces those past Missions to assigned Experts and the Chef de projet.

FR-4: Dynamic guided forms — an Expert completes a Relevé through a form adapted to the Mission's structure type, organized by Zone and Élément structurel (fondations, planchers, poteaux, poutres, voiles, façades, toiture). Changing structure type changes the form sections; every key survey action reachable in ≤3 taps.

FR-5: Rapid field entry — observations via checkboxes, 1–5 intensity sliders, dropdowns, and free text; local entry never blocks on network; UI responds to any field input within 200 ms.

FR-6: Configurable checklists — a Chef de projet or Administrateur can configure the checklist attached to a Mission before or during the engagement (back-office-side configuration; the field app consumes it).

FR-7: Offline capture with deferred sync — an Expert can create and complete an entire Relevé (forms, photos, sketches) with no network; a full offline Relevé syncs with zero data loss within 30 seconds of reconnection; force-close loses no data.

FR-8: Automatic geolocation — each survey entry and photo is automatically stamped with geolocation and timestamp; with no GPS fix, capture proceeds without blocking, stamped last-known-approximate or no location.

FR-9: In-form photo capture — camera opens from within the survey form; the photo auto-associates to the current Élément structurel and Zone with timestamp and geolocation metadata; gallery import supported.

FR-10: On-image annotation — arrows, circles, and text drawn on-device, including offline; annotations round-trip legibly and positioned as drawn into the exported Rapport.

FR-11: Automatic Désordre detection — each synced photo is analyzed and Détections proposed across the six categories and sub-disorders, each with nature (category + sub-category), bounding-box localization overlay, proposed Sévérité, and Score de confiance shown as a percentage. Fissuration and corrosion carry the accuracy commitment (>75% precision, ≥60% recall on the frozen, versioned test set); other four categories best-effort. Every photo shows analysis status pending/done/failed/no-findings ("no findings" always distinguishable from "not yet analyzed"). Analysis of a standard synced Relevé completes within 1 hour; analysis-service unavailability never blocks capture, manual Désordre entry, or Validation of already-analyzed photos. Corpus gate: acceptance testing begins only at ≥150 usable labeled examples per category across ≥2 capture devices; fallback v1 scope is fissuration + corrosion only.

FR-12: AI-drafted causes and recommendations — for each Détection the system drafts cause hypotheses and preliminary recommendations as editable text clearly marked AI-proposed, structured into the décret's three blocks (investigations complémentaires recommandées, mesures conservatoires, travaux hiérarchisés). Drafted text is a separate, later-bound artifact generated per Désordre after detection; reviewed through its own affordance with no speed target. No stability calculation or definitive causal diagnosis.

FR-13: Expert Validation — validate, correct (any field), or reject each Détection; manually add Désordres the AI missed. Detection fields (nature, localization, Sévérité) processable in <5 seconds per Détection; drafted text excluded from that budget. No unvalidated Détection in any Rapport; drafted text appears only after explicit accept/edit — accepting detection fields does not silently accept drafted text.

FR-14: Correction capture for learning — every Validation outcome (corrections and manual additions included) stored as structured training data with full traceability (Expert, date, Mission, building); exportable as a retraining dataset passing the §7.2 personal-data and geolocation filters. Distinct from the legal provenance record (FR-25).

FR-15: Coupe library and Désordre positioning — select Coupe types from a preconfigured library (mur, plancher, fondation, toiture, poteau-poutre, …) and position Désordres on them, from mobile in the field and from the back-office. The system assigns each positioned point its survey number and maintains the cross-reference to the Fiche de désordre; the Expert never types a number. Numbering is consistent across Relevé, Coupes, and Fiches; insert/remove renumbers consistently with no manual re-entry.

FR-16: Sketch tool — freehand strokes, geometric shapes, legends, and dimensions (cotations); finger drawing required, Bluetooth stylus as generic touch; a Schéma round-trips into the Rapport legibly with legends and cotations intact.

FR-17: PDF Rapport generation — structured PDF with customizable cover (cabinet logo, mission title, date, Expert), automatic TOC, pagination, per-Zone synthesis, photo annexes. The rapport de diagnostic complet contains every piece of entered data, the Expert's credentials/insurance block, and every décret-mandated content block (building description; elements examined and observed désordres; recent-works history and stability impact; recommended investigations, mesures conservatoires, prioritized works — Expert-validated); empty blocks render as explicit statements, never silent omissions. Layout (page de garde, sommaire, pagination) stays readable at any photo/Schéma volume. Rapport de visite and fiche de désordre unitaire carry the addendum's content floors.

FR-18: Offline generation, finalized at sync — Rapport generation requestable from the field offline, including mid-Mission on an in-progress Relevé; on-device render assembles all local data; AI-derived content integrates at sync-time finalization. Offline render watermarked "PROVISOIRE — non finalisé" on every page and blocked from every FR-20 path; finalized PDF supersedes provisional automatically at sync; provisional-vs-final state unambiguous to the Expert; both renders obey FR-13 gates.

FR-19: Rapport templates — an Administrateur manages configurable templates; v1 ships three: rapport de visite, rapport de diagnostic complet, fiche de désordre unitaire. The template system must absorb the coming official arrêté template; material divergence triggers a scoped change request (correct-course), not silent rework.

FR-20: Export and sharing — export locally, send by email, or share via secure link (expiring, revocable, authenticated URL); only finalized Rapports pass any export path; every share event recorded.

FR-21: Structured Désordre database — all images and annotations stored classified by structure type, Désordre nature, Sévérité, and geographic location, with full traceability (Expert, date, Mission, building).

FR-22: Dataset export for retraining — an Administrateur exports the validated dataset for retraining; retraining is offline, manual, technical-team-performed, gated on a dedicated test set before deployment. Exports strip or coarsen geolocation and filter personal data (faces, license plates, occupant-identifying interiors).

FR-23: AI performance dashboard — per Désordre category: validation-agreement statistics (validate/correct/reject rates, labeled agreement not accuracy) and true precision/recall on the frozen test set per model version, plus model version history. Descoped to v2: cross-version comparison views and drift alerts.

FR-24: Role-based access — the three roles enforced as defined (Expert: Relevés/Validation/Rapports; Chef de projet: Missions/assignment/review; Administrateur: users/settings/AI monitoring/templates).

FR-25: Rapport provenance record — for every generated Rapport, an immutable, append-only provenance record retained with the Mission: which content was AI-drafted, what the Expert corrected or authored, Validation timestamps and acting Expert per Détection and drafted-text block. Reconstructs, per Désordre, the AI's original proposal and the Expert's final validated version, with actor and timestamp; post-finalization corrections create new entries, never rewrites.

FR-26: User documentation — French-language documentation covering the Expert field flow (Mission → Relevé → photos → sync), Validation, Coupes/Schémas, and Rapport generation, sufficient for a new Expert to be productive in under one hour.

### NonFunctional Requirements

NFR-1: Offline-first (hard constraint) — full Mission + Relevé + photos with no network; sync with zero loss within 30 s of reconnection; durability across force-close (acceptance §9.1.b, §9.3.b; SM-5: zero data loss over the entire pilot).

NFR-2: Field ergonomics — any key feature in ≤3 taps; high contrast; ≥14 pt type; one-handed operation for common actions; no waiting on local entry (≤200 ms response). UX-spec-owned qualities (explicit icons, dark mode, haptics, in-app help, tooltips, clear error messages) have no UX spec in this project — they must be carried as story-level acceptance criteria or a UX workflow run later.

NFR-3: Language — product UI, forms, AI-drafted text, and Rapports entirely in French; planning documents in English.

NFR-4: Platform — iOS + Android mobile; web back-office; Bluetooth stylus usable as generic touch input; tested on the pilot Experts' actual devices plus representative models (5-device matrix, acceptance §9.3.a — list still open).

NFR-5: Trust surface — every AI output visibly marked proposed-not-validated until Validation; Score de confiance always displayed and visibly scoped to the detection only — never presented as confidence in Sévérité, causes, or recommendations.

NFR-6: Security (POC floor) — all surfaces authenticated; on-device data encrypted at rest; share links expiring, revocable, single-purpose, every share logged; pilot data retained per pilot duration + legal obligations, disposition decided at the RGPD gate.

NFR-7: Performance budgets — local entry ≤200 ms; Détection detection-fields processing <5 s median (SM-4); analysis of a standard Relevé ≤1 hour; sync ≤30 s after reconnection.

NFR-8: EU residency — Postgres, object storage, sync service, and inference service EU-hosted (AD-16); the LLM drafting path is the single documented exception, revisited at the SM-7 gate.

NFR-9: Onboarding — an untrained Expert productive in <1 hour using only the documentation (acceptance §9.3.c; measured at the SM-7 gate).

NFR-10: Detection quality gate — >75% precision and ≥60% recall for fissuration and corrosion on the frozen, versioned test set (acceptance §9.2.a), test set frozen before model tuning, including a blind-labeled slice by the second founder; the only deployment gate for retrained models.

NFR-11: v1 data minimum (RGPD) — engagement letters carry a data-basis note covering photo capture and training reuse (process, not code); geolocation stripped or coarsened in training exports; personal-data filter on FR-22 exports. Full RGPD machinery is a hard gate before any external user.

NFR-12: Compliance floor (SM-C2) — speed gains never degrade Rapport regulatory completeness; §9.1 layout criteria and décret content blocks are a floor, not a trade-off.

### Additional Requirements

**From the Architecture spine — these shape epic sequencing and story content:**

- **No starter template.** Greenfield monorepo scaffolded to the spine's normative source tree: `packages/` (domain, rapport_document, rapport_renderer, application, adapters_sync, adapters_storage, adapters_inference), `apps/` (mobile, backoffice), `services/` (api, inference, render_worker), `authz/`, `reference-data/`. This is Epic 1 Story 1 material.
- **Package purity (AD-13):** `domain`, `rapport_document`, `rapport_renderer` import zero frameworks and compile unchanged on mobile, web, and server. Dependency direction `domain ← application ← adapters`; ports (`DraftingPort`, `InferenceJobPort`, `BlobPort`) declared in `application`.
- **Stack seed:** Flutter/Dart 3.44.7, `powersync` 2.3.2, Supabase (Postgres 15+, `eu-west-3`), `pdf` 3.13.0, Python 3.12 + FastAPI + PyTorch, `claude-opus-5` behind `DraftingPort`. Detection model architecture undecided — constrained to Apache/BSD/MIT licence (AD-11).
- **Identity & numbering (AD-4):** client-minted UUIDv7 offline on every entity; désordre display number never stored — one ordering function in `packages/domain` computes it from (zone order, élément order, capture timestamp, id); every surface calls it; AI text references désordres by id, numbering substituted at render.
- **Sync layer (AD-1, AD-2, AD-3, AD-19, AD-21):** UI reads/writes only on-device SQLite; binaries travel via attachment queue to object storage addressed by content hash over bytes-as-captured; every consumer handles `blob-pending`; single-writer Relevé ownership; additive-only migrations on synced tables (expand → migrate → contract); shared sync-health telemetry event shape (queue depth, oldest pending write, last successful sync, upload failures) from both clients to one sink.
- **Authorization (AD-15):** role/scope predicates declared once in `authz/` and generated into PowerSync sync rules and Postgres RLS; neither artifact hand-edited.
- **Reference data (AD-17):** taxonomy, coupe library, checklists, Rapport templates are versioned data in the sync set (never code enums); capturing records pin the version used; rendering resolves against the pinned version; reference data reaches the device before signal loss.
- **AI event model (AD-7, AD-8, AD-9, AD-18):** `Detection` and `Validation` are separate append-only rows; terminal Validation = last by (server_received_at, id), resolved by one function; `RapportDocument` construction rejects unvalidated content (the enforcement point — UI gates are convenience); all inference behind one async job contract (submit → subscribe → terminal state) for both CV and LLM paths; every Detection carries producing model identifier + version; model registry records licence per version.
- **Rendering (AD-5, AD-6):** one `RapportDocumentBuilder` + one renderer, pure Dart, compiled into both the app and the server render worker; parameterized by content source (local SQLite or Postgres); Coupes/Schémas persist as an SVG subset with anchors in normalized 0..1 coordinates; rasterization only inside the renderer.
- **Learning loop (AD-12):** dataset export is a projection over the Validation event log; no code writes a training record directly.
- **Egress (AD-22):** shared Rapports served via expiring, single-purpose signed URLs scoped to one Rapport version; no public object-storage path; every share audit-logged with actor, Rapport version, expiry.
- **Environments (AD-20):** dev/staging/production fully isolated (separate Postgres, buckets, sync services, no shared credentials); only production holds real mission data; mobile distribution via TestFlight / Play internal tracks; every build reports its expected schema version.
- **Conventions (AD-14):** French domain nouns, singular, unaccented snake_case for files/tables; PascalCase Dart types; English technical suffixes; typed result values for domain failures (only adapters throw); UTC TIMESTAMPTZ storage with capture time and sync time as separate columns; WGS-84 (lat, lon, accuracy_m) geolocation.
- **Expert record:** carries name, qualification level, RC Pro insurer/policy/cover — printed in the Rapport's credentials/insurance block.

### UX Design Requirements

No UX design contract exists for this project (no `ux-designs/` run, no DESIGN.md/EXPERIENCE.md pair). The PRD's UX-spec-owned qualities (NFR-2) are carried as story-level acceptance criteria where they are testable; a dedicated UX workflow run remains an open option before or during implementation.

### FR Coverage Map

FR-1: Epic 1 - Mission creation with Ouvrage info and recent-works history
FR-2: Epic 1 - Expert assignment with single-device Relevé ownership
FR-3: Epic 1 - Ouvrage history surfaced from prior Missions
FR-4: Epic 2 - Dynamic guided forms by Zone and Élément structurel
FR-5: Epic 2 - Rapid field entry (checkboxes, sliders, dropdowns, free text, ≤200 ms)
FR-6: Epic 2 - Configurable checklists (back-office config, field consumption)
FR-7: Epic 2 - Offline capture with zero-loss deferred sync
FR-8: Epic 2 - Automatic geolocation and timestamp stamping
FR-9: Epic 2 - In-form photo capture with auto-association
FR-10: Epic 2 - On-image annotation (arrows, circles, text)
FR-11: Epic 5 - Automatic Désordre detection with confidence scores
FR-12: Epic 5 - AI-drafted causes and recommendations (décret three-block structure)
FR-13: Epic 5 - Expert Validation (<5 s detection fields) and manual additions
FR-14: Epic 5 - Correction capture for learning (append-only, traceable)
FR-15: Epic 3 - Coupe library and Désordre positioning with automatic numbering
FR-16: Epic 3 - Sketch tool (freehand, shapes, legends, cotations)
FR-17: Epic 4 - PDF Rapport generation with décret content blocks
FR-18: Epic 4 - Offline provisional render, finalized at sync
FR-19: Epic 4 - Rapport template management (three v1 templates)
FR-20: Epic 4 - Export and sharing (local, email, secure link)
FR-21: Epic 5 - Structured Désordre database with full traceability
FR-22: Epic 6 - Dataset export for retraining (geo-stripped, personal-data filtered)
FR-23: Epic 6 - AI performance dashboard (agreement stats + test-set metrics)
FR-24: Epic 1 - Role-based access (Expert / Chef de projet / Administrateur)
FR-25: Epic 4 - Immutable Rapport provenance record
FR-26: Epic 6 - French user documentation (<1 h to productive)

## Epic List

### Epic 1: Project Foundation, Missions & Access
Authenticated founders can create Missions with Ouvrage info and recent-works history (offline included), assign Experts with single-device Relevé ownership, consult prior Missions on an Ouvrage, under enforced three-role access. Carries the greenfield foundation: monorepo scaffolded to the spine's source tree, isolated dev/staging/production environments, Supabase + PowerSync sync foundation, and the single authz definition generated into sync rules and RLS — delivered through Mission-management user value.
**FRs covered:** FR-1, FR-2, FR-3, FR-24

### Epic 2: Offline Guided Field Survey
UJ-1 complete: an Expert surveys a building with no signal and loses nothing — dynamic forms adapted to structure type, organized by Zone and Élément structurel, versioned checklists, ≤200 ms local entry, automatic geolocation, in-form photo capture with on-image annotation, content-hash attachment queue, zero-loss sync within 30 s of reconnection, force-close durability, sync-health telemetry.
**FRs covered:** FR-4, FR-5, FR-6, FR-7, FR-8, FR-9, FR-10

### Epic 3: Coupes, Schémas & Désordre Mapping
The Rapport's visual backbone: versioned Coupe library, Désordre positioning from mobile and back-office with system-guaranteed numbering and Fiche de désordre cross-references (the Expert never types a number; the domain ordering function is the single numbering authority), and the vector sketch tool with freehand, shapes, legends, and cotations.
**FRs covered:** FR-15, FR-16

### Epic 4: Compliant Rapport Generation & Sharing
The payoff: template-driven PDF Rapports (rapport de visite, rapport de diagnostic complet, fiche de désordre unitaire) with every décret-mandated content block and the credentials/insurance block; provisional watermarked offline render finalized automatically at sync; export, email, and expiring audited secure links; the immutable provenance record. After this epic the product is usable end-to-end on real missions with zero AI — SM-1/SM-2 measurable early.
**FRs covered:** FR-17, FR-18, FR-19, FR-20, FR-25

### Epic 5: AI Detection & Expert Validation
AI proposes, Expert disposes: asynchronous inference service behind one job contract, six-category detection with bounding boxes, proposed Sévérité and scoped confidence scores, LLM-drafted causes and recommendations behind DraftingPort, <5 s validation flow with manual Désordre additions, append-only Detection/Validation event model with model-version provenance, structured Désordre database. Validated AI content flows into the Epic 4 assembly gate already enforcing. Gated by the corpus gate: acceptance testing starts only at ≥150 labeled examples per category.
**FRs covered:** FR-11, FR-12, FR-13, FR-14, FR-21

### Epic 6: Learning Loop, Dashboard & Pilot Readiness
UJ-3 closed and the pilot ready: dataset export as a projection over the Validation event log (geolocation stripped/coarsened, personal-data filtered), AI performance dashboard with per-category agreement statistics and per-model-version test-set metrics, and the French user documentation sufficient for a new Expert to be productive in under one hour.
**FRs covered:** FR-22, FR-23, FR-26

## Epic 1: Project Foundation, Missions & Access

Authenticated founders create and manage Missions offline, assign Experts with single-device Relevé ownership, under enforced three-role access — on top of the scaffolded monorepo and isolated EU environments.

### Story 1.1: Monorepo Scaffold and Environment Foundation

As a developer,
I want the monorepo scaffolded to the spine's normative source tree with isolated EU environments,
So that every subsequent story lands in its architectural home and no test data can ever touch production.

**Acceptance Criteria:**

**Given** a clean checkout
**When** the full build and test suite runs
**Then** `packages/domain`, `packages/rapport_document`, and `packages/rapport_renderer` compile as pure Dart with zero framework imports, enforced by an automated dependency check that fails the build on violation (AD-13)
**And** the tree matches the spine's source tree: `packages/`, `apps/mobile`, `apps/backoffice`, `services/`, `authz/`, `reference-data/`

**Given** the three environments
**When** dev, staging, and production are provisioned
**Then** each has its own Supabase project (Postgres 15+, `eu-west-3`), PowerSync instance, and storage bucket with no shared credentials (AD-20)
**And** all configuration is environment-injected — no compile-time branches (AD-10)

**Given** the app shells
**When** the mobile app and back-office launch against dev
**Then** both boot with a French UI shell, and naming follows AD-14 (French domain nouns, unaccented snake_case, English technical suffixes)

### Story 1.2: Sign-In on Mobile and Back-Office

As an Expert,
I want to sign in securely on my phone and the back-office,
So that mission data — a complete building diagnostic — is protected on every surface.

**Acceptance Criteria:**

**Given** an unauthenticated user on either surface
**When** they access any screen or data
**Then** they are redirected to a French-language sign-in; no route or data is reachable unauthenticated (NFR-6)

**Given** valid credentials on mobile
**When** sign-in completes
**Then** the on-device SQLite database is initialized encrypted at rest, and a session is established

**Given** an authenticated Expert whose device is offline
**When** the app is relaunched — including after a force-close
**Then** the session persists and local data is accessible with no network call on any UI path (AD-1)

### Story 1.3: Three Roles, Defined Once, Enforced Everywhere

As an Administrateur,
I want to manage users and their roles with enforcement generated from a single authorization definition,
So that the client, sync rules, and database can never disagree about who may do what.

**Acceptance Criteria:**

**Given** role and scope predicates declared in `authz/`
**When** generation runs
**Then** PowerSync sync rules and Postgres RLS policies are both emitted from that one definition, and a CI check fails if either generated artifact was hand-edited (AD-15)

**Given** a user holding only the Expert role
**When** they attempt a Chef de projet or Administrateur capability (create users, manage templates, assign Experts)
**Then** the action is refused in the UI *and* by RLS on the server

**Given** an Administrateur
**When** they create a user and assign a role
**Then** that user's capabilities match the role on next sign-in, and the three roles exist as seeded reference data, not code enums

### Story 1.4: Mission Creation — Offline Included

As a Chef de projet or Expert,
I want to create a Mission with the Ouvrage's identity and recent-works history, even with no network,
So that a Mission can be opened at the kitchen table or in the building's basement alike.

**Acceptance Criteria:**

**Given** an authenticated user on mobile with no network
**When** they create a Mission with address and structure type
**Then** the Mission is created locally with a client-minted UUIDv7 (AD-4) and syncs with zero loss within 30 seconds of reconnection

**Given** a creation attempt missing address or structure type
**When** submitted
**Then** creation is blocked with a French validation message (FR-1)

**Given** the usage field
**When** displayed
**Then** it offers exactly résidentiel / ERP / industriel — no infrastructure option exists

**Given** a Mission's recent-works history
**When** entered at creation or edited later as facts emerge
**Then** it persists for Rapport rendering; an empty history is stored as the explicit "aucuns travaux récents signalés" state, never as silent absence

**Given** the back-office
**When** a Mission is created there
**Then** the same rules and outcomes apply

### Story 1.5: Expert Assignment and Single-Device Relevé Ownership

As a Chef de projet,
I want to assign Experts to a Mission, with each Relevé editable on exactly one device at a time,
So that concurrent-edit conflicts are prevented — never merged.

**Acceptance Criteria:**

**Given** a Chef de projet on a Mission
**When** they assign one or more Experts
**Then** only assigned Experts receive the Mission's rows through sync and can edit its content (FR-2, AD-15)

**Given** an assigned Expert on device A
**When** they start the Mission's Relevé
**Then** device A holds the single owning device-assignment (AD-2)

**Given** a second assigned Expert on device B
**When** they open the same Relevé
**Then** it is read-only with the current owner indicated; requesting ownership succeeds only after device A's changes have fully synced

**Given** device A offline with unsynced Relevé changes
**When** device B requests ownership
**Then** the transfer is refused with a clear French explanation — ownership never moves ahead of unsynced work

### Story 1.6: Ouvrage History Across Missions

As an Expert,
I want past Missions on the same building surfaced when a new Mission concerns it,
So that I walk in knowing what was observed before.

**Acceptance Criteria:**

**Given** a new Mission whose address matches an existing Ouvrage
**When** the Mission is created
**Then** it links to that same `ouvrage` record and its prior Missions are visible to assigned Experts and the Chef de projet (FR-3)

**Given** an address with no match
**When** the Mission is created
**Then** a new Ouvrage record is created

**Given** the history view
**When** consulted
**Then** past Missions appear read-only with dates and status — context, not editing

## Epic 2: Offline Guided Field Survey

UJ-1 complete: an Expert surveys a building with no signal and loses nothing.

### Story 2.1: Dynamic Guided Forms by Zone and Élément

As an Expert,
I want a survey form that adapts to the Mission's structure type, organized by Zone and Élément structurel,
So that the form guides me through the building the way I actually walk it.

**Acceptance Criteria:**

**Given** a Mission with a structure type
**When** the Expert opens its Relevé
**Then** the form presents sections adapted to that structure type, organized by Zone and Élément structurel (fondations, planchers, poteaux, poutres, voiles, façades, toiture) (FR-4)

**Given** the Mission's structure type changes
**When** the form is reopened
**Then** the presented sections change accordingly

**Given** any key survey action (add Zone, record observation, capture photo)
**When** navigating from the open Relevé
**Then** it is reachable in ≤3 taps (NFR-2)

**Given** the form's taxonomy and structure definitions
**When** the Relevé starts
**Then** they come from versioned reference data already on the device via the sync set, and the Relevé pins the reference version it uses (AD-17) — a Relevé never starts against absent reference data

### Story 2.2: Rapid Observation and Désordre Entry

As an Expert,
I want to record observations and Désordres with checkboxes, sliders, dropdowns, and free text — gloved, one-handed, without waiting,
So that field entry is faster than my paper process, not slower.

**Acceptance Criteria:**

**Given** an Élément structurel in the open Relevé
**When** the Expert records observations via checkboxes, 1–5 intensity sliders, dropdowns, or free text
**Then** every input persists locally with the UI responding within 200 ms, and no entry path ever awaits the network (FR-5, AD-1)

**Given** a Désordre observed in the field
**When** the Expert records it against an Élément
**Then** a `desordre` row is created with category and sub-category from the pinned taxonomy version, client-minted UUIDv7, and capture timestamp

**Given** recorded Désordres
**When** any surface displays them
**Then** the display number comes from the single domain ordering function — (zone order, élément order, capture timestamp, id) — and is never stored (AD-4)

### Story 2.3: Configurable Checklists

As a Chef de projet,
I want to configure the checklist attached to a Mission before or during the engagement,
So that each Mission's survey reflects what that building actually requires.

**Acceptance Criteria:**

**Given** a Chef de projet or Administrateur in the back-office
**When** they configure a Mission's checklist
**Then** the checklist is saved as versioned reference data and reaches the field app through the sync set (FR-6, AD-17)

**Given** a checklist edited mid-engagement
**When** the change syncs
**Then** a new checklist version is created; entries already captured keep the version they pinned, and new entries use the new version

**Given** the field app
**When** the Expert works the checklist
**Then** checklist items render as form entries and their outcomes persist offline like any observation

### Story 2.4: Automatic Geolocation and Timestamps

As an Expert,
I want every entry and photo stamped with time and place automatically,
So that provenance exists without me thinking about it.

**Acceptance Criteria:**

**Given** any survey entry or photo
**When** captured
**Then** it is stamped with capture timestamp (UTC, stored separately from sync time) and WGS-84 geolocation (lat, lon, accuracy_m) (FR-8)

**Given** no GPS fix (basement, stairwell)
**When** capturing
**Then** capture proceeds without blocking; the entry carries the last-known location flagged approximate, or no location if none exists — never an error, never a wait

### Story 2.5: In-Form Photo Capture

As an Expert,
I want the camera inside the survey form, each photo auto-attached to its Élément and Zone,
So that no evening is ever spent re-matching photos to disorders.

**Acceptance Criteria:**

**Given** an Élément structurel in the open Relevé
**When** the Expert opens the camera from the form and takes a photo
**Then** the photo auto-associates to that Élément and Zone with timestamp and geolocation metadata (FR-9)

**Given** a gallery image
**When** imported from the form
**Then** it associates the same way

**Given** any captured or imported photo
**When** stored
**Then** its bytes are content-hash addressed exactly as captured — no re-encoding before hashing — and travel through the attachment queue to object storage, never through the sync stream (AD-3)

**Given** a photo row whose blob has not yet uploaded or downloaded
**When** any consumer displays it
**Then** the `blob-pending` state renders explicitly — the row is valid and syncable before its blob resolves

### Story 2.6: On-Image Annotation

As an Expert,
I want to draw arrows, circles, and text on photos, offline,
So that what I saw is marked where I saw it.

**Acceptance Criteria:**

**Given** a photo in the Relevé
**When** the Expert annotates it with arrows, circles, or text — including fully offline
**Then** annotations persist as overlay data separate from the photo bytes; the original photo's content hash is unchanged (FR-10, AD-3)

**Given** an annotated photo
**When** it appears in an exported Rapport
**Then** the annotations are legible and positioned as drawn (round-trip)

**Given** an annotation session interrupted by force-close
**When** the app relaunches
**Then** saved annotations are intact

### Story 2.7: Zero-Loss Offline Sync

As an Expert,
I want everything I capture offline to survive and sync by itself,
So that I never re-enter data and never lose a survey — the one failure this product may not have.

**Acceptance Criteria:**

**Given** a full Relevé (forms, Désordres, photos, annotations) captured with no network
**When** the device reconnects
**Then** everything syncs with zero data loss within 30 seconds, with no user action (FR-7); content added by later epics (Coupes, Schémas) travels through this same mechanism

**Given** the app is force-closed mid-survey
**When** relaunched
**Then** every entry and photo is intact (acceptance §9.3.b)

**Given** a device whose local schema is older than the server's
**When** it syncs
**Then** it either syncs successfully (additive migration) or refuses with a clear update prompt — it never partially applies and never discards local rows (AD-19)

**Given** both clients
**When** syncing
**Then** each emits the shared sync-health event shape (queue depth, oldest pending write, last successful sync, upload failures) to the one telemetry sink; a silent sync failure is a reportable defect (AD-21)

## Epic 3: Coupes, Schémas & Désordre Mapping

The Rapport's visual backbone — Coupes and Schémas with system-guaranteed désordre numbering; the Expert never types a number.

### Story 3.1: Coupe Library Selection

As an Expert,
I want to select relevant Coupe types from a preconfigured library,
So that the Rapport's technical drawings start from professional templates, not a blank page.

**Acceptance Criteria:**

**Given** a Mission's Relevé, on mobile or back-office
**When** the Expert browses the Coupe library
**Then** the preconfigured types (mur, plancher, fondation, toiture, poteau-poutre, …) are available — offline included, shipped as versioned reference data through the sync set (FR-15, AD-17)

**Given** a Coupe type selected for the Relevé
**When** it is added
**Then** the Coupe instance pins the library version it came from, and rendering later resolves against that pinned version, never `latest`

**Given** the Coupe geometry
**When** persisted
**Then** it is stored as the SVG subset with a declared viewbox — never rasterized at capture (AD-6)

### Story 3.2: Désordre Positioning with Automatic Numbering

As an Expert,
I want to place Désordres on a Coupe and have every number and cross-reference maintained for me,
So that numbering across Relevé, Coupe, and Fiche de désordre can never drift.

**Acceptance Criteria:**

**Given** a Coupe in the Relevé, on mobile in the field or in the back-office
**When** the Expert positions a Désordre on it
**Then** an anchor is persisted in normalized 0..1 coordinates against the Coupe's viewbox, referencing the Désordre by id (AD-6)

**Given** a positioned point
**When** displayed anywhere
**Then** its number comes from the domain ordering function and its cross-reference resolves to the correct Fiche de désordre — the Expert never types a number (FR-15, acceptance §9.1.e)

**Given** a Désordre inserted or removed mid-survey
**When** numbering is next displayed
**Then** Relevé, Coupes, and Fiches all renumber consistently with no manual re-entry

**Given** positioning done offline
**When** the device syncs
**Then** anchors and cross-references survive intact

### Story 3.3: Schéma Sketch Tool

As an Expert,
I want to draw an annotated Schéma with freehand strokes, shapes, legends, and cotations,
So that what the Coupe library can't show, I can still deliver.

**Acceptance Criteria:**

**Given** a Zone in the Relevé
**When** the Expert draws a Schéma — freehand strokes, geometric shapes, legends, dimensions (cotations)
**Then** the sketch persists as the SVG subset, vector not raster, offline included (FR-16, AD-6)

**Given** input method
**When** drawing
**Then** finger drawing works as the required input, and a Bluetooth stylus works as generic touch — no pressure or palm-rejection features (NFR-4)

**Given** Désordres relevant to the sketched Zone
**When** the Expert positions them on the Schéma
**Then** the same normalized anchors, ordering-function numbering, and Fiche cross-references apply as on Coupes (CAP-8)

**Given** a finished Schéma
**When** it appears in an exported Rapport
**Then** it renders legibly with legends and cotations intact — rasterization happens only inside the renderer

## Epic 4: Compliant Rapport Generation & Sharing

The compliant PDF deliverable, end-to-end: after this epic the product is usable on real missions with zero AI.

### Story 4.1: Rapport Document Model with the Validation Gate

As an Expert who signs under décret liability,
I want the Rapport's content assembled and gated in one place,
So that no surface — today's or a future one — can ever ship unvalidated content in my name.

**Acceptance Criteria:**

**Given** `packages/rapport_document`
**When** a `RapportDocument` is constructed via `RapportDocumentBuilder`
**Then** the builder is parameterized by a content source (local SQLite or Postgres) — offline and finalized are inputs to one code path, never two implementations (AD-5)

**Given** any content item marked AI-proposed that lacks a terminal accepted Validation
**When** document construction is attempted
**Then** construction rejects it — the gate lives in the document model; any UI check is a convenience only (AD-8, FR-13), and the rejection path has its own direct pure-Dart tests

**Given** désordre references in document content
**When** the document is assembled
**Then** numbering is substituted at assembly from the domain ordering function — no stored display numbers anywhere in the model (AD-4)

### Story 4.2: Rapport de Diagnostic Complet

As an Expert,
I want the full diagnostic Rapport generated with every décret-mandated block in a compliant layout,
So that what I read is essentially the finished deliverable.

**Acceptance Criteria:**

**Given** a Mission with a completed Relevé
**When** the Expert generates the rapport de diagnostic complet
**Then** the PDF contains: customizable cover (cabinet logo, mission title, date, Expert), automatic TOC, pagination, per-Zone synthesis, annotated photo annexes, at least one relevant Coupe, and the annotated Schéma of the diagnosed Zone (FR-17, acceptance §9.1.d)

**Given** the décret-mandated content blocks
**When** the document renders
**Then** every block is present — building description; elements examined and observed désordres; recent-works history and stability impact; recommended investigations, mesures conservatoires, prioritized works — and the Expert's credentials/insurance block (name, qualification, RC Pro insurer/policy/cover) prints from the `expert` record

**Given** a block with no content
**When** rendered
**Then** it appears as an explicit French statement (e.g. "aucuns travaux récents signalés"), never a silent omission

**Given** a high-volume Mission (many photos and Schémas)
**When** rendered
**Then** cover, TOC, and pagination stay correct and readable — no unbounded in-memory assembly of full-resolution images (acceptance §9.1.f)

**Given** a photo whose blob is still pending
**When** the document renders
**Then** the renderer handles `blob-pending` explicitly with a visible placeholder state (AD-3)

**Given** the same document model
**When** rendered from local SQLite or from Postgres
**Then** the layout is byte-stable — one renderer, golden-tested (AD-5)

### Story 4.3: Rapport Templates — Three Deliverables

As an Administrateur,
I want the three v1 templates managed as configuration,
So that the coming arrêté template is a data change, not a rework.

**Acceptance Criteria:**

**Given** template management in the back-office
**When** the Administrateur works with templates
**Then** the three v1 templates exist — rapport de visite, rapport de diagnostic complet, fiche de désordre unitaire — as versioned reference data, not code (FR-19, AD-17)

**Given** the rapport de visite
**When** generated
**Then** it carries its content floor: credentials/insurance block, Ouvrage identity and visit date, per-Zone observation summary, key annotated photos, observed Désordres with Sévérité, and the explicit statement that it is not a diagnostic — no Coupes, no Fiches, no cause/recommendation blocks

**Given** the fiche de désordre unitaire
**When** generated for one validated Désordre
**Then** it carries: credentials/insurance block, Ouvrage identity, the full Fiche (description, annotated photos, localization, Sévérité, validated causes and recommendations when they exist), its Coupe extract if positioned, and the parent-Mission cross-reference

**Given** a generating record
**When** it renders
**Then** it resolves against the template version it pinned — a template edit never silently changes an already-generated Rapport

### Story 4.4: Offline Provisional Render, Finalized at Sync

As an Expert,
I want a Rapport I can render in the field and a finalized one that supersedes it at sync,
So that I can review on-site without ever confusing draft with deliverable.

**Acceptance Criteria:**

**Given** an offline device mid-Mission, even on an in-progress Relevé
**When** the Expert requests Rapport generation
**Then** the on-device render assembles all locally captured data using the same pure-Dart renderer the server runs (FR-18, AD-5)

**Given** the provisional render
**When** produced
**Then** every page is visibly watermarked "PROVISOIRE — non finalisé", and it is blocked from every FR-20 export path

**Given** the device syncs
**When** analysis and Validation state are known
**Then** the render worker produces the finalized PDF automatically, superseding any provisional render, and the Expert can see unambiguously which state a Rapport is in

**Given** either render
**When** assembled
**Then** the Story 4.1 gate applies identically — no unvalidated content in either artifact

### Story 4.5: Export, Email, and Secure Sharing

As an Expert,
I want to export a finalized Rapport locally, by email, or by secure link,
So that the deliverable reaches the client without its data ever being public.

**Acceptance Criteria:**

**Given** a finalized Rapport
**When** the Expert exports locally, sends by email, or creates a share link
**Then** the operation succeeds; given a provisional render, every one of these paths refuses (FR-20, FR-18)

**Given** a share link
**When** created
**Then** it is an expiring, single-purpose signed URL scoped to that one Rapport version; no object-storage path is ever public (AD-22)

**Given** an active share link
**When** the Expert revokes it
**Then** access stops from that moment

**Given** any share event (create, access, revoke)
**When** it occurs
**Then** it is recorded in the audit trail with actor, Rapport version, and expiry

### Story 4.6: Immutable Rapport Provenance Record

As an Expert bearing legal liability,
I want an append-only record of who authored every piece of a Rapport,
So that years later, "who wrote this sentence — the AI or the engineer?" has an answer.

**Acceptance Criteria:**

**Given** any generated Rapport
**When** finalized
**Then** an immutable provenance record is retained with the Mission: which content was AI-drafted (none yet in this epic — the structure carries it), what the Expert authored or corrected, and per-item Validation timestamps with the acting Expert (FR-25)

**Given** a finalized Rapport
**When** the provenance record is consulted
**Then** it reconstructs, per Désordre, the original proposal and the final validated version, with actor and timestamp

**Given** a correction after finalization
**When** recorded
**Then** it creates a new entry — never a rewrite; attempts to update existing entries are rejected at the data layer

## Epic 5: AI Detection & Expert Validation

AI proposes, Expert disposes — detection, drafted text, and the validation loop, flowing into the gate Epic 4 already enforces. Gated by the corpus gate: acceptance testing starts only at ≥150 labeled examples per category.

### Story 5.1: Automatic Photo Analysis Pipeline

As an Expert,
I want every synced photo analyzed automatically, without anything ever blocking on the AI,
So that Détections are waiting for me the next morning — and the field never waits for a server.

**Acceptance Criteria:**

**Given** a photo arriving through sync
**When** analysis is triggered
**Then** it goes through the single asynchronous job contract — submit → subscribe → terminal state — served by the FastAPI inference service; no surface calls a model synchronously (AD-9, FR-11)

**Given** any photo
**When** its status is displayed
**Then** it shows exactly one of pending / done / failed / no findings — "no findings" always distinguishable from "not yet analyzed"

**Given** analysis completes
**When** results land
**Then** each Détection is written as an append-only `detection` row carrying nature (category + sub-category from the pinned taxonomy), bounding-box localization, proposed Sévérité, Score de confiance, and the producing model's identifier and version — no Détection exists without provenance (AD-7, AD-18)

**Given** a standard synced Relevé
**When** analysis runs
**Then** it completes within 1 hour

**Given** the inference service is unavailable
**When** Experts keep working
**Then** capture, manual Désordre entry, and Validation of already-analyzed photos are unaffected; failed jobs are retryable states, never lost work

### Story 5.2: Model Registry and the Test-Set Deployment Gate

As an Administrateur,
I want every model version registered with its licence and gated on a frozen test set,
So that no model reaches production unmeasured — and none can make the product AGPL.

**Acceptance Criteria:**

**Given** the corpus gate
**When** detection acceptance testing is scheduled
**Then** the per-category labeled-corpus count (target ≥150 usable examples across ≥2 capture devices) is measured and recorded; categories that fall short ship best-effort or not at all, with fissuration + corrosion as the fallback v1 scope (FR-11)

**Given** the test set
**When** frozen
**Then** it is versioned before model tuning, includes held-out pilot-mission data and a slice blind-labeled by the second founder, and never leaks into training

**Given** a candidate model version
**When** evaluated for deployment
**Then** it deploys only if fissuration and corrosion exceed 75% precision and reach ≥60% recall on the frozen test set — the only deployment gate for models (NFR-10)

**Given** the model registry
**When** a version enters
**Then** its licence is recorded and verified Apache/BSD/MIT — no AGPL in the deployed path (AD-11); rollout and rollback are configuration, auditable through Detection provenance (AD-18)

### Story 5.3: Détection Review and Expert Validation

As an Expert,
I want to validate, correct, or reject each Détection in seconds — and add what the AI missed,
So that I stay the author of every conclusion, at triage speed.

**Acceptance Criteria:**

**Given** an analyzed photo, on mobile or back-office
**When** the Expert reviews a Détection
**Then** it displays nature, localization overlay, proposed Sévérité, and Score de confiance as a percentage — visibly scoped to the detection only, never presented as confidence in Sévérité or drafted text — and is visibly marked proposed-not-validated until decided (NFR-5)

**Given** a Détection's detection fields (nature, localization, Sévérité)
**When** the Expert validates, corrects, or rejects
**Then** the action completes in under 5 seconds — writing locally and returning, sync in background — and appends a `validation` event with expert, timestamp, mission, and ouvrage; the Détection row is never updated in place (FR-13, AD-7)

**Given** validations from both mobile and web
**When** the terminal Validation is resolved
**Then** it is the last by (server_received_at, id) — never by device clock — via the single resolver function every consumer calls, with a dedicated test where device clocks disagree with server order (AD-7)

**Given** a Désordre the AI missed
**When** the Expert adds it manually
**Then** a `desordre` row is created without any linked Détection — no code path assumes every Désordre has one

**Given** the Epic 4 gate
**When** a Rapport is assembled
**Then** validated Détections flow in and unvalidated ones are rejected — exercising the gate built in Story 4.1 with real events

### Story 5.4: AI-Drafted Causes and Recommendations

As an Expert,
I want draft cause hypotheses and recommendations I can edit or discard at my own pace,
So that the writing is started for me, but never finished without me.

**Acceptance Criteria:**

**Given** a Détection (or its Désordre)
**When** drafting runs — after detection, per Désordre, over the Détection + Relevé context
**Then** French text is generated through `DraftingPort` behind the same async job contract; provider selection (claude-opus-5 default, Bedrock-EU / Vertex-EU swappable) is configuration, and no provider type crosses the port (FR-12, AD-9, AD-10)

**Given** drafted text
**When** produced
**Then** it is structured into the décret's three blocks — investigations complémentaires recommandées, mesures conservatoires, travaux hiérarchisés — clearly marked AI-proposed, referencing désordres by id only (numbering substituted at render, AD-4)

**Given** a Détection whose drafted text is still pending
**When** the Expert works
**Then** the Détection is complete and validatable on its detection fields — drafted text is a separate, later-bound artifact

**Given** drafted text review
**When** the Expert edits, reorders, or deletes blocks
**Then** it happens through its own unhurried affordance, explicitly outside the <5 s budget, and edits append events — the original draft is preserved (AD-7)

**Given** Rapport assembly
**When** drafted text is considered
**Then** it enters only after explicit accept or edit — accepting detection fields never silently accepts drafted text (FR-13); unaccepted drafts are rejected by the 4.1 gate

### Story 5.5: Correction Capture and the Structured Désordre Database

As an Administrateur,
I want every Validation outcome stored as traceable, structured data,
So that each mission makes the next model better — and the corpus becomes the asset.

**Acceptance Criteria:**

**Given** any Validation outcome — validation, correction, rejection, or manual addition
**When** it is recorded
**Then** it exists in the append-only Validation event log with full traceability (Expert, date, Mission, building), ready for projection as training data (FR-14, AD-12) — no code writes a training record directly

**Given** the stored corpus
**When** queried
**Then** images and annotations are classified by structure type, Désordre nature, Sévérité, and geographic location, with per-record traceability (FR-21)

**Given** the legal provenance record (Story 4.6)
**When** compared with training capture
**Then** they are distinct stores serving distinct requirements — the learning loop never depends on the liability trail, nor vice versa

## Epic 6: Learning Loop, Dashboard & Pilot Readiness

UJ-3 closed and the pilot ready — the export that feeds retraining, the dashboard that tells the truth about the model, and the documentation the PRD forbids dropping.

### Story 6.1: Filtered Dataset Export for Retraining

As an Administrateur,
I want to export the validated dataset with privacy filters applied,
So that retraining has clean fuel — and third parties' buildings never leak through it.

**Acceptance Criteria:**

**Given** the Validation event log
**When** the Administrateur runs a dataset export
**Then** the export is a projection over that log — validated Détections, corrections, and manual additions with per-record traceability — and no other code path writes training records (FR-22, AD-12)

**Given** geolocation in exported records
**When** the export is produced
**Then** coordinates are stripped or coarsened per the v1 data minimum (NFR-11)

**Given** photos in the export
**When** filtered
**Then** personal data — faces, license plates, interiors identifying occupants — is excluded, and exclusions are logged so filter behavior is auditable

**Given** a completed export
**When** recorded
**Then** it carries a version identifier and timestamp, so a retrained model can name the exact dataset it learned from — retraining itself stays a manual, offline, technical-team process gated by Story 5.2

### Story 6.2: AI Performance Dashboard

As an Administrateur,
I want agreement statistics and true test-set metrics per category and model version,
So that I know whether last month's corrections actually made the model better.

**Acceptance Criteria:**

**Given** the dashboard in the back-office
**When** viewed per Désordre category
**Then** it shows validation-agreement statistics — validate / correct / reject rates from Expert Validations — labeled as *agreement*, never as accuracy (FR-23)

**Given** the frozen test set and the model registry
**When** metrics are displayed
**Then** true precision and recall per model version appear alongside model version history (AD-18)

**Given** the SM-C1 counter-metric
**When** the correction+rejection rate over the rolling 50-Détection window falls below 5%
**Then** the dashboard surfaces the rubber-stamp flag so the Administrateur review can happen before the next Rapport ships

**Given** v2-descoped features
**When** the dashboard is built
**Then** cross-version comparison views and drift alerts are absent — statistically meaningless at pilot volume

### Story 6.3: French User Documentation

As a new Expert,
I want French documentation that takes me from zero to productive in under an hour,
So that the tool can outgrow its founders.

**Acceptance Criteria:**

**Given** the documentation deliverable
**When** complete
**Then** it covers, in French: the field flow (Mission → Relevé → photos → sync), Validation, Coupes and Schémas, and Rapport generation and sharing (FR-26)

**Given** a reader following it
**When** performing each documented flow against the app
**Then** every step matches the shipped UI — screenshots and vocabulary use the product's French domain terms

**Given** the <1 h productivity bar
**When** measured
**Then** the scripted cold-start session with a recruited non-founder Expert is defined and ready to run at the SM-7 gate (SM-6) — the doc ships in v1, the measurement gates v2

**Given** the documentation
**When** the pilot runs
**Then** it is reachable from both surfaces (linked or bundled), versioned alongside releases
