---
description: 2025.02.23.
---

# Linguistic Prompts

<figure><img src="../.gitbook/assets/liguistic prompts.png" alt=""><figcaption><p>Linguistic Prompts</p></figcaption></figure>

## Ⅰ. Background

&#x20;최근 언어학적인 프롬프트 작성 및 접근의 Agenda를 가진 행사를 다녀온 적이 있다. 굉장히 유명하신 Prompt Engineer분께서 주최하신 행사로 개인적으로 그분의 사고와 말씀을 좋아하는 사람으로써 매우 큰 기대를 하고 다녀왔다. 역시 기대한대로 너무나 값진 내용을 많이 얻었고 이번 아티클은 그 내용에 대해 필자가 이해한 바를 정리해놓으려고 한다. 필자는 RAG를 업으로 하고 있지만 구석구석에 Prompt의 영향력이 많이 행사되고, 이것은 성능에 직결되는 부분도 없지 않기 때문이다.&#x20;

필자는 생성형 AI 필드에 있는 사람들은 Prompt의 중요성을 간과해서는 안된다고 생각하는 사람인데, 그 이유에 대해 짧게 얘기해보도록 하겠다. 굉장히 쉬운 이야기이다. 생성형 AI의 등장배경을 뿌리까지 타고 올라가면 일반적으로 Transformer 모델의 등장이후 극단적인 활용범주의 확장으로부터 시도되었다고 보여진다. 그렇다면 우리는 Transformer라는 언어모델에 집중해봐야한다. 이 언어모델로 무엇을 하고자 했던 것일까? 다양한 의도가 존재하겠지만 본질적으로 사람의 사고체계를 모방하고자 한다는 것을 부정할 수 없을것이다. 이는 Transformer라는 특정한 모델만을 놓고 보는 것은 아니다. 모든 언어모델이 다방면으로 사용되는 이면에 사람의 사고를 흉내내고자 한다. 기술적으로 흉내낼 수 있어야 대체할 수 있고, 대체할 수 있어야 비로소 이전에 존재하지 않은 새로운 가치를 만들어 낼 수 있다. 즉, 궁극적인 목적이 생성형AI로 사람이 하는 일을 대체하여 기존에 없는 새로운 가치를 창출하고자 하는 것으로 정의내릴 수 있겠다. 그러한 관점에서 <mark style="background-color:yellow;">**왜 우리는 사람의 사고와 행동, 체계를 모방하는 것부터 하는데 어떻게 모방할지를 고민할 뿐, 기존의 사고와 행동, 체계가 어떻게 이루어지는지는 알아보려고 하지 않는가?**</mark>&#x20;

사람마다 다르겠지만 위 질문에 대해 한번쯤은 깊게 생각해봐야 한다고 필자는 주장하고 싶은것이다. 이번 아티클에서 다룰 '언어학적 프롬프트 작성 및 접근'은 앞서 언급한 기존의 사고와 행동, 체계에 대한 분석을 바탕으로한 기술적 모방에 가장 부합한 것이다. 당연하게도 나는 언어학을 알지 못한다. 외부행사에서 얻은 지식을 나를 위해 정리했을 뿐이니, 이점 참고하길 바란다.

서두가 굉장히 길었다!

Linguistic을 바탕으로 Prompt를 작성하면 다음과 같은 내용을 본질적으로 해소할 수 있다.

* 단어 의미를 못찾으면 의미 모호성을 줄여야 함.
* 맥락을 잃거나 못찾으면 인간의 인지 구조와 개념 체계를 이해해야 함.
* 문장 레벨 이상의 의미해석을 못하면 담화와 대화 구조를 이해해야 함.

## Ⅱ. Syntaxs

Syntax는 이렇게 정의내릴 수 있다. The set of rules that govern the structure and arrangement of words in sentences. 즉, 문장이 어떻게 구성되는지 (e.g. 주어, 술어, 목적어, 등) 생성문법과 구문 규칙 파싱, 구조적인 해석이 Syntax(=통사론)에 해당한다.&#x20;

### Ⅱ-1. Generation Grammer

Generation Grammer(=생성문법)은 '모든 언어는 근간을 이루는 규칙을 가진다.'고 하는 것이다.\
대표적으로 Chomsky(1970)와 Jackendoff(1977)의 X-bar Theory가 존재한다. 아래 예를 보자.

`A Cute Cat`&#x20;

위와 같은 문장에서 가장 중요한 것은 무엇인가? 바로 Cat이다. 모든 문장은 핵심(Core)이 있고 이를 중심으로 왼쪽 또는 오른쪽으로 확장될 수 있다. 보통 명사가 핵심을 차지한다.

### Ⅱ-2. Syntax Tree

Syntax Tree란 단어들 간의 수직적 위계 관계가 존재하며, 문장 구조가 지니는 계층 표상방법을 tree형태로 표현한 것이다. 이는 언어의 모호성을 표현해주기 위한 방법이다. 언어란 무릇 어순이 중요한 언어와 그렇지 못한 언어가 있기 때문에 어순 또한 의미적 모호성을 구별해주는 중요한 기준이 된다. 아래 예시를 살펴보자.

* English
  * The boy likes the girl
  * The girl likes the boy
* Korean
  * 철수가 영희를 좋아한다
  * 영희가 철수를 좋아한다
  * 좋아한다 철수가 영희를

위의 문장을 살펴보면 특징이 두드러질 것이다. 영어는 문장의 구조와 어순이 동일한 반면, 한국어는 전혀 그렇지 않다. 사람이 이를 잘 생각하지 못하는 이유는 바로 언어능력(competence)을 선천적으로 타고났기 때문인데, 언어학이라는 학문이 바로 이 능력을 규명하는 것을 목표로 삼는다고 한다. (크.. 굉장히 멋있다..)

### Ⅱ-3. Recursive Rule

Recursive Rule은 동일한 규칙이 반복되어 복잡한 구조를 형성하는 것을 말한다. LLM의 프롬프트를 설계할 때 이러한 규칙들을 명확하게 명시하거나 암시하여 모델이 문장 구조를 올바르게 이해하고 생성하도록 유도하면 좋은 결과를 야기할 수 있을 것이다. 대표적인 case는 아래와 같다.

* 명사구의 무한 반복
  * ex) 나는 철수와 영희와 명수와 재석과 숙자와 ... 놀았다.
* 동사구의 무한 반복
  * ex) 그는 걷고 먹고 자고 뛰고 걷고 먹고 자고 뛰고 ...
* 절의 무한 반복
  * ex) 철수가 영희가 명수가 예쁘다고 생각한다고 말했다.
* 관형절의 무한 반복
  * ex) 내가 지난주에 소개받은 친구가 다녔던 학교에서 유명했던 교수님이 작성했던 논문을 인용했던 학자가 쓴 책을 읽고 있다.
* 구구조규칙 : A ⇒ B C
  * The boy likes the girl. : NP ⇒ D N
  * 일부 문법적 오류\
    (e.g. 주어-동사 불일치, 비교문 오류, 동사 누락 등)
  * 낮은 빈도로 등장하는 어려운 문법 구조에서 성능이 저하됨.
  * Semantic Anomalies (의미적 이상 현상) 문장에서 논리적 모순을 제대로 인식하지 못함.

## Ⅲ. Semantics

Semantics(=의미론)의 정의는 The study of meaning in language, focusing on how words, phrases, and sentences convey meaning이라고 한다. 문장의 구성에 따라 어떤방식으로 의미가 달리지고 구성되는가에 대한 학문인것 같다. 문장은 크게 object와 function으로 구성되는데, 이를 형이상학적 관점에서 소개해주셨다.

### Ⅲ-1. Metaphysics

Metaphysics(=형이상학)에 대하여 소개하겠다.

* 존재하는 것들은 어떤방식으로 존재하는가? ⇒ object
* 사물과 개념은 어떤 관계인가? ⇒ function
  * 사과는 물리적으로 존재하는가? (변하지 않는다)
  * 빨간색은 사과가 가진 속성인가? (변한다)
* 의미를 규정하는 방식이 가장 중요하다.
  * 의미란 무엇인가?
    * Freg's Semantics/Metaphysics
    * 의미 구성 (Copositional Semantics)
      * 단어, 구, 문장이 어떻게 결합되어 전체적인 의미를 형성하는지 설명
      * 전체의 의미는 그 구성요소들의 의미와 결합 방식에 의해 결정된다. (원리적 구성)
    * **Semantic Relations** : 의미적 관계
      * 의미적 관계에 따른 분류
        * 동의어 : 의미가 유사한 단어들
        * 반의어 : 서로 반대되는 의미의 단어들
        * 다의어 & 중의성 : 하나의 단어가 여러 의미를 가질 때 발생하는 문제
        * 텍스트 해석 시, 단어의 선택과 문맥에 따라 의미가 크게 달라질 수 있음. LLM은 통계적 패턴을 바탕으로 단어의 다의성 문제에 직면
      * 빈도수가 많은 단어가 그 문서에서 중요한 단어인가? No!!
        * I went to the bank.
          * 나는 은행에 갔었다.
          * 나는 강가에 갔었다.
        * She saw the man with the telescope.
          * 그녀는 망원경으로 남자를 봤다.
          * 그녀는 망원경을 가지고 있는 남자를 봤다.
        * The chicken is ready to eat.
          * 닭이 먹을 준비가 됐어.
          * (내가) 닭을 먹을 준비가 됐어.
    * **Reference Theory** : 어떤 단어나 표현이 실제 세계에서 특정한 대상(참조대상)을 직접 지시
      * 러셀 : 고유 명사는 그 자체로 특정한 대상을 지시한다.
      * 프레게 : 의미와 지시를 구분하며, 단어의 의미는 단순히 대상이 아니라 개념적 내용도 포함할 수 있다.
        * ex) 그는 천재야.
          * 대명사 : 그
          * 속성 : 천재
      * **Deixis**(=직시) : 발화 상황에 따라 지시체가 달라지는 지시어
        * Personal Deixis
          * 그, 그녀
        * Temporal Deixis
          * 지역적 사투리(e.g. 거시기), 공간을 지칭
        * Spatial Deixis
          * 그 사람만 이해하는 표현
        * Discourse Deixis
          * 담화 내에서만 이해하는 표현 (e.g. As I Said Before)
      * **Conceptual Semantics** (=개념적 의미론)
        * 인지구조의 기본 요소
          * 감각 경험과 지각
            * 인간은 주변세계를 감각 기관을 통해 경험
              * 감각 기관
                * 시각, 청각, 촉각 등
              * 동적 경험
                * 문화, 개인 학습 경험
            * 프로토타입(=Prototype)
              * 인간은 범주를 구성할 때 전형적인 예(=prototype)를 기준으로 판단\
                e.g.) 비둘기 = Peace, 평화
            * 스키마와 프레임 (=Schema & Frame)
              * 개인이 경험한 종보를 조직화 (병원 = 가운, 의사, 흰색)
            * 개념적 은유 (=Conceptual Metaphor)
              * 시간은 돈이다.
      * **은유와 환유** : Lakoff, G. & Johnson, M. (1980). Metaphors We Live By.
        * 은유 : 한 개념 영역에서 다른 개념 영역으로 의미가 전이되는 방식\
          e.g. 시간은 돈이다.
        * 환유 : 어떤 개념을 그와 밀접하게 관련된 다른 개념으로 대체하는 것\
          e.g. 백악관이 발표했다. (= 백악관이 미국을 대변)

RAG는 Semanitc loss가 굉장히 심함. 기존 데이터가 가지는 tone & mannerr가 모두 박살나기 일쑤.

## Ⅳ. Pragmatics

Pragmatics(=화용론)는 The study of how context influences how we interpret and make meaning of communication. It is often described as the study of "language in use"로 정의한다. 언어는 Lower Context Language와 Higher Context Language가 존재한다. 대표적인 예시로 영어는 Lower Context, 한국어는 Higher Context에 속한다.

### Ⅳ-1. Personal Space

사람과 사람사이의 대화에서 Personal Space라는 개념이 존재한다. 크게 2가지 요소가 있는데 아래와 같다.

* 화자와 청자의 상황
  * 대화가 이루어지는 시간, 장소, 사회적 관계등이 의미해석에 영향을 미침
* 암시와 암묵적 의미
  * 명시적으로 언급되지 않은 정보 (예 : 암시, 전제, 배경지식)를 통해 의도된 의미를 도출함

### Ⅳ-2. Grice's Cooperative Principle (협력의 원칙)

협력의 원칙을 잘 준수하면 Human-like한 conversation이 가능하다. 내용은 아래와 같다.

* Maxim of Quality : 진실된 정보를 제공할 것
* Maxim of Quantity : 적당한 양을 말할 것
* Maxim of Relation : 관련된 정보만 말할 것
* Maxim of Manner : 매너를 지켜 말할 것

### Ⅳ-3. Speech Act Theory (발화행위이론)

발화는 단순 정보전달이 아니라, 실제로 요청, 명령, 질문, 약속등의 수행 행위(illocutionary act)를 포함한다.

* Locutionary act : The process of saying itself
* Illocutionary act : The intention of saying itself
* Perlocutionary act : The effect of saying smth
* Representative or Assertive
* Directive
* Commisive
* Expressive
* Declarative

***

약 2시간 이라는 시간안에 함축적으로 위의 내용을 다뤘고 나는 그것을 옮긴것에 불과하다. 나는 언어학을 전공하지도 않았기 때문에 어느정도 잘못되어 옮겼을 가능성도 농후하다. 이점 양해해서 봐주길 바란다. 앞으로도 언어학의 개념이 RAG를 비롯해 생성형 AI 전반적으로 스며들어야 가장 Human-Like한 발전이 될 것이라 생각한다.&#x20;

