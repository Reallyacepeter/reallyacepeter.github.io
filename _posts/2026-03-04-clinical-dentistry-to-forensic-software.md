---
title: "From Clinical Dentistry to Forensic Software: Building a Digital Dental Identification System"
date: 2026-03-04
categories: [Software Engineering, Forensic Technology]
tags: [flutter, forensic-dentistry, data-integrity, dev-log]
---

# From Clinical Dentistry to Forensic Software

One of the most interesting problems at the intersection of medicine and technology is **forensic identification**.  
Dental records often play a critical role in identifying unknown individuals, especially when other biometric methods are unavailable.

My current project focuses on building a **Centralized Dental Record & Identification System** designed to support forensic dental workflows.

## How the Project Started

The origin of this project was unexpectedly informal.

Through my church community, I became acquainted with a forensic dentist working with the **National Forensic Service (NFS)**. During our conversations he often described a recurring difficulty: when software tools were developed externally, developers typically lacked dental knowledge, making it difficult to translate forensic workflows into practical software systems. At the same time, development costs were often prohibitively high.

At one point he suggested that I try building a prototype myself.

The original idea was relatively simple — a **mobile application that could record dental identification data directly from a smartphone**.

However, after examining the workflow more closely, I realized that simple data entry alone would not solve the underlying problem.

## Expanding the Concept

I proposed extending the application beyond basic record storage. Several features became central to the design:

- **AM/PM comparison workflows**  
  Supporting comparison between **antemortem (AM)** and **postmortem (PM)** dental data.

- **Structured dental data storage**  
  Designing a schema capable of representing dental findings consistently.

- **Search and retrieval functionality**  
  Allowing investigators to retrieve and compare identification records efficiently.

These additions gradually transformed the project from a simple recording tool into a broader **digital forensic identification system**.

## From Idea to System Design

As discussions continued, the project gradually moved beyond the initial idea of simple record collection.

Rather than only recording dental information in the field, I began thinking about how the system could support the **entire identification workflow**. This meant designing a structure that would allow investigators not only to store records, but also to retrieve, compare, and analyze them efficiently.

Because of my clinical background in dentistry, I was able to interpret the forensic dentists’ requirements and translate them directly into software design decisions. This helped bridge a gap that had previously made collaboration between developers and forensic professionals difficult.

As a result, the application evolved around three core design principles:

- **Structured data integrity** for reliable identification records  
- **Efficient retrieval and comparison** of AM/PM dental information  
- **Extensibility**, allowing the system to support future analytical tools

This shift—from a simple mobile recording tool to a structured identification system—became the foundation of the project's architecture.

## Development Approach

The application is being developed independently based on requirements provided by forensic dentists familiar with NFS workflows.

The main objective is to convert traditionally fragmented dental records into **structured digital data that can be stored, queried, and eventually analyzed computationally**.

### Current Technical Stack

- **Frontend:** Flutter (Dart)
- **Android Integration:** Gradle / Android SDK
- **Backend Processing:** Java-based data structuring
- **Architecture Goal:** Modular system designed for maintainability and long-term data integrity

## Engineering Challenges

Developing the Android infrastructure introduced several practical engineering challenges.

Maintaining compatibility with modern Android standards required navigating what developers often refer to as **“Gradle Hell.”** Aligning the Android Gradle Plugin, Kotlin versions, and Flutter’s native bridge required careful dependency management.

Resolving these conflicts involved:

- migrating packages to the modern **Android namespace architecture**
- restructuring Gradle dependency trees
- adjusting **ProGuard/R8 rules** to ensure stable builds

This experience reinforced an important engineering lesson: building real-world systems often requires as much attention to infrastructure stability as to application logic.

## Motivation and Real-World Context

I began working on this project near the end of my **first year of service as a public health dentist**.

During my second year, events such as the **Muan Airport disaster**, which occurred near the region where I had been working, reinforced the real-world importance of reliable forensic identification systems.

Incidents like this highlight how critical it is to **identify victims quickly and accurately**. Experiencing this context made the project feel far less abstract and strengthened my commitment to developing practical tools for forensic workflows.

## Looking Ahead

The current focus is building a stable digital infrastructure for forensic dental records.

Once reliable structured datasets are available, the next step will be exploring **AI-assisted identification**, including:

- radiographic similarity analysis  
- automated dental pattern comparison  
- computer vision approaches to dental imaging

This project ultimately serves as a bridge between my **clinical background in dentistry** and my growing interest in **computer science and artificial intelligence**.

My long-term goal is to continue exploring how **structured medical data and machine learning** can improve forensic identification systems.

---

*This post was edited with AI assistance for clarity.*
