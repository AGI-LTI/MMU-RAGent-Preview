---
layout: page
title: Getting Started
permalink: /getting-started/
---

Welcome to the MMU-RAG competition! This page contains all the rules, resources, and APIs you need to get started.

## Rules

The goal of the competition is to create a RAG system that is **robust to real-user queries**. We provide starter code for a simple RAG system, comprising three components:

1.  An **embedding model**
2.  A **retriever**
3.  A **generation model**

### Data and Models

To encourage diversity in submissions, participants may use **any open- or close-source data or models**.

However:

-   Systems that include **close-source and/or proprietary components** will be tagged clearly on leaderboards.
-   All retrieval methods, generation models, corpora, and external tools or APIs must be **clearly specified**.
-   For systems with close-source components, please provide as much detail as possible for fair evaluation.
-   **Commercial API-only deep research systems** (e.g., perplexity sonar API, Open AI deep research API) are **not allowed**.

| Component | Open Source | Close Source |
| :--- | :--- | :--- |
| **Embedding / Retrieval Modules** | Open-weight models: publicly available for public download and use (i.e available on HuggingFace or public GitHub repository) | Model weights are unavailable for public download and use. |
| **Retrieval Corpus** | Fixed corpora that is available for public download and use. | Search implemented using commercial and/or proprietary API, often giving access to whole-of-internet or equivalent. |
| **Generation Modules** | Open-weight models: publicly available for public download and use (i.e available on HuggingFace or public GitHub repository) | Model weights are unavailable for public download and use |

Please feel free to consult the organizing team if any clarifications are required.

### System Restrictions

-   All outputs must be **model-generated**.
-   **Human-verified outputs** or any form of **human intervention** are **not allowed**, to ensure full replicability of the systems.

---

## Registration and Resources

### Registration

To participate in any track, you **must register your team** by filling out this short form: [Register Your Team](https://forms.gle/YDEnjV4PXWnZdfYG8).

Once a team is registered, the organizers will contact you on their registered email (preferably gmail) and will be assigning the following items.

-   Team ID
-   ECR Repository ARN
-   AWS ECR access keys
-   S3 bucket name and region
-   Port Number where the API needs to run
-   ClueWeb 22 API key request instructions (if you want API key access)

### Competition Tracks

Choose your track to get started with detailed instructions, starter code, and submission guidelines:

-   **[Text-to-Text Track](/MMU-RAGent-Preview/text-to-text)** - Build RAG or Deep Research systems for text generation
-   **[Text-to-Video Track](/MMU-RAGent-Preview/text-to-video)** - Build RAG systems that retrieve relevant information and generate videos from text queries

Each track page contains:
- Starter code and templates
- API access details
- Submission requirements
- Technical specifications