---
layout: page
title: Text-to-Text Track
permalink: /text-to-text
---

Welcome to the MMU-RAG competition! This page contains everything you might need to start building and submitting your system for the text-to-text track.

------



We provide a modular starter code template to help you build your RAG system efficiently. The codebase is structured with separate components for each stage of the pipeline, making it easy to experiment and iterate.



**Starter Code GitHub Repository:** [https://github.com/AGI-LTI/MMU-RAG-Starter](https://github.com/AGI-LTI/MMU-RAG-Starter)

The starter code provides a complete RAG pipeline framework with the following modular components.



## Core Pipeline Components

1. Pipeline Orchestrator (`pipeline.py`)

   - Main entry point that coordinates all RAG components
   - Handles configuration loading and pipeline execution flow
   - Manages the complete query-to-answer workflow

2. Data Processing Pipeline

   - **Loader** (`loader.py`): Load documents from various file formats
   - **Cleaner** (`cleaner.py`): Preprocess and normalize text content
   - **Tokenizer** (`tokenizer.py`): Convert text to tokens using HuggingFace models
   - **Chunker** (`chunker.py`): Split documents into overlapping chunks
   - **Indexer** (`indexer.py`): Build FAISS vector index for semantic search

3. Query Processing Components

   - **Retriever** (`retriever.py`): Search the index and retrieve relevant chunks

   - **Generator** (`generator.py`): Generate answers using retrieved context

     

**Configuration-Driven Approach**

The system uses a YAML configuration file (`config.yaml`) to manage:

- Data directories and file paths
- Chunk size and overlap parameters
- Model selections for embedding and generation
- Retrieval parameters (top-k, etc.)



## Getting Started

1. Clone the starter repository: https://github.com/AGI-LTI/MMU-RAG-Starter
2. Review the modular component structure in the `/src` folder
3. Implement the TODO sections in each component based on your approach
4. Test with local data
5. Adapt the pipeline for your specific RAG strategy

The modular design allows you to focus on the components most critical to your approach while providing a solid foundation for the complete RAG system.



## Submission Requirements and Formats

We require your submission to fulfil the following requirements for us to perform static and dynamic evaluation: 

1. Implement a static `/evaluate` endpoint
2. Implement a specific string API
3. Submit a Docker file

Detailed instructions are provided below. 



### 1. Static `/evaluate` endpoint

Your system must implement a static `/evaluate` endpoint that accepts validation queries and returns responses. This will be used to generate a `.jsonl` submission file.

#### Required Endpoint

```
POST /evaluate
Content-Type: application/json
```



#### Request Format

```json
{
  "query": "string",
  "reference": "string",
  "iid": "string"
}
```



#### Response Format

```json
{
  "query_id": "string", // same as iid from the request
  "generated_response": "string" // your system's generated answer
}
```





### 2. Streaming API

Your system must implement a specific streaming API that follows our standardized response format. This is for us to integrate your system into our RAG-Arena live evaluation system. 



#### Required Endpoint

Your service must expose the following endpoint:

```
POST /run
```



#### Request Format

**Content-Type:** `application/json`



#### Request Body

```
{
  "question": "string"
}
```

- `question` (required, string): The research question/query from the user



##### Example Request

```
{
  "question": "What are the latest developments in quantum computing?"
}
```



#### Response Format

* **Content-Type:** `text/event-stream` (preferred) or `text/plain`

* **Response Structure:** Server-Sent Events (SSE) format where each line starts with `data: `followed by JSON:

```
data: {"intermediate_steps": "...", "final_report": "...", "is_intermediate": true, "complete": false}
data: {"intermediate_steps": "...", "final_report": "...", "is_intermediate": false, "complete": true}
```



#### Required JSON Response Fields

Each JSON object in the stream must contain these fields:

##### Core Fields (Required)

| Field                | Type           | Description                                                  |
| :------------------- | :------------- | :----------------------------------------------------------- |
| `intermediate_steps` | string \| null | The thinking/reasoning process, search queries, retrieved information, etc. Use `"|||---|||"` as separator between steps |
| `final_report`       | string \| null | The final answer content being generated                     |
| `is_intermediate`    | boolean        | `true` when showing thinking process, `false` when generating final answer |
| `complete`           | boolean        | `true` on the final message to signal completion             |

##### Optional Fields

| Field            | Type   | Description                                                  |
| :--------------- | :----- | :----------------------------------------------------------- |
| `citations`      | array  | List of citation objects (see format below)                  |
| `citation_texts` | array  | List of citation texts (required if `citations`) is used. This should include the raw text strings that your model used for its citation. |
| `error`          | string | Error message if something goes wrong (stops the stream)     |

##### Citation Format (Optional)

Citation is optional. Citations are displayed in the frontend as numbered clickable links: `[1]`, `[2]`, `[3]`, etc. The numbering is automatic based on array order.

**Format: Array of URL strings**

```
{
  "citations": [
    "https://example.com/article1",
    "https://example.com/article2"
  ], 
  
  "citation_texts": [
  	"This is the informational text used in the first citation.", 
  	"This is the informational text used in the second citation."
  ]
  
}

```

> **Note:** Citations always appear as `[1]`, `[2]`, `[3]` regardless of URL content. Each number is a clickable link to the corresponding URL.



#### Streaming Response Pattern

Your service should follow this behavioral pattern:

##### 1. Thinking Phase

- Start with `is_intermediate: true`
- Populate `intermediate_steps` with research process
- Set `final_report: null`
- Set `complete: false`

##### 2. Answer Generation Phase

- Switch to `is_intermediate: false`
- Start populating `final_report` with answer content
- Keep accumulated `intermediate_steps`
- Set `complete: false`

##### 3. Completion

- Send final message with `complete: true`
- Include final complete answer in `final_report`
- Include citations if available



#### Error Handling

If your service encounters an error, send an error response and stop the stream:

```
{
  "error": "Description of what went wrong",
  "complete": true
}
```



### 3. Dockerizing Your System

Your service must be containerized for deployment. Create a `Dockerfile` in your service directory.

A basic Dockerfile template is listed below:

```
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 8000

CMD ["gunicorn", "main:app", "-w", "4", "-k", "uvicorn.workers.UvicornWorker", "--bind", "0.0.0.0:8000"]
```

#### Testing Your Implementation

You can test your service independently by sending POST requests to `/run`:

```
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



#### Notes

- The main application handles user session management, database logging, and frontend integration
- Your service only needs to focus on generating high-quality research responses
- Response times should be reasonable (typically under 2 minutes for complex queries)
- The system supports both streaming and non-streaming implementations, but streaming is preferred for better user experience