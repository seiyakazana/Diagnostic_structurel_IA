# Review — Rubric walker + input reconciliation

**Method:** judge the spine against the good-spine checklist, then check each load-bearing input (SPEC, acceptance criteria, compliance references, PRD, PRD addendum) for requirements that did not land in the `AD` structure.

**Verdict:** structurally sound — the paradigm is named, every `AD` carries Binds/Prevents/Rule, and the hard constraints (offline, liability, French vocabulary) are each fixed by a specific invariant. **One whole dimension is silent** and five input requirements did not land.

---

## Rubric findings

### R1 — HIGH · The operational / environmental envelope is silent

The checklist calls this out as the dimension a domain-focused draft skips, and this draft skipped it. There is a deployment *topology* diagram and a residency rule (AD-16), but nothing on:

- **Environments.** No dev/staging/prod separation. This matters more than usual here: the pilot runs on *real missions* producing *signed, liability-bearing deliverables*. A test Rapport and a signed Rapport must not be able to share a database.
- **Mobile distribution.** Two pilot users on iOS + Android, iterating fast, against acceptance §9.3.a (five device models). TestFlight / internal-track distribution is the mechanism by which the offline-durability criteria get exercised at all.
- **Observability.** SM-5 requires *zero data loss over the entire pilot* and §9.1.b requires sync ≤30s. Neither is assertable without sync telemetry. There is currently no way to know a sync silently failed.

### R2 — HIGH · No `Detection` → model-version binding

CAP-13 requires precision/recall/F1 **per model version** with cross-version comparison and drift alerts; CAP-12 requires gating a retrained model on a test set. Neither is computable unless each `Detection` records the model that produced it. AD-11 mentions a registry; nothing requires the link. (Same hole reached independently by the adversarial lens as A6.)

### R3 — HIGH · Schema evolution against offline devices is undecided

Local-first with long offline periods (a basement survey, then days before sync) means devices hold **stale local schemas**. Two units will handle a migrated column incompatibly, and the failure mode is the one thing the product may not do: lose field data. §9.3.b and SM-5 are absolute on this. Nothing in the spine addresses it.

### R4 — MEDIUM · At-rest encryption on the device is undecided

A phone holds a complete building diagnostic — geolocated photos, addresses, observations — for as long as it stays offline. Phones get lost. PowerSync 2.0 ships SQLCipher-backed encryption, so this is a configuration decision that costs nothing to make now and is awkward to retrofit. Silent.

### R5 — LOW · Deferred entries are safe

Checked each: none could let two units diverge. The deferred detection-model choice is properly constrained by AD-11; deferred multi-tenancy is properly pre-empted by AD-15/AD-16. Good.

### R6 — PASS · Spec coverage

All 14 CAPs appear in the Capability → Architecture Map, each with a governing invariant. No capability is unhomed.

---

## Reconciliation findings (requirements that did not land)

### C1 — MEDIUM · FR-20 export and sharing is entirely absent

"Export locally, send by email, or share via secure link" — the PRD's own assumption is *expiring authenticated URL*. This is the one path where a Rapport containing personal data leaves the system boundary to a third party (insurer, syndic, owner), and the spine says nothing about it. Given the liability framing, an unbounded public URL would be a genuine defect.

### C2 — MEDIUM · Expert credentials are not modelled

FR-17 and the compliance references require the Rapport to carry the **credentials and insurance block** — named Bac+5 engineer, RC Pro ≥ €1M/claim. Décret 2025-814 makes this a content requirement of a compliant deliverable. The ERD has `EXPERT` with no indication these fields exist, so `RapportDocument` has nothing to render.

### C3 — LOW · Configurable checklists (FR-6) not named as reference data

FR-6 configures checklists back-office-side for the field app to consume. They belong with the taxonomy, coupe library, and templates in `reference-data/` — and, critically, must be **in the offline sync set before the Expert goes offline**, or the guided form is unusable on site.

### C4 — LOW · Field ergonomics constraints are unhomed

≤3 taps, ≥14pt type, high contrast, dark mode, one-handed operation, haptic feedback, gloved use. Almost entirely UX territory, correctly not invented here — but the spine should say so rather than leave it ambiguous whether architecture owns it. One consequence *is* architectural: Bluetooth stylus support (FR-16) constrains the sketch surface's input handling.

### C5 — PASS · The quiet constraint survived

The liability posture — *AI proposes, expert disposes and signs* — is not merely mentioned but **structurally enforced**: AD-7 makes AI output additive and non-destructive, AD-8 puts the gate in the document model where no surface can bypass it. This is the constraint most likely to be reduced to a UI checkbox, and it wasn't.
