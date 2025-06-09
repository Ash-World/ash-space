---
description: 2025.01.28.
---

# UV써도 괜찮아\~ 딩딩딩딩딩

<figure><img src="../.gitbook/assets/uv_article.png" alt="" width="563"><figcaption><p>암 오케이, 암 파인 UV 써도 괜찮아~ 딩딩딩딩딩</p></figcaption></figure>

## 1. uv란?

<mark style="color:green;">**uv**</mark>는 Python의 패키지 매니저(Package Manager)로 최근에 등장한 유용한 도구입니다. 이전까지는 많은 사람들이 Poetry, Pyenv, Anaconda 등등을 사용하고 널리알려져 있지만 그에 따른 다양한 문제들이 해결되지않은것도 많았습니다. 그중에서도 가장 대두되었던 점이 바로 "속도"인데요! uv의 공식문서에는 무려 **Extreamly Fast** 라고 대놓고 언급하고 있답니다. uv속도 맛을 보니 다른 것은 손이 안가더라구요ㅎㅎ 그도 그럴것이 Rust기반으로 제작되어 매우 빠른 속도를 자랑한다고 합니다.  uv는 Ruff를 만든 Astral에서 만들었다고 하는데요, 기초적인 uv 사용법에 대해 알아보도록 합시다! 일단 아래가 uv의 공식문서입니다!

{% embed url="https://docs.astral.sh/uv/" %}
Official Documents of uv
{% endembed %}

아래 그래프가 바로 uv가 뽐내는 타 패키지&프로젝트 매니저들과의 속도차이인데요, 한눈에 봐도 엄청 빠른것을 알 수 있죠? ㅎㅎ

<figure><img src="../.gitbook/assets/image (21).png" alt=""><figcaption><p><em>Installing</em> <a href="https://trio.readthedocs.io/"><em>Trio</em></a><em>'s dependencies with a warm cache.</em></p></figcaption></figure>



## 2. uv의 등장배경

그렇다면 uv가 왜 주목받을까요?&#x20;

<figure><img src="../.gitbook/assets/image (22).png" alt=""><figcaption><p>Hell with Python Package : <a href="https://www.reddit.com/r/Python/comments/cry1fn/rant_after_going_through_literal_hell_with/">https://www.reddit.com/r/Python/comments/cry1fn/rant_after_going_through_literal_hell_with/</a></p></figcaption></figure>

위의 그림에서 볼 수 있듯 PyPI에서 pip을 거치게 되었고 현재는 pip이 거의 표준처럼 사용되고 있죠! 이후 data science 생태계가 발전하면서 conda가 부상했지만 환경의 일원화가 되지 않는다는 문제점을 혹처럼 달고 있었습니다.. 이후엔 개발자와 개발환경에 따라 poetry가 잘 사용되기도 했구요. 다만 전체적으로 온전하지 않았고 느린속도는 여전했던것이죠. uv는 이러한 환경속에서 2024년 초에 등장했습니다!!

## 2. uv의 이점

uv는 Rust로 작성된 **고속 엔진**을 필두로 앞서 이야기했던 **가상환경 생성, 의존성 관리, 파이썬 버전 관리, 패키징과 같은 기능과 다양한 서드파티 도구 실행**을 한번에 처리하고자 하였습니다. 저는 잘 모르지만 Rust에서 Cargo라는 tool 하나로 빌드, 의존성 관리, 패키징, 배포 등 모든 작업을 해결하는 것처럼, Python 환경에서도 “**uv 하나로**” 가능하게끔 디자인한 것 같네요. Astral의 CEO Charlie Marsh 및 rye 개발자 Armin Ronacher 등이 앞서 그림으로 보여진 **Python의 환경관리 문제점**을 충분히 인지하고 개선방향을 잘 해석하여 uv에 반영한 것 같습니다.

## 3. uv의 사용법

이제는 uv의 사용법에 대해 알아봅시다!

### 3.1 uv의 기본 명령어

#### 3.1.1 init

#### 새로운 uv 프로젝트 초기화, pyproject.toml 생성

```bash
$ uv init
```

#### 3.1.2 add

uv는 특정 패키지를 설치하는 경우 `pyproject.toml`과 `uv.lock` 에 자동으로 입력되어 관리됩니다.&#x20;

특정 패키지(의존성) 추가

```bash
$ uv add <패키지 명>
```

개발용 패키지(의존성) 추가

```bash
$ uv add <패키지 명> --dev
```

추가옵션이 존재하는 경우

```bash
# pip 사용시
# pip install "uvicorn[standard]"

$ uv add uvicorn --extra standard
```

requirements.txt 사용

```bash
$ uv add -r requirements.txt
```

#### 3.1.3. remove

특정 패키지(의존성) 제거

```bash
$ uv remove <패키지 명>
```

#### 3.1.4 sync

`uv.lock` 을 읽어 패키지(의존성) 설치를 진행합니다.

pyproject.toml 과 uv.lock 파일을 기준으로 가상환경 재생성 및 동기화

```bash
$ uv sync
```

만약, 개발용 의존성 없이 설치하고 싶다면 아래와 같이 사용하면 됩니다.

```bash
$ uv sync -no--dev
```

#### 3.1.5. run

가상환경을 자동 적용하여 파이썬 스크립트 실행

```bash
$ uv run <파이썬 파일>
```

#### 3.1.6. export

requirements.txt를 만들기위해서 아래와 같이 사용하면 됩니다.

```bash
# requirements.txt에 의존성을 기록하는데 hash값 없고 dev는 제외
uv export -o requirements.txt --no-hashes --no-dev

# requirements-dev.txt에 의존성을 기록하는데 hash값도 있고 dev도 포함
uv export -o requirements-dev.txt
```



### 3.2 uv로 가상환경 사용

저와 같이 기존에 conda를 사용했었고 가상환경의 이원화가 꼭 나쁘다고 생각하지 않는분들도 계실 수 있습니다.

그래서 찾아본 uv를 이용한 가상환경 사용법인데요 ㅎㅎ 같이 살펴보시죠!

먼저, 아래 명령어를 통해 가상환경을 만듭니다.

```bash
$ uv venv <가상환경 이름> --python <python 버전>
```

그 후 새롭게 만든 가상환경에 특정 패키지를 설치하고 싶을땐 아래와 같이 사용하면 됩니다.

```bash
$ uv pip install <설치 패키지 이름>
```

이후 가상환경을 실행시킬때와 종료할 때 명령어는 아래와 같이 사용하면 됩니다.

```bash
# macOS / Linux
$ source .venv/bin/activate

# Windows
$ .venv\Scripts\activate
```

```
$ deactivate
```

***

_<mark style="color:purple;">**P.S.**</mark>**&#x20;****Pycharm IDE의 latest 버전인 2024.3.2 버전 (2025년 1월 28일 빌드)으로 업데이트 하시면 uv를 지원하도록 업데이트되어있습니다! 개인적으로 만족하며 사용중이니 관심있으신분들은 한번 사용해보세요!!**_

<figure><img src="../.gitbook/assets/image (124).png" alt=""><figcaption></figcaption></figure>
