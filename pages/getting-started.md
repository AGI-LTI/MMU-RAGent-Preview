---
layout: page
title: Getting Started
permalink: /getting-started/
---

## Registration
Irrespective of any track you choose to participate you should register yourself as team by filling out the following google form! 

**Link:** [https://forms.gle/YDEnjV4PXWnZdfYG8](https://forms.gle/YDEnjV4PXWnZdfYG8)

**Note** that while we do not have a limitation on the number of participants in a team, we want a dedicated team leader who would act as a liaison between the organizers and the rest of the team.

---
<details>
<summary><h2 style="display: inline;">A: Text-to-Text</h2></summary>

<div markdown="1">

## Static Evaluation: Submission Template and API Specification

For static evaluation submissions, your system should implement an `/evaluate` endpoint that generates responses for validation set queries. This endpoint will be used to produce the `.jsonl` file required for submission.

### Required Endpoint

Your service must expose the following endpoint for static evaluation:

```
POST /evaluate
```

### Request Format

**Content-Type:** `application/json`

**Request Body:**
```json
{
  "query": "string",        // The research question/query
  "reference": "string",    // Reference answer or context from validation set
  "iid": "string"          // Instance ID from validation set
}
```

#### Parameters
- `query` (required, string): The research question/query from the validation set
- `reference` (required, string): The reference answer or context provided in the validation set
- `iid` (required, string): The instance identifier from the validation dataset

### Response Format

**Content-Type:** `application/json`

**Response Structure:**
```json
{
  "query_id": "string",           // Use the "iid" value from the request
  "generated_response": "string"  // Your system's generated text response
}
```

## Dynamic Evaluation: Submission Template and API Specification

For a validation submission, Your system must implement a specific streaming API that follows our standardized response format.

### Required Endpoint

Your service must expose the following endpoint:

```
POST /run
```

## Request Format

**Content-Type:** `application/json`

**Request Body:**
```json
{
  "question": "string"
}
```

#### Parameters
- `question` (required, string): The research question/query from the user

#### Example Request
```json
{
  "question": "What are the latest developments in quantum computing?"
}
```

### Response Format

**Content-Type:** `text/event-stream` (preferred) or `text/plain`

**Response Structure:** Server-Sent Events (SSE) format where each line starts with `data: ` followed by JSON:

```
data: {"intermediate_steps": "...", "final_report": "...", "is_intermediate": true, "complete": false}
data: {"intermediate_steps": "...", "final_report": "...", "is_intermediate": false, "complete": true}
```

### Required JSON Response Fields

Each JSON object in the stream must contain these fields:

#### Core Fields (Required)

| Field | Type | Description |
|-------|------|-------------|
| `intermediate_steps` | string \| null | The thinking/reasoning process, search queries, retrieved information, etc. Use `"|||---|||"` as separator between steps |
| `final_report` | string \| null | The final answer content being generated |
| `is_intermediate` | boolean | `true` when showing thinking process, `false` when generating final answer |
| `complete` | boolean | `true` on the final message to signal completion |

#### Optional Fields

| Field | Type | Description |
|-------|------|-------------|
| `citations` | array | List of citation objects (see format below) |
| `error` | string | Error message if something goes wrong (stops the stream) |

#### Citation Format (Optional)

Citations are displayed in the frontend as numbered clickable links: `[1]`, `[2]`, `[3]`, etc. The numbering is automatic based on array order.

**Format: Array of URL strings**
```json
{
  "citations": [
    "https://example.com/article1",
    "https://example.com/article2"
  ]
}
```

> **Note:** Citations always appear as `[1]`, `[2]`, `[3]` regardless of URL content. Each number is a clickable link to the corresponding URL.

### Streaming Response Pattern

Your service should follow this behavioral pattern:

#### 1. Thinking Phase
- Start with `is_intermediate: true`
- Populate `intermediate_steps` with research process
- Set `final_report: null`
- Set `complete: false`

#### 2. Answer Generation Phase  
- Switch to `is_intermediate: false`
- Start populating `final_report` with answer content
- Keep accumulated `intermediate_steps`
- Set `complete: false`

#### 3. Completion
- Send final message with `complete: true`
- Include final complete answer in `final_report`
- Include citations if available

### Error Handling

If your service encounters an error, send an error response and stop the stream:

```json
{
  "error": "Description of what went wrong",
  "complete": true
}
```

### Docker Requirements

Your service must be containerized for deployment. Create a `Dockerfile` in your service directory.

A basic Dockerfile template is listed below:

```dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 8000

CMD ["gunicorn", "main:app", "-w", "4", "-k", "uvicorn.workers.UvicornWorker", "--bind", "0.0.0.0:8000"]
```

### Testing Your Implementation

You can test your service independently by sending POST requests to `/run`:

```bash
curl -X POST "http://your-service-url/run" \
  -H "Content-Type: application/json" \
  -d '{"question": "Test question"}'
```

Verify that:
- Response streams in the correct SSE format
- All required fields are present
- `is_intermediate` transitions from `true` to `false`
- Final message has `complete: true`
- Intermediate steps use `|||---|||` separators

### Notes

- The main application handles user session management, database logging, and frontend integration
- Your service only needs to focus on generating high-quality research responses
- Response times should be reasonable (typically under 2 minutes for complex queries)
- The system supports both streaming and non-streaming implementations, but streaming is preferred for better user experience

---

## ClueWeb Search API Documentation

**Base URL:**  
`https://clueweb22.us/search`

### Authentication

All requests **must** include a valid API key in the header:

```text
x-api-key: <YOUR_RETRIEVER_API_KEY>
```

> **Note:** Your API key will be assigned to you after sign up for the competition.

### HTTP Request

```http
GET https://clueweb22.us/search
```

### Query Parameters

| Name   | Type    | Required | Description                    |
|--------|---------|----------|--------------------------------|
| query  | string  | yes      | The search query string.       |
| k      | integer | yes      | Number of documents to return. |

### Response

The API returns a JSON object with a single field:

- `results` – an array of Base64-encoded JSON strings, each representing one document.

Each decoded document JSON has at least the following fields:

| Field | Type   | Description                                 |
|-------|--------|---------------------------------------------|
| text  | string | The full text of the retrieved document.    |
| url   | string | The original URL of the document.           |

### Example Code (Python)

```python
import base64
import json
import requests

RETRIEVER_URL     = "https://clueweb22.us/search"
RETRIEVER_API_KEY = "YOUR_API_KEY_HERE"

def query_clueweb(query: str, k: int):
    """
    Query the ClueWeb Search API and return a list of documents.
    Each document is a dict with 'text' and 'url' keys.
    """
    headers = {
        'x-api-key': RETRIEVER_API_KEY
    }
    params = {
        "query": query,
        "k": k
    }

    response = requests.get(RETRIEVER_URL, params=params, headers=headers)
    if response.status_code != 200:
        raise Exception(f"Error querying ClueWeb: {response.status_code}")

    json_data = response.json()
    raw_results = json_data.get("results", [])

    documents = []
    for encoded_doc in raw_results:
        # decode the Base64-encoded JSON string
        decoded_json = base64.b64decode(encoded_doc).decode("utf-8")
        doc = json.loads(decoded_json)

        documents.append({
            "text": doc.get("text", ""),
            "url":  doc.get("url", "")
        })

    return documents

# Usage example
if __name__ == "__main__":
    docs = query_clueweb("open source search engines", 5)
    for i, d in enumerate(docs, 1):
        print(f"Document {i} URL: {d['url']}")
        print(f"Excerpt: {d['text'][:200]}…\n")
```

---

## Implementing Retrieval and Generation Components

We've provided a modular starter code template to help you build your RAG system efficiently. The codebase is structured with separate components for each stage of the pipeline, making it easy to experiment and iterate.

### Starter Code Repository

**GitHub Repository:** [https://github.com/AGI-LTI/MMU-RAG-Starter](https://github.com/AGI-LTI/MMU-RAG-Starter)

The starter code provides a complete RAG pipeline framework with the following modular components:

### Core Pipeline Components

1. **Pipeline Orchestrator** (`pipeline.py`)
   - Main entry point that coordinates all RAG components
   - Handles configuration loading and pipeline execution flow
   - Manages the complete query-to-answer workflow

2. **Data Processing Pipeline**
   - **Loader** (`loader.py`): Load documents from various file formats
   - **Cleaner** (`cleaner.py`): Preprocess and normalize text content
   - **Tokenizer** (`tokenizer.py`): Convert text to tokens using HuggingFace models
   - **Chunker** (`chunker.py`): Split documents into overlapping chunks
   - **Indexer** (`indexer.py`): Build FAISS vector index for semantic search

3. **Query Processing Components**
   - **Retriever** (`retriever.py`): Search the index and retrieve relevant chunks
   - **Generator** (`generator.py`): Generate answers using retrieved context

### Configuration-Driven Approach

The system uses a YAML configuration file (`config.yaml`) to manage:
- Data directories and file paths
- Chunk size and overlap parameters  
- Model selections for embedding and generation
- Retrieval parameters (top-k, etc.)

### Getting Started

1. Clone the starter repository: [https://github.com/AGI-LTI/MMU-RAG-Starter](https://github.com/AGI-LTI/MMU-RAG-Starter)
2. Review the modular component structure in the `/src` folder
3. Implement the TODO sections in each component based on your approach
4. Test with local data
5. Adapt the pipeline for your specific RAG strategy

The modular design allows you to focus on the components most critical to your approach while providing a solid foundation for the complete RAG system.

</div>
</details>
---

<details>
<summary><h2 style="display: inline;">B Text-to-Video</h2></summary>

<div markdown="1">

Once a team is registered the organizers will contact you on their registered email (preferably gmail) and will be assigning the following items.

1. Team ID
2. ECR Repository ARN
3. AWS ECR access keys
4. S3 bucket name and region    

## Text-to-Video Track: Starter Code

The starter code for the text-to-video track can be found in the following file: [https://github.com/AGI-LTI/MMU-RAG-Starter/blob/main/Text-to-Video/submission_starter_video.py](https://github.com/AGI-LTI/MMU-RAG-Starter/blob/main/Text-to-Video/submission_starter_video.py)


Participants are expected to complete two functions one for the retriever and one for the generator as mentioned in the starter code.

The main backend has a single endpoint `generate-video` that returns the s3-Storage URI, the region of the s3 bucket (where the video is stored) and retrieved docs along with some metadata like the status and the error message.

### Retriever

```python
class RetrieverResponse(TypedDict):
    status: str
    error: Optional[str]
    retrieved_docs: List[str]
def retriever(question) -> RetrieverResponse: 
        """
        To be filled by the Partipant. Ideally we expect the participant to retrive top -k documents with each document of String datatype
        as an element of the list.
        Returns a dictionary 
            {
                "status" : "error" OR "success
                "error" : "No documents retrieved" OR None
                "retrieved_docs"  : [] or retrieved document
            }
        """
        return {
            "status" : "success",
            "error" : None,
            "retrieved_docs" : ["document1", "document2", "document3"]
        }
```

Participants can use any retriever implementation but they are expected to return a dictionary of the following keys:

| Key | Type | Description |
|-------|------|-------------|
| Status | string | Must be either "error" or "success" |
| Error | string/None | An appropriate error message for debugging, or None if there is no error |
| Retrieved_docs | list | A Python list containing text from the retrieved documents, or an empty list if retrieval was unsucessfull |

### Generator

```python
class GeneratorResponse(TypedDict):
     status : str
     error : Optional[str]
     s3_BUCKET_NAME : str
     region : str
     Storage_Uri : Optional[str]
def generator(retrieved_docs, question) -> GeneratorResponse:
        """
        To be filled by the Partipant. We expect the particpants to generate a video and store it in an s3 bucket.
        Returns a dictionary
            {
                "status" : "error" OR "success
                "error" : Appropraite error message for any intermediate steps  OR None
                "s3_BUCKET_NAME" :  The s3 bucket assigned to the team, this will be used an integrity check in the main backend
                "Storage_Uri" : The storage Uri of the generated video in the assigned s3 bucket or None
            }
        """
        return {
            "status" : "success",
            "error" : None,
            "s3_BUCKET_NAME" : "ragarena-videos",
            "region" : "us-east-1",
            "Storage_Uri" : "https://ragarena-videos.s3.amazonaws.com/video.mp4"
        }
```

The generator function should return a dictionary with the following keys:


| Key | Type | Description |
|-------|------|-------------|
| status | string | Must be either "error" or "success" |
| Error | string/None | An appropriate error message for debugging, or None if there is no error |
| s3_BUCKET_NAME | string | The name of the s3 bucket assigned to the team |
| region | string | AWS region of the s3 bucket |
| Storage Uri | string | The s3 Object id under which the output video is stored |


The generator or the text-to-video model can be either an open-source text-to-video model or an API call. The core backend infrastructure of the Arena platform expects the generated video to be stored in a dedicated S3 Bucket (that is assigned to participants on registration) and expects the generated output to be named "output.mp4". 

![S3 Bucket with Video Output](/assets/img/submission/s3.png)


</div>
</details>

---

