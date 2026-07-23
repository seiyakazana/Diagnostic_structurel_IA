# Acceptance Criteria — Diagnostic Structurel IA

**The test contract.** These are the Cahier des Charges §9 acceptance criteria (French, verbatim) — the pass/fail conditions the pilot experts validate against. §9 is not in the `docs/` Cahier (which holds §1–3); it is preserved here from the brief addendum. Each criterion is mapped to the SPEC capability or constraint it verifies, with the measurable threshold made explicit.

Companion to [SPEC.md](SPEC.md).

## 9.1 — Module relevés terrain

| # | Critère (verbatim FR) | Verifies | Measurable threshold |
|---|------------------------|----------|----------------------|
| 9.1.a | Un expert terrain peut créer une mission, saisir l'intégralité d'un relevé et prendre des photos sans connexion réseau | CAP-1, CAP-2, CAP-3 | Full mission + survey + photos completed with network off |
| 9.1.b | La synchronisation s'effectue sans perte de données dans un délai de 30 secondes après reconnexion | CAP-4 | Zero data loss; sync ≤ 30s after reconnection |
| 9.1.c | Le temps de saisie d'un relevé standard est inférieur de 30 % au processus actuel (validé par les experts pilotes) | CAP-2 | Survey entry ≥ 30% faster than manual, pilot-validated |
| 9.1.d | L'expert peut générer un PDF de synthèse du relevé incluant l'ensemble des données saisies, les photos annotées, au moins une coupe technique pertinente et un schéma annoté de la zone diagnostiquée | CAP-5, CAP-6, CAP-7 | PDF includes all data + annotated photos + ≥1 relevant *coupe* + annotated *schéma* |
| 9.1.e | Le report des désordres sur la coupe/le schéma est cohérent avec leur numérotation dans le relevé et la fiche de désordre associée | CAP-8 | Disorder mapping matches survey numbering and links to correct *fiche de désordre* |
| 9.1.f | Le PDF généré respecte la mise en page définie (page de garde, sommaire, pagination) et reste lisible quel que soit le volume de photos et de schémas | CAP-5 | Layout correct (cover, TOC, pagination); readable at any photo/schema volume |

## 9.2 — Module IA

| # | Critère (verbatim FR) | Verifies | Measurable threshold |
|---|------------------------|----------|----------------------|
| 9.2.a | Pour les fissures et désordres de corrosion, le modèle atteint une précision supérieure à 75 % sur le jeu de test | CAP-9 | > 75% accuracy on cracks & corrosion on the test set |
| 9.2.b | Le score de confiance est affiché pour chaque détection | CAP-9 | Confidence score shown per detection |
| 9.2.c | L'expert peut valider, corriger ou rejeter chaque résultat en moins de 5 secondes | CAP-10 | Validate/correct/reject each result in < 5s |
| 9.2.d | Les corrections de l'expert sont bien enregistrées et exportables pour réentraînement | CAP-10, CAP-11 | Corrections stored and exportable for retraining |

## 9.3 — Critères globaux (cross-cutting)

| # | Critère (verbatim FR) | Verifies | Measurable threshold |
|---|------------------------|----------|----------------------|
| 9.3.a | L'application est disponible sur iOS et Android, testée sur les 5 modèles de smartphones les plus répandus parmi les experts | Constraint: mobile ergonomics | iOS + Android; tested on the 5 most common expert devices |
| 9.3.b | Aucune donnée n'est perdue en cas de fermeture forcée de l'application | CAP-4 | No data lost on forced app close |
| 9.3.c | La documentation utilisateur permet à un expert non formé de prendre en main l'outil en moins d'une heure | Constraint: onboarding | Untrained expert productive in < 1 hour |

> The "5 most common expert devices" (9.3.a) test matrix is not yet enumerated — see SPEC assumptions.

## Capabilities with no §9 criterion

§9 predates two capabilities, which therefore have **no inherited acceptance criterion**. Their pass/fail conditions come from the SPEC `success` fields only:

| Capability | Why no §9 criterion | Verify against |
|---|---|---|
| **CAP-14** — probable cause & recommendation inference | §9 sets an accuracy bar for detection (9.2.a) but none for inference. **No accuracy floor is committed at POC** — cause inference and severity grading are not solved problems. | Behavioural, not statistical: every inferred cause/recommendation is marked non-binding and is **blocked from the PDF until expert-validated or edited**. |
| **CAP-12** — continuous learning loop | §9.2.d covers only that corrections are stored and exportable, not how retraining runs. | Founder-operated, manual, ad-hoc; retrained model gated on test-set performance before deployment. |
