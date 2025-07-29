---
layout: home
title: MMU-RAG
subtitle: NeurIPS 2025 Competition
---

Welcome to the official website of **MMU-RAG**: the *Massive Multi-Modal User-Centric Retrieval-Augmented Generation* Benchmark. This competition invites researchers and developers to build RAG systems that perform in real-world conditions.

Participants will tackle real-user queries, retrieve from web-scale corpora, and generate high-quality responses in both text and video formats.

**MMU-RAG** features two tracks:  
1. **Text-to-Text**  
2. **Text-to-Video**

Submissions are evaluated using a blend of:
- Automatic metrics
- LLM-as-a-judge evaluations
- Real-time human feedback through our interactive RAG-Arena platform

Whether you're advancing retrieval strategies, generation quality, or multimodal outputs, this is your opportunity to benchmark your system in a setting that reflects actual user needs.

**Ready to take on the challenge?** Explore the tracks, datasets, and submission guidelines to get started.

---

## Tracks

### Track A: Text-to-Text

This track reflects the standard text-to-text RAG application. Participants are expected to submit systems that retrieve from a text corpus, and generate text responses given a text query. Participants may use our provided corpora, or augment their generations with any other background corpora or (proprietary) search APIs, provided that all external resources are clearly documented in their submissions.

[Click here to download validation queries](https://drive.google.com/file/d/1-a7VaGGMrzxqTI1rCrQTiB_lqBjLOWcv/view?usp=sharing) <!-- Replace with actual download link -->

### Track B: Text-to-Video

Participants are expected to submit systems that retrieve from a text corpus, and generate video responses given a text query that is predetermined to benefit from a video response. Participants may use our provided corpora, or augment their generations with any other background corpora or (proprietary) search APIs, provided that all external resources are clearly documented in their submissions.

[Click here to download validation queries](https://drive.google.com/file/d/1vh15gpHxYV9GBICN7_EI99TGR3uHAdSP/view?usp=sharing) <!-- Replace with actual download link -->

---

## Rules

The goal of the competition is to create a RAG system that is **robust to real-user queries**. We provide starter code for a simple RAG system, comprising three components:
1. An **embedding model**
2. A **retriever**
3. A **generation model**


### **Data and Models**

Participants may use **any open- or close-source data or models**.  
However:
- Systems that include **close-source and/or proprietary components** will be ranked on a **separate leaderboard** from fully open-source systems.
- All retrieval methods, generation models, corpora, and external tools or APIs must be **clearly specified**.
- For systems with close-source components, please provide as much detail as possible for fair evaluation.
- **Commercial API-only systems** (e.g., GPT-4o, Sonnet) are **not allowed**.


### **System Restrictions**

- All outputs must be **model-generated**.
- **Human-verified outputs** or any form of **human intervention** are **not allowed**, to ensure full replicability of the systems.

---

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