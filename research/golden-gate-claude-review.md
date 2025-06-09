---
description: 2024년 11월 3일
---

# Golden Gate Claude Review

<figure><img src="../.gitbook/assets/image (44).png" alt=""><figcaption><p>Antropic News : Goldedn Gate Claude</p></figcaption></figure>

**UPDATE** : "Golden Gate Claude"는 연구 데모로 24시간 동안 온라인 상태였으며, 현재는 더 이상 이용할 수 없습니다. Claude의 기능 활성화 및 해석 가능성에 대한 저희 연구에 대해 더 알고 싶으시다면, [이 게시물](https://www.anthropic.com/news/mapping-mind-language-model) 또는 전체 [연구 논문](https://transformer-circuits.pub/2024/scaling-monosemanticity/index.html)을 확인해 주세요. (해당 내용에 대해서 각각 따로 리뷰하겠습니다.)

화요일에 저희는 대규모 언어 모델 해석에 관한 새로운 주요 연구 논문을 발표했습니다. 이 논문에서는 AI 모델인 _**Claude 3 Sonnet**_&#xC758; 내부 작동 방식을 탐구하기 시작했습니다. Claude의 “mind” 속에서는 모델이 관련 텍스트를 읽거나 관련 이미지를 볼 때 활성화되는 수백만 개의 개념들이 발견되었으며, 저희는 이를 “features”이라고 부릅니다.

그 중 하나가 바로 Golden Gate Bridge에 대한 개념이었습니다. Claude의 신경망에는 샌프란시스코의 이 유명한 랜드마크를 언급하거나 사진을 볼 때 활성화되는 특정 뉴런 조합이 있음을 발견했습니다. 이러한 기능을 식별할 수 있을 뿐만 아니라, 활성화 강도를 조절하여 Claude의 행동에서 나타나는 변화를 확인할 수도 있습니다.

저희 연구 논문에서 설명한 바와 같이, "Golden Gate Bridge" 기능의 강도를 높이면 Claude의 응답이 금문교에 집중하기 시작합니다. 질문이 직접 관련이 없더라도, 대부분의 답변에서 금문교를 언급하게 됩니다.

예를 들어, 이 "Golden Gate Bridge Claude"에게 10달러를 어떻게 쓸지 묻는다면, golden gate bridge를 건너면서 통행료를 내라고 추천할 것입니다. 사랑 이야기를 써달라고 하면, 안개 낀 날 사랑하는 다리를 건너고 싶어하는 자동차의 이야기를 들려줄 겁니다. 만약 자신이 어떻게 생겼을지 상상해보라고 묻는다면, golden gate bridge처럼 생겼다고 상상할 가능성이 큽니다.

잠시 동안, 누구나 이 모델과 상호작용할 수 있도록 공개합니다. claude.ai에서 “금문교 Claude”와 대화해 볼 수 있으며, 오른쪽에 있는 금문교 로고를 클릭하면 됩니다. 다만, 이 모델은 연구 시연용이므로, 예상치 못한때 당혹스러운 반응이 나타날 수도 있다는 점을 유의해 주세요.

저희의 목표는 해석 가능성 연구가 실제로 어떤 영향을 미칠 수 있는지 보여드리는 것입니다. Claude 내에서 이러한 기능을 발견하고 조정할 수 있다는 사실은 대규모 언어 모델의 작동 원리를 이해하기 시작했다는 자신감을 줍니다. 이것은 단순히 모델에게 역할극을 요청하거나, Claude에게 다리라고 상상하라고 명령하는 추가 텍스트를 입력에 붙이는 “시스템 프롬프트” 방식이 아닙니다. 또한, 전통적인 “미세 조정” 기법 혹은 추가 학습 데이터를 사용해 기존 모델의 행동을 수정하는 또 다른 블랙박스를 만드는 방식도 아닙니다. 이것은 모델의 내부 활성화 구조에서 가장 기본적인 부분을 정밀하게 조정하는 작업입니다.

논문에서 설명했듯이, 동일한 기술을 사용해 위험한 컴퓨터 코드, 범죄 활동, 기만과 같은 안전 관련 기능의 강도를 조절할 수도 있습니다. 추가 연구를 통해 이 작업이 AI 모델을 더욱 안전하게 만드는 데 기여할 수 있을 것이라고 믿습니다.

***

_※ 원문 :_ [_https://www.anthropic.com/news/golden-gate-claude_](https://www.anthropic.com/news/golden-gate-claude)

