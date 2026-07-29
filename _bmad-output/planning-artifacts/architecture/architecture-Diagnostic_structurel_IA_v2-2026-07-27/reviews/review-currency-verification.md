# Review — Currency & reality-check (configured lens 1)

**Method:** was every committed decision web-researched or reality-checked, rather than asserted from training data? Flag anything that could be stale and wasn't confirmed.

**Verdict:** pass with three caveats. No stale-prior failures; one important negative result was caught that a training-data answer would have got wrong.

---

## Verified this session

| Claim | Status |
| --- | --- |
| Flutter stable 3.44.x (3.44.7 as of 2026-07-10) | ✅ confirmed, docs.flutter.dev |
| `powersync` Dart SDK 2.3.2, Apache-2.0, published 2026-07-24 | ✅ confirmed, pub.dev |
| PowerSync v2.0 consolidated packaging, built-in encryption, Rust connection pool (May 2026) | ✅ confirmed, PowerSync changelog |
| `pdf` Dart package 3.13.0, Apache-2.0, **pure Dart** (runs headless server-side), SVG + page numbering | ✅ confirmed, pub.dev — AD-5 and AD-6 both depend on this and it holds |
| Ultralytics YOLO26 exists (early 2026), AGPL-3.0 or Enterprise | ✅ confirmed — motivates AD-11 |
| PowerSync ↔ Supabase is a first-party documented integration with a Flutter path | ✅ confirmed, Supabase partner page + PowerSync connector guide |
| Supabase EU regions incl. Paris `eu-west-3`, Frankfurt `eu-central-1` | ✅ confirmed |
| Claude model IDs and pricing (`claude-opus-5`, `claude-sonnet-5`) | ✅ confirmed against the bundled provider reference |

## Stale priors that would have produced a wrong architecture

**MongoDB Atlas Device Sync / Realm.** For this exact problem shape — offline-first mobile with server sync — Realm is the answer a training-data prior reaches for. It was deprecated Sept 2024 and **shut down 30 Sept 2025**. Naming it would have been a dead dependency at the centre of the spine. Correctly excluded.

**Anthropic EU inference region.** The spine asserts there is no `eu` value for `inference_geo`. Verified: the parameter accepts only `us` and `global`. This is a negative result, and the kind most likely to be got wrong by assumption — "a major provider surely has EU residency" is the intuitive answer and it is false for the first-party API. AD-10 and the corresponding Deferred entry are correctly grounded.

## Caveats

1. **CAVEAT · Supabase EU regions run on AWS under US-jurisdictional control.** "EU region" is not the same as "EU sovereignty" — CLOUD Act exposure remains. AD-16 should not be read as closing the RGPD question, and the Deferred section should say so plainly rather than implying residency is solved.
2. **CAVEAT · PowerSync Cloud EU-region placement not directly confirmed.** Pricing tiers and the self-hosted Open Edition were confirmed; region placement on the managed tier was not. The self-hosted fallback is the mitigation and should be stated in the Stack row, not only in the memlog.
3. **UNPINNED · FastAPI / PyTorch / detection model.** Left unpinned. Defensible — the model architecture is explicitly deferred under AD-11 and pinning a serving stack around an undecided model would be false precision. Flagged so it is a choice, not an oversight.
