---
layout: page
title: Full System Submission Guidelines
permalink: /full-submission/
---

This page provides instructions for **Option 2: Full System Submission**. This is the main competition path for the final leaderboard and cash prizes.

Participants in this option must submit a single, complete, containerized system. This Docker submission will undergo a two-part evaluation:

1.  **Live Evaluation in the RAG-Arena:** Your system will be randomly and anonymously served to human evaluators for side-by-side comparisons against other systems on live user queries.
2.  **Static Evaluation on an Unseen Test Set:** The same Docker image will be used to run your system against a private test set to finalize the rankings.

The combined results from both evaluations will determine the final leaderboard and prize winners.

#### DockerImage Creation and ECR repository push Sample Commands

```
# Build the image
docker build --platform linux/amd64 -t my-app:latest .

# Authenticate to ECR
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 123456789012.dkr.ecr.us-east-1.amazonaws.com

# Tag for ECR
docker tag my-app:latest 123456789012.dkr.ecr.us-east-1.amazonaws.com/my-app:latest

# Push to ECR
docker push 123456789012.dkr.ecr.us-east-1.amazonaws.com/my-app:latest
```

#### Docker Image specifications

Each image should:
1.  Be no larger than **20 GB**.
2.  Operate on a **single GPU with 24GB** of memory.

#### Image Submission guidelines

Once you have pushed your Docker image to the ECR repository with the tag **latest**, you should notify the organizers by filling out the following Google Form.

**Submission Form Link:** [https://forms.gle/9uNcyrwDuZNZA569A](https://forms.gle/9uNcyrwDuZNZA569A)

**NOTE:**
1.  Any team can only make 1 submission per week.
2.  Ensure that the Image tag of your final submission is **latest**.
3.  Ensure that the Image you build supports **amd64** architecture.

---

## Track-Specific Submission Details

For detailed technical requirements for your Docker image and API implementation for each track, please refer to:

-   **[Text-to-Text Track Details](/MMU-RAGent-Preview/text-to-text)**
-   **[Text-to-Video Track Details](/MMU-RAGent-Preview/text-to-video)**

## Repository Submission Requirement

To ensure compliance with competition rules, **all participants must submit a GitHub repository** containing their system code and documentation.

-   The repository can be either:
    -   **Public**, or
    -   **Private**, shared directly with the organizers via GitHub access or a downloadable archive.
-   This allows us to verify that your system does **not fully rely on commercial API-only models**, which are **not permitted** in this competition.

Please ensure that your repository includes:
-   Your full system pipeline (retriever, generator, and any custom components).
-   A README with clear setup and run instructions.
-   Documentation of any external tools, models, or corpora used.
