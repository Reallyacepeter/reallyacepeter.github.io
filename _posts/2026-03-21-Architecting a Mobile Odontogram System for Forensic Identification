---
title: "Architecting a Mobile Odontogram System for Forensic Identification"
date: 2026-03-21
categories: [AI, Dentistry, Software]
tags: [odontogram, flutter, firebase, forensic, data-architecture]
---

## Introduction

Forensic identification often relies on dental records due to their durability and uniqueness. However, most dental data in forensic contexts are still recorded in fragmented and non-standardized formats.

As a dentist and developer, I recognized a deeper issue:

> Dental data exists, but it is not structured.

This project is an attempt to solve that problem.

---

## From UI to Data System

Initially, I set out to implement a digital odontogram.

However, a key question emerged:

> Is this just a visual tool, or a data system?

This led to a shift in approach:  
the odontogram became a **data interface**, not just a UI component.

---

## Odontogram Design

The system represents all 32 teeth using the FDI numbering system.

Each tooth is divided into five regions:

- Center (C)  
- Top-left (TL)  
- Top-right (TR)  
- Bottom-right (BR)  
- Bottom-left (BL)  

Each interaction is treated as structured data input rather than a visual annotation.

---

## Hierarchical Data Modeling

Instead of free-text input, the system uses a structured coding hierarchy:

- SystemCode  
- Text  
- Nat.Code  

Categories include:

- Bite / Occlusion  
- Bridges  
- Fillings  
- Periodontium  

This approach ensures consistency, scalability, and machine readability.

---

## System Architecture

- **Frontend:** Flutter  
- **Backend:** Firebase (Firestore + Storage)  
- **State Management:** Provider  

All data is managed in a centralized state and only persisted after a final review step.

---

## Key Challenges

### State Management
Maintaining data consistency across multiple screens required a centralized architecture.

### UI Complexity
Designing an interactive odontogram required balancing usability and precision.

### Data Modeling
Mapping complex dental findings into structured data required iterative design.

---

## Future Directions

- AI-based dental image analysis  
- Automated identification systems  
- Integration with large-scale forensic databases  

---

## Conclusion

This project is not just an application.

It is an attempt to redefine how dental data is structured, stored, and utilized.
