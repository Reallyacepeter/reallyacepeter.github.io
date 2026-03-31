---
title: "From Interaction to Data: Designing a Computable Dental Record System"
date: 2026-03-31
categories: [Dentistry, Software, Data-Architecture]
tags: [odontogram, data-modeling, forensic, flutter]
---

## Introduction

Traditional odontograms focus on visual interaction.

However, in forensic identification systems operating at scale,  
interaction alone is not sufficient.

The key question becomes:

> How can a user interaction be transformed into structured, computable data?

This post explores the design approach for converting user input  
into a consistent and queryable data representation.

---

## Problem: Interaction Does Not Equal Data

A naive approach treats user interaction as simple UI events.

This approach is intuitive, but fundamentally limited:

- It does not encode standardized meaning  
- It cannot be reliably compared across cases  
- It lacks consistency between different users  

In forensic scenarios, where data must be analyzed and compared at scale,  
this becomes a critical limitation.

In large-scale disaster situations, even small inconsistencies in records  
can delay identification or introduce critical errors.

This is not merely a usability issue —  
it is a problem of reliability.

---

## Design Choice: A Minimal Data Structure

Each user interaction is represented using three components:

- Tooth ID (FDI system)  
- Region (C, TL, TR, BR, BL)  
- Code (hierarchical classification)  

This structure defines a minimal unit of information.

> A localized finding with standardized meaning

By constraining input into this format,  
the system ensures consistency without eliminating expressiveness.

---

## From Interaction to Record

A user interaction follows a transformation process:

- User Click  
- Region Selection  
- Code Assignment  
- Structured Record Generation  

Each step reduces ambiguity and introduces semantic meaning.

The result is not a visual artifact,  
but a data point that can be stored, queried, and compared.

---

## Why Structure Matters (Trade-off Analysis)

There are multiple ways to represent user input:

### Option 1: Free-form interaction
- ✔ Flexible  
- ✘ Inconsistent  
- ✘ Difficult to compare across cases  

### Option 2: Fully constrained system
- ✔ Highly consistent  
- ✔ Suitable for large-scale analysis  
- ✘ Reduced flexibility  

---

### Final Decision: Structured Interaction Model

The system adopts a structured interaction model.

This design makes a deliberate trade-off:

> It sacrifices flexibility  
> in exchange for consistency, comparability, and computability

This balance is essential for systems that must operate reliably  
across large datasets and multiple users.

---

## System Implications

The structured model enables:

- Standardized data input across users  
- Consistent encoding of dental findings  
- Reliable comparison between cases  
- Direct compatibility with algorithmic processing  

Most importantly, it allows dental data to be:

> queried, compared, and computed at scale.

---

## Limitations

This approach has inherent limitations.

- It depends on correct user input  
- Some complex conditions are simplified  
- Flexibility is reduced compared to free-form input  

These limitations are intentionally accepted.

The goal is not to maximize input freedom,  
but to ensure consistency and computational usability.

---

## Future Extensions

The current design serves as a foundation.

Possible extensions include:

- Validation mechanisms for input consistency  
- Integration with radiographic image analysis  
- Mapping structured data to machine learning models  

These extensions build upon the same principle:  
transforming interaction into computable data.

---

## Conclusion

A digital odontogram should not only capture interaction —  
it should encode meaning.

By transforming user input into structured data,  
the system moves beyond visualization  
and becomes a computational tool.

This transition is not optional,  
but necessary for scalable and reliable forensic identification systems.
