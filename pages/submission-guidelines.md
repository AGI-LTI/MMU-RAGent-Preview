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

## 1. Live Evaluation using RAG-Arena System. 

In this setting, your system runs **dynamically** in response to live user queries. You must:

- Package your system as a Docker image,
- Push it to an AWS ECR repository, and
- Notify the organizers for deployment.

This evaluation will be used to determine the **final leaderboard rankings and cash prize winners**.



### DockerImage Creation and ECR repository push

```
# Build the image
docker buildx build --platform linux/amd64 -t my-app:latest .

# Authenticate to ECR
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 123456789012.dkr.ecr.us-east-1.amazonaws.com

# Tag for ECR
docker tag my-app:latest 123456789012.dkr.ecr.us-east-1.amazonaws.com/my-app:latest

# Push to ECR
docker push 123456789012.dkr.ecr.us-east-1.amazonaws.com/my-app:latest
```

### Docker Image specifications

Each image should:

1. No larger than **20 GB**
2. Operate on a **single GPU with 24GB** of memory



### Image Submission guidelines

Once you have pushed your Docker image to the ECR repository with the tag **latest** you should notify the organizers by filling the following google form.

**Link:** https://forms.gle/9uNcyrwDuZNZA569A

NOTE :

1. Any team can only make 1 submission per week
2. Ensure that the Image tag of your final submission must be **latest**
3. Ensure that the Image you build is supports **amd64** architecture.



## 2. For Static Evaluation on Validation set ONLY

Once a team is registered the organizers will contact you on their registered email (preferably gmail) and will be assigning the following items.

1. Team ID
2. Google Drive folder for uploading your text-to-text results
3. Google Drive folder for uploading your text-to-video results

You should upload your results to your assigned Google Drive folder and fill out the following google form.

**Link:** https://forms.gle/wRVKH7YfZXaM5QS1A

Note: Submission of validation set generations entitles you to a chance to win **non-cash prizes only**. These submissions are **not eligible for cash prizes** to ensure fairness.

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

### 2. Static Evaluation on the Validation Set ONLY (Optional)

This is an **offline submission** where you run your model on a shared validation set and submit your outputs as files. This allows you to:

- Test your pipeline,
- Benchmark your model, and
- Be eligible for **non-cash prizes** (e.g., honorable mentions, spotlight features).

You may submit for either or both evaluation types, but **only live evaluation submissions are eligible for cash prizes**.

Please read the instructions below carefully and follow the correct format and process based on your submission type.