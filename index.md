---
layout: home
title: MMU-RAG
subtitle: NeurIPS 2025 Competition
---
Welcome to the official website of **MMU-RAG**: the *Massive Multi-Modal User-Centric Retrieval-Augmented Generation* Benchmark. This competition invites researchers and developers to build RAG systems that perform in real-world conditions.

Participants will tackle real-user queries, retrieve from web-scale corpora, and generate high-quality responses in both text and/or video formats. 

**MMU-RAG** features two tracks:

1. **Text-to-Text**
2. **Text-to-Video**

Submissions are evaluated using a blend of:

- Automatic metrics
- LLM-as-a-judge evaluations
- Real-time human feedback through our interactive RAG-Arena platform

Whether you’re advancing retrieval strategies, generation quality, or multimodal outputs, this is your opportunity to benchmark your system in a setting that reflects actual user needs.

**Ready to take on the challenge?** [**Register your team and get started!**](/MMU-RAGent-Preview/getting-started/)

------

## How to Participate

For each of our competition tracks (**Text-to-Text** and **Text-to-Video**), there are two participation options designed to accommodate different goals:

### Option 1: Static Evaluation on the Validation Set (Non-Cash Prizes)

This option is perfect for teams who want to benchmark their models, test their pipelines, or contribute to the research landscape without committing to the full live evaluation.

-   **What you do:** Run your system on our public **validation set** for your chosen track.
-   **How to submit:** Upload your generated outputs as files (`.jsonl` for text, a compressed folder for video) to a designated Google Drive folder.
-   **What you're eligible for:** These submissions are eligible for **non-cash prizes**, such as honorable mentions and features on our website. This option does not qualify for cash prizes.

[See detailed static submission guidelines here](/MMU-RAGent-Preview/static-submission/)

### Option 2: Full System Submission (Cash Prizes)

This is the main competition path for teams aiming for the top of the leaderboard and cash prizes. It involves a two-part evaluation.

-   **What you do:** Build a robust RAG system and package it into a **Docker image** that exposes specific API endpoints for streaming and evaluation.
-   **How to submit:** Push your Docker image to an AWS ECR repository provided by the organizers.
-   **How it's evaluated:**
    1.  **Live Evaluation:** Your system will be deployed on our **RAG-Arena platform** to face live user queries in real-time.
    2.  **Static Evaluation:** Your system will also be evaluated on a **private, unseen test set** to determine the final rankings.
-   **What you're eligible for:** This is the only path to be eligible for the **final leaderboard rankings and cash prizes**.

[See detailed full submission guidelines here](/MMU-RAGent-Preview/full-submission/)

------

## Tracks

### Track A: Text-to-Text

This track reflects the standard text-to-text RAG application. Participants are expected to submit systems that retrieve from a text corpus, and generate text responses given a text query. Participants may use our provided corpora, or augment their generations with any other background corpora or (proprietary) search APIs, provided that all external resources are clearly documented in their submissions.

!! **Now Accepting Deep Research Systems** !!
In addition to standard RAG pipelines, we welcome submissions from *deep research systems*—models that perform multi-hop retrieval, structured reasoning, or integrate external tools or knowledge bases beyond conventional retrievers. If your system pushes the boundaries of retrieval and generation, we encourage you to participate.

[Click here to download validation queries](https://drive.google.com/file/d/1-a7VaGGMrzxqTI1rCrQTiB_lqBjLOWcv/view?usp=drive_link)

### Track B: Text-to-Video

Participants are expected to submit systems that retrieve from a text corpus, and generate video responses given a text query that is predetermined to benefit from a video response. Participants may use our provided corpora, or augment their generations with any other background corpora or (proprietary) search APIs, provided that all external resources are clearly documented in their submissions.

[Click here to download validation queries](https://drive.google.com/file/d/1vh15gpHxYV9GBICN7_EI99TGR3uHAdSP/view?usp=sharing)





# Calendar

### Aug 1: **Competition launch & dataset release**
- Two tracks: [Text-to-Text](/MMU-RAGent-Preview/text-to-text) and [Text-to-Video](/MMU-RAGent-Preview/text-to-video)

### Aug 1 - Oct 15: ACTION REQUIRED: Competition Submission
- Two options: 
    - [Static Submission](/MMU-RAGent-Preview/static-submission) (run on public validation set and upload a `.jsonl` for non-cash prizes)
    - [Full System Submission](/MMU-RAGent-Preview/full-submission) (submit a Docker image for private test set evaluation + real-time user evaluation, to top the leaderboard and get cash prizes)

### Oct 15–21: Organizers Running Evaluations
- Submissions will be evaluated using a blend of:
    - Automatic metrics
    - LLM-as-a-judge evaluations
    - Real-time user feedback from our RAG-Arena

### Dec 6-7: **MMU-RAG Workshop at NeurIPS 2025**
- Presentations by selected teams
- Winners and runner(s)-up announced

---

## Prizes

We’re excited to offer both **monetary prizes** and **academic exposure opportunities** to recognize outstanding submissions.

### 💰 Prize Pool

Thanks to the support of Amazon, MMU-RAG offers a **$10,000 prize pool in AWS credits**. Prizes will be awarded to top-performing teams across both tracks.

### 🎤 Present at NeurIPS

Top teams will also be invited to **present their systems** during the **MMU-RAG competition session at NeurIPS 2025**. This is a unique opportunity to share your work with the community.

### 🥇 Eligibility

Prize eligibility requires full system reproducibility and clear documentation of all components. Only participants in the **Full System Submission** option are eligible for cash prizes.

---

## Submission and Workshop

All participants are required to submit a report detailing their system, methods, and results. Top-performing and innovative teams will be invited to present their work at our associated NeurIPS 2025 workshop. Further details on the report format and submission deadlines will be announced soon.

---

# Contact Us

For any questions or clarifications, email the organizers directly at: mmu-rag@andrew.cmu.edu

## Organizers

- [Luo Qi Chan](https://luoqichan.github.io), DSO National Laboratories / Carnegie Mellon University  
- [Tevin Wang](https://tevinwang.com), Carnegie Mellon University  
- [Shuting Wang](https://shootingwong.github.io), Renmin University of China / Carnegie Mellon University  
- Zhihan Zhang, Carnegie Mellon University  
- Alfredo Gomez, Carnegie Mellon University  
- [Prahaladh Chandrahasan](https://prahaladhchandrahasan.github.io), Carnegie Mellon University  
- Lan Yan, Carnegie Mellon University  
- Andy Tang, Carnegie Mellon University  
- Zimeng (Chris) Qiu, Amazon AGI  
- Morteza Ziyadi, Amazon AGI  
- [Sherry Wu](https://www.cs.cmu.edu/~sherryw/), Carnegie Mellon University  
- Mona Diab, Carnegie Mellon University  
- [Akari Asai](https://akariasai.github.io), University of Washington
- [Chenyan Xiong](https://www.cs.cmu.edu/~cx/), Carnegie Mellon University  