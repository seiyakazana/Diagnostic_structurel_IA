# Reconciliation: Cahier des Charges v1.1 (§1–3) vs PRD + Addendum

Source: `docs/Cahier_des_Charges_DiagnosticStructurel_IA.md` (feature-detail authority)
Targets: `prd.md`, `addendum.md` (2026-07-23)
Known intentional divergences (not reported): buildings-only scope / no infrastructure usage option; AI-drafted editable causes/recommendations; signature outside the tool; RGPD deferred.

Findings ordered by importance.

---

## F1 — Sub-disorder vocabulary entirely absent (HIGH)

**Cahier:** §2.2.1 table enumerates example sub-disorders per category:

- Fissuration: fissures de retrait, fissures structurelles, lézardes, microfissures
- Corrosion / carbonatation: épaufrures, armatures apparentes, taches de rouille, bullage
- Humidité / infiltrations: remontées capillaires, moisissures, efflorescences, salpêtres
- Déformations: flambement, désaffleurements, tassements différentiels, dévers
- Désordres maçonnerie: déliaisonnements, décollements, joints dégradés, faïençage
- Dégradations diverses: éclatements, défauts d'enrobage, cavités, impacts

**PRD:** Absent everywhere. The Glossary "Désordre" entry and FR-11 stop at the six category names; FR-11 requires classification by "nature (category + sub-category)" yet neither PRD nor addendum defines a single sub-category. The addendum's "Exploitable gaps" even cites "French pathology vocabulary" as the moat, making the omission self-undermining.

**Impact:** Downstream UX (dropdown contents), the AI taxonomy, and the Fiche de désordre all need this list. It is also the answer to task (c): ~24 French domain terms (lézarde, épaufrure, salpêtre, faïençage, désaffleurement, tassement différentiel, remontée capillaire, défaut d'enrobage, …) missed by the glossary.

**Fix:** Add the cahier's table (marked "liste évolutive") to the Glossary or as an annex referenced by FR-11.

---

## F2 — FR-17's blanket Rapport content requirement contradicts the cahier's template intent (MEDIUM-HIGH)

**Cahier:** §2.1.4 "Modèles de rapport configurables : choix entre plusieurs trames (rapport de visite, rapport de diagnostic complet, fiche de désordre unitaire) **selon le besoin de la mission**" — i.e., some templates are deliberately partial documents.

**PRD:** FR-17 mandates for *the* Rapport: "at least one relevant Coupe, and an annotated Schéma", with consequence "The Rapport includes every piece of entered data". Applied across FR-19's three templates this is contradictory: a fiche de désordre unitaire covers one Désordre, not all data; a rapport de visite need not carry a Schéma.

**Fix:** Scope FR-17's content/consequence to the "rapport de diagnostic complet" template, or restate per-template content floors.

---

## F3 — Contextual-guidance specifics dropped: aide en ligne and tooltips (MEDIUM)

**Cahier:** §3.1 "Guidage contextuel : **aide en ligne, tooltips** et messages d'erreur clairs" — one of five "non négociables" UX principles.

**PRD:** §9 NFRs compress this to "contextual guidance and clear errors". In-app help ("aide en ligne") and tooltips are never named, yet SM-6 (new Expert productive in <1h "using only the documentation") implicitly depends on them.

**Fix:** Restore "in-app help and tooltips" explicitly in the Field ergonomics NFR.

---

## F4 — Mid-mission PDF generation and in-app Coupe positioning underspecified (MEDIUM-LOW)

**Cahier:** §2.1.4 intro: "À l'issue **(ou en cours)** d'une mission, l'expert doit pouvoir générer un document PDF de synthèse" — generation is allowed while the Mission is still open. Same section: the Expert positions relevés on coupes "**directement depuis l'application**" (mobile, field-side).

**PRD:** FR-17/FR-18 cover offline/field-initiated generation but never state that a partial, in-progress Relevé can be exported; UJ-2 frames Coupe positioning as next-morning deskwork ("web or mobile" only as an [ASSUMPTION] on Validation, not on Coupes), and FR-15 is surface-silent.

**Fix:** Add a testable consequence to FR-17/18 (Rapport generable from an incomplete Mission) and state in FR-15 that Coupe positioning is available on the mobile app.

---

## F5 — Building history mechanism narrowed and audience shifted (LOW)

**Cahier:** §2.1.1 "Consultation de l'historique des missions passées sur un même **ouvrage**" — a general consultation capability, no mechanism stated.

**PRD:** FR-3 (a) keys history on *address match* (an unfLagged inference — same ouvrage may be recorded under variant addresses) and (b) surfaces it only "to the assigned Experts", while UJ-3 has the Chef de projet using past-mission history at creation time. Minor internal tension plus an untagged assumption.

**Fix:** Tag the address-match mechanism as [ASSUMPTION] and extend visibility to the Chef de projet.

---

## F6 — Glossary term: "Ouvrage" itself not an entry (LOW)

**Cahier:** uses "ouvrage" throughout as the core noun (§1.1, §1.3, §2.1.1: "informations de l'ouvrage").

**PRD:** Glossary keeps Mission/Relevé/Zone/Désordre etc. in French but renders "ouvrage" as English "building" everywhere (Mission entry: "one diagnostic engagement on one building"). Given the glossary rule "French domain terms are kept in French — they are the product's native vocabulary", "Ouvrage" deserves an entry, especially since the product UI ships in French.

**Fix:** Add **Ouvrage** to the Glossary (the built structure a Mission targets; buildings only in v1).

---

## Verified as covered (no action)

- §1.3 exclusions (gestion comptable, rapports d'expertise judiciaire) → PRD §6 Non-Goals.
- §2.1.1–2.1.3 mission creation surfaces, expert assignment, form mechanics (checkboxes, 1–5 sliders, dropdowns, free text), configurable checklists, offline + deferred sync, geolocation, in-form camera, auto-association photo→Élément/Zone, photo metadata, gallery import, on-image annotation (arrows/circles/text) → FR-1 to FR-10.
- §2.1.4 coupe-type library contents, numbered points cross-referenced to Fiche de désordre, sketch tool (freehand, shapes, legends, cotations), cover page fields, TOC, pagination, per-Zone synthesis, photo annexes, export local/email/secure link, three template names → FR-15 to FR-20.
- §2.2.2–2.2.3 Sévérité four levels, Score de confiance %, validate/correct/reject, manual additions, corrections as training data → Glossary + FR-11/13/14. Bounding box vs mask: cahier says "ou", so the PRD's bbox-first [ASSUMPTION] is compatible.
- §2.3 database classification axes, traceability, dataset export, learning cycle, dashboard metrics (precision/recall/F1 per category), model version history, drift alerts → FR-21 to FR-23.
- §3.1 3-clicks, offline robustness, contrast/14pt/icons, no local-entry wait → FR-4 consequence + §9 NFRs.
- §3.2 role table → Glossary + FR-24 (Expert's photos/consultation implicit but acceptable).
- §3.3 one-handed use, dark mode, haptics, Bluetooth stylus/keyboard (stylus tied to sketching) → §9 NFRs + FR-16.
