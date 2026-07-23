# Compliance References — Diagnostic Structurel IA

Regulatory backdrop and signer/liability requirements that drive the **"AI never diagnoses or signs autonomously"** constraint. Not present in the `docs/` Cahier des Charges (§1–3); preserved here from the brief addendum's research digest (§4).

Companion to [SPEC.md](SPEC.md).

> **Caveat.** These figures come from a research digest, not independent legal counsel. Verify article numbers, thresholds, and liability amounts against the current texts before relying on them for build or contractual decisions.

## Regulatory tailwind (France)

- **Décret n°2025-814 (12 Aug 2025)**, under the *loi Habitat dégradé* (art. **L.126-6-1 CCH**), mandates structural diagnosis of collective housing in commune-designated zones. This raises the volume of mandatory diagnoses against an unchanged manual bottleneck — the demand-side reason the product matters now.

## Signer & liability requirements

- A **mandatory structural diagnosis** must be signed by a **Bac+5 engineer** (≥ 2 years' experience, independent) carrying **RC Pro (professional liability) ≥ €1M per claim, €1.5M per year**.
- **DTG (copropriété)** requires a **Bac+3** qualified *diagnostiqueur* with RC Pro.

**Implication (the load-bearing point):** an **AI can never sign** a diagnosis — a **named human engineer bears the liability**. This is why the product is built as *AI proposes, expert disposes and signs*, not as automated diagnosis. It shapes the permission model (who can validate, who can sign), the audit trail (which named expert signed what), and the non-goal against autonomous diagnosis.

## Positioning consequence

Because a liable human always signs, the AI's honest role is **expert-validated assist/triage**, not automated diagnosis — consistent with the state of the art (structural-vs-cosmetic distinction, severity grading, and cause inference are not solved CV problems). This reinforces the SPEC's AI accuracy floor being a *validation-gated* target (>75% on cracks/corrosion), never an autonomous-decision threshold.
