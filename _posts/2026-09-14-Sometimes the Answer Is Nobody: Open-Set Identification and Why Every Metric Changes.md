---
title: "Sometimes the Answer Is Nobody: Open-Set Identification and Why Every Metric Changes"
date: 2026-09-14 09:00:00 +0900
categories: [AI, Dentistry]
tags: [open-set-recognition, evaluation, retrieval, calibration, forensic-dentistry, machine-learning]
---

## Introduction

The last post ended by deferring a question: once the flywheel produces enough confirmed pairs to evaluate against, what does "it works" actually mean? I deferred it because the honest answer dismantles the metrics I would otherwise have reached for. Every standard retrieval benchmark shares one silent assumption — that the thing you are searching for is somewhere in the database. Forensic identification cannot make that assumption. Sometimes the person on the table was never in any record you hold, and a system that cannot say so is not a forensic tool. It is a liability with a ranking function.

## 1. The Assumption Hidden in Recall@k

Recall@k, mean reciprocal rank, mAP — all of them assume that every query has at least one relevant item in the gallery. Under that assumption a ranked list is always meaningful; the only question is how far down the true match sits.

Remove the assumption and the list becomes something stranger. A retrieval system always returns a top-1. It returns a top-1 for a query with no match at all, with the same sorted list, the same score column, the same interface. Closed-set evaluation never penalizes this, because in closed-set evaluation the case does not exist. In the field, it is the case that produces a misidentification.

```text
   closed-set question:  "where in the list is the true match?"
   open-set question:    "is there a true match at all — and if so, where?"
```

The first question is one number. The second is two numbers, and the first of them is the dangerous one.

## 2. Two Regimes, One System

Forensic identification runs in two regimes that look identical on screen and differ on exactly this axis.

The first is disaster victim identification: an aircraft, a ferry, a collapsed building. There is a manifest or a missing-persons list. The candidate set is bounded, and nearly every victim is guaranteed to be in it. This is close to closed-set, and rank-k accuracy is a reasonable proxy for usefulness.

The second is the single unidentified body. No list. Antemortem records, if they exist, sit in a clinic that has not been asked for them. The gallery is whatever has been collected so far, and there is no guarantee — often no likelihood — that the right record is in it. This is fully open-set, and it is the routine case, not the exception.

The same tool serves both. It cannot be told which regime it is in, because the examiner often does not know either. So it has to behave well in the open-set regime by default. A 90% rank-1 hit rate measured in the DVI setting says nothing about what the system does when confronted with a query whose correct answer is *nobody*.

## 3. The Asymmetry That Sets the Threshold

Biometrics solved the vocabulary problem years ago for 1:N face and fingerprint search, and there is no reason to reinvent it. Two error types: a false positive identification — the system asserts a match to the wrong person, or asserts a match when the person is absent from the gallery entirely — and a false negative identification — the true match is present, but the system fails to raise it above threshold. FPIR and FNIR. A threshold on the similarity score trades one against the other, and the entire evaluation collapses into a single question: at the false positive rate the field can tolerate, what miss rate does the system achieve?

The field's tolerance is not symmetric. A false negative means a case stays open. The body waits, and the examiner keeps looking. A false positive means a family buries a stranger, a death certificate carries the wrong name, and somewhere a missing person is quietly declared found. Every downstream legal process inherits the error. One of these is recoverable. The other, in practice, is not.

So the operating point does not sit where the two curves cross. It sits far toward the side that would rather say "I don't know" than "it's him." A forensic system must tolerate a miss rate that no consumer retrieval product would accept, and any metric that scores the two errors identically is measuring the wrong thing. Rank-1 accuracy scores them identically. It is not a forensic metric.

## 4. Rules Say No; Embeddings Say Maybe

Here the architecture from earlier in this series pays off a second time. Rule-based retrieval does not produce a similarity score. It produces hard constraints — and the constraints run in exactly one direction.

An antemortem record shows tooth 46 extracted; the postmortem finding shows 46 present. Exclusion. Teeth do not regrow. This is not a low score. It is a proof. The reverse — present antemortem, absent postmortem — is not an exclusion, because extraction can happen after the last record was made. The temporal asymmetry that complicated ranking earlier in the series becomes, in the open-set setting, the most valuable thing the system has: a logically certain *no* that no learned model could improve on.

```text
   postmortem findings (query)
            ↓
   rule layer — hard exclusions        → gallery shrinks by proof, not probability
            ↓
   embedding layer — similarity        → ranked survivors, one score each
            ↓
   threshold — is the best score high enough to show at all?
            ↓
   output: candidate(s) / "no candidate above threshold"
```

This narrows the open-set problem in a specific way. The embedding layer is never asked whether an excluded record is a match; it has already been removed. Its job reduces to: among records that *could* be this person, is any one likely enough to put in front of an examiner? The open-set recognition literature spends enormous effort on "unknown" detection because the model is the only line of defense. Here it is the second line. The first is anatomy, and anatomy does not make probabilistic errors.

## 5. What the Score Has to Mean

Once there is a threshold, the number under it has to mean something, and raw cosine similarity does not. It is larger for more similar inputs and carries no promise that 0.8 means anything like "80%."

What matters is separation: the distribution of scores for mated pairs — confirmed same person — against the distribution for non-mated pairs. If the two overlap heavily, no threshold works. If they are cleanly separated, almost any does. This is the point at which the flywheel's confirmed pairs stop being training data and become the evaluation itself; they are the only source of true mated scores in existence. The non-mated distribution, by contrast, is nearly free: every confirmed pair also confirms a non-match against every other record in the gallery. One closed case yields one positive and thousands of negatives. The scarce quantity is positives, and positives are what govern how well the miss rate can be estimated.

Calibration sits on top. With hundreds rather than millions of pairs, elaborate methods are off the table; a monotone one-dimensional map from raw score to something an examiner can read as a probability, fitted on held-out pairs, is what the data supports. Anything fancier would be fitting noise.

## 6. Small Numbers, Stated Honestly

Which brings me to the part reviewers will look for. A miss rate estimated from a few hundred positives has a wide confidence interval, and at a strict false-positive operating point it is wider still, because the events being counted are rare by design. A headline like "rank-1 accuracy 93%" computed on forty cases is noise wearing a result's clothing.

The evaluation plan for this project therefore reports intervals, bootstrapped over confirmed pairs, and reports them at fixed FPIR rather than as a single accuracy. It also does not report a percentage to the examiner at all. Forensic odontology already has an output vocabulary — positive identification, possible identification, insufficient evidence, exclusion — and the system's output should map onto those categories, with *insufficient evidence* as the default it falls back to whenever the score does not clear threshold. The categories exist because a century of practice found that a number alone invites overconfidence. I see no reason a model should be exempt.

## Conclusion

"It works," in this field, means two things at once: when the true record exists, how often it surfaces at a false-positive rate the field can live with — and when it does not exist, how reliably the system stays silent. The second half is the one nobody benchmarks, and it is the one that decides whether an examiner can trust the tool with a signature.

That word — *trust* — is where this series has to go next. A calibrated score and a category label are only useful if the person reading them can see why the system arrived there: which findings drove the match, which drove the exclusion, and what the system is *not* confident about. What an examiner actually needs to see before signing, and how badly most retrieval interfaces get that wrong, is the next post.
