---
title: "Querying Dental Data for Identification: From Static Records to Computable Intelligence"
date: 2026-04-03 18:32:00 +0900
categories: [AI, Dentistry, Software Engineering]
tags: [forensic-dentistry, odontogram, flutter, firebase, data-architecture, querying]
---

## Introduction

In forensic dentistry, data collection is only half of the problem.  
The real challenge begins when we try to **use that data for identification**.

Traditional dental records—charts, notes, radiographs—are designed for human interpretation.  
They are inherently static.

However, in real-world scenarios such as mass disasters or unidentified remains, this approach quickly becomes inefficient.

The critical question is:

> How can we transform dental records into a system that is **queryable, comparable, and computationally meaningful**?

This post focuses on that transformation:  
**querying dental data for identification.**

---

## Limitations of Traditional Workflows

In most current workflows:

- Dental findings are manually recorded  
- Comparisons are performed visually  
- Matching depends heavily on expert experience  

This leads to fundamental limitations:

### 1. Poor Scalability  
Comparing dozens or hundreds of cases becomes impractical.

### 2. Inconsistency  
The same clinical finding may be described differently across practitioners.

### 3. Lack of Queryability  
There is no structured way to ask:

- “Find all cases with a missing upper left first molar”  
- “Match this restoration pattern”  

In short:

> The data exists, but it is not **computable**.

---

## Designing a Queryable Data Structure

To address this, I structured the system around a single principle:

> Every dental observation must be stored in a **machine-readable, queryable format**.

---

### 1. Tooth-Level Indexing (FDI System)

Each tooth is indexed using the FDI numbering system (1–32),  
providing a consistent coordinate system across all records.

---

### 2. Surface-Level Representation

Each tooth contains surface-specific data:

- O (Occlusal)  
- M (Mesial)  
- D (Distal)  
- B (Buccal)  
- L (Lingual)  

This enables fine-grained queries such as:

- Comparing occlusal restorations  
- Identifying mesial surface treatments  

---

### 3. Global Attributes

Each tooth also includes global attributes:

- Crown status  
- Root condition  
- Position  
- Pathology  
- Bite/Occlusion  

These fields are essential for higher-level comparisons.

---

### 4. Hierarchical Code System

Free-text input introduces inconsistency.  
To prevent this, all inputs are constrained through a hierarchical code system.

- Users select from a structured tree  
- Each selection is stored as a path  

This ensures:

- Standardization  
- Machine interpretability  
- Future compatibility with AI models  

---

## From Data to Query

Once structured, the data becomes actionable.

---

### Exact Matching

Example:

- Tooth 11 has a crown  
- Tooth 26 is missing  

This can be resolved through direct filtering.

---

### Pattern Matching

More advanced cases involve patterns:

- Distribution of restorations across quadrants  
- Symmetry between left and right arches  

This transforms the problem into:

- Feature extraction  
- Similarity comparison  

---

### Partial Matching

Real-world data is often incomplete.

- Missing teeth  
- Partial records  

This requires:

- Flexible query logic  
- Weighted scoring  
- Tolerance for missing data  

---

## Why This Matters

This transition fundamentally changes the system:

- Identification becomes faster  
- Large datasets become manageable  
- Matching can be assisted or automated  

Most importantly:

> The system evolves from **documentation to computation**.

---

## Toward AI-Assisted Identification

Once the data is structured and queryable:

- Pattern-based models can be trained  
- Similar cases can be suggested  
- Identification can be partially automated  

Without structured data, none of this is possible.

---

## Conclusion

The key insight is simple:

> Dental data only becomes valuable when it is **queryable and comparable at scale**.

Building such a system is not just about recording information.  
It is about transforming it into something that can be **computed, searched, and reasoned about**.

This is where software engineering meets forensic science.
