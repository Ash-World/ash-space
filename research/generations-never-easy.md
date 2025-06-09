---
description: 2025.03.01.
---

# Generations Never Easy

<figure><img src="../.gitbook/assets/image (125).png" alt="" width="375"><figcaption><p>Generations Never Easy</p></figcaption></figure>

오늘은 재미난 내용을 들고왔다. 큰 주제는 **"Generations Never Easy"**&#xC778;데, 말 그대로 "생성은 절대 쉽지 않다."라는 의미의 아티클이다. 최근 Prompt에 대한 강연을 다녀온 후 옛날부터 생각했었던 아이디어에 큰 불이 지펴진것 같았고, 이를 직접 구현해보고자 했다. 바로 주어진 문맥을 통해 meta information(=메타 정보)를 추출하는 것이다. 이게 왜 awesome한 작업인지 말을 얹어보겠다. 우리는 일상생활에서 주 언어로 된 대화를 하거나 글로 표현된 내용을 항상 접하면서 살아간다. 여기서 주 언어란, 모국어를 말하고 표현되지 않은 Hidden Meaning에 대해 생각할 필요가 없을만큼 자연스러운 것이다.&#x20;

앞서 언급한 필자가 과거에서부터 생각한 아이디어란, LLM과 RAG Approach를 통해 High-Quality Synthetic Dataset을 생성하는 것인데, 이 역시 많은 어려움이 존재한다. 아직, 실험중인 내용도 있지만 일단 초기 실험결과를 공유해보고자 한다. 필자가 예제로 사용한 데이터를 아래와 같다. Paragraph가 sentence 단위로 주어진 list가 존재하고, 이를 입력값으로 넣어 meta information을 추출하였다.

예제로 케로로에 대한 Paragraph는 다음과 같다.

Keroro is the leader of a platoon from the Keron Army, an alien frog species aiming to conquer Earth.\
However, instead of carrying out his invasion plans, he becomes obsessed with building Gunpla models\
and watching anime, leading him to settle down with a human family. \
with his unique humor and quirky antics, he often causes trouble, but in the end, his strong bond with \
his comrades and warm friendships make him a lovable character.

위 paragraph로부터 생각한 작업을 수행하기 위해 아래와 같은 형태로 변형시켜주었다.

```python
# Sample Data List Version
[
  'Keroro is the leader of a platoon from the Keron Army, an alien frog species aiming to conquer Earth.',
  'However, instead of carrying out his invasion plans, he becomes obsessed with building Gunpla models and watching anime, leading him to settle down with a human family.',
  'With his unique humor and quirky antics, he often causes trouble, but in the end, his strong bond with his comrades and warm friendships make him a lovable character.'
]
```

분석한 결과를 보자!

```json
{
    "sentence": "Keroro is the leader of a platoon from the Keron Army, an alien frog species aiming to conquer Earth.",
    "syntax_tree": {
        "Topic": "The sentence introduces Keroro as a commanding figure leading a military unit within an extraterrestrial context, specifically as the leader of a platoon from the Keron Army, which is identified as an alien frog species with ambitions to conquer Earth.",
        "Causation": {
            "Cause": "The establishment of the Keron Army by an ambitious alien frog species sets the stage for a structured military organization.",
            "Effect": "Keroro is designated as the leader of a platoon, emphasizing his pivotal role in the army’s conquest agenda."
        },
        "5W1H": {
            "Who": "Keroro",
            "What": "Is identified as the leader of a platoon from the Keron Army",
            "When": "Not specified in the sentence",
            "Where": "Keroro belongs to the Keron Army, with Earth as the intended target of conquest",
            "Why": "To illustrate the organizational structure and conquest motives of the alien species",
            "How": "By assigning leadership roles within a hierarchical military framework"
        }
    }
}
```

정말 완벽하게 meta information을 추출했다고 볼 수는 없지만 초안인 점을 감안하면 형태는 완성되었다고 생각했다. 이제, 이를 바탕으로 주어진 paragraph에 대한 meta information을 추출한 것을 보자.

```json
"paragraph": {
    "Topic": "The sentence captures a nuanced transformation where Keroro, once an ambitious military leader with extraterrestrial conquest plans, is now shown as a multifaceted character. He displays a quirky, mischievous side that sometimes causes trouble, yet his strong social bonds and domestic interests render him endearing.",
    "Causation": {
        "Cause": "Keroro’s shift from his aggressive, conquest-driven mindset to embracing a lifestyle filled with personal hobbies, quirky antics, and humor inadvertently leads to episodes of trouble.",
        "Effect": "His troubles are offset by the strong, genuine relationships and domestic stability he builds, making him ultimately lovable despite his mischievous behavior."
    },
    "5W1H": {
        "Who": "Keroro – the male character transitioning from a formidable military leader to a person defined by his quirky behavior and warm social connections.",
        "What": "He abandons rigid military ambitions in favor of engaging in personal hobbies, humorous antics, and developing strong bonds that sometimes create trouble but ultimately foster a lovable persona.",
        "When": "Not explicitly specified; the transformation is implied to occur over an undefined period marked by evolving priorities.",
        "Where": "On Earth, contrasting his previous extraterrestrial military setting with a modern, domestic environment enriched by social interactions.",
        "Why": "A profound change in his interests and priorities drives him to forsake a life of conquest for one centered around personal fulfillment and meaningful relationships.",
        "How": "By transitioning from organized military strategies to pursuing leisure activities, humorous endeavors, and nurturing interpersonal bonds that redefine his identity."
    }
}
```

굉장히 기뻤다! 특히 When에 대한 요소에 특정할 수 없다고 나온것을 보면 나의 접근방식이 유효한 결과였다는 것을 실감했다. 물론, 성능면에서 더 고도화해야하고, hallucination을 잘 잡아줘야하는 문제나, 사용하는 모델의 성능이 뒷받침이 되어야하기 때문에 모든 상황에서 사용할 수는 다양한 이슈가 존재한다. 또한, Api 비용적인 측면에서도 만만치 않다. 그러나, 그런부분은 성능이 확보된 후에 최적화를 수행하면서 잡아가면 되는 부분이다.&#x20;

해당 아티클에서 차츰차츰 업데이트하겠다 🤣
