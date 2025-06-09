---
description: 2025.01.12.
---

# 웅장한 대결 AI Agent와 Agentic AI

<figure><img src="../.gitbook/assets/agent_vs_agentic.png" alt=""><figcaption><p>AI Agent와 Agentic AI의 웅장한 대결</p></figcaption></figure>

2025년의 가장 화제가 될 내용이 무엇이 될지 미리 파악할 수 있다면 얼마나 좋을까요? 대비를 하기 위해서는 그 대상에 대해 잘 알아야 하는데요, 제가 중점적으로 보는 키워드 중 굉장히 애매한 개념이 있습니다. 바로 AI Agent와 Agentic AI에 대한 개념입니다. 여러분은 이 2가지의 차이점이 무엇인지 생각해본적이 있으신가요? 최근들어 이에 대한 정의를 다시 생각해보게 되었고, 다른 많은 빅테크 혹은 회사들이 어떠한 정의를 내리는지 한번 살펴보겠습니다 😊



일단은 Agent에 대한 정의부터 생각해봐야 할 것 같네요.

대부분 Agent가 무엇입니까? 라고 여쭤보면 비슷하면서도 다르게 말을 하곤 합니다. 그럴수밖에 없는것이 Agent의 정의는 사람마다, 회사마다 다르거든요. 개인적으로 아직 성숙해지지 않은 분야여서 의견이 분분할 수밖에 없다고 생각합니다. 대표적인 케이스 몇가지를 정리해보면요,&#x20;

Claude로 유명한 Anthropic의 Research Blog의 포스트인 '[Building effective agents](https://www.anthropic.com/research/building-effective-agents)'에 따르면 Workflow는 사전에 정의한 코드 흐름에 따라 LLM과 도구가 사용되는 시스템이고, Agent는 LLM이 직접 흐름이나 도구 사용 여부를 결정하고 어떻게 목표를 달성할지 관리하는 시스템을 의미한다고 합니다. 제가 생각하는 정의에 가장 가깝기도 한데요, 실제로 해당 블로그에서도 Langgraph를 언급하고 있습니다.

그렇다면 최강자 OpenAI는 어떻게 정의하고 있을까요?\
OpenAI의 Agent는 '[OpenAI Cookbook](https://cookbook.openai.com/examples/orchestrating_agents)'에서 찾아볼 수 있습니다. 여기서는 루틴(Routine)과 핸드오프(handoffs)라는 개념을 소개하는데요 해당 샘플을 OpenAI Swarm 프레임워크에서 다루고 있다고 합니다. 루틴(Routine)은 엄격하게 정해진 것은 아니며 일련의 명령(혹은 지시사항)과 명령을 수행하기 위한 도구를 다룬 단계를 의미하며 이는 시스템 프롬프트와 각종 도구들이 해당한다고 합니다. 핸드오프(handoffs)는 agent들 간의 상호작용이라고 볼 수 있을것 같은데 해당 블로그에서는 예시로 전화통화를 넘겨주는 상황을 언급하고 있습니다. 결국 동적인 행동(Dynamically Action)으로써의 의미가 강한 것 같네요.

Google은 어떨까요?\
최근 Google에서 배포한 AI Agent 백서 [WhitePaper](https://www.kaggle.com/whitepaper-agents)에 따르면 Agent는 세상을 관찰하고 자신이 가진 도구를 사용하여 목표를 달성하려고 시도하는 애플리케이션으로 정의내려볼 수 있을것 같습니다. 또한 특징을 굉장히 명확하게 정의내리는데 자율적으로 작동하며 명확한 목표나 목적이 주어질 경우 인간의 개입 없이 독립적으로 행동할 수 있어야 한다고 한다. 즉, 인간이 명시적인 지침을 제공하지 않아도 Agent는 궁극적인 목표를 달성하기 위해 다음에 무엇을 해야 할지 스스로 판단할 수 있다고 합니다.&#x20;

Langchain팀의 정의는 꽤나 다른 특징을 띄고 있습니다. Langchain팀의 [테크 블로그](https://blog.langchain.dev/what-is-an-agent/)에 따르면 Agent는 애플리케이션의 작업 흐름을 선택할 때 LLM을 활용하는 시스템이라고 하는데 그걸 꽤나 디테일하게 구분한다. 아래 그림은 해당 분류를 나타낸 내용이다.

<figure><img src="../.gitbook/assets/image (111).png" alt="" width="563"><figcaption><p>Langchain : Levels of autonomy in LLM Applications</p></figcaption></figure>

이와 비슷하게는 huggingface팀에서도 정의내린 내용이 있다. 가장 최근 나온 huggingface의 [smolagents blog](https://huggingface.co/blog/smolagents)를 보면 agent의 정의에 대해서도 나오는데 LLM 출력이 워크플로우를 제어하는 시스템으로 정의내립니다. Langchain팀처럼 Agency Level에 대해 분류하는데  아래그림과같다.&#x20;

<figure><img src="../.gitbook/assets/image (113).png" alt=""><figcaption><p>How agency varies across systems</p></figcaption></figure>



결국, Agent와 Agentic에 차이점을 분명히 나눌 수 있지만 그 기준은 회사나 개인마다 다르게 정의될 수 있습니다. 여기서 공통적으로 보여지는 특징들을 정리할 수 있을 것 같은데 그 내용은 아래와 같습니다.&#x20;

그래서 나름 제가 정리해보자면 Agent와 Agentic의 차이점은 LLM이 자율성과 의사결정을  가지고 사람의 개입없이 수행하여 행동까지 연결할 수 있다면 Agent, 한 가지 요소라도 위배되면 Agentic이라고 생각합니다. 즉, 앞으로의 Agent 시대를 대비하여 우리가 가장 집중해야 할 부분은 개인적으로 <mark style="color:orange;">**LLM의 자율성과 의사결정**</mark>이라고 생각되는 것 같네요😊&#x20;

