---
layout: page
title: Getting Started
permalink: /getting-started/
---

Welcome to the MMU-RAG competition!

## Registration

To participate in any track, you **must register your team** by filling out this short form: [Register Your Team](https://forms.gle/YDEnjV4PXWnZdfYG8)

- There’s **no limit** to the number of team members.

- Every team must designate **one team leader** to serve as the main point of contact with the organizers.

  

MMU-RAG features two exciting tracks:

#### **Track A: Text-to-Text (T2T)**

Build a retrieval-augmented system that answers complex, open-ended user queries using textual sources.
 ⟶ Click [here](/MMU-RAGent/text-to-text) to view T2T track details 

#### **Track B: Text-to-Video (T2V)**

Develop a system that can ground open-ended queries in retrieved video content, generating coherent video-based responses.
 ⟶ Click [here](/MMU-RAGent/text-to-video) to view T2V track details 



Both tracks offer:

- A **validation set** for local testing.

- A **starter codebase** to speed up development.

- API access to ClueWeb-22 and FineWeb

- Support for **static** and **dynamic** submission modes.

### FineWeb Search API

```
GET https://clueweb22.us/fineweb/search
```

**Description:** This endpoint is for the FineWeb dataset. You may use FineWeb without an API key for temporary testing while awaiting ClueWeb API key approval.

**Parameters:**
- `query` (string): The search query
- `k` (integer): The number of documents to return

**Response Format:**

```json
{
  "results": [Base64-encoded JSON documents]
}
```

**Example Code (Python):**
```python
import requests
import base64
import json

def query_fineweb(query, num_docs):
    request_url = f"https://clueweb22.us/fineweb/search?query={query}&k={num_docs}"
    
    response = requests.get(request_url)
    
    if response.status_code != 200:
        raise Exception(f"Error querying FineWeb: {response.status_code}")
    
    json_data = response.json()

    results = json_data.get("results", [])
    for returned_document in results:
        # Assuming each document in 'results' is a base64 encoded JSON string
        decoded_result = base64.b64decode(returned_document).decode("utf-8")
        parsed_result = json.loads(decoded_result)
        
        text = parsed_result["text"]
        url = parsed_result["url"]
```


  

## ClueWeb-22 Search API Access

**Base URL:**`https://clueweb22.us/search`



#### Authentication

All requests must include an API key:

```
x-api-key: <YOUR_RETRIEVER_API_KEY>
```

> Your API key will be sent to you after your ClueWeb application is approved.

------



### HTTP Request

```
GET https://clueweb22.us/search
```

**Query Parameters:**

| Name  | Type    | Required | Description                   |
| ----- | ------- | -------- | ----------------------------- |
| query | string  | yes      | The search query string       |
| k     | integer | yes      | Number of documents to return |
| cw22_a | boolean | no      | Use ClueWeb22-A instead of default ClueWeb22-B |

> **Note:** The ClueWeb search API uses ClueWeb22-B by default, but it also supports ClueWeb22-A. To use ClueWeb22-A, add the parameter `cw22_a=True` to your query.
>
> **Examples:**
> - ClueWeb22-B (default): `https://clueweb22.us/search?query=cmu&k=1`
> - ClueWeb22-A: `https://clueweb22.us/search?query=cmu&k=1&cw22_a=True`

**Response Format:**

```json
{
  "results": [Base64-encoded JSON documents]
}
```

Each decoded document will contain:

| Field | Type   | Description                |
| ----- | ------ | -------------------------- |
| text  | string | Full text of the document  |
| url   | string | Source URL of the document |



------

### Example Code (Python)

```python
import base64
import json
import requests

RETRIEVER_URL     = "https://clueweb22.us/search"
RETRIEVER_API_KEY = "YOUR_API_KEY_HERE"

def query_clueweb(query: str, k: int, use_cw22_a: bool = False):
    """
    Query the ClueWeb Search API and return a list of documents.
    Each document is a dict with 'text' and 'url' keys.
    
    Args:
        query: The search query string
        k: Number of documents to return
        use_cw22_a: If True, use ClueWeb22-A instead of default ClueWeb22-B
    """
    headers = {
        'x-api-key': RETRIEVER_API_KEY
    }
    params = {
        "query": query,
        "k": k
    }
    
    if use_cw22_a:
        params["cw22_a"] = True

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
    # Search using ClueWeb22-B (default)
    docs = query_clueweb("open source search engines", 5)
    print("ClueWeb22-B Results:")
    for i, d in enumerate(docs, 1):
        print(f"Document {i} URL: {d['url']}")
        print(f"Excerpt: {d['text'][:200]}…\n")
    
    # Search using ClueWeb22-A
    docs_a = query_clueweb("open source search engines", 5, use_cw22_a=True)
    print("\nClueWeb22-A Results:")
    for i, d in enumerate(docs_a, 1):
        print(f"Document {i} URL: {d['url']}")
        print(f"Excerpt: {d['text'][:200]}…\n")
```



## Validation Sets

We provide a **validation set for each track** to help you test your pipeline and debug your submissions.

- The validation data includes input queries and gold reference answers.
- You can use it to verify your system's outputs and test compatibility with the evaluation format.
- Learn more and download the validation sets [here](/MMU-RAGent-Preview/datasets)

