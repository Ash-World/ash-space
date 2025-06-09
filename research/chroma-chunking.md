---
description: 2025.02.23.
---

# Chroma야, Chunking 평가를 어떻게 한다고?

<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption><p>Chroma Technical Report</p></figcaption></figure>

> 본 아티클은 [Chroma Techincal Report](https://research.trychroma.com/evaluating-chunking)를 번역한 것이다. 원문링크를 참조하여 보아도 좋다.



* [Brandon Smith](https://x.com/@brandonstarxel) : Researcher in Residence - Chroma
* [Anton Troynikov](https://x.com/atroyn) : Cofounder - Chroma



문서 분할이 전처리 단계로 사실상 보편적으로 사용되고 있음에도 불구하고, 그 영향이 검색 성능에 미치는 효과를 조사한 연구는 거의 이루어지지 않았다. 이는 주로 전체 문서 검색 작업을 대상으로 하는 일반적인 정보 검색 벤치마크의 구조 때문이라고 할 수 있다. 본 연구에서는 토큰 수준의 관련성을 고려한 평가 방식을 제시하며, 이를 통해 여러 인기 있는 Chunking Strategies을 평가할 수 있도록 하였다. 연구 결과, Chunking Strategy의 선택이 검색 성능에 상당한 영향을 미칠 수 있으며, 일부 Chunking Strategy은 다른 것보다 Recall에서 최대 9%까지 더 우수한 성능을 보이는 것으로 나타났다.

<figure><img src="../.gitbook/assets/image (16).png" alt=""><figcaption><p>다양한 인기 있는 Chunking Strategy와 함께 새롭게 제안하는 (★) 전략들을 평가하였다. <br>그 결과, chunking strategy의 선택이 정확도와 효율성 측면에서 검색 성능에 큰 영향을 미칠 수 있음을 확인하였다. 여기서 “Size”는 토큰 단위의 chunk size를 의미하며, 괄호 안의 값은 chunking strategy에 따라 달라질 수 있는 평균 chunk size를 나타낸다. “Overlap”은 토큰 단위의 chunk overlap을 의미한다. 굵은 글씨로 표시된 값은 각 범주에서 가장 우수한 성능을 나타낸다. 각 평가 지표에 대한 자세한 내용은 지표 섹션을 참고하라.</p></figcaption></figure>

문서를 AI 애플리케이션의 검색 대상으로 활용할 때 전처리 단계로 흔히 chunking이 사용된다. chunking은 문서를 정보 단위로 나누어, 임베딩 기반 검색 및 LLM(대형 언어 모델)에 의한 처리가 가능한 의미 있는 내용으로 구성하는 역할을 한다. <mark style="background-color:orange;">**본 기술 보고서의 목적은 AI 애플리케이션 환경에서 문서 분할 및 검색이 어떻게 사용되는지를 대표하는 방식으로, 문서 분할 전략 선택이 검색 성능에 미치는 영향을 평가**</mark>하는 것이다.

최근 LLM의 context length가 늘어나 전체 문서나 텍스트 코퍼스를 context window에 삽입하는 것이 가능해졌지만, 실제로는 이러한 방식이 비효율적이며 모델의 주의를 분산시킬 수 있다. 주어진 query에 대해 텍스트 코퍼스의 일부만이 관련 있을 가능성이 높지만, context window에 있는 모든 토큰이 각 추론 시 처리된다. 이상적으로는 각 query마다 LLM이 관련 있는 토큰만 처리하면 되며, 따라서 AI 애플리케이션의 검색 시스템의 주요 목표 중 하나는 주어진 query에 대해 관련 있는 토큰만을 식별하고 검색하는 것이다.&#x20;

일반적으로 사용되는 벤치마크, 예를 들어 MTEB와 같은 경우 전통적인 정보 검색(IR) 접근 방식을 취하여, 검색 성능을 문서 전체의 관련성을 기준으로 평가합니다. 이로 인해 문단이나 토큰 단위의 chunking을 반영할 수 없습니다.

<figure><img src="../.gitbook/assets/image (17).png" alt=""><figcaption><p>AI 응용 분야에서는 query와 관련된 모든 토큰을 포함하는 발췌문이 여러 문서 내 또는 여러 문서에 걸쳐 존재할 수 있다. chunk는 관련 토큰과 무관한 토큰을 모두 포함할 수 있으며, 관련 발췌문이 여러 chunk에 걸쳐 분산될 수도 있다.</p></figcaption></figure>

전통적인 IR 벤치마크는 또한 검색된 문서들의 상대적 순위에 중점을 두지만, 실제로 LLM은 context length 내에서 관련 정보의 위치에 대해 상대적으로 둔감합니다. 게다가, 특정 query에 해당하는 관련 정보가 여러 문서에 걸쳐 분포할 수 있어, 문서 간 상대 순위 평가가 모호해집니다.

이러한 한계점에 착안하여, 우리는 토큰 단위에서 검색 성능을 평가하는 새로운 평가 전략을 제안합니다. \
본 평가 방식은 LLM을 사용해 주어진 텍스트 말뭉치에서 쿼리와 그에 해당하는 관련 발췌문들을 생성 및 필터링한 후, 검색된 토큰을 기반으로 precision, recall, 그리고 Intersection-over-union(w/ Jaccard Index)을 통해 검색 성능을 평가합니다.

<figure><img src="../.gitbook/assets/image (18).png" alt=""><figcaption><p>우리의 evaluation은 검색 정확도와 효율성을 토큰 단위로 세밀하게 분석하는 접근 방식을 취하여, chunking과 같은 구성 요소를 반영할 수 있습니다. AI 응용 프로그램에서는 관련 토큰이 존재하는지가, 그들의 상대적 순위보다 더 중요하게 작용합니다.</p></figcaption></figure>

우리는 다양한 Domain에서 데이터의 품질이 서로 다른 구체적인 데이터셋을 생성하여, 이를 통해 여러 인기 있는 chunking strategy의 성능을 평가하였다. 평가 결과, 분할 전략의 선택이 검색 정확도와 효율성에 상당한 영향을 미친다는 사실을 확인하였다. 특히, 일부 인기 있는 분할 전략의 기본 설정이 상대적으로 낮은 성능을 초래할 수 있음을 발견하였다. 또한, 대표적인 RecursiveCharacterTextSplitter와 같은 휴리스틱 분할 전략은 적절한 매개변수 설정 하에서 실제로 우수한 성능을 보이는 경우가 많았다.

마지막으로, 두 가지 새로운 분할 전략을 제안하고 평가하였다. 첫 번째는 **ClusterSemanticChunker**로, 임베딩 모델을 직접 활용하여 의미적 유사성을 바탕으로 주어진 크기까지 문서 조각을 구성하는 방식이며, 두 번째는 **LLMChunker**로, LLM에게 직접 텍스트 코퍼스의 분할 작업을 수행하도록 요청하는 방식이다. 평가 결과, 두 방법 모두 경쟁력 있는 결과를 보여주었다.

또한, 우리의 결과를 재현할 수 있도록 모든 코드를 [GitHub 저장소](https://github.com/brandonstarxel/chunking_evaluation)로 제공하며, 유사한 실험을 위한 유용한 도구들도 함께 제공하였다. 이 초기 연구가 AI 응용 분야의 검색 시스템 실제 성능에 영향을 미치는 요인들에 대한 추가 연구를 촉진하기를 기대한다.

***

## Ⅰ. Introduction

질문에 답하거나 텍스트를 생성하는 것 외에도, 최근에는 대형 언어 모델(LLM)이 새로운 컴퓨팅 기본 요소로 등장하여 비정형 정보를 “상식”적인 방식으로 처리할 수 있게 되었고, 이로 인해 다양한 AI 응용 프로그램들이 만들어지고 있다. AI 응용 프로그램의 핵심 요소 중 하나인 소위 Retrieval-Augmented Generation (RAG)은 현재 작업과 의미론적으로 관련 있는 외부 데이터를 검색하여 처리하는 방식을 채택한다. 전통적인 정보 검색(IR)과 달리, AI 응용 프로그램을 위한 검색은 가장 관련성 높은 정보를 LLM에 제공하기 위해 종종 여러 문서에 걸친 문서 일부(chunk)를 저장하고 검색하는 방식에 의존한다. 따라서 AI 응용 프로그램 사용 사례에서는 반환되는 문서뿐만 아니라 어떤 토큰과 chunk가 반환되는지가 매우 중요한 요소로 작용한다.

자연어 처리 분야에서 정보 검색(IR) 전반, 특히 passage retrieval은 오랜 기간 연구되어온 문제로 상당한 문헌이 축적되어 있다. 그러나 Massive Text Embedding Benchmark (MTEB)과 MSMARCO와 같은 벤치마크는 토큰 효율성이나 chunking을 고려하지 않고 보통 검색된 전체 문서의 관련성을 기준으로 평가한다. 또한 정보 검색에서 널리 사용되는 nDCG@K 지표는, 검색된 문서의 순위가 덜 중요한 RAG환경에서는 그 유용성이 떨어진다. 연구 결과에 따르면, 관련 토큰들이 존재하고 전체 문맥이 적당한 길이라면 대형 언어 모델(LLM)은 토큰의 위치에 상관없이 정보를 정확하게 처리할 수 있음이 밝혀졌다.

이러한 단점을 보완하기 위해, 우리는 AI 응용 프로그램의 맥락에서 검색 성능의 본질적인 세부 사항을 포착할 수 있는 새로운 평가 방식을 제안합니다. 이 평가는 생성형 데이터셋과 새로운 성능 측정 지표로 구성됩니다. 데이터셋을 생성하는 파이프라인을 자세히 설명하여, 다른 사용자들도 자신들의 데이터와 사용 사례에 맞는 도메인 특화 평가를 생성할 수 있도록 했습니다. 데이터셋이 생성되었기 때문에, 일반적으로 이는 어떠한 범용 임베딩 모델의 학습 세트에도 포함되지 않아야 합니다. 또한, 우리는 토큰 수준의 Jaccard 유사도 계수를 기반으로 한 새로운 성능 측정 지표를 도입하였으며, 이를 간단히 **Intersection over Union (IoU)**&#xB77C;고 부릅니다. 이 지표는 문서 단위가 아니라 <mark style="color:orange;">**토큰 단위에서 재현율과 정밀도를 평가**</mark>합니다. <mark style="background-color:orange;">컴퓨터 비전에서와 유사하게 text chunk를 bounding box로 간주하고, IoU를 검색된 chunk의 bounding box와 관련 토큰의 bounding box가 얼마나 겹치는지를 측정하는 척도로 활용할 수 있습니다.</mark>

새로운 평가 전략의 응용으로, RecursiveCharacterTextSplitter와 같은 인기 있는 문서 분할 방법들의 성능을 비교하였습니다. 또한, 검색에 사용되는 임베딩 모델을 고려한 ClusterSemanticChunker와, 텍스트 코퍼스에 대해 LLM에게 직접 프롬프트를 제공하여 분할을 수행하는 LLMChunker라는 두 가지 새로운 분할 방법을 개발하고 평가하였습니다.

결과적으로 다음 내용을 제시합니다:

* AI 애플리케이션의 맥락에서 검색 품질을 평가하기 위한 도메인 특정 데이터셋을 생성하는 프레임워크와, 문서 분할 전략을 고려한 새로운 검색 품질 측정 지표.
* 이 프레임워크를 활용하여 제한된 인기 도메인에 대한 구체적인 평가 데이터셋을 생성한 후, 제안된 측정 지표에 따라 여러 일반적으로 사용되는 문서 분할 전략을 평가함.
* 임베딩 모델을 활용하여 청크를 분할하는 ClusterSemanticChunker와 LLM에게 직접 프롬프트를 제공하여 문서 분할 작업을 수행하는 LLMChunker 등 새로운 분할 전략들을 제시하고 평가함.
* 마지막으로, 이 프로젝트의 전체 코드베이스를 제공합니다.

## Ⅱ. Related Work

현재 인기 있는 정보 검색(IR) 벤치마크로는 Massive Text Embedding Benchmark (MTEB)와 Benchmarking-IR (BEIR)가 있다. MTEB는 58개의 데이터셋과 8개의 작업을 통해 텍스트 임베딩 모델을 평가하며, BEIR는 9개의 검색 작업에 걸쳐 18개의 데이터셋을 포함하고 있다. 또한, MTEB의 텍스트 검색 데이터셋은 모든 공개적으로 이용 가능한 BEIR 검색 데이터셋을 포함한다. 이들의 주요 평가지표는 순위 10에서의 nDCG@10이며, 두 벤치마크 모두 추가로 순위 k에서의 MAP@k, Precision@k 및 Recall@k를 제공한다.

nDCG@K 지표는 상위 K개의 검색 문서에 대해 관련성을 평가하며, 순위가 높은 문서에 더 큰 가중치를 부여하고 순위가 낮은 위치에 나타난 관련 결과에는 패널티를 줍니다. 이와 유사하게, MAP@K는 상위 K개의 검색 문서 각 위치에서의 정밀도의 평균을 계산합니다. 서론에서 언급했듯이, **RAG의 맥락에서는 검색된 문서의 순서가 그다지 중요하지 않습니다.** 또한 기존의 벤치마크는 문서 단위에서 검색 시스템의 성능을 평가하는 반면, AI 애플리케이션에서는 어떤 문서의 일부 토큰만 특정 query와 관련될 수 있으며, 관련 토큰은 전체 corpus에 걸쳐 분포할 수 있습니다. 따라서 우리가 제안하는 평가 지표는 이러한 토큰 수준의 관련성을 고려하는 것을 목표로 합니다.

일반적인 정보 검색과 함께, MS MARCO 데이터셋과 같이 대규모 및 머신러닝 문맥에서 문단 검색에 대한 광범위한 연구가 이루어졌습니다. 그러나 이러한 연구들은 주로 문서 검색을 위한 중간 단계나 검색 및 회수의 일반적인 맥락에서 다루어졌지, AI 응용 분야의 검색이라는 구체적인 관점이나 토큰 효율성을 고려한 것은 아니었습니다.

최근 LLM들은 ARAGOG와 같이 LLM에게 프롬프트를 제공해 이진 분류 방식으로 관련성을 평가함으로써 검색 성능을 직접 평가하는 데 사용되고 있습니다. 또한, LLM은 텍스트 코퍼스로부터 합성 데이터를 생성하거나 RAGAS와 같이 RAG 파이프라인의 최종 생성 결과를 평가하는 데에도 활용되고 있습니다. 반면, 본 연구에서 제안하는 평가는 모델의 특성과 프롬프트에 민감할 수 있는 복잡한 다단계 합성 및 평가 파이프라인 대신, 문서 코퍼스에서 쿼리를 합성하는 제한된 단계를 포함하여 오직 검색에만 초점을 맞춥니다. 우리는 LLM에 의존하지 않고 직접 검색 성능을 평가하며, 원칙적으로 제안된 접근 방식은 다른 평가 방법과 결합되어 보다 완전한 평가를 제공할 수 있습니다.

비록 RAG 파이프라인에서 데이터 수집의 첫 단계로 chunking이 자주 활용되지만, chunking strategy를 평가하는 연구는 드뭅니다. Greg Kamradt의 semantic chunking은 LangChain \[14]에 도입되었고, Aurelio AI도 자체 버전을 개발했습니다. 또한 Unstructured는 최적의 text chunking보다는 문서 내 위치를 기반으로 한 chunk의 메타데이터에 초점을 맞춰 금융 분야에서의 chunking을 탐구했습니다. 이러한 노력들은 RAG에서 chunking evaluation에 대한 관심이 점차 높아지고 있음을 보여주지만, 포괄적인 평가에서는 여전히 큰 격차가 존재하며, 저희 연구는 이를 해결하는 데 목적을 두고 있습니다.

## Ⅲ. Evaluating Retrieval for AI Applications

우리는 AI 응용 분야에서의 검색 평가를 위한 프레임워크를 소개한다. 이 프레임워크는 제공된 문서 코퍼스를 기반으로 한 생성 평가 데이터셋과, 잘 알려진 자카드 유사도(Jaccard similarity coefficient)를 기반으로 하여 문서 분할 전략을 반영하는 토큰 단위의 interseaciton of union(IoU)이라는 새로운 지표로 구성되어 있으며, 이는 검색 시스템의 토큰 효율성을 나타낸다. 우리는 이 지표를 recall과 같은 다른 측정치와 함께 사용하여 AI 응용 분야에서의 검색 시스템 효율성을 평가한다.

이 부분 합성 데이터 생성 방식은 이렇게 생성된 데이터가 일반 목적의 임베딩 모델 학습 시점에 사용되지 않았음을 보장하여 잠재적인 편향을 방지합니다. 또한, 이 방식은 도메인별 평가가 가능해 어떤 데이터셋에서도 검색 품질을 평가하는 데 적합합니다. 마지막으로, 이 방법은 인간 레이블 사용을 포함한 다른 평가 방법과 결합하여 활용될 수 있습니다.

### Ⅲ-1. Dataset Generation

주어진 text corpus(여러 문서들로 구성된)에 대해, 우리는 LLM을 활용하여 해당 문서들과 관련된 query와 생성된 query와 관련 있는 corpus 발췌문을 샘플링합니다. 평가 코퍼스의 문서들, 코퍼스에 관한 사실 기반 쿼리를 생성하라는 지시사항, 그리고 생성된 질문에 해당하는 발췌문을 포함한 프롬프트를 LLM에 제공합니다.

query의 고유성을 보장하기 위해, 프롬프트에 이전에 생성된 쿼리 중 무작위로 최대 50개를 포함합니다. 코퍼스 내에서 전체 텍스트가 일치하는 발췌만 유효한 것으로 인정하며, 하나라도 유효하지 않은 발췌가 있다면 해당 쿼리와 그에 연관된 모든 발췌를 거부합니다.

```
Query: What were the main characteristics of the armor used on the ship
Atlanta? Excerpts:

"Her armor was backed by 3 inches ( 76 mm ) of oak , vertically oriented , 
and two layers of 7 @.@ 5 inches ( 191 mm ) of pine , alternating in direction "

"The upper portion of Atlanta 's hull received two inches of armor "
```

평가를 복잡하게 만들 수 있는 복합 쿼리 (예: “게티스버그 연설의 날짜와 의미는 무엇인가요?”)의 발생을 줄이기 위해, 우리는 LLM에게 연결 접속사 ‘and’의 사용을 피하도록 요청하며, 고유 명사의 일부로 사용되는 경우에만 허용합니다.

더 자세한 prompt를 보고 싶다면 아래를 참고하세요.

_**Appendix 01. Synthetic Dataset Prompt (**_[_**Link**_](https://research.trychroma.com/evaluating-chunking#synthetic-dataset-prompt)_**)**_

```
You are an agent that generates questions from provided text. Your job is to generate a question and provide the relevant sections from the text as references.

 

Instructions:

For each provided text, generate a question that can be answered solely by the facts in the text.
Extract all significant facts that answer the generated question.
Format the response in JSON format with two fields:
'question': A question directly related to these facts, ensuring it can only be answered using the references provided.
'references': A list of all text sections that answer the generated question. These must be exact copies from the original text and should be whole sentences where possible.
 

Notes: Make the question more specific. Do not ask a question about multiple topics. Do not ask a question with over 5 references.

 

Example:

 

Text: "Experiment A: The temperature control test showed that at higher temperatures, the reaction rate increased significantly, resulting in quicker product formation. However, at extremely high temperatures, the reaction yield decreased due to the degradation of reactants.

 

Experiment B: The pH sensitivity test revealed that the reaction is highly dependent on acidity, with optimal results at a pH of 7. Deviating from this pH level in either direction led to a substantial drop in yield.

 

Experiment C: In the enzyme activity assay, it was found that the presence of a specific enzyme accelerated the reaction by a factor of 3. The absence of the enzyme, however, led to a sluggish reaction with an extended completion time.

 

Experiment D: The light exposure trial demonstrated that UV light stimulated the reaction, making it complete in half the time compared to the absence of light. Conversely, prolonged light exposure led to unwanted side reactions that contaminated the final product."

 

Response: { 'oath': "I will not use the word 'and' in the question unless it is part of a proper noun. I will also make sure the question is concise.", 'question': 'What experiments were done in this paper?', 'references': ['Experiment A: The temperature control test showed that at higher temperatures, the reaction rate increased significantly, resulting in quicker product formation.', 'Experiment B: The pH sensitivity test revealed that the reaction is highly dependent on acidity, with optimal results at a pH of 7.', 'Experiment C: In the enzyme activity assay, it was found that the presence of a specific enzyme accelerated the reaction by a factor of 3.', 'Experiment D: The light exposure trial demonstrated that UV light stimulated the reaction, making it complete in half the time compared to the absence of light.'] }

 

DO NOT USE THE WORD 'and' IN THE QUESTION UNLESS IT IS PART OF A PROPER NOUN. YOU MUST INCLUDE THE OATH ABOVE IN YOUR RESPONSE. YOU MUST ALSO NOT REPEAT A QUESTION THAT HAS ALREADY BEEN USED."
```

### Ⅲ-2. Dataset Prefiltering

고품질의 query와 발췌문을 보장하기 위해 초기 생성 단계의 출력 결과를 필터링합니다.

중복 쿼리 생성을 피하라는 프롬프트에도 불구하고, LLM은 때때로 중복되거나 거의 동일한 쿼리를 생성할 수 있습니다. 이를 해결하기 위해, 각 쿼리를 임베딩한 후 모든 쿼리 쌍 간의 코사인 유사도를 계산하여 생성된 데이터셋에서 중복을 제거합니다. 그 후, 코사인 유사도가 특정 임계값을 초과하는 모든 query와 해당 발췌문들을 필터링합니다.

또한, LLM이 가끔 문맥에서 벗어나거나 query와 관련 없는 발췌문을 생성하는 경우가 있음을 확인하였습니다. 이를 완화하기 위해, 각 쿼리와 해당 발췌문들을 임베딩하고, 쿼리에서 각 발췌문까지의 코사인 유사도를 계산합니다. 만약 쿼리와 그에 연결된 발췌문 중 어느 하나라도 유사도가 설정된 임계값보다 낮을 경우, 해당 쿼리와 모든 관련 발췌문을 필터링합니다.

중복 query와 발췌 관련성에 대한 임계값 선택은 데이터셋에 따라 달라집니다. 구체적인 데이터셋 세부 정보에서 사용한 임계값에 대해 자세히 설명하고 있으며, 임베딩 모델로는 OpenAI의 text-embedding-3-large를 사용합니다.

또한, 후자의 임계값은 query와 관련 발췌 사이에 최소한의 코사인 유사도를 설정합니다. 비록 이로 인해 샘플링 편향이 발생할 수 있지만, 각 발췌가 하나 이상의 텍스트 청크의 일부이며, 각 청크는 주어진 코퍼스 내 여러 청크 중 하나라는 점을 감안해야 합니다. 게다가 여러 임베딩 모델을 대상으로 평가를 수행하기 때문에, 실제로 이 편향의 영향은 미미합니다.

또한, 각 query와 관련성이 있도록 생성된 발췌문들을 엄격하게 필터링하지만, query와 관련 있을 수 있으나 생성되지 않은 다른 발췌문들을 코퍼스에서 검색하지는 않는다는 점을 주목합니다. 이는 현재 접근 방식의 한계이며, 향후 연구에서는 supervised이나 semi-supervised 방법을 통해 이를 보완할 수 있을 것입니다.

### Ⅲ-3. Metrics

특정 코퍼스와 관련된 query의 경우, 해당 코퍼스 내에서 유효한 토큰은 일부에 불과합니다. 효율성과 정확성을 위해 이상적으로는, 검색 시스템이 전체 코퍼스에 걸쳐 각 쿼리마다 정확히 필요한 토큰만을 검색해야 합니다.

실제로 AI 응용 프로그램의 검색 단위는 보통 관련 발췌문이 포함된 text chunk입니다. 이 chunk는 종종 추가 연산이 필요한 불필요한 토큰들을 포함하며, 관련 없는 방해 요소들로 인해 RAG 응용 프로그램의 전체 성능이 저하될 수 있습니다. 게다가, chunking strategy에서 chunk가 겹치는 경우, 예를 들어 관련 발췌문의 토큰이 중복되어 여러 chunk에 포함될 수 있기 때문에, 검색기가 중복된 토큰들을 반환할 가능성이 있습니다. 마지막으로, 특정 검색기는 주어진 쿼리에 대해 필요한 모든 발췌문을 검색하지 못할 수도 있습니다.

따라서 우리는 관련 발췌문이 검색되었는지 여부뿐만 아니라, 함께 검색된 불필요하거나 중복되며 방해가 될 수 있는 토큰의 수도 고려할 수 있는 evaluation metric을 찾고자 합니다. 컴퓨터 비전 및 데이터 마이닝 분야에서 유사한 척도로 사용되는 **자카드 유사도**에 영감을 받아, 검색 시스템의 효율성을 평가하기 위해 토큰 단위의 Intersection over Union (IoU) metric을 제안합니다.

주어진 query와 chunking된 코퍼스 $$\mathbf{C}$$ 에 대해 IoU를 다음과 같이 계산합니다. 쿼리 $$\mathbf{q}$$에 대해, 모든 관련 발췌문에서 추출된 토큰들의 집합을 $$t_{e}$$로 나타내며, 검색된 모든 chunk에 포함된 토큰들의 집합을 $$t_{r}$$로 나타냅니다. chunking된 코퍼스에서 주어진 query에 대한 IoU는 다음과 같이 계산됩니다 :&#x20;

$$
\mathrm{IoU_{q}}(\mathbf{C}) = \frac{|t_{e} \cap t_{r}|}{|t_{e}|+|t_{r}|-|t_{e} \cap t_{r}|}
$$

분자에서는 $$t_{e}$$에 속한 각 토큰을 $$t_{r}$$ 내에서 단 한 번만 계산하고, 분모에서는 검색된 모든 토큰($$|t_{r}|$$)을 계산합니다. 이는 중복된 발췌 토큰이 여러 검색된 chunk에 나타나는 경우, 예를 들어 chunk가 겹치는 전략을 사용할 때를 반영합니다.

또한, IoU와 함께 전통적인 정보 검색에서 사용되는 방식과 유사하게 문서 단위가 아닌 토큰 단위에서 precision과 recall 지표를 사용할 것을 제안합니다 :&#x20;

$$
\mathrm{Precision_{q}}(\mathbf{C}) = \frac{|t_{e} \cap t_{r}|}{|t_{r}|}
$$

$$
\mathrm{Recall_{q}}(\mathbf{C}) = \frac{|t_{e} \cap t_{r}|}{|t_{e}|}
$$

평가를 수행할 때, 데이터셋 내 모든 쿼리와 모든 코퍼스에 대해 IoU, Precision, Recall의 평균값을 보고합니다. 또한, 발췌 토큰이 포함된 모든 Chunk가 성공적으로 검색된 경우의 precision를 $$\mathrm{Precision_{Ω}}$$으로 보고하는데, 이는 완벽한 recall을 가정했을 때 토큰 효율성의 상한을 제시합니다.

우리는 F-score (F1)를 다음과 같이 계산할 수도 있음을 주목합니다:

$$
F1 = 2 \times \frac{Precision \times Recall}{Precision + Recall} = 2 \times \frac{|t_{e} \cap t_{r}|}{|t_{e}| + |t_{r}|}
$$

그러나 실제로 IoU는 보다 직관적인 ‘바운딩 박스’ 유추를 유지하면서 유사한 정보를 제공합니다.

## Ⅳ. Chunking Evaluation Dataset

우리의 평가 프레임워크를 사용하여 AI 응용 프로그램의 검색을 위한 문서 분할 전략을 평가할 수 있는 데이터셋을 생성하였습니다.

### Ⅳ-1. Corpora

데이터셋에는 깔끔한 텍스트와 복잡한 텍스트 소스를 모두 포함한 다섯 개의 다양한 코퍼스를 선정했습니다. 깔끔한 코퍼스는 잘 구성된 텍스트를 포함하는 반면, 복잡한 코퍼스는 일반적으로 웹 스크래핑으로 얻은 비구조적인 텍스트의 특성을 반영합니다. 각 코퍼스에 대한 간단한 설명은 아래에 제공되어 있습니다. 각 코퍼스의 길이는 OpenAI의 GPT-4에서 사용하는 표준인 “cl100k\_base”에 맞춰 Tiktoken으로 측정한 토큰 수로 제공됩니다. 모든 Hugging Face 코퍼스는 전체 코퍼스의 하위 집합이며, 제공된 길이는 우리가 사용한 하위 집합을 기준으로 합니다. 이 하위 집합은 항상 Hugging Face에 있는 텍스트의 시작 부분부터 시작합니다.

비록 이 코퍼스들이 대부분의 대규모 검색 벤치마크에 비해 상대적으로 작지만, 우리의 목표는 대표적인 데이터를 대상으로 한 평가 프레임워크의 유용성을 입증하고, 쉽게 재현 가능한 결과를 제공하며, 향후 연구를 위한 출발점을 마련하는 데 있습니다. 우리가 제시하는 구체적인 데이터셋이 포괄적이라고 주장하는 것이 아니라, 오히려 이 평가 프레임워크의 유용성을 보여준다는 점을 강조합니다.

* State of the Union Address 2024
  * 이는 2024년 국가 연설(State of the Union Address)의 단순한 녹취록으로, 잘 구성되어 있고 명료합니다. 이 코퍼스는 총 10,444 토큰으로 이루어져 있습니다.
* WikiText
  * WikiText 언어 모델링 데이터셋은 위키백과의 검증된 우수 및 추천 기사에서 추출된 1억 개 이상의 토큰으로 구성됩니다. 우리의 하위 데이터셋은 Hugging Face에 표시된 해당 텍스트의 처음 26,649 토큰으로 이루어져 있습니다.
* Chatlogs
  * UltraChat 200k 데이터셋은 ChatGPT가 생성한 140만 개의 대화 중에서 품질이 높은 대화를 선별한 데이터셋입니다. 이 코퍼스는 실제 원시 텍스트를 보다 정확하게 반영하기 위해 모든 주변 JSON 구문을 포함하고 있으며, 우리의 하위 데이터셋은 해당 텍스트의 처음 7,727 토큰으로 구성되어 있습니다.
* Finance
  * ConvFinQA 데이터셋은 금융 분야의 대화형 질문 응답에서 수치적 추론을 연구하기 위해 설계되었습니다. 이 코퍼스는 금융 보고서를 기반으로 한 복잡한 다중 회전 질문과 답변을 포함하며, 장기적인 수치 추론 경로 모델링에 중점을 두고 있습니다. 우리의 하위 데이터셋은 해당 텍스트의 처음 166,177 토큰으로 이루어져 있습니다.
* Pubmed
  * PMC Open Access Subset은 국립 의학 도서관에서 제공하는 생의학 및 생명과학 학술 문헌 모음입니다. 우리의 하위 데이터셋은 이 텍스트의 처음 117,211 토큰으로 구성되어 있습니다.

### Ⅳ-2. Generation & Prefiltering

중복 임계값을 결정하기 위해, 주어진 임계값에 대해 생성된 쿼리 쌍에서 샘플을 추출한 후 이 샘플 쌍들을 수동으로 검토하고, 각 쌍의 쿼리가 충분히 구별되는지에 따라 임계값을 조정하는 이진 탐색을 수행합니다.

<figure><img src="../.gitbook/assets/image (127).png" alt=""><figcaption><p>Pubmed 코퍼스에서 쿼리와 해당 발췌문 간 코사인 유사도 분포</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (128).png" alt=""><figcaption><p>Pubmed 코퍼스에서 모든 쿼리 쌍 간 코사인 유사도 분포</p></figcaption></figure>

유사한 접근 방식이 발췌 필터에도 사용됩니다. 향후 연구에서는 이 과정을 자동화할 수 있을 것으로 기대됩니다. 또한 채택된 임계값들이 코퍼스 간에 유사하게 나타난다는 점은 실제 적용 시 휴리스틱 값으로 사용할 수 있음을 시사합니다.

<figure><img src="../.gitbook/assets/image (129).png" alt=""><figcaption><p>각 코퍼스에 대한 토큰 수, 쿼리 수, 발췌 임계값 및 중복 쿼리 임계값.</p></figcaption></figure>

모든 텍스트 발췌문과 질문은 GitHub 저장소에서 확인할 수 있습니다.

### Ⅳ-3. Time & Cost Analysis

질문 생성에 걸린 시간은 질문 당 3초에서 16초까지 다양했으며, 평균 약 10초였습니다. 시간이 더 길어진 경우는 주로 잘못된 발췌문을 기반으로 한 질문을 배제하기 위한 추가 처리 과정 때문이었습니다.

기본 프롬프트로는 OpenAI의 cl100k Tokenizer로 측정하여 703 토큰을 사용했습니다. 질문 생성 시 원본 코퍼스에서 무작위로 선택한 4,000자 분량과 데이터셋에서 샘플링한 50개의 질문을 포함하여, 평균 프롬프트 크기는 2,000 토큰이 되었습니다. 평균 응답 길이는 200 토큰으로, 질문 당 전체 입력 길이는 2,000 토큰, 출력 길이는 200 토큰이었습니다.

현재 시점에서 GPT-4를 사용해 한 개의 질문을 생성하는 비용은 약 $0.01입니다. 따라서 1,000개의 질문으로 구성된 데이터셋을 생성하는 데 약 $10의 비용이 소요됩니다. 이 추정치는 최종 질문 수를 줄이는 후속 필터링 단계를 고려하지 않은 값임을 유의해 주시기 바랍니다.

## Ⅴ. Chunking Algorithms

아래에서는 이 보고서에서 사용한 chunking 방법들을 소개합니다. 우리는 범용적이고 일반적으로 사용되는 chunking뿐만 아니라 새로운 접근법들도 평가하였습니다. 이 섹션에서 언급된 토큰은 모두 OpenAI의 cl100k Tokenizer를 기준으로 한 것입니다. ★ 기호는 이후에 소개되는 알고리즘이 본 연구를 위해 새로 개발된 chunking 방법임을 나타냅니다.

### Ⅴ-1. RecursiveTextSplitter & TokenTextSplitter

`RecursiveCharacterTextSplitter`와 `TokenTextSplitter`는 가장 널리 사용되는 chunking strategy중 일부이며, 많은 RAG 시스템에서 기본적으로 사용됩니다. 이러한 chunking strategy들은 코퍼스의 의미적 내용에는 민감하지 않고, 대신 문자 시퀀스의 위치에 의존하여 문서를 최대 길이가 지정된 chunk로 분할합니다. 우리는 인기 있는 Langchain 라이브러리의 구현 방식을 따랐습니다.

공정한 결과를 얻기 위해 몇 가지 기본 설정을 변경할 필요가 있음을 발견했습니다. 기본적으로 RecursiveCharacterTextSplitter는 다음과 같은 구분자들을 사용합니다: \["\n\n", "\n", " ", ""]. 그러나 이 방식은 일반적으로 너무 짧은 chunk를 생성하여, 기본적으로 고정 길이의 chunk를 생성하는 TokenTextSplitter에 비해 성능이 떨어지는 것으로 나타났습니다. 따라서 우리는 구분자 집합으로 \["\n\n", "\n", ".", "?", "!", " ", ""]를 사용합니다.

### Ⅴ-2. KamradtSemanticChunker

Greg Kamradt는 후에 LangChain에 통합된 새로운 Semantic Chunking 알고리즘을 제안했습니다. 이 chunker는 먼저 코퍼스를 문장 단위로 분할한 후, 문서의 토큰에 대해 sliding window의 임베딩을 계산하고 연속 윈도우 간의 코사인 거리에서 불연속성을 탐지하는 방식으로 작동합니다. 기본적으로 불연속성을 감지하기 위한 임계값은 모든 연속 거리의 95번째 백분위수를 초과하는 거리로 설정되며, 이는 상대적인 척도이기 때문에 코퍼스가 클수록 더 큰 청크가 생성될 수 있습니다.

#### Ⅴ-2-1. KamradtModifiedChunker

사용자가 임베딩 모델의 컨텍스트 창에 chunk가 맞도록 chunk size를 직접 설정할 수 있으면 바람직합니다. \
이를 해결하기 위해, 지정된 길이보다 큰 chunk가 없도록 불연속성 임계값에 대해 이진 탐색을 수행하는 방식으로 `KamradtSemanticChunker`를 수정하였습니다.

#### Ⅴ-2-2. ClusterSemanticChunker

또한, 임베딩 기반 semantic chunking의 원리를 확장하여 `KamradtSemanticChunker` 알고리즘이 본질적으로 탐욕적(greedy)이라는 점을 관찰했습니다. 이로 인해 내부 유사성이 높은 chunk를 보존하는 목표 하에서 최적의 chunking이 이루어지지 않을 수 있습니다. 이에, 사용자 지정 최대 길이까지 chunk 내 코사인 유사도의 합을 최대화하여 전역적으로 최적의 chunk를 생성하는 것을 목표로 하는 새로운 알고리즘인 `ClusterSemanticChunker`를 제안합니다. 직관적으로 이는 문서 전체에서 유사한 의미 정보를 최대한 보존하면서 각 chunk내에 관련 정보를 압축하는 역할을 합니다.

문서는 `RecursiveCharacterTextSplitter`를 사용하여 최대 50 토큰 단위로 분할되며, 각 조각은 개별적으로 임베딩됩니다. 이후, 동적 프로그래밍 기법을 활용하여 각 chunk 내의 모든 조각 쌍 간의 코사인 유사도를 최대화하도록 합니다. 이 알고리즘의 코드는 GitHub 저장소에서 확인할 수 있습니다.

이 방법의 한계점은 전역 최적의 패킹이 코퍼스의 전반적인 통계에 의존하기 때문에, 데이터가 추가될 때마다 chunk를 다시 계산해야 한다는 점입니다. 이 한계는 데이터가 추가되거나 제거될 때 소규모 조각들에 대해 적절한 자료구조를 유지함으로써 개선할 수 있을 것으로 보이며, 이는 향후 연구 과제로 남겨둡니다.

#### Ⅴ-2-3. LLMSemanticChunker

“just \[ask] the model”는 취지에 따라, 텍스트를 chunking하도록 LLM에 직접 프롬프트를 주는 방법을 실험해보았습니다. 그러나 LLM에게 <|split|> 토큰을 사용해 코퍼스를 반복하도록 하는 순진한 접근 방식은 비용이 많이 들고 느리며, 생성된 텍스트에서 환각 현상이 발생하는 문제가 있음을 확인했습니다.

이 문제를 해결하기 위해 RecursiveCharacterTextSplitter를 사용하여 텍스트를 50 토큰 단위의 청크로 나눈 후, 각 청크를 청크 태그로 감싸는 방식으로 텍스트를 다시 결합하였습니다. 그 결과 다음과 같은 형태의 텍스트가 생성되었습니다.

```
<start_chunk_0>On glancing over my notes of the seventy odd cases in which I 
have during the last eight years <end_chunk_0><start_chunk_1>studied the 
methods of my friend Sherlock Holmes, I find many tragic, some comic, 
a large number <end_chunk_1><start_chunk_2> . . .
```

그 후, LLM에게 아래와 같은 형식으로 분할할 chun의 인덱스를 반환하도록 지시했습니다.

```
split_after: 3, 5
```

GPT-4o와 Llama 3 8B Instruct 모두 이 형식을 일관되게 준수하는 것을 확인했습니다. 이 알고리즘에 대한 전체 프롬프트와 코드는 GitHub 저장소에서 확인할 수 있습니다.

## Ⅵ. Results

평가에 사용된 모든 쿼리와 코퍼스에 대해 각 청크 분할 방법의 $$\mathrm{Recall}$$, $$\mathrm{Precision}$$, $$\mathrm{Precision_{Ω}}$$, $$\mathrm{IoU}$$ 점수의 평균을 제시합니다. ±는 표준 편차를 나타냅니다. OpenAI의 text-embedding-3-large 임베딩 모델을 사용하여, n=5개의 검색된 청크에 대한 결과를 보고합니다.

<figure><img src="../.gitbook/assets/image (130).png" alt=""><figcaption><p>text-embedding-3-large에 대한 결과입니다. 여기서 Size는 토큰( cl100k 토크나이저 기준) 단위의 chunk size를 의미하며, 괄호 안의 수치는 chunking strategy에 따라 달라질 수 있는 평균 chunk size를 나타냅니다. Overlap은 chunk 간의 중첩되는 토큰 수를 의미하며, 굵은 글씨는 각 범주에서 최고의 성능을 나타냅니다.</p></figcaption></figure>

연구 결과, chunk size 200에 overlap이 없는 휴리스틱 RecursiveCharacterTextSplitter가 우수한 성능을 보였습니다. 비록 최고 결과를 달성하지는 못했지만, 모든 평가 지표에서 일관되게 높은 성능을 기록했습니다. 또한, chunk overlap이 없는 상태에서 chunk size가 400 이하일 경우, TokenTextSplitter보다 모든 지표에서 더 우수한 성능을 보였습니다. 흥미롭게도, 이 결과는 chunk size가 더 크거나 overlap이 있는 경우에는 적용되지 않았습니다. 예상대로, 중복 정보를 페널티하는 IoU 점수의 특성상, chunk overlap을 줄이면 IoU 점수가 개선됨을 확인할 수 있었습니다.

OpenAI 어시스턴트는 표준 RAG 프로세스를 따라 파일 검색을 활용해 응답의 질을 향상시킬 수 있습니다. 문서에 따르면, 기본 chunking strategy는 800 토큰 크기의 chunk를 사용하며 400 토큰의 중복을 포함합니다. TokenTextSplitter 방식을 사용한다고 가정할 때, 이 설정은 평균보다 약간 낮은 recall과 다른 모든 지표에서 가장 낮은 점수를 나타내어, recall과 효율성 사이의 trade-off가 특히 좋지 않음을 시사합니다.

최대 chunk size를 400 토큰으로 설정한 ClusterSemanticChunker는 0.913의 두 번째로 높은 recall을 달성합니다. 최대 chunk size를 200 토큰으로 줄이면 평균 recall을 보이지만, PrecisionΩ와 IoU에서는 가장 높은 수치를 기록합니다. LLMSemanticChunkers는 0.919의 가장 높은 recall을 달성하면서 나머지 지표에서는 평균 수준의 점수를 보여, LLM이 이 작업에 비교적 능숙함을 시사합니다.

KamradtSemanticChunker는 기본 설정에서 모든 지표에서 약간 평균 이하의 점수를 보이며, recall은 0.836입니다. KamradtModifiedChunker의 수정사항을 적용하면 recall이 0.871로 상승하며, 다른 지표들에서도 유사한 향상을 보입니다. 평균 chunk size가 줄어들었음에도 recall이 개선된 점은 주목할 만합니다.

우리는 관련 정보가 chunk 내에서 희석되어 검색이 어려워지기 전에 recall이 최대치에 도달할 수 있다고 추정하며, 반대로 chunk가 너무 작으면 단일 단위 내에서 필요한 맥락을 포착하지 못한다고 생각합니다.

또한, 임베딩 모델을 Sentence Transformers의 ‘all-MiniLM-L6-v2’로 변경하여 실험을 반복하였습니다.

<figure><img src="../.gitbook/assets/image (131).png" alt=""><figcaption><p>MiniLM-L6-v2 결과입니다. 여기서 Size는 토큰 단위의 chunk size(측정 도구: cl100k tokenizer)를 의미하며, 괄호 안의 크기는 chunking strategy에 따라 달라질 수 있는 평균 chunk size를 나타냅니다. Overlap은 청크 간 중첩되는 토큰 수를 의미합니다. 굵은 값은 각 범주에서 최고의 성능을 나타냅니다.</p></figcaption></figure>

ClusterSemanticChunker는 가장 높은 Precision, PrecisionΩ 및 IoU를 달성했으나, 평균 chunk size가 작기 때문에 recall은 약간 평균 이하로 나타났습니다. TokenTextSplitter는 chunk size 250, chunk overlap 125로 구성했을 때 0.824의 최고 recall을 기록했습니다. chunk overlap을 0으로 설정하면 recall이 0.771로 떨어지는데, 이는 작은 문맥에서는 높은 recall을 위해 chunk overlap이 필요함을 시사합니다.

## Ⅶ. Limitations & Future Work and Conclusion

### Ⅶ-1. Limitations & Futuer Work

우리의 합성 평가 파이프라인의 **한계 중 하나는 LLM이 특정 스타일의 질문을 생성하는 경향이 있다는 점**입니다. 이를 완화하기 위해 이전 질문들을 제공하여 새로운 질문을 유도하려 하지만, 때로는 이로 인해 창의성 부족이 더욱 두드러지기도 합니다. 보다 <mark style="color:red;">**현실적이고 다양한 질문이 제시된다면 평가 척도의 정확도는 더욱 향상될 것**</mark>입니다. 향후 연구에서는 다양한 온도 설정이나 다른 프롬프트 기법을 실험할 수 있을 것입니다.

우리가 제시한 구체적 평가의 한계는 평가 규모가 상대적으로 작고 다양성이 제한적이라는 점입니다. 향후 연구에서는 더 큰 코퍼스로부터 더 큰 데이터셋을 생성하여 이 평가를 쉽게 확장할 수 있을 것입니다. 또한, 기존 쿼리를 포함하는 벤치마크에서 발췌문 데이터셋을 생성하는 방법을 탐구할 수도 있는데, 이는 다양한 검색 분야를 대표할 수 있다고 여겨집니다. 나아가, 평가 품질을 향상시키기 위해 사람 주도 데이터와 합성 데이터를 혼합하는 방법도 고려해볼 수 있습니다.

마지막으로, 우리의 연구는 이러한 chunking strategy를 실행하는 데 소요되는 시간을 고려하지 않았습니다. 실제 검색 환경에서는 LLMChunker의 경우 거의 즉각적으로 실행되는 것부터 수십 분이 걸리는 경우도 있어, 이는 중요한 고려 사항입니다.

### Ⅶ-2. Conclusion

본 연구에서는 실제 AI 응용 프로그램에서 사용되는 검색의 중요한 특성(예: IoU(Intersection over Union)를 통한 정보 밀도)을 정확하게 포착하는, 도메인 및 데이터셋별 평가를 생성하기 위한 종합적인 프레임워크를 도입하였습니다. 이 프레임워크를 활용하여 여러 인기 있는 도메인에 걸친 구체적인 평가를 수행함으로써, 다양한 chunking stratgey들을 견고하게 비교할 수 있었습니다.

또한, 인기 있는 chunking stratgey에 대한 평가 결과, 이 평가가 선택된 청크 분할 전략에 따른 검색 성능의 차이를 효과적으로 포착함을 보여주었습니다. 평가 전반에서 꾸준히 우수한 성능을 보인 임베딩 모델을 고려한 chunking stratgey인 ClusterSemanticChunker를 개발하였으며, 또 다른 새로운 chunking stratgey인 LLMSemanticChunker 또한 이 평가에서 좋은 성능을 달성하는 것을 확인하였습니다.

***

## My Insight

기존 chunking 방식의 한계점을 파악하고 새로운 chunking 방식을 제안한 것은 매우 논리적이고 합리적인 이유로 보여지나 chunking의 방식이 RecursiveTextSplitter, TokenTextSplitter, SemanticChunker의 3가지만 비교분석하였다는 것이 아쉬웠던 것 같습니다. 또한, Chunk의 본질적인 의의를 생각해본다면 Chunk 구성에 따른 Retriever를 평가해야 이것이 chunking에 대한 성능을 대변할 수 있으리라 생각됩니다. 또한 실험에 대하여 고정적인 chunk size가 너무 작은 것을 고려하면 다양성이 조금 부족하지 않을까 싶습니다. 그러나 IoU라는 평가방식은 굉장히 참신하고 의미가 짙다고 보여지는 것 같습니다. LLM 역시 데이터에 의존할 수 밖에 없는 현실을 부정할 수 없습니다. RAG도 마찬가지입니다. 데이터의 본질에 집중하는 Chunking Strategy와 Chunk의 구성에 대한 연구가 활발해지면 좋겠습니다.
