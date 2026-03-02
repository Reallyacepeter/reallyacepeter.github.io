---
title: "Engineering for Justice: Navigating Gradle Hell and Android Package Evolution"
date: 2026-03-03 06:00:00 +0900
categories: [Software Engineering, Debugging]
tags: [flutter, android, gradle, dev-log]
---

# Redefining Forensic Workflows through Code

In the intersection of medical expertise and software engineering lies a powerful opportunity to solve critical real-world problems. My current project involves developing a **Centralized Dental Record & Identification System** in collaboration with the **National Forensic Service (NFS)**.

## The Problem: Legacy and Fragmentation
Forensic dental identification is a race against time. However, fragmented paper records and inconsistent data formats often slow down the process of matching unidentified remains with existing dental history. The need for a robust, cross-platform digital solution has never been more urgent.

## The Solution: Why Flutter?
For this project, I chose **Flutter** as the primary framework. The decision was driven by the need for cross-platform consistency and rapid prototyping of complex dental charting forms. But the choice brought its own set of engineering challenges.

[Image of Android Build Process with Gradle]

## Real-world Friction: Navigating 'Gradle Hell'
As a developer aiming for the highest standards, I insisted on using the latest Google Android packages. This led me straight into what developers call **"Gradle Hell."** Aligning the **Android Gradle Plugin (AGP)**, Kotlin versions, and Flutter’s native bridge was a significant hurdle. 

I spent countless nights debugging dependency resolution errors—where one package demanded a newer SDK while another was tethered to legacy structures. Handling **Namespace migrations** and fine-tuning **ProGuard/R8 rules** taught me that real-world engineering is as much about resilience and infrastructure management as it is about writing clean logic.

## Technical Architecture
The system is designed with a focus on data integrity. By leveraging a centralized database and building a structured schema for dental characteristics, we are moving toward an automated matching process.

* **Frontend:** Flutter (Dart)
* **Backend Logic:** Structured Java-based data processing
* **Infrastructure:** Modular dependency management to mitigate future breaking changes

## Impact and Future Path: The Road to AI
This app is more than a tool; it is a foundation for **Automated Identification**. As I refine the architecture, the next step is integrating **Computer Vision** to automate radiographic matching—a challenge that leads me directly into **Artificial Intelligence research**.

---

*This project serves as the technical bridge between my clinical background and my future research goals at **MIT**.*
