---
title: "Product Brief: Diagnostic Structurel IA"
status: final
created: 2026-07-10
updated: 2026-07-10
---

# Product Brief: Diagnostic Structurel IA

> Working document, in English. **The product itself ships in French** (hard requirement).

## Executive Summary

**Diagnostic Structurel IA** is a mobile-first, offline-capable field tool for structural-diagnostic experts. It guides the on-site survey, uses AI to pre-identify structural disorders (*désordres*) from site photos, and auto-assembles the compliant PDF deliverable — technical cross-sections (*coupes*), annotated schematics (*schémas*), and disorder mapping included. The insight driving it is simple and lived: **for a practicing diagnostician, the site visit is the fast part.** A full diagnostic takes ~3–3.5 days, yet only ~0.5 day is on-site. The other ~2.5–3 days is deskwork — writing the report, placing annotated photos, drawing coupes and schémas. This product attacks the deskwork, not the inspection.

Two things make now the moment. France's **Décret n°2025-814 (Aug 2025)**, under the *loi Habitat dégradé*, makes structural diagnosis of collective housing mandatory in designated zones — raising demand against an unchanged manual bottleneck. And computer-vision defect detection is now mature enough to reliably *assist* expert judgment. Critically, French regulation requires a **Bac+5 engineer to sign every diagnosis** and carry **≥€1M professional liability** — so the AI can never diagnose on its own. This product is built around that reality: **the AI proposes, the expert disposes and signs.** That is not a limitation to hedge; it is the only legally and professionally honest model.

It is being built by a practicing structural-diagnostic expert who is also a **data engineer**, with a second expert as pilot — **for his own daily use first, and as a subscription product for other French diagnosticians second.** That dual profile matters: he holds the crown-jewel inputs (his own site photos, past reports, worked examples, and reference norms) needed to seed the AI, and he is his own first user and first labeler.

## The Problem

Structural diagnosis today rests on expert judgment, manual site surveys, and empirical interpretation — robust, but slow, variable between operators, and hard to reuse or standardize at scale. For a solo expert the cost is concrete and personal:

- A single diagnostic consumes **~3–3.5 days**. Only **~0.5 day is on-site**; the rest is deskwork.
- **The report write-up alone is ~1.5 days** — the part most dreaded and least reusable.
- Placing annotated photos against the right disorders: **~0.5 day**. Drawing technical coupes and schémas: **~0.5–1 day**.

This deskwork caps throughput: fewer diagnostics per month, slower turnaround, and consistency that depends on the operator's day. Meanwhile new French regulation is set to raise the volume of mandatory diagnoses — **more demand hitting the same manual ceiling.** Generic field-inspection apps don't relieve this: they manage snags and photos, but none produce the *compliant structural deliverable* — coupes, schémas, disorder mapping, regulatory layout — that the job actually requires.

## The Solution

Diagnostic Structurel IA compresses the ~2.5–3 days of deskwork into a **review-and-sign step**.

- **On site, offline:** guided dynamic survey by structure type, checklists, intensity sliders (1–5), geolocated photos pinned to structural elements — no network needed, no data lost.
- **AI-assisted reading:** submitted photos are pre-classified by disorder type (cracking, corrosion/carbonation, moisture/infiltration, deformation, masonry, misc.) with a severity indication and a confidence score. The expert **validates, corrects, or rejects** each detection in seconds — and adds what the model missed.
- **Compliant deliverable, generated:** the app assembles the PDF — cover page, TOC, per-zone synthesis, annotated photos, ≥1 relevant coupe, an annotated schéma, and automatic disorder mapping consistent with the survey numbering — in one of several configurable report templates.
- **A learning loop:** every validated correction is stored as training data and exported to periodically retrain the model, with a dashboard tracking precision/recall by disorder category.

The outcome the expert feels: **the same diagnostic, minus two days at a desk.**

## What Makes This Different

The defensible edge is **not** the AI. Crack detection is ~99% on open benchmarks and commoditizing; the honest in-the-wild target here is **>75% accuracy on cracks and corrosion, always expert-validated.** The real edge is elsewhere:

- **Deep fit to the actual French diagnostic workflow and its compliant deliverable** — the coupes, schémas, disorder mapping, and regulatory report structure that generic tools (e.g. **PlanRadar**, the EU field-inspection incumbent) and enterprise/drone players (**STRUCINSPECT**, **Niricson**) do not serve at the level of the individual on-foot diagnostician.
- **A founder who is both the domain expert and the data engineer** — building for a workflow he lives daily, with his own labeled data to seed the model and beat the cold start that stops most entrants.
- **Francophone + regulatory alignment as a first-class design goal**, not an afterthought.

Two things we deliberately **will not** claim as moats: the CV model itself (commoditized), and the learning loop as "defensibility" (a genuine quality *flywheel*, but incumbents hold more and better-structured data — we frame it as continuous improvement, not a wall). Honest beats impressive here.

## Who This Serves

- **Primary (v1): the founding expert himself**, plus his supporting expert as pilot — solo/duo structural diagnosticians who run the full mission end-to-end. Success for them: reclaim the deskwork days, do more diagnostics, and produce consistent, defensible, regulation-ready reports every time.
- **Next (commercial):** independent structural diagnosticians, *cabinets d'expertise*, and *bureaux d'études* in **France** facing the same bottleneck and rising regulatory demand.

Three in-product roles carry across: **field expert** (surveys, photos, AI validation, schémas, PDF), **project lead** (creates missions, assigns experts, reviews reports), **administrator** (users, settings, AI-model monitoring, report templates).

## Success Criteria

**Product acceptance** (from the working spec §9 — full list in [addendum.md](addendum.md)):

- Offline: create a mission, complete a full survey, capture photos with no network; sync with **zero data loss within 30s** of reconnection.
- **Survey entry ≥30% faster** than the current manual process, validated by pilot experts.
- Generates a **compliant PDF**: all survey data, annotated photos, ≥1 relevant coupe, an annotated schéma, disorder mapping consistent with survey numbering, correct regulatory layout.
- AI: **>75% accuracy on cracks and corrosion** on the test set; confidence score per detection; validate/correct/reject each in **<5s**; corrections exportable for retraining.
- iOS + Android on the 5 most common expert devices; no data loss on force-close; an untrained expert productive in **<1 hour**.

**Personal ROI** (the reason it exists): cut per-diagnostic turnaround from ~3.5 days toward **~1.5 days** — by compressing the deskwork into a review step, not the site visit — enabling roughly **2× throughput**.

**Commercial signal** (gate before productizing): after a pilot, a handful of target-segment experts (*cabinets d'expertise*, *bureaux d'études*) express willingness to pay. Pricing model (per-month vs per-report) and price point are **still open — validate with the pilot.**

## Scope

**In (v1):**
- **Structure types: buildings** — collective housing, commercial/ERP, industrial.
- **Module 1** — guided offline field surveys + photo capture/annotation + compliant PDF generation (coupes, schémas, disorder mapping, configurable templates).
- **Module 2** — AI disorder detection/classification for the main categories, with severity, confidence, expert validation.
- **Module 3** — structured disorder database + learning loop + AI performance dashboard.
- Platforms: iOS + Android. **UI language: French** (hard requirement). Offline-first.

**Out (v1):**
- **Ouvrages d'art** (bridges, tunnels) — deferred to Vision.
- Mission accounting/billing; automated legal-expertise report authoring (per spec exclusions).
- Multi-tenant subscription / commercial rollout — **gated behind personal validation.**

## Vision

If it works for one expert, it works for many. In **2–3 years**, Diagnostic Structurel IA graduates from a personal tool to **the reference field-and-report platform for francophone structural diagnosticians** — extended to *ouvrages d'art*, sold as a subscription, with an accumulated corpus of expert-validated diagnoses that compounds into a real quality advantage (honestly: a flywheel earned over time, not a claimed moat on day one).

The near-term proof gate is unglamorous and specific: **does it cut the founder's own deskwork in half, on real missions, this year?** Everything else follows from that.

## Open Questions

*Decided in review:* name, ROI target, target segment, and France-first market are settled (reflected above).

Still open, to firm up as the work progresses:

- **Subscription pricing** — model (per-month vs per-report) and price point; validate with a pilot before productizing.
- **Timeline & budget** — not yet determined; set once the personal-use v1 scope is locked.
