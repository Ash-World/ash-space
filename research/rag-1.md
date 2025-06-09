---
description: 2025.03.29.
---

# RAG 평가지표 정복하기

<figure><img src="../.gitbook/assets/goto_eval.png" alt=""><figcaption><p>RAG 평가지표 정복하기</p></figcaption></figure>

## 0. Overview

사실 예정되어있던 포스트는 아니었고 RAG 성능평가에 대한 고민이 많던 요즘 총정리를 한번 해보자는 생각으로 작성하게 되었다. 널리 알려진 평가 metric부터 개인적으로 생각하는 부분까지 이 한번의 포스트로 끝내보자. 평가 metric은 크게 아래의 3가지 카테고리로 정리했다.

* **Data** : Chunking에 대한 평가지표
* **Retrieval** : Ranking을 고려했을 때와 그렇지 않았을 때의 Retrieval 평가지표
* **Generation** : 공통된 기준이나 Domain / Scenario에 따른 Generation 평가지표

## 1. Data

### (1). Chunking ([post](chunking.md))

* [IoU](chroma-chunking.md)
* Retrieval Metric

## 2. Retrieval

### (1). Rank Unaware Metric

* Precision
* Recall
* F1 Score

### (2). Rank Aware Metric

* MRR
* MAP
* nDCG

## 3. Generation

### (1). Task specific metric

* QA task
  * G-Eval (e.g. Answer Relevancy, Coherence, Correctness, Fluency, Faithfulness, etc )
* Summerization task
  * Sem Score, ROUGE
* Translation task
  * BLEU, BERT Score
* Conversation task
  * G-Eval (e.g. Answer Relevancy, Coherence, Correctness, Fluency, Faithfulness, etc )
  * Knowledge Retention
* Safety(=SafeGuard) task
  * G-Eval (e.g. Toxicity, Bias, Harmfulness, etc )

### (2). Domain specific / Scenario based metric

* Design a custom metric that reflects domain-specific characteristics. The evaluation method is generally as follows:
  * MCQA (=Multiple Choices Question Answering)
  * G-Eval
