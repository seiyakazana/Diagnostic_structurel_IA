---
project_name: 'Diagnostic_structurel_IA_v2'
user_name: 'Jean-pierreelias'
date: '2026-08-02'
sections_completed: ['technology_stack', 'language_rules', 'framework_rules', 'testing_rules', 'code_quality_rules', 'workflow_rules', 'critical_rules']
existing_patterns_found: 22
status: 'complete'
rule_count: 79
optimized_for_llm: true
---

# Project Context for AI Agents

_This file contains critical rules and patterns that AI agents must follow when implementing code in this project. Focus on unobvious details that agents might otherwise miss._

---

## Technology Stack & Versions

| Technology | Version / Constraint |
| --- | --- |
| Flutter / Dart (mobile iOS+Android, web back-office) | 3.44.7 |
| `powersync` Dart SDK (sync engine + attachment queue) | 2.3.2 |
| SQLite on device | bundled via `sqlite3_flutter_libs` (mobile) / `sqlite3_web` (web) — never pin separately |
| Supabase (Postgres, Auth, object storage, RLS) | Postgres 15+, region `eu-west-3` (Paris) — EU residency is an invariant (AD-16), not a preference |
| PowerSync service | Cloud if EU-region placement available; self-hosted Open Edition otherwise |
| `pdf` (pure-Dart Rapport renderer) | 3.13.0 |
| Python (inference service) | 3.12 |
| FastAPI + PyTorch (detection serving) | current stable |
| Detection model architecture | UNDECIDED — must be Apache/BSD/MIT-licensed; no AGPL in the deployed path (AD-11) |
| Claude API (`DraftingPort` default adapter) | `claude-opus-5`, behind a port — Bedrock-EU / Vertex-EU are config-swappable (AD-10) |

**Version constraints agents must respect:**
- The spine is the version seed; once code exists, `pubspec.yaml` / `pyproject.toml` own versions. Do not "upgrade while you're in there."
- All EU-hosted services (Postgres, storage, sync, inference) stay EU-hosted. The LLM path is the single documented exception, tracked as an open question.
- No AGPL-licensed dependency may enter the deployed inference path.

## Critical Implementation Rules

### Language-Specific Rules

**Dart — package purity is the load-bearing rule:**
- `packages/domain`, `packages/rapport_document`, `packages/rapport_renderer` import **zero** frameworks — no Flutter, no PowerSync, no Supabase, no `dart:ui`. They compile unchanged on mobile, web, and server (AD-13). If an import from an adapter or app package appears in these, the change is wrong regardless of how convenient it is.
- Dependency direction is `domain ← application ← adapters`. Ports (`DraftingPort`, `InferenceJobPort`, `BlobPort`) are declared in `packages/application`; adapters implement them. No provider/framework type ever crosses a port boundary (AD-10).
- Domain failures are **typed result values**, not exceptions. Only adapters throw. An agent adding `throw` inside `packages/domain` or `packages/application` is violating the error model.
- French domain nouns are the code vocabulary, verbatim from the SPEC glossary, unaccented: `Desordre`, `Releve`, `FicheDesordre`, `Coupe`, `Ouvrage` — never translate to English (`Disorder`, `Survey`…). Technical suffixes stay English: `_repository`, `_port`, `_adapter` (AD-14).
- Identifiers are client-minted UUIDv7, generated offline at creation. Never a DB sequence, never `AUTOINCREMENT`, on any synced table (AD-4).
- The human-visible désordre number is **never stored, never computed outside `packages/domain`**. One ordering function computes it from `(zone order, élément order, capture timestamp, id)`; every surface calls it. AI-drafted text references désordres by id; numbering is substituted at render.

**Python (inference service, 3.12):**
- The inference service orchestrates detection + drafting; it writes results only as `Detection` rows through the job contract — it never mutates domain records directly.
- Every `Detection` written must carry the producing model's identifier and version (AD-18). Code that writes a Detection without provenance is incomplete, not minimal.
- LLM calls go through the `DraftingPort` abstraction's server-side equivalent — provider selection is configuration, never an `if provider == "anthropic"` branch in domain logic (AD-10).

**Both languages:**
- Timestamps: UTC `TIMESTAMPTZ` in storage, ISO-8601 on the wire. Capture time and sync time are separate fields — never conflate or "simplify" into one.
- Geometry: normalized `0..1` coordinates against a declared viewbox (AD-6). Geolocation is WGS-84 `(lat, lon, accuracy_m)`.

### Framework-Specific Rules

**Flutter (mobile + back-office):**
- The UI reads and writes **only on-device SQLite**. No widget, bloc, or provider ever awaits a network call on a user-facing path (AD-1). If a feature "needs" fresh server data, the answer is the sync engine, not an HTTP call from UI code.
- Both apps and the render worker consume the same pure-Dart packages. Never fork rendering or ordering logic "just for web" or "just for the worker" (AD-5).
- Coupes and Schémas are captured and persisted as an SVG subset with normalized anchors — never rasterize at capture; rasterization happens only inside `rapport_renderer` (AD-6).

**PowerSync + SQLite (sync layer):**
- Binaries never enter the sync stream. Photos and sketch rasters go through the attachment queue to object storage, addressed by content hash over the bytes **exactly as captured** — no re-encoding before hashing (AD-3).
- Every consumer of a photo/raster reference handles `blob-pending` explicitly — a row is valid and syncable before its blob resolves.
- A Relevé has exactly one owning device-assignment at a time (AD-2). Do not write merge/conflict-resolution code for concurrent Relevé edits — concurrent writes are a bug to surface, not a case to handle.
- Migrations touching synced tables are **additive only**: new nullable columns, new tables. Renames/drops go expand → migrate → contract across releases (AD-19). An older-schema device syncs successfully or refuses with an update prompt — never partially applies, never discards local rows.
- Sync-health telemetry is one shared event shape (queue depth, oldest pending write, last successful sync, upload failures) emitted by both clients to one sink (AD-21). Don't invent a per-client health metric.

**Supabase (Postgres, Auth, storage, RLS):**
- Authorization predicates are declared once in `authz/` and **generated** into both PowerSync sync rules and Postgres RLS policies. Never hand-edit either generated artifact (AD-15).
- No object-storage path is ever public. Rapport sharing uses expiring, single-purpose signed URLs scoped to one Rapport version, and every share is written to the audit trail (AD-22).
- Reference data (taxonomy, coupe library, checklists, Rapport templates) is versioned data in the sync set — not code, not enums. Capturing records pin the reference version they used; rendering resolves against the pinned version, never `latest` (AD-17).

**FastAPI (inference service):**
- All inference is asynchronous behind one job contract: `submit` → `subscribe` → terminal state. No endpoint calls a model synchronously in the request path (AD-9). CV and LLM paths use the **same** contract — don't wire them differently.

**`pdf` renderer:**
- `RapportDocumentBuilder` is parameterized by a content source (local SQLite or Postgres). Offline vs finalized are inputs to one code path — never two builder implementations, never hand-built PDF primitives anywhere else (AD-5).
- `RapportDocument` construction **rejects** any Détection or drafted text lacking a terminal Validation event. That gate lives in the document model, not the UI — the UI check is a convenience only (AD-8).

### Testing Rules

**Test placement follows package purity:**
- `packages/domain`, `rapport_document`, `rapport_renderer` are tested as pure Dart — no Flutter test bindings, no emulators, no mocks of frameworks (they import none). If a test for these packages needs a mock, the code under test is in the wrong package.
- Adapter tests mock the **port interfaces** declared in `packages/application`, never concrete providers. A test importing an Anthropic or Supabase type outside `adapters_*` is a boundary violation.

**Invariants that must have dedicated tests (not incidental coverage):**
- The désordre ordering function: same input set → same numbering on every surface; insertion mid-survey renumbers deterministically (§9.1.e).
- The terminal-Validation resolver: last Validation by `(server_received_at, id)` — include a test where device clocks disagree with server order.
- The `RapportDocument` gate: construction rejects unvalidated Détections/drafted text; the rejection path is tested directly, not through the UI (AD-8).
- Renderer determinism: one document model → byte-stable layout whether content source is SQLite or Postgres (AD-5). Golden/snapshot tests for the compliant layout (cover, TOC, pagination — §9.1.f), including a high-volume photo/schéma fixture.
- Additive-migration safety: a fixture DB at schema N syncs against schema N+1 without loss or partial application (AD-19, §9.1.b, §9.3.b).
- Blob-pending handling: every consumer renders/behaves correctly with an unresolved content hash (AD-3).

**Offline is the default test condition, not an edge case:**
- Field-flow tests (mission → relevé → photos → offline PDF) run with network disabled first (§9.1.a). Connectivity is the special case.
- Forced-close durability (§9.3.b): kill-and-relaunch tests must show zero data loss.

**AI/inference testing:**
- Detection quality is gated on a held-out test set: >75% precision on cracks and corrosion (§9.2.a). A retrained model must pass the test-set gate before deployment — this is the only deployment gate for models.
- CAP-14 (cause/recommendation) has **no accuracy floor at POC** — do not invent one. Its tests are behavioural: inferred text is marked non-binding and blocked from the PDF until validated or edited.
- Inference job contract tests cover the full lifecycle: `submit` → `subscribe` → terminal state, including failure terminals. Never test a model call synchronously through an API route.

**General:**
- Test names and fixtures use the French domain vocabulary (`releve`, `desordre`…) — same AD-14 rule as production code.
- Coverage has no numeric mandate; the invariant list above is the coverage contract.

### Code Quality & Style Rules

**Naming (the one system, applied everywhere):**
- Files, tables, columns: French domain noun, singular, unaccented `snake_case` — `element_structurel`, `fiche_desordre`.
- Dart types: `PascalCase` of the same noun — `ElementStructurel`, `FicheDesordre`.
- Technical suffixes stay English: `releve_repository`, `drafting_port`, `powersync_adapter`.
- Never mix registers: `DisorderRepository` and `desordre_service` are both wrong — the domain half is French, the technical half is English.

**Code organization — the source tree is normative:**
- New code goes where the spine's source tree says it goes: entities and the ordering function in `packages/domain`; document model + gate in `packages/rapport_document`; the single renderer in `packages/rapport_renderer`; use cases + ports in `packages/application`; framework code only in `adapters_*`, `apps/*`, `services/*`.
- Authorization predicates only in `authz/`; taxonomy/checklists/templates only in `reference-data/` as versioned data. If a change wants to put a rule in two places, the design is wrong — find the single owner.
- Enumerations that mirror reference data (désordre categories, sub-disorders, Sévérité levels, roles) are **seeded data, not code enums**. Adding a sub-disorder is a data change, not a release.

**Configuration:**
- All config is environment-injected. Provider selection (`DraftingPort` adapter, object storage) is runtime configuration — never a compile-time branch, build flavor, or `const` switch (AD-10).

**Formatting & linting:**
- Dart: `dart format` defaults; Python: standard formatter defaults. No custom style rules exist yet — when lint configs land in the repo, they own style and this section defers to them.

**Documentation in code:**
- Comments state constraints the code can't show — typically which AD or FR a non-obvious guard enforces (e.g. `// AD-7: append-only — corrections are new Validation rows`). No narrating comments.
- Public APIs of the pure packages (`domain`, `rapport_document`, `rapport_renderer`) get doc comments — they are consumed by three surfaces and are the project's real contract surface.

### Development Workflow Rules

**Git:**
- Commit messages: imperative mood, capitalized, no prefix convention — matching existing history ("Add architecture spine with adversarial and currency reviews"). No branch-naming or PR convention is defined yet; don't invent one silently — propose it when the first feature branch is needed.

**BMAD planning-to-code flow:**
- Planning artifacts live in `_bmad-output/` (specs, PRD, architecture, this file); implementation stories land in `_bmad-output/implementation-artifacts/`. Agents implement from story files, which carry their context — when a story conflicts with the architecture spine, the spine wins and the conflict is surfaced, not resolved ad hoc.
- The authority chain for requirements: SPEC + acceptance criteria → PRD → architecture spine. `docs/` holds the source Cahier des Charges (client input, French) — treat it as source material, not a spec to implement from directly.

**Environments & deployment (AD-20):**
- dev, staging, and production are separate Postgres instances, buckets, and sync services with **no shared credentials**. Never point a dev build at production, even read-only — pilot data is real mission data with legal weight; there is no "just a test" corpus in production.
- Mobile builds ship to pilot devices via platform internal-test tracks (TestFlight / Play internal testing) only.
- Every build reports the schema version it expects (AD-19). A release touching synced tables must state its migration phase (expand / migrate / contract) explicitly.

**Model deployment:**
- A model version enters the registry with its licence recorded (AD-11) and deploys only after passing the held-out test-set gate (§9.2.a). Model rollout is config, not code — the Detection rows' provenance (AD-18) is what makes rollback auditable.

### Critical Don't-Miss Rules

**Anti-patterns — never do these, even when they look like the simpler fix:**
- **Never update an AI proposal in place.** Correcting a Détection = append a new `Validation` row (expert, timestamp, mission, ouvrage). An UPDATE on a Detection or Validation destroys the training signal and the liability trail (AD-7).
- **Never store or cache a désordre display number.** It is derived at render by the one domain ordering function. Persisting it "for performance" creates the numbering drift AD-4 exists to prevent.
- **Never resolve "the latest Validation" by device timestamp.** Terminal = last by `(server_received_at, id)`. Device clocks lie; mobile and web both validate.
- **Never write a training record directly.** Dataset export is a projection over the Validation event log (AD-12). A second write path will silently drift.
- **Never bypass the `RapportDocument` gate.** Any new export surface (batch job, API, back-office button) gets validation enforcement for free from the document model — do not re-implement or skip it (AD-8).
- **Never re-encode a photo before hashing.** The content hash is over the bytes as captured; re-encoding gives one photograph two addresses (AD-3).
- **Never hand-edit generated sync rules or RLS policies** — change the `authz/` definition and regenerate (AD-15).
- **Never resolve reference data against `latest`.** Rendering and display use the version the record pinned at capture (AD-17) — a renamed sub-disorder must not rewrite what an Expert selected in a signed document.

**Edge cases that are core cases here:**
- A row whose blob hasn't arrived (`blob-pending`) — every consumer handles it, including the renderer.
- A device offline for days with a full Relevé on it — sync after schema migration must not lose a single row (AD-19). This is the one failure the product may not have.
- An Expert adds a désordre the model missed — `Desordre` links optionally to `Detection`; code must not assume every désordre has one.
- Reference-data version on device predates capture: the sync set ships reference data **before** the Expert loses signal — a Relevé must never start against absent reference data.

**Security & compliance:**
- Rapports contain addresses, geolocation, and site photos, signed by a named engineer under décret 2025-814 liability. Egress only via expiring, single-purpose signed URLs, each share audit-logged (AD-22). No public bucket paths, ever.
- The on-device SQLite database is encrypted at rest — a phone carries a complete building diagnostic.
- The `expert` record must carry name, qualification level, and RC Pro insurer/policy/cover — the Rapport prints them; don't trim "unused" fields.
- Everything is EU-hosted (AD-16) except the documented LLM exception. Don't add a non-EU service dependency, including "just for telemetry."

**Performance gotchas:**
- The renderer must stay readable at any photo/schéma volume (§9.1.f) — no unbounded in-memory assembly of full-resolution images.
- Validation UX budget is <5s per detection (§9.2.c) — the validate action writes locally and returns; sync happens in the background like everything else.

---

## Usage Guidelines

**For AI Agents:**
- Read this file before implementing any code.
- Follow ALL rules exactly as documented. AD-n references resolve to the architecture spine (`_bmad-output/planning-artifacts/architecture/architecture-Diagnostic_structurel_IA_v2-2026-07-27/ARCHITECTURE-SPINE.md`); §9 references resolve to `_bmad-output/specs/spec-diagnostic-structurel-ia/acceptance-criteria.md`.
- When in doubt, prefer the more restrictive option.
- If a story or requirement conflicts with a rule here, surface the conflict — do not resolve it silently.

**For Humans:**
- Keep this file lean and focused on agent needs.
- Update when the stack changes — especially when the detection model architecture is decided (AD-11) and when lint configs land (they take over style).
- Review at each phase gate; remove rules that become enforced by tooling or obvious from code.

Last Updated: 2026-08-02
