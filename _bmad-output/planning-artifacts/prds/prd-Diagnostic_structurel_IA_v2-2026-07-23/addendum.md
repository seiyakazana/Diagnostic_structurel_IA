# PRD Addendum — Diagnostic_structurel_IA_v2

Depth that informs downstream documents (architecture, UX spec, epics) but does not belong in the PRD narrative.

## Competitive landscape (web research, 2026-07-23)

- **T2D2** (t2d2.ai, US, Thornton Tomasetti spin-off) — closest comparable. CV damage detection (cracks, spalls, corrosion) on facade/drone/phone photos + "Inspection Cloud" auto-generating condition-assessment reports. Seat-based pricing from $99/mo.
- **Spectora** (spectora.com, US) — dominant home-inspection report writer (10k+ inspectors), "AI Comment Assist" drafts defect comments. $109/mo solo — proves the solo-pro SaaS price point.
- **Hosta a.i.** — insurtech photo-to-assessment (floorplans, measurements, damage classification, repair estimates); enterprise-only.
- **SpaceCapture** — real-time snag-list generation from site photos; claims ~50% inspection-time reduction.
- **TÜV SÜD × Contilio** — 3D-AI scan-based defect analysis; enterprise construction QA.
- **Francophone**: mostly drone *service providers* with AI crack detection (GEO2R, Dronelis, Immodrone, Buildwise demo) — no established French-language self-serve equivalent of T2D2.
- **From the brief's own digest** (kept, not replaced): **PlanRadar** is the EU mass-market field-SaaS incumbent (photos/snags, no compliant structural deliverable); **Inspectly360** (offline + on-device AI) is the closest UX overlap; STRUCINSPECT/Niricson occupy the enterprise/drone tier; AQC "Batist"/Sycodés (~60k claims, 30 yrs) is the institutional dataset signal.

### Positioning norms
Universal framing: assistant, never replacement; liability stays with the licensed professional; AI output presented as draft/triage. Report generation is the monetized wedge everywhere. Vendor accuracy claims (95%+) exceed academic benchmarks (~78% mIoU crack segmentation, arxiv.org/pdf/2508.11517).

### Exploitable gaps
1. Serious players are enterprise/drone/3D-scan oriented — no lightweight phone-photos-in, draft-report-out tool for the independent structural engineer.
2. Nothing francophone-native (French pathology vocabulary, report conventions, FR/BE/CA norms).
3. Detection tools stop at annotated images — none draft the diagnostic narrative (the engineer's actual deliverable).
4. No tool maps output to the new French regulatory report template (décret 2025-814; official template coming by arrêté).

## Désordre taxonomy (cahier §2.2.1 — liste évolutive)

Sub-disorders per category; this table drives AI classification labels, survey dropdowns, and Fiche de désordre vocabulary:

| Category | Example sub-disorders |
|---|---|
| Fissuration | fissures de retrait, fissures structurelles, lézardes, microfissures |
| Corrosion / carbonatation | épaufrures, armatures apparentes, taches de rouille, bullage |
| Humidité / infiltrations | remontées capillaires, moisissures, efflorescences, salpêtres |
| Déformations | flambement, désaffleurements, tassements différentiels, dévers |
| Désordres maçonnerie | déliaisonnements, décollements, joints dégradés, faïençage |
| Dégradations diverses | éclatements, défauts d'enrobage, cavités, impacts |

## AI cold start (seed corpus)

The founding Expert is both domain expert and data engineer: he assembles and labels the seed dataset himself from crown-jewel in-house inputs — own site photos, past reports, worked examples, norms/reference documents. This mitigates the cold-start risk behind SM-3; the dedicated test set is drawn from this corpus plus pilot-mission corrections (FR-14, FR-22).

## Regulatory depth (décret n° 2025-814, 12 août 2025)

**Who may sign:** the diagnosis must be signed by an independent Bac+5 construction/GC engineer with ≥2 years' experience, carrying RC Pro ≥ €1M per claim / €1.5M per year; a DTG (copropriété) requires a Bac+3 qualified diagnostician with RC Pro. Consequence: **AI can never sign; a named human engineer bears liability** — and the Rapport must carry the credentials/insurance block (FR-17).

Report must contain: professional credentials + insurance; building description; structural elements examined and observed désordres; recent works and stability impact; recommended further investigations; mesures conservatoires; prioritized works. Official report template expected by arrêté — the report generator should mirror this structure and be updatable when the arrêté lands. (Légifrance: JORFTEXT000052097247)

Mission framing: existing-structure mission sequencing R0 (investigations) → R1 (diagnostic) → R5 (expertise), modeled on NF P 94-500. The product sits at **R0/R1**: observations + hypotheses, explicitly **not** a stability calculation — the tool must keep that scope boundary and engineer sign-off explicit.

## Source-document notes

- Brief (2026-07-10) is the newer **scoping** authority; Cahier des Charges v1.1 (May 2026) is the **feature-detail** authority.
- Cahier file on disk is truncated: §1–3 only; §9 acceptance criteria survive via the brief's addendum; §4–8 unavailable.
- No stack, framework, hosting, or model-architecture choices are made anywhere in the source docs.
- AI benchmark context: binary crack detection ~96–99% (commoditized); hard problems = structural-vs-cosmetic distinction, cross-device/lighting generalization, severity grading, cause inference.
