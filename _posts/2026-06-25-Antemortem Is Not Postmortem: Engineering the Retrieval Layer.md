---
title: "Antemortem Is Not Postmortem: Engineering the Retrieval Layer"
date: 2026-06-25 09:00:00 +0900
categories: [AI, Dentistry]
tags: [retrieval, forensic-dentistry, system-design, ranking, healthcare-ai]
---

## Introduction

In the previous post, I argued that a search engine must come before the AI model, and I closed with a promise to get specific about the engineering. This post is that promise paid off. When I finally sat down to build the retrieval layer of `dental_record_app`, the hard part was not writing a query. It was discovering that the single most intuitive design choice — "just match the records that are identical" — is quietly, catastrophically wrong for forensic identification. What follows are the three tradeoffs that actually cost me time, beginning with the one that forced me to throw away my first implementation entirely.

## 1. The First Real Problem: Antemortem Is Not Postmortem

Every textbook on retrieval implicitly assumes the query and the document describe the same object at the same moment. In forensic odontology, that assumption is simply false, and it is false in a way that destroys naive systems.

An antemortem (AM) record is a snapshot from life. It might come from a dental chart, an X-ray, or a treatment note that is several years old. A postmortem (PM) finding is an observation made directly on the body at the time of examination. And here is the crucial point: between those two moments, a living person kept visiting the dentist. Teeth were filled, crowned, extracted. Decay advanced.

```text
   Antemortem record                 Postmortem finding
   (years before death)              (at examination)
          │                                  │
          ●──────────────────────────────────●
                   ↑
        treatment, extraction, decay
        all happen inside this gap
```

So the instant you write `WHERE am_record = pm_record`, you have built a system that silently discards the correct match the moment any dental work happened in that interval. And dental work almost always happens. The true positive is precisely the record you just filtered out. This was not a theoretical concern for me. The failure that forced the rewrite was a real candidate pair where the antemortem chart and the postmortem exam had simply diverged through ordinary treatment, and my first exact-match query threw the true match away without leaving a trace. A silent discard is the worst possible failure in this field.

## 2. Encoding Monotonic Consistency Instead of Equality

The fix is to stop asking "are these identical?" and start asking "is this AM record *compatible* with this PM finding, given that time only moves in one direction?"

Dental status advances along a treatment progression, and it advances only forward. A virgin tooth can become restored; a restored tooth can be crowned; any tooth can ultimately be extracted. It never runs backward. A restoration does not vanish on its own, and an extracted tooth does not grow back. These are absolute facts, and they are exactly the kind of hard constraint a rule-based filter can exploit.

```text
   virgin → restored → crowned → extracted (absent)
   ───────────────────────────────────────────────▶
              status can only advance, never reverse
```

That gives a clean, asymmetric filter rule. For each tooth position, the PM status must be *at least as advanced* as the AM status, and never less:

*   A tooth **present** in AM may be present, restored, or absent in PM. All three are consistent.
*   A surface **restored** in AM must still show that restoration, or a larger one, in PM. It cannot revert to sound enamel.
*   A tooth **absent** in AM must be absent in PM. This is the hard gate: missing can only stay missing.

The asymmetry is the entire point. Equality treats AM and PM symmetrically and punishes any change at all. Monotonic consistency treats the missing-tooth direction as a one-way gate, which is what physical reality actually enforces. Under this rule, a candidate is excluded only when it genuinely violates the progression, not when it merely differs. That single shift is what turned my filter from a system that looked like it worked into one that actually does.

## 3. Ranking by Rarity, Not by Count

Once monotonic filtering hands back a compatible candidate set, you still have to rank it. And the obvious heuristic — count the matching features and sort descending — is the second trap.

Counting treats every match as equal evidence, which it absolutely is not. A missing third molar is one of the most common findings in any adult population. Two complete strangers sharing an absent wisdom tooth tells you almost nothing. A gold crown on a specific tooth, with a specific surface configuration, is rare. Two records sharing *that* is powerful evidence that they belong to the same person.

This is the same insight that drives TF-IDF in text search: a feature's value as evidence is inversely related to how often it appears. So I weight each matching feature by its rarity rather than by a flat count. A match on a rare restoration lifts a candidate far higher than a match on a common absence.

```text
   match on missing 3rd molar    → small score bump   (almost everyone has it)
   match on gold crown, tooth 46 → large score bump   (very few people do)
```

When I first ranked by raw count, the genuinely distinctive cases were buried under candidates that merely shared common absences. Switching to rarity weighting was what pulled the truly informative matches to the top of the shortlist — which is exactly where a busy examiner's limited attention needs to land.

## 4. Where the Compute Lives: Flutter Versus FastAPI / Cloud Run

The final tradeoff is architectural. I split record entry from matching, and I deliberately put them on opposite sides of a network boundary.

Entry lives in Flutter, on the examiner's device, because the work there is structured data input and it has to run anywhere. Matching — the monotonic filter and the rarity-weighted ranking — runs on a FastAPI service on Cloud Run. The tempting shortcut is to do matching on the client and skip the round trip, but that would have been a serious mistake, for three reasons.

First, the database is centralized and sensitive; it does not belong copied onto every device. Second, the matching logic *is* the product, and keeping it server-side means I can correct a filter rule or retune a weight without shipping a new app build to every user. Third, forensic identification is bursty, low-volume work — long quiet stretches punctuated by a single case. Cloud Run scales to zero between cases, so I pay for compute only when an examiner actually runs a search. An always-on server would simply charge me to sit idle.

## 5. What This Bought Me

None of these were abstract correctness for its own sake. Each one paid off concretely in the field:

*   **No silent failures.** Monotonic consistency means the system stops discarding true matches the moment treatment history enters the picture. This is the entire difference between a tool that demos well and one a medical examiner can stake an identification on.
*   **Attention spent where it matters.** Rarity-weighted ranking surfaces the distinctive cases first, so an examiner reviews the handful of candidates that actually carry identifying signal before anything else.
*   **Logic in one place.** Server-side matching keeps the rules in a single codebase I can audit, debug, and retune, instead of scattered across frozen app installs.
*   **Ground truth, accumulating.** Every search an examiner runs and every match they confirm becomes labeled data — precisely the training set the embedding layer will need later. The funnel quietly feeds its own future.

## Conclusion

Building the retrieval layer taught me that the defining challenges in this domain are rarely the flashy ones. The hardest problem was not a model architecture; it was recognizing that antemortem and postmortem records describe the same person across a gap in time, and designing a filter that honors the one-way arrow of dental history. Equality is the intuitive choice and the wrong one. Monotonic consistency, rarity-weighted ranking, and a clean split between entry and compute are what gave me a system examiners can actually trust.

In the next post, I will move to the top of the funnel — the embedding layer, and the ViT pipeline I am applying *only* to the small set of candidates that survive everything described here.
