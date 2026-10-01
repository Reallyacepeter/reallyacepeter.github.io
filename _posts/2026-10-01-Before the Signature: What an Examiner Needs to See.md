---
title: "Before the Signature: What an Examiner Needs to See"
date: 2026-10-01 09:00:00 +0900
categories: [AI, Dentistry]
tags: [explainability, human-ai-interaction, cognitive-bias, interface-design, forensic-dentistry, machine-learning]
---

## Introduction

The last post ended on a word I had been circling for the whole series: *trust*. A calibrated score and a category label are only useful if the person reading them can see why the system arrived there. This post is about that seeing — what has to be on the screen before an examiner can put their name under a conclusion, and why the interface most retrieval systems ship with is the wrong one for that moment.

The short version: the system does not sign. The examiner does. So the job of the interface is not to persuade the examiner that the answer is correct. Its job is to hand over everything the examiner needs to reach that conclusion independently — or to refuse it.

## 1. A Signature Is Not a Click

Before this project, I worked as a forensic examiner. The document you sign after an examination does not stay a document for long. It becomes a fact that other people act on: a family plans a funeral around it, an investigator opens or closes a file because of it, a registry updates a name. Nobody downstream asks what was on your screen when you signed. They ask whether you are sure, and they hold you to the answer.

These days I work night shifts at a care hospital, where many decisions are made alone at three in the morning and explained to the day team a few hours later. Both jobs taught me the same test, and it has become the design rule for this part of the project:

> If I cannot explain a conclusion without the screen in front of me, I am not entitled to sign it.

There is a second lesson underneath that one, and it took me longer to learn. For a long time I believed confidence was something you perform — that the strong move was to sound certain. The clinicians and examiners I ended up trusting most did the opposite. They told you exactly where their certainty ended, and that boundary was what made everything on the near side of it believable. I want the system to behave like them, not like the younger version of me.

## 2. How the Ranked List Fails

The default interface for retrieval looks like this:

```text
   rank   record      score
   1      AM-0412     0.87
   2      AM-1189     0.84
   3      AM-0076     0.81
   ...
```

It is compact, sortable, and familiar from every search engine. In forensic identification it fails in three specific ways.

First, it changes the examiner's task without announcing it. The moment a top-1 appears, it becomes the working hypothesis. The examiner's job quietly shifts from *identifying* to *confirming*, and confirmation is a much easier standard to meet. Human-factors research has a name for leaning on an automated suggestion in place of independent judgment — automation bias — and a sorted list with a number beside the first row is close to ideal conditions for it.

Second, large galleries manufacture convincing strangers. The more records you search, the more likely it is that some non-matching record happens to look very similar. Forensic science has learned this the hard way. In 2004, after the Madrid train bombings, the FBI wrongly attributed a latent fingerprint to an Oregon attorney named Brandon Mayfield; several experienced examiners agreed with the identification before Spanish authorities linked the print to someone else. The Justice Department's review pointed partly to the unusual similarity that a search of a very large database had turned up, and partly to examiners reasoning backward from the candidate's known prints to the evidence. A ranked list invites exactly that backward reasoning.

Third, a score without reasons gives the examiner nothing to argue with. "0.87" cannot be cross-examined. It can only be accepted or ignored, and neither of those is a judgment.

## 3. The Order of Disclosure

The most important design decision in the examiner view is not what to show. It is *when* to show it.

Research on cognitive bias in forensic science, especially Itiel Dror's work, has shown that examiners can reach different conclusions about the same evidence when the surrounding context changes. One practical response is linear sequential unmasking: examine and document the unknown evidence first, and only then reveal the reference material, so that what you expect to see cannot shape what you record. I am building that order into the workflow rather than leaving it to discipline.

```text
   step 1  chart the postmortem findings      → locked and timestamped
           (no candidates visible yet)
              ↓
   step 2  show exclusions                    → which records fell, and the
                                                 finding that excluded each
              ↓
   step 3  show candidates above threshold    → evidence ledger per candidate
              ↓
   step 4  examiner records a conclusion      → category + reasons, in the
                                                 examiner's own words
```

Step 1 matters most. Once the postmortem chart is locked, it cannot be quietly edited to fit a candidate that appears later. If a finding is revised after the candidates are visible, the revision is allowed — examiners do revise — but it is recorded as a revision, with a time and a reason.

## 4. Evidence in the Examiner's Language

For every candidate that survives to step 3, the screen shows a ledger rather than a score. Each row is a finding the examiner can verify on the records themselves:

```text
   candidate AM-0412          illustrative example — not from any case

   concordant
     27  gold onlay                 rare      ████████
     16  root canal + crown         uncommon  █████
     36  amalgam, occlusal          common    █

   explainable discrepancies
     46  present AM → absent PM     extraction after the last record is possible
     14  sound AM → composite PM    restoration after the last record is possible

   unexplainable discrepancies
     none
```

Three groups, and the groups matter more than the bars. Concordant findings are weighted by rarity, because a matching gold onlay says far more than a matching occlusal amalgam. Explainable discrepancies are differences that time can account for — the same temporal asymmetry that made ranking harder earlier in the series and made exclusion certain in the last post. Unexplainable discrepancies are differences time cannot account for, and a single one should stop the examiner cold. If it holds up, it is an exclusion the rule layer missed, usually because the charting was incomplete.

What about the neural part of the system? The tempting answer is a heatmap over the radiograph. I am deliberately not leaning on one. Adebayo and colleagues showed in 2018 that some popular saliency methods produce maps that barely change even when the model's weights are randomized — maps that look like explanations without depending on what the model learned. An explanation an examiner cannot falsify is decoration. So the embedding contributes exactly one line to the ledger, its calibrated similarity, and every *reason* on the screen points to a finding the examiner can check against the images with their own eyes. The rule is simple: an explanation should point to something you can verify, not something you have to trust.

## 5. Showing Where Certainty Ends

The ledger answers "why this candidate." The second half of the screen answers "how sure, and what would make us surer."

```text
   uncertainty panel                   illustrative example — not from any case

     category              possible identification
     margin                candidate 1 vs candidate 2 is narrow
     record quality        AM radiograph is 9 years older than the date of death;
                           posterior region partly out of frame
     what would decide it  an AM radiograph of the lower right
                           posterior region taken after that date
```

The margin matters because a candidate that barely beats the runner-up is a different situation from one that stands alone, even when both clear the threshold. The record-quality flags matter because a strong match against a poor record is weaker than it looks. And the last line matters most: it turns uncertainty into the next action. Instead of "the system is not sure," the examiner sees which piece of evidence would separate the candidates — usually a request that can actually be made to a clinic.

The empty result gets the same treatment. "No candidate above threshold" is not a blank page. It is a full result with its own reasons: how many records were excluded by proof, how many survived but scored too low, how close the best of them came, and which missing records would be worth requesting. In the open-set setting from the last post, silence is often the correct answer. It still has to be explained.

## 6. What the Screen Leaves Behind

A forensic conclusion may be questioned months or years after it is signed, by people who were not in the room. So the interface keeps a record of the decision, not just the decision itself: the locked postmortem chart and any revisions, the order in which the examiner saw everything, the model version and threshold in force at the time, and the examiner's stated reasons. If someone later asks how the conclusion was reached, the answer should be reconstructible without trusting anyone's memory — including mine.

## Conclusion

The usual way to judge an AI-assisted interface is to ask how often the human agrees with the machine. For this one, that is the wrong number. The questions I care about are whether the examiner can defend the conclusion without the system in front of them, and whether they disagree with the system when they should.

I came to software late, from the clinical side, because I believed AI would eventually reshape every field — mine included. The most useful thing building this system has taught me is the same thing forensic work taught me first: trust is not built where you sound most certain. It is built where you show exactly what you do not know.

Whether this interface actually earns that trust is an empirical question, and an uncomfortable one, because the honest test involves real examiners making real decisions under controlled conditions. How to design that test — what to measure, what counts as an improvement, and how to avoid fooling myself with a small study — is the next post.

---

*This post was drafted with the help of an AI writing assistant, working from my notes and the system described in earlier posts in this series. I reviewed and edited the draft, and the views are my own.*
