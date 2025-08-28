---
layout: page
title: Static Submission Guidelines
permalink: /static-submission/
---

This page provides instructions for **Option 1: Static Evaluation on the Validation Set**. This participation option is for teams who wish to benchmark their models on the public validation set and are eligible for non-cash prizes.

## Submission Process

Once a team is registered, the organizers will contact you via your registered email with the following:

1.  **Team ID**
2.  A **Google Drive folder** for uploading your results for the specific track you have registered for (either Text-to-Text or Text-to-Video).

You should upload your results to your assigned Google Drive folder and then fill out the submission form.

**Submission Form Link:** [https://forms.gle/wRVKH7YfZXaM5QS1A](https://forms.gle/wRVKH7YfZXaM5QS1A)

**Note:** Submission of validation set generations entitles you to a chance to win **non-cash prizes only**. These submissions are **not eligible for cash prizes**.

## Submission Format and Requirements

### Track A: Text-to-Text Generation

Submit a `.jsonl` file with your generated text outputs. Each line should contain a JSON object with the following keys:

```json
{
  "query_id": "string",  // Corresponding query_id from the validation set
  "generated_response": "string"  // Your system's generated text response
}
```

### Track B: Text-to-Video Generation

Submit a **compressed folder** containing:
- The generated video files.
- A `.jsonl` file mapping queries to the generated video files.

Each line in the `.jsonl` file should be a JSON object with the following keys:

```json
{
  "query_id": "string",  // Corresponding query_id from the validation set
  "generated_video_fname": "string"  // The filename of the generated video in the compressed folder
}
```
