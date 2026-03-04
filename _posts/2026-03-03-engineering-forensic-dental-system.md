---
title: "Engineering for Justice: Navigating Gradle Hell and Android Package Evolution"
date: 2026-03-03 06:00:00 +0900
categories: [Software Engineering, Debugging]
tags: [flutter, android, gradle, dev-log]
---

# Redefining Forensic Workflows through Code

At the intersection of medical expertise and software engineering lies a powerful opportunity to solve critical real-world problems. My current project is the development of a **Centralized Dental Record & Identification System**, built independently based on requirements provided by forensic dentists working within the **National Forensic Service (NFS)**.

## The Problem: Legacy and Fragmentation

Forensic dental identification is often a race against time. However, fragmented paper records and inconsistent data formats frequently slow the process of matching unidentified remains with existing dental histories.  

In many cases, identification depends not on the lack of information but on the **difficulty of structuring and accessing existing data**. The need for a robust, standardized digital system is therefore critical.

## The Solution: Why Flutter?

To address this challenge, I chose **Flutter** as the primary development framework. The decision was driven by the need for a cross-platform system capable of handling complex dental charting interfaces while maintaining consistency across devices.

Flutter allows rapid iteration of structured dental data entry systems while maintaining a unified codebase for Android deployment.

However, the decision also introduced a series of engineering challenges within the Android build ecosystem.

## Real-world Friction: Navigating "Gradle Hell"

While attempting to maintain compatibility with the latest Android development standards, I encountered what many developers refer to as **"Gradle Hell."**

Aligning the **Android Gradle Plugin (AGP)**, Kotlin versions, and Flutter’s native bridge required careful dependency management. Certain libraries demanded newer SDK targets, while others remained tied to older structures.

Resolving these conflicts involved:

- Migrating Android packages to the modern **namespace architecture**
- Reconfiguring **Gradle dependency trees**
- Adjusting **ProGuard/R8 rules** to ensure stable builds

This experience reinforced an important lesson: real-world engineering is not only about writing application logic, but also about **maintaining stable infrastructure across evolving software ecosystems**.

## Technical Architecture

The system architecture prioritizes **data integrity and structured medical information**.

By building a centralized database and defining a standardized schema for dental characteristics, the project aims to transform fragmented records into machine-readable forensic datasets.

**Core stack**

- **Frontend:** Flutter (Dart)
- **Backend Processing:** Java-based data structuring
- **Infrastructure:** Modular dependency management designed to minimize future breaking changes

## Impact and Future Path: Toward Automated Identification

This application represents more than a clinical tool. It forms the foundation for **automated forensic identification systems**.

As the architecture matures, the next phase will involve integrating **computer vision techniques** to assist in radiographic comparison and pattern matching.

Exploring these problems naturally leads toward **artificial intelligence research**, particularly in areas where structured medical data intersects with machine learning.

---

*This project serves as a technical bridge between my clinical background in dentistry and my long-term goal of pursuing research in computer science, particularly in AI-driven forensic identification systems.*
