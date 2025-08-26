---
layout: page
title: Full System Submission Guidelines
permalink: /full-submission/
---

This page provides instructions for **Option 2: Full System Submission**. This is the main competition path for the final leaderboard and cash prizes.

Participants in this option must submit a complete, containerized system. This single Docker submission will be used for both evaluation stages: the live RAG-Arena and the final static evaluation on an unseen test set.

## 1. Live Evaluation using RAG-Arena System

Your submitted Docker image will be deployed to our platform to handle live user queries in real-time. The instructions below detail how to package and submit your system.

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

## 2. Static Evaluation on Unseen Test Set

After the live evaluation period concludes, the same Docker image you submitted for the RAG-Arena will be used by the organizers to run your system against a private, unseen test set. The results of this offline evaluation will be a key component in determining the final leaderboard rankings. **No separate submission is required for this step.**

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
