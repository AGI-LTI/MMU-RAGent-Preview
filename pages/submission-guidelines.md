---
layout: page
title: Submission Guidelines
permalink: /submission-guidelines/
---


We're excited to receive your submissions. Each participating group may enter one or both tracks.

To participate:

1. *(Optional)* Download the starter code provided by the organizing team.
2. Develop your RAG system.
3. To participate only in static evaluation:
   - Download the validation set.
   - Submit generated outputs to the [submission page](#) *(to be released when the competition begins)* <!-- Replace with actual link -->
   - Submissions are ranked but are **not eligible for cash prizes**, for fairness.
4. Submit a RAG system via a Docker image on the [submission page](#) *(to be released when the competition begins)* <!-- Replace with actual link -->
   - Participants may submit **at most one Docker image per week**.

---

### A. Submission of Docker Image

Submission of a Docker image of your full RAG system entitles you to a chance to win both **non-cash and cash prizes**, as your submission will be evaluated using both static and live evaluation.

**Submission format and requirements:**

- Each image should be:
  - No larger than **20 GB**
  - Host a **web API** that encapsulates the full RAG system
  - Operate on a **single GPU with 24GB** of memory
- Participants may configure the retriever and generator components in one of the following ways:
  - **Self-contained**: Run entirely within the Docker image
  - **External API**: Accessed through an API you host
  - **Standardized API**: Use provided endpoints from the organizers

---

### B. Submission of Validation Set Generations

Submission of validation set generations entitles you to a chance to win **non-cash prizes only**. These submissions are **not eligible for cash prizes** to ensure fairness.

**Submission format and requirements:**

#### Track A: Text-to-Text Generation

Submit a `.jsonl` file with your generated text outputs. Each line should contain a JSON entry that minimally contains the following keys:

```json
{
  "query_id": "string",  // Corresponding query_id from validation set
  "generated_response": "string"  // Generated text response
}
```

#### Track B: Text-to-Video Generation

Submit a **compressed folder** containing:
- The generated video files
- A `.jsonl` file mapping queries to the generated video file

Each line in the `.jsonl` should be a JSON entry that minimally contains the following keys:

```json
{
  "query_id": "string",  // Corresponding query_id from validation set,
  "generated_video_fname": "string"  // Video filename in compressed folder
}
```

---