---
description: 2024년 7월 7일
---

# How to reduce Hallucinations

![thumnail](https://velog.velcdn.com/images/ash-hun/post/dccf256e-b44d-4e6d-8a04-266fc3d4c668/image.png)

## Ⅰ. How to reduce hallucinations in RAG?

현업에서 RAG를 적용하지 못하는 이유는 여러가지가 있지만 가장 크게 작용하는 요소 중 하나라고 생각되는것이 바로 거짓정보(=Hallucination)제어입니다. 오늘은 RAG approach에서 Hallucination을 제어하는 방법에 대해서 알아보겠습니다.

## Ⅱ. Really We can control the hallucinations in RAG?!

정말 우리가 RAG를 사용함에 있어 hallucination을 완벽하게 제어할 수 있을까요? 아직까지 hallucinations을 완벽하게 제어할 수 있는 방법은 존재하지 않는것 같습니다..😂😂 그래서 우리는 최대한 통제하는 방향으로 연구하며 실험해야하는데요, 그전에 왜 hallucination이 RAG에서도 골칫덩이인지 한번 알아봅시다.

#### ⅰ. Why we show hallucinations of RAG?

우리는 왜 hallucination을 마주할까요? 제가 정리해본 바로는 다음과 같습니다.  RAG 자체가 hallucination을 줄여주긴 하지만 완벽하게 방지하는 방법이 아닙니다. 외부의 맥락이 주입되면 답변을 생성하는데 사용하는 LLM과 Knowledge Conflict가 발생할 우려가 있죠. 또한 주입되는 외부지식이 정교하지 않을수도 있습니다. RAG는 하나의 방법론에 불과하기에 어떠한 목적으로 어떠한 Domain에서 사용하냐에 따라 거짓정보의 기준과 이를 최소화하는 접근전략을 다르게 정해야하기도 합니다. 이와 같은 내용들을 바탕으로 곰곰히 생각해보니 어쩌면 RAG라는 방법론에서 Hallucination을 없애는 방법이라는것은 존재하지 않을수도 있겠다는 생각을 하곤 합니다. 즉, 최소화 하는 방법을 접근해야 맞다는 것이죠.

<figure><img src="https://velog.velcdn.com/images/ash-hun/post/2b742bc0-2845-4195-b03e-7e4854830155/image.png" alt=""><figcaption><p>Why we show the Hallucinations?</p></figcaption></figure>

#### ⅱ. General method to reduce hallucinations of RAG

그럼 일반적인 방법을 좀 살펴봅시다. 우리가 어떠한 접근을 해볼 수 있을까요? \
가장 쉬운것을 RAG와 동시에 Grounding LLM을 사용해볼 수 있을것 같습니다.&#x20;

<figure><img src="https://velog.velcdn.com/images/ash-hun/post/5ade0011-622f-45fb-b9f1-802bac02e626/image.png" alt=""><figcaption><p>Vanila RAG + Grounding</p></figcaption></figure>

쉽게 말하면 외부지식과 LLM 사이의 Knowledge Conflict를 최소화하기 위해 LLM에도 해당 Domain, task, your purpose에 대한 지식을 학습시켜 사용하는 것이죠.

System적으로 접근해보면 답변을 생성한 이후 hallucination이 포함된 답변인지 아닌지 체크해볼 수 있을것 같습니다. 체크방법에는 여러가지가 있겠지요. 자체적으로 scoring을 해볼 수도 있고 외부 API를 사용해볼 수도 있겠습니다.&#x20;

<figure><img src="https://velog.velcdn.com/images/ash-hun/post/a0afcc0c-92f3-46e8-aefd-2ffe1a8bcc8e/image.png" alt=""><figcaption><p>Vanila RAG + Ground Check API (w/ Upastage API)</p></figcaption></figure>

물론 Generation 단계 이후가 아닌 Retriever 이후에 이 과정을 수행해볼 수 있을것 같습니다. 다만 대상이 달라지는 만큼 기존에는 "답변이 거짓되었느냐 아니냐"를 확인했다면 Retriever 이후에 수행하는 과정에서는 "추출된 Relavant Docs가 정말 relavant하냐 아니냐?"로 접근해야할 것입니다.

![Agentic Work flow](https://velog.velcdn.com/images/ash-hun/post/d00cd6ca-509e-4102-b9f9-9d448f8384b0/image.png)

한편, Advanced RAG에서도 방법론적으로 알려주긴 합니다. Agentic RAG, CRAG, Self-RAG, Self-Reflection등등 여러가지 방법론이 존재하지만 hallucination의 유무를 LLM-as-a-judge로 한다는점에서 여전히 신뢰성이 의심됩니다.

그밖에 Prompt를 이용해 Verification하는 방식(=CoVe)도 존재합니다만 여전히 해당분야의 연구가 부족하긴 합니다. 조금 더 정확하게는 해당분야의 연구가 현업 도메인에 응용되어 사용되는것이 어려운것이죠. 이밖에 더 다양한 방법을 알고계신다면 같이 공유하면 좋을 것 같아요 :)
