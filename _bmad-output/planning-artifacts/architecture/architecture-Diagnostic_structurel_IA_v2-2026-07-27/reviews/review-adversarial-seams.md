# Review — Adversarial seams (configured lens 2)

**Method:** construct two units one level down that each obey every AD to the letter yet still build incompatibly. Every pair found is a hole to close.

**Verdict:** 6 incompatible pairs found. Four are load-bearing.

---

## A1 — CRITICAL · Two `RapportDocument` *builders* (AD-5 fixes only the renderer)

- **Unit A** (`apps/mobile`) builds a `RapportDocument` from local SQLite for the offline export (FR-18).
- **Unit B** (`services/render_worker`) builds one from Postgres for the finalized export.

Both obey AD-5 (one renderer) and AD-8 (gate in the document model). Neither is told that **construction** is also single. A obtains photos as local file handles and omits AI text; B obtains signed blob URLs and includes it. The two documents legitimately differ in *content* — but nothing stops them differing in section order, empty-section handling, or which template floor applies. §9.1.f ("layout correct regardless of volume") is then verified against one path and shipped on two.

**Close:** AD-5 must bind the builder, not just the renderer — one `RapportDocumentBuilder`, parameterized by a content source, with offline/finalized as *inputs* to one code path.

## A2 — CRITICAL · Display numbers computable outside the Dart domain

AD-4 puts the ordering function in `packages/domain` (Dart). But `services/inference` is **Python**, and CAP-14 has it drafting French prose about specific désordres. The natural thing for a drafting prompt to emit is *"le désordre n°7 en façade nord"*. Python cannot call the Dart ordering function, so it either reimplements it or receives a number computed at submit time — and either way the number is now frozen inside generated text while AD-4 says the number is derived at render. Add one désordre earlier in the Relevé and the prose is silently wrong. This breaks acceptance §9.1.e in the one place nobody re-checks: inside AI-drafted narrative.

**Close:** tighten AD-4 — no unit outside the Dart domain computes or embeds a display number. Drafted text references désordres by id; numbering is substituted at render.

## A3 — CRITICAL · "Terminal Validation" is undefined, and two surfaces validate

AD-8 rejects content "lacking a terminal `Validation` event". PRD UJ-2 explicitly allows validation on **both** mobile and web. AD-2 scopes single-writer ownership to the *Relevé*, not to Détection validation. So:

- **Unit A** (mobile) appends `Validation{corrected, severity: sévère}` at 09:00 device-clock.
- **Unit B** (back-office) appends `Validation{validated, severity: modéré}` at 08:59 server-clock.

Both are valid appends under AD-7. Nothing says which one the Rapport uses. Two builders will pick differently, and device clock skew makes "latest" ambiguous.

**Close:** define terminal explicitly — last by `(server_received_at, id)`, resolved in one function that every consumer calls. Ordering by device clock is the trap.

## A4 — HIGH · Reference data is a *liste évolutive* with no version pinning

The désordre taxonomy is explicitly evolving (SPEC/PRD), and the coupe library, checklists, and Rapport templates are all mutable reference data. A Relevé captured against taxonomy v3 and rendered against v4 — where a sub-disorder was renamed or removed — produces a Fiche de désordre stating something the Expert never selected, inside a document a named engineer signs.

Two units diverge the moment one resolves reference data at capture and the other at render.

**Close:** reference data is versioned; every capturing record pins the version it was captured against; rendering resolves against the pinned version, never `latest`.

## A5 — MEDIUM · Two content-address schemes for the same photo

AD-3 says blobs are content-addressed but not *over which bytes*. In-form capture (FR-9) hashes the camera bytes; gallery import re-encodes on the way in and hashes the re-encoded bytes. Same photograph, two addresses, duplicate storage, and any dedupe assumption quietly false.

**Close:** hash the bytes exactly as captured or imported; no re-encoding before hashing.

## A6 — MEDIUM · No `Detection` → model-version link

AD-11 mentions a registry holding the model version; nothing requires a `Detection` to *record* which model version produced it. Unit A stores it; Unit B doesn't. CAP-13 (per-version comparison, drift alerts) and CAP-12 (gate a retrained model on a test set) are then not computable from the data. Also folded into the rubric review as a coverage gap.

**Close:** every `Detection` records the model identifier + version that produced it.
