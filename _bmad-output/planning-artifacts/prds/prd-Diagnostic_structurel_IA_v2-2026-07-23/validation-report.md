# Validation Report — PRD: Diagnostic Structurel IA

- **PRD:** `_bmad-output/planning-artifacts/prds/prd-Diagnostic_structurel_IA_v2-2026-07-23/prd.md`
- **Rubric:** `.claude/skills/bmad-prd/assets/prd-validation-checklist.md`
- **Run at:** 2026-08-02T08:05:05Z
- **Grade:** Poor

## Overall verdict

This PRD holds up as the scope contract for its downstream chain: the thesis ("compress ~85% deskwork into a review-and-sign step; the AI proposes, the Expert disposes") is stated once and enforced everywhere — in Non-Goals, in FR-13's "No unvalidated Détection can appear in a Rapport", and in counter-metric SM-C1. Scope honesty and traceability are unusually clean (11/11 assumption roundtrip, contiguous FR-1–FR-24, every FR mapped to a UJ and SM). What's at risk is the tail of the pipeline it bets everything on: FR-18 (offline Rapport generation) is the most architecturally consequential FR and ships with zero testable consequences, and the acceptance surface for AI detection outside fissures/corrosion is undefined — both will surface as ambiguity in story creation, exactly where this PRD is otherwise strongest.

The adversarial pass materially shifts the picture and drives the grade. Three findings landed critical: SM-3 is a precision-only, self-graded metric with no recall floor (C-1); the <5-second Validation target manufactures the rubber-stamping SM-C1 warns about, while no FR creates a legal audit trail distinguishing AI-drafted from Expert-authored Rapport text under décret 2025-814 (C-2); and the décret-mandated Rapport content — recent works, mesures conservatoires, prioritized works, further investigations — is collected by no FR, making the "compliant Rapport" structurally non-compliant (C-3). Six further high findings attack the FR-15 auto-mapping claim, the unquantified training corpus, unmeasured severity grading, the zero-security-NFR state of a `status: final` document, FR-18's provisional-artifact leak, and the "RGPD deferred entirely" position, which the v1 training-data reuse of third-party building photos does not actually permit. The mechanical grading rule (any critical finding → Poor) sets the headline grade; read it as "structurally sound document, three contract-level holes to close," not as a rewrite order.

## Dimension verdicts

- Decision-readiness — strong
- Substance over theater — strong
- Strategic coherence — strong
- Done-ness clarity — adequate
- Scope honesty — strong
- Downstream usability — strong
- Shape fit — strong

## Findings by severity

### Critical (3)

**[Adversarial]** — C-1. SM-3 is a self-graded, precision-only, recall-blind vanity metric (§8 SM-3; FR-11; addendum "AI cold start")
The 75% precision target is measured on a test set built and labeled by the founding Expert — the same person who pilots the product and wants the venture to succeed. No recall target exists anywhere: a model that flags one obvious lézarde per building achieves 100% precision and passes SM-3 while missing every désordre that matters. For a triage tool on potentially dangerous buildings, false negatives are the actual risk; precision-only targets are trivially gameable by raising the confidence threshold.
Fix: Add a recall (or miss-rate) floor per category; require held-out pilot-mission data blind-labeled by the second founder; freeze and version the test set before model tuning.

**[Adversarial]** — C-2. The 5-second Validation target manufactures rubber-stamping, and there is no legal audit trail (FR-13; §8 SM-4; SM-C1; FR-14)
FR-13/SM-4 target processing each Détection — nature, localization, severity, plus AI-drafted cause hypotheses and recommendations — in under 5 seconds. Nobody critically evaluates a causal hypothesis in 5 seconds. SM-C1 acknowledges the rubber-stamp risk but has no threshold, trigger, action, or owner. FR-14 stores corrections as training data, not as a legal audit trail: nothing distinguishes AI-drafted text the Expert accepted verbatim from text the Expert authored — exactly what insurer and opposing counsel will ask post décret 2025-814.
Fix: (a) Restrict the <5s target to accept/reject of detections; give cause/recommendation text its own review affordance with no speed KPI. (b) Give SM-C1 a numeric floor and an owner. (c) Add an FR for an immutable per-Rapport provenance record retained with the Mission.

**[Adversarial]** — C-3. The "compliant Rapport" is built from data no feature collects (FR-17; §5.2 FR-4/FR-5; SM-C2)
The décret-mandated report contents — recent works and their stability impact, recommended further investigations, mesures conservatoires, prioritized works — are collected or produced by no FR. FR-17's testable consequence only checks the credentials/insurance block and layout. SM-C2 declares regulatory completeness "a floor" the FRs never build.
Fix: Add FRs (or extend FR-4/FR-17) covering every décret-mandated content block, or amend §6/§7 to state explicitly that v1 Rapports are not décret-compliant deliverables.

### High (7)

**[Done-ness clarity]** — FR-18 has zero testable consequences on the most complex behavior in the PRD (§5.6 FR-18)
Two artifacts are defined (offline render, sync-finalized PDF) without saying which is authoritative, whether the offline render is shareable via FR-20, or how the Expert knows a Rapport is provisional vs final. An engineer cannot know what "done" is; an architect must guess the sync semantics.
Fix: Add consequences: the offline render is visibly marked provisional and cannot be shared via FR-20; the finalized PDF supersedes it; finalization occurs automatically at sync.

**[Adversarial]** — H-1. FR-15 "automatic mapping onto the Coupe" hand-waves an unsolved spatial problem (FR-15; UJ-2)
No FR captures positional input that could place a désordre on a generic section drawing; the testable consequence quietly retreats to numbering consistency. A hidden dependency on a building spatial model that no FR creates.
Fix: Rewrite FR-15 honestly (manual positioning, system-guaranteed numbering consistency), or add the FR that captures the positional data enabling auto-placement.

**[Adversarial]** — H-2. The v1 model's existence rests on an unquantified shoebox of photos (§5.7; addendum "AI cold start"; SM-3; FR-11)
A six-category, ~24-sub-disorder classifier is required at v1 with no corpus count, per-category coverage estimate, labeling budget, or minimum-dataset gate. Cross-device/lighting generalization is admitted hard; a one-phone corpus maximizes that risk. The biggest feasibility dependency in the product occupies one paragraph of optimism.
Fix: Add a minimum-labeled-examples gate per category before FR-11 acceptance testing; define fallback scope (v1 = fissuration + corrosion only) if the corpus falls short.

**[Adversarial]** — H-3. v1 commits to Sévérité grading and cause inference — the two things the addendum says don't work (FR-11; FR-12; addendum)
AI severity ships on a four-level scale with no accuracy threshold; cause hypotheses ship via FR-12. An unmeasured severity output with an authoritative confidence score is an anchoring machine feeding the 5-second workflow. The structural-vs-cosmetic distinction has no dedicated metric.
Fix: Add a severity-agreement metric with a floor, or demote v1 severity to a default the Expert must actively set; state that Score de confiance applies to detection only and require the UI to reflect that.

**[Adversarial]** — H-4. `status: final` on a PRD whose feature-detail authority is missing its technical, security, and planning sections (front matter; §0; §10 Q1)
The Cahier des Charges §4–8 are unavailable and the PRD has zero security NFRs: no auth requirements, no at-rest encryption, no FR-20 secure-link requirement beyond an assumption tag, no device-loss story — for a phone full of geolocated photos of possibly dangerous occupied buildings.
Fix: Downgrade status to draft-pending-cahier, or add a minimal security NFR block and declare the cahier §4–8 formally superseded.

**[Adversarial]** — H-5. FR-18 offline Rapport generation quietly breaks the "nothing unvalidated in a Rapport" invariant (FR-18; FR-13; FR-20)
An offline artifact exists that looks like a Rapport, lacks validated Détections, and has no stated guard preventing FR-20 sharing. Also implies two render paths for a pixel-stable PDF — a hidden architecture cost. (Same root gap as the rubric's FR-18 finding, attacked from the invariant side.)
Fix: Watermark the offline artifact "PROVISOIRE — non finalisé", exclude it from FR-20 until finalized, add a testable consequence, and flag the dual-render cost to architecture.

**[Adversarial]** — H-6. "RGPD deferred entirely" is not the founders' decision to make (§7.2; §10 Q3; FR-8, FR-14, FR-21)
The Missions are the founders'; the data is not. Third-party property photos with GPS, stored by address, reused as training data starting in v1 — data subjects never consented to training reuse, and GDPR has no POC exemption. The obligation is now.
Fix: Keep the machinery deferral but add a v1 minimum: data-basis note in engagement letters, geolocation stripped/reduced in training exports, no personal data in the retraining dataset (FR-22 filter).

### Medium (11)

**[Strategic coherence]** — FR-23 exceeds what the POC thesis needs (§5.7 FR-23)
Precision/recall/F1 dashboards, model version comparison, and drift alerts are MLOps machinery for a two-founder pilot; drift alerts are also undefined. Build effort competes with the SM-1 proof gate.
Fix: Keep per-category precision from Validation outcomes as v1; defer version comparison and drift alerts to v2.

**[Done-ness clarity]** — The "content floor" of two of the three Rapport templates is never specified (§5.6 FR-17)
Rapport de visite and fiche de désordre unitaire have no content spec anywhere in PRD or addendum.
Fix: Enumerate each template's minimum content in the addendum, or delegate explicitly to the UX/template spec.

**[Done-ness clarity]** — No acceptance bar for four of six Détection categories (§5.4 FR-11, §8 SM-3)
Precision >75% covers fissures/corrosion only; whether v1 ships if the other four categories perform poorly is unstated.
Fix: State the other four categories as best-effort with no threshold, or set one.

**[Done-ness clarity]** — FR-8 geolocation has no failure behavior in the exact setting UJ-1 describes (§5.2 FR-8)
Unconditional geolocation stamping vs a basement survey with no GPS: does capture block, stamp last-known, or stamp null?
Fix: When no fix is available, capture proceeds; entry stamped with last-known location flagged approximate.

**[Adversarial]** — M-1. SM-1 is unfalsifiable as written (§8 SM-1)
"Toward ~1.5 days" is not a threshold; self-timed n=2 with no baseline protocol and learning-effect confounds.
Fix: Deskwork hours ≤50% of documented pre-tool baseline over N ≥ 5 real Missions with a defined logging method; same for SM-2.

**[Adversarial]** — M-2. SM-6 cannot be tested by anyone who exists in v1 (§8 SM-6; §7.1; §3.2)
"A new Expert productive in under one hour" — no new Expert exists in the v1 universe; a v2 metric wearing a v1 badge.
Fix: Move SM-6 to the SM-7 gate, or define a proxy tester under NDA.

**[Adversarial]** — M-3. FR-23's dashboard measures Expert-agreement, calls it precision, on sample sizes that make drift alerts fiction (FR-23; SM-C1)
The dashboard and the counter-metric feed on the same corruptible signal; rubber-stamping makes "precision" improve.
Fix: Label metrics validation-agreement; anchor true precision/recall to the frozen held-out test set; drop or gate drift alerting on minimum sample size.

**[Adversarial]** — M-4. Offline-first with multi-user editing and zero conflict semantics (FR-1, FR-2, FR-7, SM-5; §9)
"Zero data loss" specified five ways; conflict resolution nowhere. Two Experts on one Mission is the realistic pilot case.
Fix: Define conflict policy at minimum granularity and state whether concurrent Relevé editing is supported or serialized in v1.

**[Adversarial]** — M-5. Détection/FR-12 pipeline ambiguity: where do causes and recommendations attach? (§4 Glossary; FR-11 vs FR-12; FR-18)
Glossary says a Détection carries cause text; FR-11 doesn't produce it; FR-12 generates it later. Ordering is load-bearing for FR-18 and the <5s validation. Architecture will trip on this within the first week.
Fix: Split the Glossary entry (Détection = CV output; drafted text = later-bound artifact on the Fiche de désordre); state pipeline ordering in §5.4.

**[Adversarial]** — M-6. No failure-mode requirements for the AI pipeline the whole workflow depends on (§5.4; UJ-2; §9)
No analysis SLA, no per-photo status (pending/done/failed/no findings), no AI-service-down behavior, no confidence-threshold policy.
Fix: Add FR/NFR covering analysis-completion target, visible per-photo status, graceful degradation, and confidence-display policy.

**[Adversarial]** — M-7. SM-7 is the softest gate that has ever gated anything (§8 SM-7)
"Express willingness to pay" is the most notoriously worthless validation signal; competitors charge real money today.
Fix: Define the gate in behavior: signed pilot agreements, prepaid pilots, or LOIs with pricing attached.

### Low (8)

**[Decision-readiness]** — SM-1 date commitment precedes the timeline decision (§8 SM-1 vs §10 Q7)
Fix: Phrase SM-1's gate relative to pilot missions completed, or mark the date provisional pending Q7.

**[Substance over theater]** — Ergonomics list mixes bounded and unbounded items (§9 Field ergonomics)
Fix: Mark haptics/icons/dark-mode as UX-spec-owned qualities.

**[Done-ness clarity]** — Drawing/annotation tools lack done criteria (FR-10, FR-16)
Fix: Add one round-trip consequence tying annotations/Schémas to legible appearance in the exported PDF.

**[Scope honesty]** — Security/auth expectations for the POC are silent (§9)
Fix: One NFR line or §6/§7.2 entry stating the POC security floor. (Escalated to high by the adversarial review — see H-4.)

**[Downstream usability]** — User documentation has no FR (§7.1, §8 SM-6)
Fix: Add a small FR under §5.8 or an explicit deliverable line for the epics workflow.

**[Adversarial]** — L-1. "5 most common smartphone models among the pilot Experts" — the pilot Experts are two people (§7.1; §10 Q4)
Fix: "The pilot Experts' actual devices, plus N representative iOS/Android models chosen at architecture time."

**[Adversarial]** — L-2. Bluetooth stylus and external keyboard support is gold-plating for a two-user POC (FR-16; §9)
Fix: Finger-drawing required; stylus as generic touch input; delete external keyboard from v1 unless a journey needs it.

**[Adversarial]** — L-3. "Why now" overstates the whitespace its own addendum undercuts (§2; addendum)
Fix: Tighten §2 to the defensible claim (francophone + décret-template alignment); add the arrêté-divergence contingency to §10/FR-19.

## Mechanical notes

- Assumptions Index roundtrip clean: 11 inline tags, 11 index entries; UJ-3's two tags merged into one index line (harmless).
- ID continuity clean: FR-1–FR-24, UJ-1–UJ-3, SM-1–SM-7 + SM-C1/C2; all cross-references resolve.
- §5.8 (Roles) is the only feature group without a "Realizes UJ-x" tag — cross-cutting, but the epics workflow should not treat it as orphaned.
- Cahier §9.x citations survive only via the brief's addendum; downstream workflows should treat the brief addendum as the acceptance-text source.
- No glossary drift found.
- Title block still reads "*Working title — confirm.*" in a `status: final` document.

## Reviewer files

- `review-rubric.md`
- `review-adversarial-general.md`
