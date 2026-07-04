---
title: "DNR Is Not DNC (Do Not Care): Debugging Palliative Protocols"
date: 2026-07-04 10:00:00 +0900
categories: [Healthcare-AI, System-Design]
tags: [palliative-care, medical-ethics, protocols, failure-modes]
---

## Introduction

In system design, we talk endlessly about edge cases and failure modes. But when you step away from the IDE and work as a night duty doctor in a geriatric hospital, you realize that human systems have failure modes too, and they are far more devastating. When I sat down to analyze the palliative care protocols in my current ward, the hard part was not diagnosing the patient. It was discovering that the single most intuitive administrative choice — "just follow the daytime orders to avoid liability" — is quietly, catastrophically wrong for end-of-life care. What follows is a breakdown of how hospital protocols optimize for defensive medicine at the expense of patient dignity, beginning with the false binary that forces doctors to do nothing when they should act.

## 1. The First Real Problem: DNR Does Not Mean 'Do Not Care'

Every administrative guideline implicitly assumes that a patient's status can be cleanly categorized. In end-of-life care, that assumption is simply false, and it is false in a way that destroys naive medical systems. 

A Do Not Resuscitate (DNR) order is a specific directive: do not perform CPR in the event of cardiac arrest. It is not a mandate to withdraw comfort. And here is the crucial point: between the signing of a DNR and actual death, a living person is still breathing. They can still experience severe dyspnea, extreme distress, and the terrifying sensation of air hunger, using every accessory muscle just to draw a breath.

    Active Treatment                 Palliative/Comfort Care                 Death
           │                                   │                               │
           ●───────────────────────────────────●───────────────────────────────●
                                               ↑
                                Severe dyspnea, pain, and distress
                                all happen inside this gap

So the instant a hospital treats a DNR order as `WHERE care_level = 0`, you have built a system that silently discards the patient's dignity the moment their symptoms escalate. The true positive—the exact moment a patient needs a sedative like a benzodiazepine to ease their suffering—is precisely the intervention the system just filtered out. A silent acceptance of suffering is the worst possible failure in this field.

## 2. Encoding Defensive Medicine Instead of Patient Comfort

The fix is to stop asking "are we legally insulated?" and start asking "is this protocol *compatible* with the reality of a human being suffocating in front of us?"

Current hospital dynamics advance along a liability progression. Administering a sedative carries the inherent risk of respiratory depression. Doing nothing carries zero legal risk for the attending physician, even if it guarantees maximum suffering for the patient. This is an absolute fact of medical administration, and it is exactly the kind of distorted reward function that creates bad systems.

    Administer Sedative → Risk of Respiratory Depression → Doctor Penalized
    Do Nothing          → Patient Suffers Horribly       → Doctor is "Safe"
    ───────────────────────────────────────────────────────────────────────▶
               The system optimizes for administrative convenience.

That gives a toxic, asymmetric filter rule. For each episode of acute distress, the system demands absolute certainty that the patient is in their final hours before authorizing relief. It punishes proactive comfort care and rewards inaction. The asymmetry is the entire point. Defensive medicine treats action and inaction as unequal risks, prioritizing the hospital's administrative convenience over the patient's refractory distress. Under this rule, an intervention is excluded not because it is medically wrong, but because it is bureaucratically risky.

## 3. The Gap Between Observation and Authority

Once you realize the system is biased toward inaction, you still have to navigate the hierarchy. And the obvious heuristic — let the daytime attending make all the decisions — is the second trap.

Centralizing authority assumes the patient's condition is static, which it absolutely is not. The attending physician writes the orders during the day, but the night duty doctor actually watches the patient's chest heave and their face contort in agony at 2 AM. Separating the authority to prescribe from the real-time observation of suffering creates a massive latency in care.

This is the same insight that drives distributed systems: you cannot have the node experiencing the traffic spike wait 12 hours for the master server to authorize scaling. When a patient with a history of severe hiccups and compounding pneumonia begins to struggle violently for air, waiting for the morning rounds is not a medical strategy; it is negligence masked as protocol. 

## 4. Where the Decision Lives: Real-Time vs. Batch Processing

The final tradeoff is architectural. The hospital splits patient observation from critical decision-making, deliberately putting them on opposite sides of a bureaucratic boundary.

Observation lives with the night duty doctor on the ward. Decision-making—specifically, the authorization for palliative sedation—runs on a "batch processing" schedule during daytime rounds. The tempting shortcut for the hospital is to enforce rigid compliance to daytime orders, but this is a serious mistake. 

The clinical reality is highly volatile, low-latency work. Pain and air hunger do not scale to zero just because the primary physician is off the clock. A rigid, top-down protocol simply charges the patient the cost of sitting idle.

## Conclusion

Working on the ward taught me that the defining challenges in healthcare are rarely just clinical. The hardest problem was not diagnosing the dyspnea; it was recognizing that a DNR patient is a living human being trapped in a system that honors administrative safety over palliative relief. Inaction is the intuitive choice for the bureaucracy and the wrong one for the patient. 

Clear, symptom-driven protocols that empower the on-duty physician to act, an acknowledgment of the difference between treating a disease and treating a symptom, and a clean split from defensive medicine are what will give us a healthcare system patients can actually trust.
