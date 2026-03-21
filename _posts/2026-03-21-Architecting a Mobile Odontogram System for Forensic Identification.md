---
title: "Architecting a Mobile Odontogram System for Forensic Identification"
date: 2026-03-21
categories: [AI, Dentistry, Software]
tags: [odontogram, flutter, firebase, forensic, data-architecture]
---

## Introduction

Forensic identification often relies on dental records due to their durability and uniqueness. However, most dental data in forensic contexts are still recorded in fragmented and non-standardized formats.

As a dentist and developer, I recognized a deeper issue:

> Dental data exists, but it is not structured in a way that is computable at scale.

This project is an attempt to address that limitation by transforming dental records into a structured, machine-readable system.

---

## From UI to Data System

Initially, I set out to implement a digital odontogram.

However, a key question emerged:

> Is this just a visual tool, or a data system?

This question led to a fundamental shift in approach.  
Instead of treating the odontogram as a graphical interface, I redefined it as a **data interface**—a system where each interaction produces structured, meaningful data.

---

## Odontogram Design

The system represents all 32 teeth using the FDI numbering system.

Each tooth is divided into five regions:

- Center (C)  
- Top-left (TL)  
- Top-right (TR)  
- Bottom-right (BR)  
- Bottom-left (BL)  

This design decision was not arbitrary.

A single-point representation is insufficient for capturing localized dental findings,  
while high-resolution segmentation introduces excessive UI complexity and data fragmentation.

The 5-region model is a deliberate trade-off between spatial precision and computational simplicity.  
It provides enough granularity to localize findings while maintaining a consistent and scalable data structure.

Each interaction is therefore not a visual annotation, but a structured data event.

---

## Hierarchical Data Modeling

Traditional dental records rely heavily on free-text descriptions.  
While flexible, this approach lacks consistency and is not suitable for computational processing.

To address this, the system adopts a hierarchical coding structure based on:

- SystemCode  
- Text  
- Nat.Code  

These codes are organized into expandable categories such as:

- Bite / Occlusion  
- Bridges  
- Fillings  
- Periodontium  

This approach enforces structure while preserving extensibility.

It represents a deliberate shift:

> from descriptive flexibility to machine readability.

This trade-off is essential for enabling large-scale comparison and analysis in forensic contexts.

---

## System Architecture

The system is built using:

- **Frontend:** Flutter  
- **Backend:** Firebase (Firestore + Storage)  
- **State Management:** Provider  

A centralized Provider-based architecture was chosen over route-based state management.

In multi-step data entry systems, route-based approaches often lead to fragmented or inconsistent state.  
By maintaining a single unified state object, the entire odontogram and associated metadata can be treated as a coherent dataset.

Data is only persisted after a final review step, ensuring consistency and reducing partial or invalid entries.

---

## Key Challenges

### State Management

Maintaining consistency across multiple input screens required a shift to centralized state management.  
This was essential to prevent data loss and fragmentation.

---

### UI Complexity

Designing an interactive odontogram required balancing usability and precision.  
Increasing spatial resolution improves accuracy but introduces interaction complexity.

---

### Data Modeling

Dental findings are inherently complex and context-dependent.  
Mapping them into a structured, hierarchical model required iterative refinement and domain-driven decisions.

---

## Future Directions

This system is designed as a foundation rather than a final product.

Future extensions include:

- AI-based dental image (radiograph) analysis  
- Automated matching between unidentified individuals and existing records  
- Integration with large-scale forensic identification systems  

The long-term goal is to enable dental data to function as a fully computable layer in identification workflows.

---

## Conclusion

This project began as an attempt to digitize an odontogram.

It evolved into something more fundamental:

> a system that treats dental data as structured, computable information.

By prioritizing consistency, scalability, and machine readability,  
this work aims to redefine how dental records are created, stored, and utilized in forensic contexts.
