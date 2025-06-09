---
description: 2024.12.02.
---

# 나야, Chunk

<figure><img src="../.gitbook/assets/나야 청크 (1).png" alt=""><figcaption><p>나야, 청크</p></figcaption></figure>

RAG하는 사람들의 공통적인 고민거리중 하나는 데이터를 어떻게 잘 정제하느냐에 있다. 여기서 말하는 '정제'는 기본적인 텍스트 전처리가 아닌 어떠한 기준을 두고 **Chunking Strategy**를 취할 것인가? 를 의미한다. 무릇 RAG를 한다고 하면 아래와 같은 생각을 한번쯤은 해봤을것이다. (당연히, 나만 했을수도 있다 😆)

> Langchain 예제를 보면 몇가지 Chunking 방식이 구현체로 존재하지만, 왜 성능좋은 Chunking Strategy에 대한 연구나 가장 좋은 성능을 이끌어낼 수 있는 Chunk를 만드는 방법은 없을까?

먼저 본질적으로 생각해보자. **Chunking**이란 무슨 작업을 의미할까?

여러가지 자료를 찾다보니 다음자료가 가장 잘 표현한듯 싶다. Nielson Norman Group의 Kate Meyer가 작성한 "[How Chunk Helps Content Processing](https://www.nngroup.com/articles/chunking/)"에 따르면 일반적으로 chunk란 더 큰 무언가의 일부인 조각을 의미하며 인지심리학에서는 기억에서의 구성단위(organizational unit)이라 말한다고 한다.  우리는 RAG(Retrieval Augmented Generation)의 기술을 익히 사용하기에 그저 User Query에 유사한 정보 집합(chunk)으로 LLM의 input prompt에 증강되어 들어갈 context로 일반화하지만 원론적인 Chunking은 작게 잘린 Chunk만으로 하나의 완결된 구성을 지녀야 한다고 생각이 든다. 이것을 내용의 구성관점에서 다시 풀어보면 아래와 같이 정의내려볼 수 있을것 같다.

**"Chunking**이란?**"**\
👉 한 집합의 데이터를 잘게 쪼개어 최소 단위로 구성하는 작업을 말하고, 여기서의 최소단위가 Chunk인것이다.

위 내용으로 미루어보아 Chunk는 다음과 같은 특성이 보존되어야 한다.

* 단일 Chunk가 하나의 완결된 의미론적 구성을 지녀야 한다.
* 단일 Chunk는 밀집된 정보포화도를 가져야 한다.
* 단일 Chunk는 정보계층도를 반영하여 구성될 수록 완성도가 높다.

하나씩 살펴보자.

#### Ⅰ.  Feature 01 : 단일 Chunk가 하나의 완결된 의미론적 구성을 지녀야 한다.

위에서 언급했듯이 **인지심리학**에서는 Chunk를 _**기억에서의 구성단위**_&#xB85C; 부르기도 한다. 즉, 조각이라는 것은 더 큰 집합의 영역을 쪼갠 작은 단위인것이지 온전하지 않다는 말은 아닌것이다. 많은 사람들이 해당 사실은 간과하는듯 하지만 나는 이것이 옳다는 것을 암암리에 보여주는 것이 Token Split 방식의 Chunking보다 Sentence Window방식의 Chunking이  더 효과가 좋다는것이 입증해준다고 생각한다.&#x20;

<figure><img src="../.gitbook/assets/image (40).png" alt="" width="375"><figcaption><p><strong>출처</strong> : AutoRAG Velog (<a href="https://velog.io/@autorag/%EC%B2%AD%ED%82%B9%EC%9D%80-%ED%95%9C%EA%B5%AD%EC%96%B4-RAG-%EB%8B%B5%EB%B3%80-%EC%84%B1%EB%8A%A5%EC%9D%84-%EC%96%BC%EB%A7%88%EB%82%98-%EC%98%AC%EB%A0%A4%EC%A4%84%EA%B9%8C">https://velog.io/@autorag/%EC%B2%AD%ED%82%B9%EC%9D%80-%ED%95%9C%EA%B5%AD%EC%96%B4-RAG-%EB%8B%B5%EB%B3%80-%EC%84%B1%EB%8A%A5%EC%9D%84-%EC%96%BC%EB%A7%88%EB%82%98-%EC%98%AC%EB%A0%A4%EC%A4%84%EA%B9%8C</a>)</p></figcaption></figure>

누군가는 "얼마 차이 안나는구만\~"이라고 말할수 있겠으나 그것은 실험환경에 영향을 받기 때문에 차이가 난다는 사실이 중요한것이라고 보여진다. 한편 또 다른 중요포인트는 "완결된 의미론적 구성"이라고 생각한다. 구성된 Chunk가 그 자체만으로 완성된 정보를 전달하는가를 판단해봐야 하는것이다. 여기서 강조하고 싶은 것은 완성된 정보를 전달하기 위해서는 필수적으로 완전한 구성을 이루어야 한다는 점이고 이를 쉽게 이해하기 위해 아래 2가지 예시를 준비하였다. 해당 chunk들은 모두 자카드 유사도에 대한 설명을 가져온 것이다.

아래 예시가 A이고

```
자카드 유사도는 집합의 개념을 이용하는데요, 한줄로 정리하자면 합집합과 교집합 사이의 비율이라고 할 수 있습니다. 
두 집합이 얼마나 많은 아이템을 동시에 갖고있는가? 를 수치로 환산한거죠. 
만약 두 집합이 함께 갖고 있는 아이템이 없다면 0, 아이템 전체가 겹치면 1로 계산이 되겠네요. 
집합 X와 Y가 있다고 합시다. 
X=[A,B,C,D], Y=[A,C,F,G]를 값으로 갖는다고 가정하겠습니다. 
합집합은 Union(X, Y) = [A,B,C,D,F,G] 이고, 교집합은 Intersection(X,Y) = [A, C] 임을 알 수 있죠. 
그럼 자카드 유사도는 (교집합의 원소 개수)/(합집합의 원소 개수)로 2/6=0.33임을 알 수 있습니다.
집합을 이용하기 때문에 구현도 굉장히 단순합니다.
```

아래 예시가 B이다.

```
집합 X와 Y가 있다고 합시다. 
X=[A,B,C,D], Y=[A,C,F,G]를 값으로 갖는다고 가정하겠습니다. 
합집합은 Union(X, Y) = [A,B,C,D,F,G] 이고, 교집합은 Intersection(X,Y) = [A, C] 임을 알 수 있죠.
```

보기에 어떠한가? A의 경우 '자카드 유사도'라고 하는 정보구성의 목표에 부합되는 완결된 구성을 이루고 있지만 B의 경우 A의 일부내용임에도 불구하고 내용만 봐서는 자카드 유사도에 대한 완결된 의미론적 구성을 가지는지 판별할 수 없다. 그렇기 때문에 B보다는 A가 더 좋은 Chunk라 봐도 무방한 것이다.&#x20;

다른 관점에서는 한번 더 생각해보자. 결국 Chunk도 정보를 분할하는 과정에서부터 나온 결과물이다. Chunk의 최종 쓰임새를 고려해보면 **결국 User Query에 대한 답변을 생성하기 위해 필요한 정보인 것**이고 이는 사람과 사람 사이의 대화를 하는데 있어서 최소단위를 규정해야 본질적으로 의미를 해치지 않을 수 있다. 우리가 누군가와 대화를 하는데 필요한 요소를 잘 되짚어 보면 대화를 하는 주체인 본인과 상대방이 존재해야 하고, 원활한 소통을 가능케하는 도구가 필요한데 이것이 바로 대화의 최소단위인 '화행'인것이다. Chunk도 이러한 특성이 잘 반영되어야 완결적 의미론적 구성을 온전히 유지할 수 있고 이러한 Chunk들이 모여 소통을 원활하게 할 수 있는 것이다. &#x20;

{% hint style="info" %}
물론, 소통이라고 하는 것은 직접적으로 와닿지 않는 표현일 수 있다. 하지만 이전에 언급했듯이 Chunking의 목적은 주어진 Large Corpus를 분할하여 Chunk 집합으로 만드는 것이고, 이렇게 만들어진 Chunk들은 User Question에 대한 가장 적합한 Answer를 LLM이 생성하는데 사용되는 정보가 된다. \
\
즉, 우리가 보기엔 단순한 Bot의 응답일지라도 그 응답의 품질을 최대한 향상시키기 위해서는 Conversation의 관점으로 바라보는것이 좋다고 생각한다. 이 또한 Person과 LLM 사이의 대화이니까😁
{% endhint %}

#### Ⅱ. Feature 02 : 단일 Chunk는 밀집된 정보포화도를 가져야 한다.

여기서 언급한 밀집된 정보포화도는 무슨의미로 사용한것일까? 아까부터 괜시리 이상한 단어쓴다고 열불내지말고 진정해보자. 우리는 아까부터 Chunk가 보존해야 하는 특성에 대한 이야기를 하고 있었다. Chunk는 RAG의 Answer를 생성할 때 LLM의 input으로 같이 동봉된다. 이는 곧 포함할 수 있는 양이 명백한 한계점을 가진다는 의미이다.&#x20;

그도 그럴것이 한번 상상해보라. 당신 앞에 10살 꼬맹이가 있다. 이 꼬맹이한테 오늘 저녁은 쭈꾸미를 먹어야하는 이유를 설명하는데 필요한 근거를 1가지 얘기했을 때와 23가지를 얘기했을 경우, 어느경우에 쉽게 이해하겠는가? 때로는 정보의 집약성이 빛을 발할때도 있는 법이다. **Chunk의 길이(=양)에는 분명한 상한선이 존재**하고 이것은 대부분 LLM이 가지는 context length과 연계된다. 그렇기때문에 **한정된 양에 컴팩트한 정보를 잘 집약해야한다**는 것이다. 여기서 정보의 불필요함을 판단하여 걸러낼수도 있고 부가적인 정보를 덧붙일 소요도 발생할 수 있다.&#x20;

<figure><img src="../.gitbook/assets/image (41).png" alt="" width="375"><figcaption><p><strong>[Figure. 01]</strong> Lost in the Middle: How Language Models Use Long Contexts</p></figcaption></figure>

익숙하게 봐왔던 Figure. 01 일것이라 생각된다. 위 그림을 발췌한 논문에서 시사하듯 과도한 정보를 LLM한테 줘봐야 아무 의미가 없다. 결국 사람이 사고하는것과 비슷하게 중간에 위치한 정보는 까먹어 버린다. 그러니 최소한의 양으로 중요한 정보들로 구성해야 하는 것이 정보의 질을 올릴 수 있는 가장 효과적인 방법이고 이것이 결국 최종적인 답변을 생성할때도 영향을 미친다.

#### Ⅲ. Feature 03 : 단일 Chunk는 정보계층도를 반영하여 구성될수록 완성도가 높다.

반복해서 말하지만 Chunking이라는 작업은 큰 집합에서 chunk를 분할하는 작업이다. 각각의 chunk가 완결된 의미론적 구성을 가지는것과 별개로 **다수의 chunk가 모여야만 본래 데이터의 의미를 보존하는 경우**도 있다. 그렇기 때문에 데이터의 구조(Structured-Aware)를 반영한 Chunking 역시 상당히 좋은 효과를 발휘하는 것이다. 다행히도 우리가 현실세계(Real-World)에서 사용하는 Corpus 데이터들은 대부분 Document base이다. 세상 그 누구도 문서를 작성하는데 하고싶은말을 멋대로 끄적이진 않는다. 구조를 잡고 스토리텔링을 하기 마련이다. **즉, 작성자의 의도를 정확하게 파악하여 chunk를 구성하더라도 해당 chunk의 범주를 같이 기재해주는것이 좋은 효과**를 야기할 것이다. 실제로 해당 방식은 증명되었는데 아래를 살펴보자.

<figure><img src="../.gitbook/assets/image (42).png" alt=""><figcaption><p><strong>[Figure. 02]</strong> Contextual Retrieval (출처 : <a href="https://www.anthropic.com/news/contextual-retrieval">https://www.anthropic.com/news/contextual-retrieval</a>)</p></figcaption></figure>

위의 Prompt Template을 보면 알겠지만 Chunk를 구성할 때 해당 문서의 Summary를 넣어준다. 이것은 거시적인 관점에서 Chunk가 자기 스스로 완결된 의미구성을 가지면서 동시에 여러 문서의 chunk가 섞여있을 때 이것을 확실하게 분리해주며, 각 Chunk들의 큰 범주를 명시적으로 표시해주는것으로 굉장히 스마트하게 구성한 것이라 할 수 있다.

이처럼 chunking은 단순하게 corpus를 splitting하는 작업이 아닌 chunk의 구성까지 고려해야 하는 굉장히 까다로운 작업인 것이다. 그러니 데이터의 중요성과 Chunking의 중요성도 같이 인지해야한다.
