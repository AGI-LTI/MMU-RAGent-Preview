---
layout: page
title: Datasets
permalink: /datasets/
---

To support the development and debugging of your models, we are releasing a **validation set for each track** of the MMU-RAG competition:

- [**Track 1: Text-to-Text**](https://drive.google.com/drive/u/0/folders/1cVIuEJsz2Pow-USGh1cnFrcn98KGypS4)
- [**Track 2: Text-to-Video**](https://drive.google.com/file/d/1-a7VaGGMrzxqTI1rCrQTiB_lqBjLOWcv/view?usp=share_link)

Each validation set consists of a small number of example queries and their gold text references. These sets are meant to help teams test their pipelines and ensure compatibility with our evaluation format before final submission.

Note: Validation sets are *not* used in the final evaluation and are safe to use for model tuning and format debugging.

The links above should direct you to a page where can directly download these queries. 

Each line in the validation set is a JSON object with the following fields:

- **`query`** *(string)*: The user’s information-seeking question.
- **`reference`** *(string)*: A gold reference response that accurately and completely answers the query. This can be used for tuning or evaluation with automatic metrics such as ROUGE or BERTScore.
- **`iid`** *(string)*: A unique instance identifier. This can be used to track and match outputs in your system.

---