---
description: 2025.03.09
---

# Model Context Protocol

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption><p>Model Context Protocol</p></figcaption></figure>

필자는 주로 GeekNews를 통해 최신정보를 업데이트하는 편으로 AI의 분야는 여러방면에서 빠른 발전을 이루고 있지만 이제 막 태동하고 있는 분야도 존재한다. 오늘 소개할 MCP (= Model Context Protocol)는 그중 하나로 Agent가 점점 화두가 되는 트렌드에서 앞으로 각광받을 수 있는 내용이라 소개하고자 한다.&#x20;

## Ⅰ. MCP란 무엇인가?

이전에 MCP에 대해 들어본사람은 아마 많지 않을것이다. 대중적으로 많은 사람들이 사용하는 Cursor도 MCP를 활용하여 구축된 애플리케이션이다. Anthrophic에서 24년 11월경 처음 소개한 프로토콜인 MCP는 Model Context Protocol이라고 하는 것으로 LLM 애플리케이션과 외부 데이터 소스 및 도구들 간의 원활한 통합을 가능하게 하는 개방형 프로토콜이다. AI 기반 IDE를 구축하든, 채팅 인터페이스를 개선하든, 혹은 커스텀 AI 워크플로우를 만들든 관계없이, MCP는 LLM이 필요로 하는 컨텍스트와 연결하기 위한 표준화된 방법을 제공한다. 가장 보편적인 구조는 아래와 같다.

<figure><img src="../.gitbook/assets/image (12).png" alt=""><figcaption><p>[ Fig. 01 ] General MCP Architecture</p></figcaption></figure>

우리가 보편적으로 사용하는 HTTP 프로토콜이 서로 다른 웹사이트와 브라우저가 동일한 규칙에 따라 정보를 교환할 수 있도록 하는 것처럼, MCP는 AI 세계의 HTTP 프로토콜과 같은 역할을 한다. MCP는 다양한 AI 모델이 다양한 데이터 소스와 도구에 표준화된 방식으로 연결할 수 있게 하고 이러한 표준화는 개발자들이 각각의 모델이나 데이터 소스를 위한 특수한 인터페이스를 만들 필요 없이 AI 애플리케이션을 더 쉽게 구축할 수 있게 만든다. 즉, 사용자 정의 개발이 필요없어지는 공통된 형식이라 보면 되겠다.

## Ⅱ. MCP가 왜 필요한가?

LLM은 여전히 강력한 퍼포먼스를 뽐내고 있지만 그에 따른 여러가지 제약사항도 존재한다.

* 지식의 한계와 정보 최신화의 어려움
* 전문 도메인 지식의 부족
* 외부 데이터에 액세스할 공통표준의 부재

위와 같은 고질적인 문제점을 표준 프로토콜을 제시하고 적용함으로써 어느정도 해소할 수 있다.

## Ⅲ. MCP는 어떻게 구성되는가?

MCP에는 크게 3가지 핵심구성이 존재한다.

* MCP 서버(도구 및 데이터 액세스 제공)
* MCP 클라이언트(LLM에 포함되어 MCP 서버와 통신)
* MCP 호스트(Claude Desktop, Cursor 등과 같은 LLM 및 클라이언트를 통합하는 애플리케이션)&#x20;

이 세 가지 역할이 어떻게 유기적으로 구성되는지 가볍게  살펴보자.&#x20;

### 1. MCP 서버

MCP 서버는 LLM이 사용할 수 있는 도구와 데이터 액세스 기능을 제공하는 프로그램이다. 전통적인 원격 API 서버와는 달리, MCP 서버는 사용자의 장치에서 로컬 애플리케이션으로 실행되거나 원격 서버에 배포될 수 있는것이 특징이다. 각 MCP 서버는 로컬 데이터나 원격 서비스에서 정보를 검색하는 특정 도구 세트를 제공하는데 LLM이 작업을 처리하는 동안 특정 도구를 사용해야 한다고 결정하면, MCP 서버가 제공하는 도구를 사용하여 필요한 데이터를 얻고 이를 LLM에 반환하는 구조를 가진다.

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption><p>[ Fig. 02 ] MCP Server</p></figcaption></figure>

### 2. MCP Client

MCP 클라이언트는 LLM과 MCP 서버를 연결하는 다리 역할로 LLM에 포함되어 LLM으로부터의 요청 수신, 적절한 MCP 서버로 요청 전달, MCP 서버로부터 결과를 LLM에 반환하는 등의 기능을 담당한다.

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption><p>[ Fig. 03 ] MCP Client </p></figcaption></figure>

### 3. MCP Host

Claude Desktop, IDE(Cursor 등), 또는 MCP를 통해 데이터를 액세스하고자 하는 AI 도구와 같은 프로그램은 MCP 호스트라고 부른다. 이러한 애플리케이션은 사용자가 LLM과 상호작용할 수 있는 인터페이스를 제공하면서, MCP 클라이언트를 통합하여 MCP 서버가 제공하는 도구를 사용하여 LLM 기능을 확장하게 된다.

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption><p>[ Fig. 04 ] MCP Host</p></figcaption></figure>

### 4. MCP Workflow

위의 3가지 구성요소가 포함되어 아래와 같은 Workflow를 따르게 된다.

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption><p>[ Fig.05 ] MCP Workflow</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption><p>[ Fig. 06 ] MCP Request &#x26; Response Flow</p></figcaption></figure>

이때, MCP와 외부 시스템간의 통신은 아래와 같은 구성으로 이루어진다.

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption><p>[ Fig. 07 ] MCP Protocol with External API</p></figcaption></figure>

AI 모델과 외부 시스템을 연결하는 MCP 서버를 구현할 때, MCP 구현에서 두 가지 주요 보안 과제가 존재하는데, 바로

* **인증**: 전통적인 애플리케이션과 달리, MCP 환경에서는 사용자가 외부 시스템에 접근하기 위해 전통적인 로그인 플로우(예: 사용자 이름/비밀번호, 이메일/인증 코드 등)를 통해 로그인할 수 없다.
* **MCP 서버 요청의 접근 제어**: AI 도구를 통해 시스템에 접근하는 사용자는 동일한 시스템을 직접 사용할 수 있는 사람이다. MCP 서버는 AI 도구를 통해 상호작용할 때 그들의 대리인 역할을 하며 MCP 서버에 맞는 접근 제어 메커니즘을 새로 설계하려면 막대한 노력과 비용이 필요하다.

위와 같은 문제이다. 이것에 대한 주요 해결책은 PAT(개인 엑세스 토큰)를 구현하는 것이다. PAT는 사용자가 자격 증명을 공유하거나 인터랙티브 로그인을 필요로 하지 않고 안전하게 접근할 수 있게 제공한다.

***

## Ⅳ. Insight

요즘 들어 Agent에 대한 관심이 뜨겁다. 과거와 다르게 Agent라는 것은 일종의 System적인 구성이 반드시 내포하게 된다. 앞으로의 Agent에 대한 활용성, 장기적으로 LLM의 활용 및 유지보수, 외부 데이터 및 레거시 시스템과의 마이그레이션을 생각한다면 공통표준이 생기는 것은 시간문제일 것이다. 사실 어느정도 개념화되지 않아서 그렇지 이미 많은 사람들이 유사한 방식으로 개발하고 있으리라 생각한다. 이미 개발된 클라이언트 도구들을 활용하는 기업도 있을 것이고, 온프레미스 환경에 맞게 직접 구현하는 회사도 존재할 것이다. 필자는 이것을 보고 <mark style="color:orange;">**일종의 Design Pattern**</mark>과 같다고 생각했다. 다만, Cursor라는 선례가 존재하는 만큼 활용방식이나 운용방안에 대하여 점진적 고도화가 이루어질 가능성은 충분한 것 같다. 아래 원문 링크를 남겨놓을테니 참고바란다 : [https://modelcontextprotocol.io/introduction](https://modelcontextprotocol.io/introduction)

