---
title: "There Is No ImageNet for the Dead: The Cold-Start Problem in Forensic AI"
date: 2026-08-18 09:00:00 +0900
categories: [AI, Dentistry]
tags: [data-scarcity, self-supervised-learning, transfer-learning, forensic-dentistry, machine-learning]
---

## Introduction

The last post argued that Vision Transformers, not CNNs, are the right architecture for postmortem dental imagery. The sharpest readers will have had one objection loaded before they finished the first section: ViTs are famously data-hungry, and there is no such thing as a large labeled dataset of the dead. That objection is correct, and this post is my answer to it. The defining constraint in forensic AI is not architecture, and it never was. It is that ground truth in this field cannot be scraped, purchased, or crowdsourced. It can only be earned — one confirmed identification at a time.

## 1. Why Forensic Ground Truth Cannot Be Scraped

In mainstream computer vision, a label is cheap. A crowdworker looks at a photograph for three seconds, types "cat," and moves on. Repeat a million times and you have a benchmark.

Now look at what a labeled example means in forensic identification. It is an antemortem record and a postmortem finding that are *confirmed to belong to the same person*. That confirmation is not an annotation someone produces by looking at the image. It is the outcome of the entire forensic process — records requested, findings charted, a match reviewed and signed by an examiner. The label is not metadata attached to the data. The label *is* the case, closed.

```text
   Mainstream CV                         Forensic identification
   ─────────────                         ───────────────────────
   label  = a three-second glance        label  = a completed identification
   supply = effectively unlimited        supply = bounded by case volume
   access = public benchmarks            access = legally and ethically sealed
```

Stack the rest on top. Postmortem data sits behind privacy law and ethics review, as it should. Cases are rare, and rarer still at any single institution. The antemortem side is scattered across clinics and hospitals with no mechanism to pool it. There will never be an ImageNet here, and building one should not even be the goal.

## 2. The Label Is the Output of the System

This creates a circularity that stalls most healthcare AI projects before they begin. To train the matcher, you need confirmed matches. To produce confirmed matches at scale, you need the matcher.

The way out was built into this series from the first post, although I did not frame it this way at the time. Rule-based retrieval needs zero training data. It runs on anatomical facts — a gold crown on tooth 46, an extracted 14 — from day one. And every time an examiner uses it to close a case, the confirmed AM–PM pair that results is one unit of gold-standard training data. The search engine was never a placeholder while waiting for the model. It is the label factory.

```text
   rule-based system helps the examiner
                 ↓
   examiner confirms a match
                 ↓
   confirmed AM–PM pair enters the training set
                 ↓
   embedding layer improves
                 ↓
   system helps the examiner more    ↺
```

This flywheel is slow, and it should be — its speed is bounded by real casework, not by ambition. Which raises the practical question: what do you train on while it spins up?

## 3. Borrow the Living, Then Simulate the Gap

Living-patient dental imagery is comparatively abundant. Public panoramic radiograph datasets exist, built for tasks like caries detection and tooth segmentation. None of them know anything about postmortem degradation, but they know a great deal about jaws — and anatomy transfers even when image conditions do not.

The pretraining strategy that fits this domain almost suspiciously well is masked image modeling. The recipe: hide a large fraction of the image patches, and train the encoder to reconstruct what is missing from the structure of what remains. Read that sentence again in a forensic register. Inferring what is missing from the structure of what remains is not an analogy for forensic odontology. It is the job description. A model pretrained this way spends its entire curriculum practicing the exact reasoning the final task demands — reasoning about dental context around absence — before it ever encounters a real case.

The remaining gap is degradation itself, and that gap can be manufactured. Fragmentation, exposure variation, the artifacts of a portable autopsy X-ray machine — these can be simulated and applied to living-patient images as augmentation, so the encoder meets damage in training before it meets damage in the field. That is the curriculum I am designing the embedding layer around: public anatomy first, self-supervised structure second, synthetic damage third. The flywheel's confirmed pairs then teach the only lesson that cannot be faked — what *identity* looks like across the antemortem–postmortem gap.

## 4. Owning the Counterargument to My Own Last Post

Here is the part I owe the skeptics. The original ViT results were unambiguous: trained from scratch on modest data, the transformer *loses* to convolutional networks, and only pulls ahead once pretraining data reaches a scale no forensic dataset will ever approach. A CNN's inductive biases — locality, translation equivariance — are not limitations in a small-data regime; they are precisely the built-in prior knowledge that lets a model learn from little. An entire line of research on data-efficient transformers exists because vanilla ViTs are this hungry.

So let me state plainly what last month's post left implicit: a ViT trained from scratch on a few hundred forensic cases would lose to a modest CNN, and it would deserve to. The architectural argument for transformers — global structure over local texture — is *conditional*. It holds only once the data strategy above supplies the scale that attention requires. Architecture and data are not two separate decisions in this project. They are one decision, and the data half is the binding half.

## 5. The Honest Sequencing

```text
   public living-patient datasets    → anatomical pretraining
   unlabeled dental imagery          → masked-patch self-supervision
   simulated postmortem degradation  → domain-gap adaptation
   confirmed matches (the flywheel)  → forensic ground truth, the only kind
```

Each stage exists to make the next one possible. Only the last row produces labels that mean anything in this field. Everything above it is scaffolding — necessary, cheap, and honest about being scaffolding.

## Conclusion

Anyone can download model weights. What cannot be downloaded is a position inside the loop where forensic labels come into existence. That loop runs through examiners closing real cases with a tool they trust — and as the examiner building that tool, I am not trying to acquire access to the data flywheel. I am standing where it turns. In a field where ground truth is earned rather than scraped, that position is the entire project.

Which surfaces the question this series has been quietly deferring. Once the flywheel produces enough data to evaluate against, what does "it works" even mean? Standard retrieval metrics assume the true match exists somewhere in the database. Forensics cannot assume that — sometimes the person was never in any record you hold. That open-set problem, and why it changes every metric that matters, is the next post.

