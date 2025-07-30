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
 ⟶ Click [here](/MMU-RAGent-Preview/text-to-text) to view T2T track details 

#### **Track B: Text-to-Video (T2V)**

Develop a system that can ground open-ended queries in retrieved video content, generating coherent video-based responses.
 ⟶ Click [here](/MMU-RAGent-Preview/text-to-video) to view T2V track details 



Both tracks offer:

- A **validation set** for local testing.

- A **starter codebase** to speed up development.

- API access to ClueWeb-22A

- Support for **static** and **dynamic** submission modes.

  

## ClueWeb-22A Search API Access

**Base URL:**`https://clueweb22.us/search`



#### Authentication

All requests must include an API key:

```
x-api-key: <YOUR_RETRIEVER_API_KEY>
```

> Your API key will be emailed to you after your team registers.

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



## Validation Sets

We provide a **validation set for each track** to help you test your pipeline and debug your submissions.

- The validation data includes input queries and gold reference answers.
- You can use it to verify your system's outputs and test compatibility with the evaluation format.
- Learn more and download the validation sets [here](/MMU-RAGent-Preview/datasets)

