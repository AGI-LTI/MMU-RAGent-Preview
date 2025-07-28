---
layout: page
title: Getting Started
permalink: /getting-started/
---

This page provides everything you need to prepare your system for submission. You'll find:

- **API documentation** with clear examples to help you query our web-scale corpora  
- **Step-by-step instructions** for building your Docker image  
- **Guidance on integrating retrieval and generation components**  
- **A validation tool** to test your submission locally before upload  

Whether you're entering one or both tracks, this page is your starting point for building, testing, and refining your RAG system.

---
# Track A: Text-to-Text

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

---