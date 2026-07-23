# Addendum — Diagnostic Structurel IA

Downstream detail that supports the brief but belongs in the PRD / architecture / market-research stage. Preserved here so it isn't lost.

**Source of functional detail:** [docs/Cahier_des_Charges_DiagnosticStructurel_IA.md](../../../../docs/Cahier_des_Charges_DiagnosticStructurel_IA.md) (v1.1, May 2026, French) — the authoritative feature-level spec. This addendum captures only what the user volunteered in conversation that extends or refines it.

---

## 1. Time breakdown per diagnostic (current, manual)

| Phase | Time | Notes |
|-------|------|-------|
| On-site capture | ~0.5 day | Fast — **not** the pain |
| Report write-up | ~1.5 days | **The core pain** — most dreaded, least reusable |
| Placing annotated photos against disorders | ~0.5 day | Repetitive |
| Drawing coupes + schémas | ~0.5–1 day | Skilled but time-consuming |
| **Total** | **~3–3.5 days** | **~85% is post-site deskwork** — the product's target |

## 2. Team / build capacity

- **Founder:** data engineer **and** practicing building/structural-diagnostic expert. Can build the app + AI *and* label/validate as the domain expert.
- **Support:** a second building expert (pilot). Together = the "pilot experts" who validate acceptance criteria.
- **Crown-jewel inputs held in-house:** own site photos, past diagnostic reports, worked examples, and all required norms/reference documents — softens the AI cold-start.

## 3. Acceptance criteria (spec §9, verbatim, French)

Distilled into the brief's Success Criteria; full text preserved for the PRD.

### 9.1 Module relevés terrain
- Un expert terrain peut créer une mission, saisir l'intégralité d'un relevé et prendre des photos sans connexion réseau
- La synchronisation s'effectue sans perte de données dans un délai de 30 secondes après reconnexion
- Le temps de saisie d'un relevé standard est inférieur de 30 % au processus actuel (validé par les experts pilotes)
- L'expert peut générer un PDF de synthèse du relevé incluant l'ensemble des données saisies, les photos annotées, au moins une coupe technique pertinente et un schéma annoté de la zone diagnostiquée
- Le report des désordres sur la coupe/le schéma est cohérent avec leur numérotation dans le relevé et la fiche de désordre associée
- Le PDF généré respecte la mise en page définie (page de garde, sommaire, pagination) et reste lisible quel que soit le volume de photos et de schémas

### 9.2 Module IA
- Pour les fissures et désordres de corrosion, le modèle atteint une précision supérieure à 75 % sur le jeu de test
- Le score de confiance est affiché pour chaque détection
- L'expert peut valider, corriger ou rejeter chaque résultat en moins de 5 secondes
- Les corrections de l'expert sont bien enregistrées et exportables pour réentraînement

### 9.3 Critères globaux
- L'application est disponible sur iOS et Android, testée sur les 5 modèles de smartphones les plus répandus parmi les experts
- Aucune donnée n'est perdue en cas de fermeture forcée de l'application
- La documentation utilisateur permet à un expert non formé de prendre en main l'outil en moins d'une heure

## 4. Regulatory & competitive context (research digest)

**Regulatory tailwind (France):** Décret n°2025-814 (12 Aug 2025), under *loi Habitat dégradé* (art. L.126-6-1 CCH), mandates structural diagnosis of collective housing in commune-designated zones. Diagnosis must be signed by a **Bac+5 engineer** (≥2 yrs, independent) with **RC Pro ≥ €1M/claim, €1.5M/year**; DTG (copropriété) requires a Bac+3 qualified diagnostician with RC Pro. → **AI can never sign; a named human engineer bears liability.** Reinforces the "assist, not diagnose" model.

**Competitors:**
- *Structural-specific AI:* **STRUCINSPECT** (AT — bridges/infra, digital twin), **Niricson** (CA — drone + acoustic/IR, enterprise), **Trendspek+Datature** (AU), **betonexpertise** (BE/NL, francophone-adjacent). Mostly enterprise/drone tier.
- *Field-inspection SaaS (adjacent):* **PlanRadar** (dominant EU incumbent, 150k+ users, not AI-diagnostic), **Dalux**, **Fieldwire/Hilti**, **Procore**, **Inspectly360** (offline + on-device AI — closest UX overlap).
- *France institutional:* **AQC "Batist"** predictive AI on the Sycodés claims DB (~60k sinistres, 30 yrs) + "Hiiro" detection robot — not commercial, but signals institutional appetite.

**Gap (takeaway):** nobody obviously owns "francophone, mobile, engineer-grade guided survey + AI classification + compliant PDF report" at the *individual diagnostician* level.

**State of the AI:** binary crack detection ~96–99% on benchmarks (commoditized). Genuinely hard: structural-vs-cosmetic distinction, generalization across devices/lighting/structure types, **severity grading** (under-researched), **cause inference** (not a solved CV problem). Honest positioning = "expert-validated assist/triage," not "automated diagnosis."

**Market sizing (treat vendor CAGRs skeptically):** SHM ~$4–8B (2024/25, ~19–22% CAGR); building-inspection software ~$0.8–1.3B → ~$2.7B by 2033 (~9–11% CAGR).

**Honest wedge vs weak moats:** durable edge = francophone + regulatory-compliant report generation + engineer-as-signer workflow + founder's dual expert/data-engineer profile. Weak/fabricated moats to avoid = the CV model itself, and the learning loop framed as *defensibility* (it's a flywheel, not a wall).

*Sources on file in the research digest; figures are third-party estimates, vendor accuracy claims are marketing not independently benchmarked.*
