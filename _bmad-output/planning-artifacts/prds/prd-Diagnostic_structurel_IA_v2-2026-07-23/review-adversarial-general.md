# Adversarial Review — PRD: Diagnostic Structurel IA (v1)

**Reviewed:** `prd.md` (updated 2026-07-27, status: final) + `addendum.md`
**Review date:** 2026-08-02
**Stance:** Cynical. The PRD is well-written, which is exactly why it deserves suspicion — polish hides holes. Findings below, each with severity, location, and a fix. Severity scale: critical / high / medium / low.

---

## Verdict

This PRD reads like a scope contract but behaves like a pitch. Its success metrics are self-graded homework, its flagship compliance claim (décret-compliant Rapport) is not backed by any FR that collects the décret-mandated content, and its central legal safety device (Expert Validation) is actively undermined by its own 5-second ergonomics target with no audit trail to show, in litigation, who wrote what. It cannot honestly carry `status: final` while its declared feature-detail authority is missing sections §4–8.

---

## Critical

### C-1. SM-3 is a self-graded, precision-only, recall-blind vanity metric
**Location:** §8 SM-3; FR-11 consequences; addendum "AI cold start".
The 75% precision target is measured on a test set built from the founders' own seed corpus, labeled by the founding Expert — the same person who pilots the product, validates the outputs, and wants the venture to succeed. The person grading the model wrote the answer key. Worse: **there is no recall target anywhere.** A model that flags one obvious lézarde per building and nothing else achieves 100% precision and passes SM-3 while missing every désordre that matters. For a triage tool on potentially dangerous habitat-dégradé buildings, false negatives are the actual risk — the metric structure optimizes for exactly the wrong failure mode, and precision-only targets are trivially gameable by raising the confidence threshold.
**Fix:** Add a recall (or miss-rate) floor per category alongside precision; require the test set to include held-out pilot-mission data not labeled by the model's own trainer (the second founder can blind-label a slice); freeze the test set before model tuning and version it.

### C-2. The 5-second Validation target manufactures the rubber-stamping the PRD claims to fear, and there is no legal audit trail
**Location:** FR-13 consequence; §8 SM-4; SM-C1; FR-14; addendum "Regulatory depth".
The PRD's entire liability posture is "the Expert disposes and signs." Then FR-13/SM-4 set a target of processing each Détection — nature, localization, severity, *plus reading AI-drafted cause hypotheses and recommendations* — in **under 5 seconds**. Nobody critically evaluates a causal hypothesis in 5 seconds; that target is a KPI for rubber-stamping. SM-C1 acknowledges the risk and then does nothing: no threshold, no trigger, no defined action, no owner. A counter-metric with no floor is decoration.
Compounding it: FR-14 stores corrections as *training data*, not as a *legal audit trail*. Nothing requires the final Rapport (or its retained record) to distinguish AI-drafted text the Expert accepted verbatim from text the Expert authored. Post décret 2025-814, when a validated-in-4-seconds "hypothèse d'origine" turns out wrong and a building fails, the engineer's insurer and opposing counsel will ask exactly that question, and the product will have no answer.
**Fix:** (a) Restrict the <5s target to accept/reject of *detections*; give cause/recommendation text its own review affordance with no speed KPI. (b) Give SM-C1 a numeric floor (e.g. correction+rejection rate below X% over a rolling window triggers review) and an owner. (c) Add an FR: immutable per-Rapport provenance record — which content was AI-drafted, what the Expert changed, timestamps — retained with the Mission.

### C-3. The "compliant Rapport" is built from data no feature collects
**Location:** FR-17; §5.2 FR-4/FR-5; SM-C2; addendum "Regulatory depth".
The addendum lists the décret-mandated report contents: recent works and their stability impact, **recommended further investigations**, **mesures conservatoires**, **prioritized works**. Now search the FRs: the Relevé form (FR-4/FR-5) captures observations per Zone/Élément; FR-12 drafts "preliminary surveillance/intervention recommendations"; nothing anywhere collects recent-works history, produces a mesures-conservatoires section, or supports prioritizing works. FR-17's testable consequence only checks the credentials/insurance block and layout. SM-C2 declares regulatory completeness "a floor" — but the FRs never build the floor. The payoff feature ships structurally non-compliant while the PRD congratulates itself on compliance.
**Fix:** Either add FRs (or extend FR-4/FR-17) covering every décret-mandated content block — recent works capture in the Mission/Relevé, mesures conservatoires and prioritized-works sections in the Rapport — or amend §6/§7 to state explicitly that v1 Rapports are *not* décret-compliant deliverables and the compliance claim in §2/§8 is aspirational.

---

## High

### H-1. FR-15 "automatic mapping onto the Coupe" hand-waves an unsolved spatial problem
**Location:** FR-15; UJ-2.
"Numbered survey points map automatically onto the selected Coupe" — onto a *generic library drawing* of "mur" or "plancher". Automatic mapping requires knowing *where on the section* each désordre sits. No FR captures positional input beyond GPS (useless indoors and on a vertical section) and Zone/Élément (far too coarse: "façade nord, voile" does not place a point on a wall section). The one testable consequence quietly retreats to *numbering consistency* — an admission that "automatic mapping" is really "the Expert drags points manually and the numbers match." A hidden dependency on a building spatial model that no FR creates.
**Fix:** Rewrite FR-15 honestly: Expert positions désordres manually on the Coupe; the *system* guarantees numbering and cross-reference consistency. If genuine auto-placement is intended, add the FR that captures the positional data enabling it.

### H-2. The v1 model's existence rests on an unquantified shoebox of photos
**Location:** §5.7 description; addendum "AI cold start"; SM-3; FR-11.
FR-11 requires a working six-category, ~24-sub-disorder classifier at v1. The training data is "own site photos, past reports, worked examples" — no count, no per-category coverage estimate, no labeling-effort budget, no minimum-dataset gate before FR-11 is deemed feasible. If the corpus yields 40 usable examples of "flambement," the model is fiction for that class and SM-3 is unfalsifiable noise. The addendum's own benchmark note admits cross-device/lighting generalization is a hard open problem; a corpus from one engineer's phone maximizes that exact risk. This is the single biggest schedule/feasibility dependency in the product and it occupies one paragraph of optimism.
**Fix:** Add an explicit gate: minimum labeled examples per category (and per capture device/condition) before FR-11 acceptance testing; define fallback scope (e.g. v1 detects fissuration + corrosion only — which is all SM-3 measures anyway) if the corpus falls short. Say out loud that the other four categories have no accuracy commitment.

### H-3. v1 commits to Sévérité grading and cause inference — the two things the addendum says don't work
**Location:** FR-11 (Sévérité in every Détection); FR-12; addendum final bullet ("hard problems = structural-vs-cosmetic distinction, … severity grading, cause inference").
The PRD's own addendum flags severity grading and cause inference as beyond the current state of the art, then FR-11 ships AI severity on a four-level scale with **no accuracy threshold whatsoever** (the 75% applies only to fissures/corrosion detection precision), and FR-12 ships cause hypotheses. An unmeasured severity output shown with an authoritative-looking percentage confidence score is an anchoring machine: the Expert sees "sévère — 82%" and the 5-second workflow does the rest. The structural-vs-cosmetic distinction — the one classification that actually matters for an R0/R1 mission — has no dedicated metric at all.
**Fix:** Either add a severity-agreement metric (AI grade vs Expert-corrected grade, per category) with a floor, or demote v1 severity to a default the Expert must actively set (AI pre-fills nothing above "modéré", say). State explicitly that Score de confiance applies to detection, not to severity or causes, and require the UI to reflect that.

### H-4. `status: final` on a PRD whose feature-detail authority is missing its technical, security, and planning sections
**Location:** Front matter; §0; §10 Open Question 1; addendum "Source-document notes".
The Cahier des Charges — declared "feature-detail authority" — is truncated: §4–8 (presumed technical requirements, security, planning) are unavailable, and the PRD marks this Open Question #1 while stamping itself `final`. Consequence visible in the document: there are **zero security NFRs**. No authentication requirements, no at-rest encryption for a phone full of geolocated photos of possibly dangerous occupied buildings, no requirement on FR-20's "secure link" beyond an assumption tag, no device-loss story. These photos are potential evidence in insurance and habitat-dégradé proceedings; "POC, founders only" does not make a stolen phone less lost.
**Fix:** Downgrade status to draft-pending-cahier, or add a minimal security NFR block (auth, at-rest encryption on device, link expiry + revocation for FR-20, retention) and declare the cahier §4–8 formally superseded rather than "presumed."

### H-5. FR-18 offline Rapport generation quietly breaks the "nothing unvalidated in a Rapport" invariant
**Location:** FR-18; FR-13 consequence; FR-20.
FR-18 lets the Expert render a Rapport offline, "mid-Mission on an in-progress Relevé," with AI content merged in "when the PDF is finalized at sync." So an offline artifact exists that looks like a Rapport, contains no validated Détections (or pre-validation data), and FR-20's export/email/share has no stated guard preventing that artifact from leaving the device. FR-13's proud consequence — "no unvalidated Détection can appear in a Rapport" — is silent on the inverse: a Rapport with the AI layer *absent* and no marking that it is provisional. It also implies two render paths (on-device + post-sync merge) for a pixel-stable paginated PDF — a hidden architecture cost tossed off in one sentence.
**Fix:** Define the offline artifact's status explicitly (watermarked "PROVISOIRE — non finalisé", excluded from FR-20 sharing until finalized), and add a testable consequence to FR-18. Flag the dual-render cost to architecture instead of letting them discover it.

### H-6. "RGPD deferred entirely" is not the founders' decision to make
**Location:** §7.2; §10 Open Question 3; FR-8, FR-14, FR-21.
The deferral rationale is "the POC runs exclusively on the founders' own Missions." The *Missions* are the founders'; the *data* is not. The photos capture third parties' property — occupied collective housing — stamped with GPS coordinates (FR-8), stored in a structured database by address (FR-21), and reused as AI training data (FR-14) starting in v1, not v2. The data subjects (owners, syndics, occupants whose interiors/belongings appear in basement and stairwell photos) never consented to training reuse. GDPR does not have a "we're a POC" exemption; the founders' client contracts may not even permit this secondary use. The training-loop reuse happens now, so the obligation is now.
**Fix:** Keep the machinery deferral, but replace "deferred entirely" with a v1 minimum: a data-basis note in the founders' mission engagement letters covering photo capture and training reuse, geolocation precision reduced or stripped from training exports, and no personal data (faces, plates, interiors identifying occupants) in the retraining dataset (FR-22 filter).

---

## Medium

### M-1. SM-1 is unfalsifiable as written
**Location:** §8 SM-1.
"Drops from ~3.5 days **toward** ~1.5 days" — "toward" is not a threshold; 3.4 days qualifies. Measured on n=2 users who are the product's owners, with no baseline measurement protocol, self-timed, and confounded by ordinary learning effects (the founders get faster at their own missions regardless of tooling). The "proof gate" question mark ("does it cut deskwork in half?") is better, but it's phrased as a rhetorical aside, not the metric.
**Fix:** Make the gate the metric: deskwork hours per Mission ≤50% of a documented pre-tool baseline, over N ≥ 5 real Missions, with a defined time-logging method. Same treatment for SM-2's "≥30%".

### M-2. SM-6 cannot be tested by anyone who exists in v1
**Location:** §8 SM-6; §7.1; §3.2.
"A new Expert is productive in under one hour using only the documentation." §3.2 explicitly excludes every human who is not a founder, and both founders built the product. There is no "new Expert" in the v1 universe; the metric validates FR-4/FR-13 against a user who cannot legally exist until after the SM-7 gate. It's a v2 metric wearing a v1 badge.
**Fix:** Move SM-6 to the SM-7/commercialization gate, or define a proxy (a recruited non-user colleague under NDA performs a scripted cold-start session).

### M-3. FR-23's dashboard measures agreement with the Expert, calls it precision, on sample sizes that make drift alerts fiction
**Location:** FR-23; SM-C1.
Precision/recall/F1 require ground truth; the only available truth source is Expert Validations — the very signal SM-C1 warns may collapse into rubber-stamping. So the dashboard measures Expert-agreement, and if complacency sets in, "precision" *improves* while real accuracy is unknown: the dashboard and the counter-metric feed on the same corrupted signal. Add "performance-drift alerts" computed over two users and a handful of missions per month, and you have statistically meaningless alarms that either cry wolf or never fire.
**Fix:** Label dashboard metrics as validation-agreement, not precision; anchor true precision/recall to the frozen held-out test set (C-1 fix) per model version; drop drift alerting from v1 or define the minimum sample size at which it activates.

### M-4. Offline-first with multi-user editing and zero conflict semantics
**Location:** FR-1 (offline Mission creation), FR-2 (multiple Experts per Mission), FR-7, SM-5; §9 NFRs.
"Zero data loss" is specified five different ways; conflict *resolution* is specified nowhere. Two assigned Experts (FR-2 allows it; both founders on one Mission is the realistic pilot case) edit the same Relevé, or a Mission is created offline on mobile while the Chef de projet edits it in the back-office. Last-write-wins? Merge? Duplicate Missions on address collision (FR-3 matches by address)? Every offline-first product dies on this hill, and the PRD — whose hardest constraint is offline-first — never mentions it. "Zero data loss" without merge semantics can mean "both versions kept, Rapport built from the wrong one."
**Fix:** Add an NFR or FR-7 consequence defining conflict policy at minimum granularity (e.g. per-Élément entries merge, concurrent edits to the same field surface a conflict for the Expert), and state whether concurrent Relevé editing by two Experts is supported or serialized in v1.

### M-5. Détection/FR-12 pipeline ambiguity: where do causes and recommendations actually attach?
**Location:** §4 Glossary "Détection"; FR-11 vs FR-12; FR-18.
The Glossary defines a Détection as *carrying* cause hypotheses and recommendations; FR-11 (CV output) doesn't produce them; FR-12 generates them "over the Détection + Relevé context" — i.e., after detection, dependent on Relevé completeness. So: are causes generated per-photo at analysis time, or per-Désordre after the Relevé closes? Can a Détection exist awaiting its FR-12 text? What does the Expert validate in <5s — with or without the draft text? FR-18's "AI content integrates at sync" makes the ordering question load-bearing. Architecture will trip on this within the first week.
**Fix:** Split the Glossary entry (Détection = CV output; drafted text = separate, later-bound artifact on the Fiche de désordre), and state the pipeline ordering explicitly in §5.4.

### M-6. No failure-mode requirements for the AI pipeline the whole workflow depends on
**Location:** §5.4; UJ-2; §9.
UJ-2 assumes the synced Relevé "has been processed" by next morning — an implicit analysis SLA that appears nowhere as a requirement. No FR covers: AI service down or backlogged at sync; a photo that yields zero Détections (indistinguishable from "not yet analyzed"?); partial analysis; low-confidence behavior (is there a threshold below which Détections are suppressed, or does the Expert wade through 5% confidence noise — directly attacking the <5s and SM-1 targets?). For a product whose pitch is "next morning it's drafted," analysis latency and failure UX are requirements, not implementation details.
**Fix:** Add FR/NFR: analysis-completion target after sync, visible per-photo analysis status (pending / done / failed / no findings), defined behavior on AI-service unavailability (manual flow degrades gracefully), and a stated confidence-display/threshold policy.

### M-7. SM-7 is the softest gate that has ever gated anything
**Location:** §8 SM-7.
"A handful of target-segment experts **express willingness to pay**." Expressed willingness is the single most notoriously worthless signal in product validation — the addendum itself documents competitors charging real money ($99–109/mo), so the honest bar exists and is known. A commercialization decision hinging on polite conversation is not a gate, it's a formality.
**Fix:** Define the gate in behavior: N signed pilot agreements, prepaid pilots, or LOIs with pricing attached — anything where the expert gives something up.

---

## Low

### L-1. "5 most common smartphone models among the pilot Experts" — the pilot Experts are two people
**Location:** §7.1; §10 Open Question 4.
Two founders own, at most, two or three phones. "5 most common models" among them is arithmetic that doesn't parse; it's a phrase inherited from a multi-user cahier and pasted into a two-user POC.
**Fix:** "The pilot Experts' actual devices, plus N representative iOS/Android models chosen at architecture time."

### L-2. Bluetooth stylus and external keyboard support is gold-plating for a two-user POC
**Location:** FR-16; §9 Platform NFR.
Cross-platform Bluetooth stylus support (pressure? palm rejection? which protocols?) is a real engineering cost with no acceptance criterion, serving a sketch tool that UJ-2 uses once per mission. External keyboard support serves no journey in the document.
**Fix:** Downgrade to "finger-drawing required; stylus works as a generic touch input; dedicated stylus features deferred," and delete external keyboard from v1 unless a journey needs it.

### L-3. "Why now" overstates the whitespace its own addendum undercuts
**Location:** §2; addendum "Competitive landscape" / "Exploitable gaps".
§2 says "no francophone-native, phone-photos-in / draft-report-out tool exists" — but the addendum shows T2D2 does phone-photos-in, report-out today at $99/mo, and the narrative-drafting gap ("none draft the diagnostic narrative") is contradicted by Spectora's AI Comment Assist in the adjacent segment. The real, defensible gap is narrower and stated more honestly in the addendum: French pathology vocabulary + FR regulatory template. And that template (the arrêté) doesn't exist yet — the sharpest differentiator is a dependency on an unpublished government document (Open Question 2).
**Fix:** Tighten §2 to the defensible claim (francophone + décret-template alignment), and note in §10/FR-19 the contingency if the arrêté's template diverges structurally from the current Rapport model.

---

## Counts

| Severity | Count |
|---|---|
| Critical | 3 |
| High | 6 |
| Medium | 7 |
| Low | 3 |
| **Total** | **19** |
