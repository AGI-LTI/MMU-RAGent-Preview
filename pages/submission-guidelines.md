---
layout: page
title: Submission Guidelines
permalink: /submission-guidelines/
---


We're excited to receive your submissions. Each participating group may enter one or both tracks.

To participate:

1. Register yourself as a team.
2. *(Optional)* Download the starter code provided by the organizing team.
3. Develop your RAG system.
4. To particpate in both static and live evalution checkout [tracks A and B]({{ site.baseurl }}/getting-started/) from the getting started page.
5. To submit your results on only the validation set checkout the Validation set output submission header below.
  - Submission of validation set generations entitles you to a chance to win **non-cash prizes only**. These submissions are **not eligible for cash prizes** to ensure fairness.

---
<details>
<summary><h2 style="display: inline;">Docker image submission</h2></summary>

<div markdown="1">

### Text to VideoTrack Sample Docker File 

```dockerfile
FROM python:3.11-slim

# Set working directory
WORKDIR /app

# Copy requirements first to leverage Docker cache
COPY requirements.txt .

# Install system dependencies
RUN apt-get update && apt-get install -y \
    postgresql-client \
    libpq-dev \
    && rm -rf /var/lib/apt/lists/*

# Install Python dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy application code (includes .env file)
COPY . .

# Expose port
EXPOSE 4001

# Use Gunicorn to manage Uvicorn workers for production
# This command starts Gunicorn with 15 worker processes.
# Each worker is a Uvicorn process, allowing for parallel request handling.
CMD ["gunicorn", "-w", "2", "-k", "uvicorn.workers.UvicornWorker", "video_baseline:app", "--bind", "0.0.0.0:4001", "--timeout", "2000"]
```

### DockerImage Creation and ECR repository push commands

```bash
# Build the image
docker build --platform linux/amd64 -t my-app:latest .

# Authenticate to ECR
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 123456789012.dkr.ecr.us-east-1.amazonaws.com

# Tag for ECR
docker tag my-app:latest 123456789012.dkr.ecr.us-east-1.amazonaws.com/my-app:latest

# Push to ECR
docker push 123456789012.dkr.ecr.us-east-1.amazonaws.com/my-app:latest
```


### Text to VideoTrack Docker Image specifications

Each image should:
1.  No larger than **20 GB**
2.  Operate on a **single GPU with 24GB** of memory

### Image Submission guidelines

Once you have pushed your Docker image to the ECR repository with the tag **latest** you should notify the organizers by filling the following google form.

**Link:** [https://forms.gle/9uNcyrwDuZNZA569A](https://forms.gle/9uNcyrwDuZNZA569A)

NOTE :
1. Any team can only make 1 submission per week 
2. Ensure that the Image tag of your final submission must be **latest**
3. Ensure that the Image you build is supports **amd64** architecture.

</div>
</details>

---

<details>
<summary><h2 style="display: inline;">Validation set output submission</h2></summary>

<div markdown="1">

Once a team is registered the organizers will contact you on their registered email (preferably gmail) and will be assigning the following items.

1. Team ID
2. Google Drive folder for uploading your text-to-text results
3. Google Drive folder for uploading your text-to-video results

You should upload your results to your assigned Google Drive folder and fill out the following google form.

**Link:** [https://forms.gle/wRVKH7YfZXaM5QS1A](https://forms.gle/wRVKH7YfZXaM5QS1A)

Note:
Submission of validation set generations entitles you to a chance to win **non-cash prizes only**. These submissions are **not eligible for cash prizes** to ensure fairness.


**Submission format and requirements:**

#### Text-to-Text Generation

Submit a `.jsonl` file with your generated text outputs. Each line should contain a JSON entry that minimally contains the following keys:

```json
{
  "query_id": "string",  // Corresponding query_id from validation set
  "generated_response": "string"  // Generated text response
}
```

#### Text-to-Video Generation


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
</div>
</details>