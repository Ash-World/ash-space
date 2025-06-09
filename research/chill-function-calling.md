---
description: 2025.03.15.
---

# Chill칠치 못한 Function Calling

<figure><img src="../.gitbook/assets/function_calling_cover.png" alt="" width="375"><figcaption><p>Chill칠치 못한 Function Calling</p></figcaption></figure>

이번 포스팅의 제목은 'Chill칠치 못한 Function Calling'이다. 표현 그대로 Function Calling에 대한 이야기이긴 한데, 왜 Chill칠치 못하냐고 붙였는지 의아해 하는 지인들이 있어 그 이유로 가볍게 시작해보려고 한다.&#x20;

여러분은 **Tool Biniding, Function Calling, Tool Calling**의 차이를 아는가? 생각보다 아는사람이 없을것이다. 왜냐하면 이 개념들은 모두 같은 개념을 가리키지만 표현이 다르기 때문이다. 그러면 헷갈리기만 하는 요 개념이 어째서 다르게 불리우냐? 그것은 조금 더 광활하게 생각해봐야 한다. Agent의 시대가 떠오르는 현재 기존의 LLM base Application과 Agent base Application의 가장 큰 차이점은 바로 외부도구(=tool)의 사용일것이다. 근데 Agent의 상용화는 이제 막 시작되고 있으니 현재는 과도기로 바라보아도 무방하다. 즉, 개념의 정착이 되지 않았기 때문에 사람들이 편한대로 부르는 것이다. 이러한 점과 더불어 갑작스레 화제가 된 MCP(Model Context Protocol) 개념(=[이전포스팅](model-context-protocol.md)을 참고하라)을 헷갈려하는 사람들도 꽤나 있는듯 하여 **칠칠치 못하다**고 표현하였다. (절대, 저 이미지를 쓰고싶어서 끼워맞춘것이 아니다!! 🤣🤣)

이제, Function Calling에 대해 알아보자.

## Ⅰ. Function Calling이 뭐야?

본격적으로 function calling에 대해 알아보기 전에 API 호출과 무엇이 다른지 먼저 짚어보자. 실무자들에게 API는 이미 익숙한 개념일 것이다.&#x20;

**API (Application Programming Interface)** : 응용 프로그램에서 사용할 수 있도록 운영체제나 프로그래밍 언어가 제공하는 기능을 제어할 수 있게 만든 인터페이스

그리고 이 API들은 사용자의 트리거가 바탕이 되어 실행이 된다. 즉, 독자적으로 호출되지는 않는다는 점과 공통된 규격(=인터페이스 정의)을 정해서 상호간 소통을 쉽게 만든 것이 주요한 특징이라고 할 수 있다.

그렇다면 Function Calling은 무엇일까? OpenAI에서 정의한 Function Calling은 아래와 같다.

> **Function Calling**
>
> Enable models to fetch data and take actions. Function calling provides a powerful and flexible way for OpenAI models to interface with your code or external services, and has two primary use cases:
>
> * _Fetching Data_
> * _Taking Action_

위의 정의를 살펴보면 모델이, <mark style="background-color:red;">데이터를 가져오거나 액션을 수행하는 것을 가능하게 만드는 것</mark>이라고 설명되어 있는데, 데이터를 가져오는 것도 특정한 개발영역의 관점에서는 GET api의 호출과 동일하므로 <mark style="color:red;">**"Action을 수행하는 것"**</mark> 하나로 정의내려볼 수 있겠다. 이 정의는 결국 모델이 필요한 시점에 필요한 요청(Request)을 날려서, API를 호출한다는 뜻이 되는데 그렇다 LLM은 어떻게 스스로 **API**를 호출한다는 걸까요?

## Ⅱ. 그럴듯해 보이는 Function Calling

사실 LLM은 스스로 API 호출을 할 수 없고 겉으로 보기에 그저 호출한것 처럼 보여진 것에 불과하다.

```python
from openai import OpenAI

client = OpenAI()

tools = [{
    "type": "function",
    "name": "get_weather",
    "description": "Get current temperature for a given location.",
    "parameters": {
        "type": "object",
        "properties": {
            "location": {
                "type": "string",
                "description": "City and country e.g. Bogotá, Colombia"
            }
        },
        "required": [
            "location"
        ],
        "additionalProperties": False
    }
}]

response = client.responses.create(
    model="gpt-4o",
    input=[{"role": "user", "content": "What is the weather like in Paris today?"}],
    tools=tools
)

print(response.output)
```

```python
# Output
[{
    "type": "function_call",
    "id": "fc_12345xyz",
    "call_id": "call_12345xyz",
    "name": "get_weather",
    "arguments": "{\"location\":\"Paris, France\"}"
}]
```

위의 코드는 OpenAI의 Function Calling Docs에서 가져온 것인데, 사용자들이 보기엔 아래와 같이 느껴질 것이다.

* **User** : What is the weather like in Paris today?
* **AI** : The weather of Paris is sunny!

근데 실제로는 보이지 않는 호출에 의해 아래와 같이 동작한다.

* **User** : What is the weather like the Paris today?
  * _Function Calling_ : "get\_weather"
    * _Input params_ : { "location" : "Paris, France"}
    * _Output params_ : sunny _<mark style="color:green;">(예시이다 !)</mark>_
  * _Generate Answer with Function Calling Output_
* **AI** : The weather of Paris is sunny!

즉, LLM은 어디까지나 Tool을 고르고, 그 고른 Tool에 맞게 인자를 채워 요청 객체를 만들어주는 것만 가능한 것이다. 필자는 여기서 **Tool에 맞게 인자를 채우는 부분에 대해 깊이있는 이해가 필요하다고 주장**하고 싶다.&#x20;

## Ⅲ. Tool에 맞게 인자를 채우는 부분을 이해하라고?

이제까지의 내용을 요약해보면 아래와 같다.

> LLM은 API호출을 할 수 없고 대신 API 호출을 위한 객체를 완성한다. 호출은 그 다음 문제.

위 정리된 문장을 보면 가장 중요한게 어디일 것 같은가? 바로 객체를 완성하는 부분이다. 호출은 앞서 설명했듯 정해진 형태(e.g. RESTFull API)로 transforming되기만 하면 수행하는데 어려움이 없다. 여기서 중요한 것은 자연어에서 도구에 필요한 객체를 어떻게 추출해서 완성할 것인가? 아래 가벼운 예제를 보자.

* **USER** : 안녕? 너에 대해 설명해줘.
*   **AI** : 안녕하세요! 😊 저는 ChatGPT라고 해요. \
    OpenAI에서 만든 인공지능 챗봇으로, 여러분과 대화를 나누고 정보를 제공하는 역할을 해요. \
    \
    제가 할 수 있는 것들:

    ✔️ 정보 제공 – 다양한 주제(과학, 역사, 기술, 문화 등)에 대한 설명

    ✔️ 문서 작성 및 수정 – 보고서, 에세이, 이메일, 코드 작성 및 편집

    ✔️ 코딩 도움 – 프로그래밍 코드 작성, 디버깅, 최적화

    ✔️ 번역 및 언어 학습 – 여러 언어 번역, 문법 설명, 언어 학습 팁 제공

    ✔️ 아이디어 브레인스토밍 – 창의적인 글쓰기, 프로젝트 아이디어 추천

    ✔️ 데이터 분석 – 엑셀, CSV 파일 분석 및 시각화\
    \
    어떤 도움이 필요하세요? 😊
* **USER** : 방금 내용을 바탕으로 메일좀 보내줄래?
* **AI** : ...&#x20;

위의 짧은 대화에서 메일좀 보내달라는 사용자의 질의를 바탕으로 Function Calling을 접목시킨다고 생각해보자. 할 수 있을것 같은가? 적어도 필자는 못한다고 생각한다. 어디로 메일을 보내야하고, 어떤 제목을 적어야 하며, 그외 기타정보들을 어떻게 알 수 있겠는가? 최소한의 정보를 알 수 없다면 Function Call을 하기 위한 객체를 채울 수 없을것이고, 궁극적으로 우리는 사용자와 chatbot의 대화 흐름속에서 최소한의 정보를 어떻게 추출할지 깊게 고민해야할 것이다.&#x20;

## Ⅳ. 그럼 어떻게?

해당 부분에 대해서 다양한 연구와 적용을 해봐야 알겠지만 가장 간단한 pattern 하나만 소개해보겠다.

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption><p>[ Fig 01. ] Re-Question Pattern</p></figcaption></figure>

추출이 어렵다면 다시 되물으면 된다. 해당 로직의 핵심은 Function Calling을 위해 필요한 parameter가 구체적으로 알 고있을 때 사용할 수 있는 패턴이다. 물론, 해당 패턴을 사용했을 때 성공적으로 function calling하기위해서는 최초에 정보를 extraction하는 성능도 좋아야 한다는 점을 명심하자!



