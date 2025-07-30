---
layout: page
title: Submission Guidelines
permalink: /submission-guidelines/
---


This page outlines how to submit your system outputs for **evaluation** in the MMU-RAG competition. There are **two types of evaluations**, each with its own process:

## Track-Specific Submission Details

For detailed submission requirements and implementation guidelines for each track, please refer to:

- **[Text-to-Text Track Details](/MMU-RAGent-Preview/text-to-text)** - Complete submission requirements and API specifications for the Text-to-Text track
- **[Text-to-Video Track Details](/MMU-RAGent-Preview/text-to-video)** - Complete submission requirements and implementation guidelines for the Text-to-Video track 



### 1. Live Evaluation via RAG-Arena (Required for Final Judging)

In this setting, your system runs **dynamically** in response to live user queries. You must:

- Package your system as a Docker image,
- Push it to an AWS ECR repository, and
- Notify the organizers for deployment.

This evaluation will be used to determine the **final leaderboard rankings and cash prize winners**.



### 2. Static Evaluation on the Validation Set ONLY (Optional)

This is an **offline submission** where you run your model on a shared validation set and submit your outputs as files. This allows you to:

- Test your pipeline,
- Benchmark your model, and
- Be eligible for **non-cash prizes** (e.g., honorable mentions, spotlight features).

You may submit for either or both evaluation types, but **only live evaluation submissions are eligible for cash prizes**.

Please read the instructions below carefully and follow the correct format and process based on your submission type.



## 1. Live Evaluation using RAG-Arena System. 

In this setting, your system runs **dynamically** in response to live user queries. You must:

- Package your system as a Docker image,
- Push it to an AWS ECR repository, and
- Notify the organizers for deployment.

This evaluation will be used to determine the **final leaderboard rankings and cash prize winners**.



### DockerImage Creation and ECR repository push

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