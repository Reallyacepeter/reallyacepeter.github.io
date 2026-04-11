---
title: "Evaluating a Forensic Dental Identification System: From Data Integrity to System Validation"
date: 2026-04-11 21:10:00 +0900
categories: [AI, Dentistry, Software Engineering]
tags: [evaluation, system-validation, forensic-dentistry, odontogram, data-integrity, flutter, firebase]
---

## Introduction

Building a system is only half the problem.

In forensic identification, correctness is not optional.  
A system that stores dental records is not useful unless it can be **trusted under real-world conditions**.

This raises a critical question:

> How do we validate that a forensic dental identification system actually works?

This post explores the evaluation and validation of a mobile-based dental record system designed for forensic use.

---

## 1. What Does “Correctness” Mean in This Context?

Unlike typical CRUD applications, this system must satisfy stricter criteria:

- **Data Accuracy** — Dental findings must be recorded without distortion
- **Data Consistency** — Identical inputs must produce identical stored structures
- **Retrievability** — Stored data must be queryable without loss
- **Interpretability** — Data must remain clinically meaningful

This is not just a software problem.  
It is a **data integrity problem**.

---

## 2. System Overview

The system is built using:

- **Flutter** for cross-platform UI (Android/Web)
- **Firebase Firestore** for persistent storage
- A structured odontogram interface based on:
  - FDI numbering system
  - Tooth surface encoding (`O`, `M`, `D`, `B`, `L`)
  - Hierarchical clinical codes (e.g., caries, prosthetics)

The odontogram is treated not as a visual chart, but as a **data interface**.

---

## 3. Validation Strategy

Validation was approached in three layers:

### 3.1 Input-Level Validation

Goal:
- Ensure that the UI correctly captures clinical intent

Method:
- Simulate realistic dental inputs:
  - Multiple surfaces per tooth
  - Mixed conditions (e.g., caries + prosthetics)
- Verify that selections map correctly to structured fields

Key insight:
> UI ambiguity leads directly to data corruption.

---

### 3.2 Data Structure Validation

Goal:
- Ensure that stored data matches the intended schema

Method:
- Inspect Firestore documents after input
- Validate:
  - FDI indexing correctness
  - Surface mapping consistency
  - Hierarchical code integrity

Example checks:
- No duplicated surfaces
- No orphan subcategories
- Correct parent-child relationships

Key insight:
> A clean UI does not guarantee a correct database.

---

### 3.3 Retrieval and Query Validation

Goal:
- Ensure that stored data can be reliably retrieved and interpreted

Method:
- Query records using:
  - Partial dental patterns
  - Specific code filters
- Compare retrieved results with original input

Focus:
- Lossless reconstruction of dental findings
- Query stability under varying conditions

Key insight:
> If retrieval distorts meaning, the system fails its purpose.

---

## 4. Failure Modes Identified

During validation, several critical failure modes emerged:

### 4.1 Structural Drift
- Inconsistent encoding of similar clinical conditions
- Result: unreliable querying

### 4.2 UI-Induced Errors
- Users misinterpreting tooth surfaces or codes
- Result: incorrect data entry despite correct schema

### 4.3 Over-Simplification
- Attempting to compress complex dental findings into flat structures
- Result: loss of clinical nuance

These failures highlight a core principle:

> Data systems fail silently before they fail visibly.

---

## 5. Limitations of Rule-Based Validation

Initial validation relied on deterministic checks:

- Schema validation
- Field constraints
- Rule-based consistency checks

However, this approach has limits:

- Cannot capture **semantic similarity**
- Fails in ambiguous or incomplete cases
- Prone to **overfitting predefined rules**

This leads to an important conclusion:

> Rule-based validation is necessary, but not sufficient.

---

## 6. Toward Intelligent Validation

A robust forensic system must eventually answer:

- Are two dental records *similar enough* to suggest identity?
- Can incomplete data still produce meaningful matches?

These questions cannot be solved with rules alone.

Future direction:

- **Similarity-based evaluation**
- **Machine learning for pattern recognition**
- Integration of imaging data (e.g., panoramic X-rays)

This transforms the problem from:

- Data validation → **Identity inference**

---

## Conclusion

Validation is not a final step.  
It is an ongoing process that defines the system’s credibility.

In forensic dental identification:

- Incorrect data is worse than no data
- Inconsistent data is unusable data

The system must therefore guarantee:

> Not just that data exists,  
> but that it can be trusted.

This post marks the transition from building a system  
to questioning its reliability—and ultimately, its intelligence.
