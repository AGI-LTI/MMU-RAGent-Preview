---
layout: page
title: Frequently Asked Questions
permalink: /faq/
---


##### 1. Are closed-source models allowed for processes other than the generation? For example, can we use GPT-5 for reranking retrieved contexts?

Yes. Closed-source models (e.g., GPT-5) may be used for tasks such as reranking or query rewriting. However, their use will make the system be explicitly labeled as "closed-source".

##### 2. Can we host and call our own trained models (e.g., for query rewriting) from external servers during the deep research loop?

Yes. You may host and invoke your own models from external servers as part of the deep research loop. However, note that if you are submitting your system / docker, you will need to make sure we have the right access to those external servers as well.

##### 3. For the generator, is fine-tuning GPT permitted?

Yes, fine-tuning GPT is permitted as long as the use of fine-tuning is transparently reported. Please note that as long as the weight is not released, the system will still be categorized as using a closed-source generator.

##### 4. Can you fine-tune closed-source models?

Yes. Fine-tuning closed-source models is allowed, but if you do so, your system will be classified as using closed-source models.

##### 5. Is there any limitation on generator model size (number of parameters)?

**If you are self-hosting:** There is no restriction on model size.

**If you are submitting the model to be hosted by us:** The entire system must fit within a Docker image no larger than 20 GB and must run on a single GPU with approximately 24 GB memory.

##### 6. Are there efficiency constraints, such as timeouts for the research loop?

Yes. There is a hard limit of 10 minutes for the entire process.

##### 7. How will the offline evaluation process be conducted?

###### What criteria or metrics will be applied?

**For Text-to-Text systems:**

- **Automatic metrics:** ROUGE-L, BERTScore.
- **LLM-as-a-Judge & Human Likert Ratings:** Assess semantic similarity, coverage (completeness), factuality (accuracy and grounding), and citation quality (precision and recall of cited evidence).

**For Text-to-Video systems:**

- **Video quality metrics:** Standard automatic metrics from VBench (e.g., subject/background consistency, motion smoothness, dynamic degree, aesthetic quality, imaging quality).
- **Utility-focused metrics (via LLM-as-a-Judge and human Likert ratings):** Relevance, precision, recall, and usefulness of the generated video in addressing the query.

For detailed info, please visit our [getting started page](/MMU-RAGent-Preview/getting-started/).

##### 8. How will the online evaluation be carried out?

###### Will users submit their own questions, which will then be shown across multiple systems for ranking or preference selection?

All systems will be deployed in the RAG-Arena platform (similar to Chatbot Arena). For each interaction, two systems are randomly selected and presented side by side. The user provides a question or prompt, and after both systems respond, the user votes for their preferred output without knowing the system identities.

###### Will there be any assessment of answer faithfulness (i.e., grounding in the reference documents), or is the task limited to presenting a final answer/snippet for human judges?

Yes, faithfulness is explicitly assessed. Evaluation includes factuality checks and citation quality metrics, ensuring that responses are not only fluent but also grounded in retrieved documents or reference material.

###### Will responses be displayed as they are generated (meaning faster systems are shown before slower ones)?

Yes. Responses are streamed as they are generated, so faster systems will surface results sooner. Voting is enabled only after both systems have completed their outputs. A hard limit of 10 minutes applies to the entire process.

##### 9. When can we expect to receive API access to the search system?

To obtain ClueWeb22 API access, you must complete the request process described on the [official ClueWeb22 website](https://lemurproject.org/clueweb22/obtain.php). Submit the required forms by email to the ClueWeb team and cc DeepResearch Gym (deepresearchgym@cmu.edu). Approval may take 2–4 weeks.

While waiting for ClueWeb22 access, you may use the FineWeb API without an authentication key. Documentation and implementation details are available on each track's info page: [Text-to-Text](/MMU-RAGent-Preview/text-to-text/) and [Text-to-Video](/MMU-RAGent-Preview/text-to-video/).

##### 10. What are the exact hardware configurations used to deploy the Docker image?

###### If a specific AWS instance is used, could you please share the instance type?

If hosting is required, the environment will be provisioned on an AWS instance equipped with a GPU with approximately 24 GB of memory. We are planning to use **g5.4xlarge**. If GPU resources are not required, we will use an equivalent CPU-only configuration, **m7i.4xlarge**.

##### 11. Could you provide sample questions from the RAG-Arena so we better understand the expected query types?

RAG-Arena questions are generated directly by users. They may ask any question of their choice; there is no predefined set of queries.