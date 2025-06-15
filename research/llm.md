---
description: 2025.05.05.
---

# LLM의 답변 컨트롤을 알아보자!

<figure><img src="../.gitbook/assets/그림1.png" alt="" width="375"><figcaption><p>에휴..</p></figcaption></figure>



흔히 이런말을 들어본적이 있을 것이다.&#x20;

> **" LLM이 응답을 할 때 temperature가 높으면, 할루시네이션이 높은 답변이 나와!! "**

이 말은 과연  올바른 말인지 생각해본적이 있나? 필자의 경우 무심코 **"그치, 그말이 맞지"**&#xB77C;고 오랫동안  생각했었던 것 같다. 오늘은 방금말한 내용이 정말 올바른 말인지 알아보고, 상황에 따라 Temperature값, 그리고 그에 준하는 다른 파라미터들을 어떻게 적절하게 조절하면 되는지 한번 생각해보자.

## Ⅰ. LLM이 답변을 생성하는 원리

일단 가장 먼저 LLM이 답변을 생성하는 원리에 대해 짚고 넘어가자. \
아주 단순하게 비유하면 <mark style="color:green;">**LLM은 주어진 말에이어서 나오기 가장 적당한 단어를 LLM이 알고있는 단어사전에서 선택하는 행동을 반복하는 고성능 프로그램**</mark>이다. 그리고 단어를 선택하는 근본적인 원리가 확률분포이고, 이 확률분포에 영향을 미치는 대표적인 파라미터 값이 **Temperature, Top-K, Top-P**이다. 앞서 언급한 것을 잘 생각하며 아래과정을 살펴보자.

### 1. 입력 & Logit 값 생성

만약 LLM의 입력 값으로 아래와 같은 문장이 들어왔다고 가정해보자.

```
The cat is
```

들어온 입력 컨텍스트에 기반해 LLM은 모든 토큰에 점수를 매기고 이 점수들을 로짓(Logit)값 이라고 한다.&#x20;

즉, 입력된 "The cat is"라는 문장이 들어오면 아래와 다음 토큰에 대해 아래와 같은 Logit값이 생성된다고 볼 수 있다.

| Next Token | Logit Value |
| ---------- | ----------- |
| on         | 0.40        |
| sleeping   | 0.30        |
| black      | 0.10        |
| the        | 0.05        |
| ...        | ...         |

### 2. Temperature 연산 & 확률분포 생성

그 다음 Next Token별 logit값을 temperature$$(T)$$로 나누어 정규화한다.&#x20;

만약, $$T<1$$라면 next token에 대한 분포를 더 뾰족하게 만들어주며 높은 logit값을 가진 token은 더 높게, 낮은 logit값을 가진 token은 더 낮게 만들어준다. 즉, next token의 후보군이 줄어들게 되어 다양성이 낮아진다.

한편, $$T>1$$라면 next token에 대한 분포를 더 평탄하게 만들며 token들의 logit값에 대한 차이를 줄이게 된다. 즉,  더 다양한 token들이 next token의 후보군으로 존재할 수 있다.&#x20;

이렇게 temperature를 이용한 정규화를 수행한 후 $$\text{softmax}$$를 취해 확률분포로 만들어 준다. 아래 예시를 보면 더 직관적으로 알 수 있다.

#### 2-1. 원래 Logit

| Token    | Logit |
| -------- | ----- |
| on       | 3.2   |
| sleeping | 2.7   |
| black    | 1.1   |

#### 2-2. Temperature가 0.5인 경우

| Token    | Logit (temperature 정규화) |
| -------- | ----------------------- |
| on       | 3.2/0.5 = 6.4           |
| sleeping | 2.7/0.5 = 5.4           |
| black    | 1.1/0.5 = 2.2           |

#### 2-3. 확률분포 생성

| Token    | P(token) |
| -------- | -------- |
| on       | 0.722    |
| sleeping | 0.267    |
| black    | 0.011    |

### 3. Filtering

token별로 확률분포를 만들고 바로 내뱉는 것은 아니다. LLM의 Inference에는 워낙 많은 hyperparameter가 존재하기 때문에 이들을 고려하게 된다. 그중에서 `top p` , `top k` 라는 parameter들은 직접적으로 token을 선별하는데 관여하는데 각 방식에 차이가 있다.

#### 3-1. Top-K 필터링

top k 필터링은 확률이 높은 상위 k개의 token만 후보로 남기고 나머지는 제거하는 방식이다. 제거 후 남은 token들 사이에서 확률분포를 재계산한다. 아래 예시를 살펴보자. $$top-k = 2$$인 경우이다.

상위 2개 token의 확률 총합인 `0.722+0.267 = 0.989` 를 이용해 재정규화 한다.

| Token    | P'(token)           |
| -------- | ------------------- |
| on       | 0.722/0.989 = 0.730 |
| sleeping | 0.267/0.989 = 0.270 |
| black    | -                   |



#### 3-2. Top-P 필터링

top p 필터링은 Nucleus Sampling이라고도 불리는데 확률분포순으로 sorting후 누적확률이 p를 넘지않도록 후보군을 제한한 뒤 그 안에서 확률분포를 재계산한다. 아래 예시를 살펴보자. $$top-p = 0.8$$인 경우이다.

`top-p = 0.8` 이므로 누적확률이 0.8이하인 token까지만 포함한다.

| Token    | Probablity | P'(token) |
| -------- | ---------- | --------- |
| on       | 0.722      | 1.0       |
| sleeping | 0.989      | 0.0       |
| black    | 1.000      | 0.0       |

{% hint style="warning" %}
이때, top-p의 임계치값이 낮은경우 가장 확률이 큰 token 1개가 남는다.
{% endhint %}

### 4. Sampling

parameter를 사용한 token selection에 이어 실제 답변 token으로 내보내는 추가단계까지 알아보자.

우리는 이제까지 Input context에 대해 temperature 정규화, Filtering을 거쳐 token의 정규화된 확률분포를 확보하였다. 실제 자연어 토큰으로 decoding하여 내보내기 직전에 2가지 방식중 1개를 선택할 수 있는데 아래와 같다.

#### 4-1. Greedy Sampling

Greedy Sampling은 확보한 token들의 확률분포에서 가장 높은 확률값을 가지는 token을 next token으로 내보내는 방식을 말한다. 항상 같은 입력에 대해 같은 출력을 내뱉게 된다.

#### 4-2. Random Sampling

Random Sampling은 확보한 token들의 확률분포에서 각 token들의 확률값에 비례해 무작위로 next token을 선택한다. 같은 입력에 대해 다른 결과가 나올 수 있다.

