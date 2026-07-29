# PRD Quality Review — Diagnostic Structurel IA

*Calibration: POC PRD; first users are the two founders on their own real missions. Severity ranks impact on downstream UX/architecture/epics work, not launch-grade completeness.*

## Overall verdict

This is an unusually disciplined POC PRD: it has a real thesis ("compresses that deskwork into a review-and-sign step"), scope honesty that does actual work (RGPD deferral with an explicit re-entry gate, "No claimed AI moat"), and a clean assumption/index roundtrip. The main risk is concentrated in one place: done-ness leans on "(acceptance §9.1/§9.2/§9.3)" cross-references that resolve to nothing inside this PRD or its addendum — they point at a Cahier des Charges §9 the addendum itself says is only recoverable "via the brief's addendum," which is not in this workspace. Fix that resolution path and tighten the tail of consequence-free FRs, and this PRD is ready to feed UX, architecture, and epics.

## Decision-readiness — strong

Decisions are stated as decisions, with what was given up named. "The AI proposes, the Expert disposes and signs" (§1) is declared "non-negotiable" and then actually enforced downstream (FR-13 "No unvalidated Détection can appear in a Rapport"; §9 Trust surface). The RGPD deferral (§7.2) is the kind of call most PRDs would bury — here it is explicit, justified by the POC framing ("the POC runs exclusively on the founders' own Missions"), and paired with a `[NOTE FOR PM]` naming it "a hard gate before any external user." §10 Open Questions are genuinely open (arrêté template date, retraining cadence, pricing), each with an owner or a trigger (e.g. "revisit at SM-7 gate"). No findings needed.

## Substance over theater — strong

Content is earned throughout. Two personas, both of whom drive decisions: Marc's gloves/no-signal context (UJ-1) directly produces the §9 field-ergonomics NFRs ("≤3 taps," "≥14pt type," one-handed operation), and the Chef de projet exists only to carry the back-office pipeline (UJ-3). NFRs carry product-specific thresholds, not boilerplate: "sync with zero loss within 30s of reconnection," "durability across force-close." The Vision quantifies its claim ("~3.5 days, of which only half a day is spent at the building… ~85% is deskwork") — it could not swap into another PRD. The Non-Goal "No claimed AI moat" (§6) is a rare piece of anti-theater: it disclaims novelty rather than manufacturing it, and the addendum grounds differentiation in researched gaps (no francophone-native, "phone-photos-in / draft-report-out" tool) rather than template filler. No findings.

## Strategic coherence — strong

The thesis — deskwork compression via expert-validated AI assist — organizes everything. Feature order in §5 follows the pipeline (Mission → Relevé → photos → Détection/Validation → Coupes → Rapport → learning loop), and SM-1 tests the thesis directly ("does it cut the founder's own deskwork in half, on real missions?") rather than measuring activity. Counter-metrics exist and are sharp: SM-C1 treats a near-zero correction rate as "a red flag (automation complacency), not a success" — exactly the failure mode of this product category. MVP scope kind is coherently problem-solving (founders' real missions as the proving ground), and SM-7 explicitly "validates nothing in v1 — it decides v2," keeping commercialization out of the thesis.

### Findings
- **medium** SM-6 depends on a deliverable no FR produces (§8 SM-6) — "productive in under one hour using only the documentation" presumes user documentation exists, but no FR or scope item commits to writing it. Downstream epics will have nothing to trace this metric to. *Fix:* either add documentation as an FR/scope line item, or restate SM-6 against the product's contextual guidance (§9 "contextual guidance and clear errors") and drop the documentation dependency.

## Done-ness clarity — adequate

The load-bearing FRs are genuinely testable: FR-7 ("syncs with zero data loss within 30 seconds"), FR-11 ("precision exceeds 75% on the dedicated test set"), FR-13 ("in under 5 seconds"), FR-1 (exact enum of three usage values). But the dimension has two soft spots. First, nine FRs and five SMs anchor their acceptance in "(acceptance §9.1/§9.2/§9.3)" — and those references do not resolve to anything in this document set (see the high finding below). Second, roughly half the FRs ship without any testable consequence: FR-2, FR-3, FR-6, FR-8, FR-9, FR-10, FR-16, FR-18, FR-20, FR-21, FR-23, FR-24. For a POC most of these are tolerable (FR-8, FR-9 are near-self-evident), but FR-18 and FR-23 hide real ambiguity.

### Findings
- **high** Acceptance references "§9.1/§9.2/§9.3" resolve nowhere (§5 FR-7/11/13/14/15/17, §8 SM-2–SM-6) — the PRD's own §9 is "Cross-Cutting NFRs" with no numbered subsections; the addendum says the Cahier des Charges "§9 acceptance criteria survive via the brief's addendum," a document not in this workspace. Story creation will lean on these hardest and will find dangling pointers. *Fix:* inline the §9.1–§9.3 acceptance criteria into this PRD (a short Acceptance section or per-FR expansion), or copy them into addendum.md with stable anchors and repoint every reference.
- **medium** FR-18 offline Rapport generation has undefined semantics (§5.6) — "request Rapport generation from the field while offline; the PDF is finalized at sync" leaves open what the Expert sees between request and sync, whether a preview exists offline, and what "finalized" means. This is an architecture-shaping behavior with no testable consequence. *Fix:* add a consequence, e.g. "an offline generation request survives force-close and produces the finalized PDF within N minutes of sync, with the request's status visible to the Expert."
- **medium** Unbounded phrases in field-performance FRs (§5.2 FR-5, §9) — "no perceptible wait on local entry" is exactly the "reasonable performance" pattern; §9 repeats it ("no waiting on local entry"). FR-23's "performance-drift alerts" has no drift definition or threshold. *Fix:* bound FR-5 (e.g. local entry renders in <100ms) and define drift for FR-23 (e.g. alert when per-category precision drops >X points vs. the deployed baseline).
- **low** FR-17 "remains readable regardless of photo/Schéma volume" (§5.6) — "readable" is subjective; the intent (layout does not break at high volume) is clear but the check is not. *Fix:* restate as concrete invariants: TOC/pagination correct, no clipped images, cover page intact at N photos.

## Scope honesty — strong

The best dimension of this PRD. §6 Non-Goals do real work — each one closes off a plausible silent assumption (in-app signature, ouvrages d'art "including as a form option," multi-tenant billing). All eight `[ASSUMPTION]` tags are inferences a reader would otherwise absorb silently (bounding boxes vs. segmentation, LLM-vs-CV mechanism for FR-12, secure-link semantics), and every one is indexed in §11. `[NOTE FOR PM]` callouts sit at the two genuine tensions: the arrêté template (FR-19) and the RGPD gate (§7.2). Open-items density (6 Open Questions + 8 assumptions + 2 PM notes) is high in absolute terms but exactly right for a POC with a truncated upstream source document — and Open Question 1 confronts that truncation head-on rather than papering over it. No findings.

## Downstream usability — adequate

The Glossary (§4) is the PRD's backbone and is used with discipline — Mission/Relevé/Zone/Élément structurel/Désordre/Détection/Validation appear identically across UJs, FRs, and SMs, and §4 opens with "Downstream workflows must use these terms exactly." FR IDs run contiguously 1–24, UJs 1–3, SMs 1–7 plus C1/C2; SM→FR validation links resolve. The one structural defect is the dangling acceptance-§9 references (counted under Done-ness — it hurts here equally: source-extraction from FR-7 or SM-5 hits a pointer with no target). Sections pull out cleanly on their own.

### Findings
- **low** UJ-3 protagonist unnamed (§3.3) — UJ-1/UJ-2 carry "Marc" inline; UJ-3's protagonist is only "the second founding Expert acting as Chef de projet." Minor, but the rubric's UJ convention is a named protagonist carrying context. *Fix:* name her/him (with the same `[ASSUMPTION]` pattern as Marc).
- **low** "standard Relevé" undefined (§8 SM-2) — the 30%-faster claim is measured against a "standard Relevé" that no section defines, so the benchmark unit is unfixed. *Fix:* one sentence defining the reference survey (e.g. building size/zone count typical of the founders' missions).

## Shape fit — strong

The PRD correctly reads its own situation: chain-top (feeds UX → architecture → epics), meaningful field UX, two-person pilot. UJs are load-bearing, not overhead — UJ-1's offline/gloves context and UJ-2's validation-flow timing generate most of the NFRs and acceptance thresholds. The three-role model could have been over-formalization for two founders, but §5.8 pre-empts the objection explicitly: "In v1 the two founders cover all roles; the model exists so the pilot mirrors real cabinet structure." Splitting competitive/regulatory depth into addendum.md keeps the PRD narrative at the right altitude. English-PRD/French-product is flagged as a hard requirement in §0 and enforced as a §9 NFR. No findings.

## Mechanical notes

- **Assumptions Index roundtrip: clean.** Eight inline `[ASSUMPTION]` tags (UJ-1, UJ-2, UJ-3, FR-6, FR-11, FR-12, FR-20, FR-22) ↔ eight §11 entries; both directions match.
- **ID continuity: clean.** FR-1–FR-24 contiguous, no duplicates; UJ-1–UJ-3; SM-1–SM-7 + SM-C1/C2. All "Realizes UJ-x" and "Validates FR-x" references resolve.
- **Broken cross-refs:** the "(acceptance §9.x)" family — covered as the high finding above; it is the only unresolvable reference pattern in the document.
- **Glossary drift: none found.** French domain terms are used with consistent casing and accents throughout (Désordre, Sévérité, Élément structurel, Fiche de désordre).
- **Title unresolved:** "*Working title — confirm.*" (line under the H1) is still pending; harmless for downstream but should be settled before the PRD is cited by name in architecture/epics.
- **SM-1 timeframe:** "this year" is a relative date in a dated document — fine for a 2026 POC, but "by end 2026" would age better.
