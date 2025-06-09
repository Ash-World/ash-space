---
description: 2024년 1월 23일
---

# Moducon 2023 행사 후기

<figure><img src="https://blog.kakaocdn.net/dn/bBXLTB/btsDQelFu3D/0EJ91iAArowr1oWCGugnK0/img.png" alt=""><figcaption><p>MODUCON 2023</p></figcaption></figure>

여러분, 여러분은 'MODUCON(모두콘)'을 아시나요?\
MODUCON(모두콘)’은 모두의연구소의 연구원분들이 1년 동안의 연구 활동을 공유하는 모두연만의 특별한 축제에요. 인공지능 세미나 및 인공지능 분야 전문가분들과 네트워킹 할 수 있는 자리로 지난 몇년간 꾸준히 이어져오고있는 행사랍니다. 저도 굉장히 좋아하는 행사인데요, 매번 직접참가하지 못하지만 추후 올라오는 영상들을 꾸준히 보고있답니다 ㅎㅎ 이번에는 특히나 제가 취업준비를 하는 시기이기도 하고 많은 성장을 이룬 시기이기때문에 듣고 알아본 내용을 회고로써 남겨보려고 합니다!\


당연하게도 모두의 연구소 Youtube 채널에서 다시보기를 할 수 있으니 관심이 있으시다면 한번씩 꼭 보세요!!! 좌표 남겨놓을게요!!\
👉 [https://www.youtube.com/@modulabs\_/videos](https://www.youtube.com/@modulabs_/videos)

[\
모두의연구소Share Value, Grow Together 누구나 함께 지식을 공유하고 성장하는 연구 문화를 전파합니다www.youtube.com](https://www.youtube.com/@modulabs_/videos)

\


본격적으로 리뷰를 한번 해볼까요? 저는 개인적으로 NLP, HCI에 관심이 가장 많지만 부가적으로 LLM, PLM, Korean LM등등에도 관심이 많습니다! 그래서 가장인상깊었던게 '언어모델은 어떻게 평가해야 할까? - 손규진' 과 '모두를 위한 한국어 오픈 LLM - 이준범' 영상이었는데요! 요 2가지를 중점적으로 리뷰해볼게요.

[https://www.youtube.com/watch?v=5xqbyZZX2cw](https://www.youtube.com/watch?v=5xqbyZZX2cw)

<figure><img src="https://scrap.kakaocdn.net/dn/KVJei/hyU8QgOHV8/7GTeXdgKBHB6Xggc2rPjpk/img.jpg?width=1280&#x26;height=720&#x26;face=878_254_1018_406" alt="" height="484" width="860"><figcaption><p>MODUCON2023 : 언어 모델은 어떻게 평가해야 할까? - 손규진</p></figcaption></figure>

최근에 구글이 제미나이를 선보이면서 했던 에피소드중에 이런말이 있었죠!\
"우리가 GPT-4보다 성능이 좋다!" 요 부분에서 많은 리서쳐들이 어떻게 평가했는지를 중점적으로 바라보았다고 합니다.\
근데 사실 요 언어모델에 대한 평가가 Frontier LAB수준에서도 굉장히 모호한 기준을 가지고 있다고 해요!\
언어모델을 Academic하게 바라보면 당연 선보일수있는 평가방법이 존재하겠지만 이마저도 너무 오래되었고, 더 나아가서 Industrial면에서는 그 평가방식이 맞지않는 경우가 엄청 많다는 거죠!&#x20;

<figure><img src="https://blog.kakaocdn.net/dn/wFCKI/btsDKAi1Fxo/14q9JZQcWMKWitF54dwt50/img.png" alt=""><figcaption><p>한국어 언어모델 평가 데이터셋</p></figcaption></figure>

위에 이미지는 세션영상 일부를 캡쳐한건데요, 보시면 알겠지만 한국어는 특히나 더 언어모델에 대한 평가가 어려울 수 밖에 없음을 나타내주고 있어요. 해당 강연의 주 시사점은 다음과 같은 크게 2가지로 말할 수 있는데&#x20;

* 다양한 포맷으로 평가해서 Average Score 도출
* 정성적인 평가 (실제 결과물을 보고 비교해보기)

라고 합니다. 휴.. 한국어의 연구영역자체가 작은것도 있지만 발전속도도 많이 더디다고 해요.\
굉장히 공감하는게, 이번에 프로젝트로 RAG를 활용한 검색엔진 개발에서 주어진 Document를 한국어 문장임베딩 해야하는 경우가 있었어요.  그때도 저희가 임베딩모델이 어떤 성능을 잘 낼 수 있는지 너무 기준을 잡기 어려웠거든요. 그때 저와 저희팀이 생각한것이 언어모델의 성능 평가에 대한 기준이 명확하지 않으니 최소한 Downstream Task만이라도 평가해보자! 라는 생각으로 진행했던게 있습니다. 조금 다르지만 평가기준이 모호했던 상황에서 비슷한 진행방식을 가져 올바른 방식으로 프로젝트를 수행했다는 생각에 뿌듯하네요ㅎㅎ

요기서도 손규진 연사분께서 궁극적으로 말씀하시는 바가 벤치마크를 만드는것이 중요하고, Industrial한 측면에선 Domain Adaptation 이 필히 수행될 것, 그이후엔 Downstream task(특정한 테스크)에 따라 평가를 잘하는게 중요하다고 합니다.&#x20;

***

[https://www.youtube.com/watch?v=mtrtqEk\_dUE](https://www.youtube.com/watch?v=mtrtqEk_dUE)

<figure><img src="https://scrap.kakaocdn.net/dn/l4KQx/hyU82O5eXT/qtSazCG4pKaT2NZA3Zv9e1/img.jpg?width=1280&#x26;height=720&#x26;face=976_298_1072_402" alt="" height="484" width="860"><figcaption></figcaption></figure>

어후 두번째 연사는 아주 제가 존경하는 분께서 진행하셨는데, 바로 이준범님께서 하신 강연입니다😆

<figure><img src="https://blog.kakaocdn.net/dn/b8LFQV/btsDQg4Zlnp/wdBCOtLIvaR83w7MPH0KwK/img.png" alt=""><figcaption><p>Pretrained Model vs Finetuned Model</p></figcaption></figure>

제가 또 복지 도메인 특화 언어모델(WelSSiSKo) 개발 프로젝트를 할 당시에 이준범님 자료를 많이 참고하였는데요, 최근에 또 이분께서 만드신 Llama-2-ko를 기반으로 다시 finetune한 모델을 만드는 작업에 있답니다\~ 너무  들떴나요,,ㅎㅎ\
요 강연에선 Llama-2-ko/en을 제작하는 제작기를 소개해주셨는데 과정하나하나가 너무나도 값진 내용들이 많았어요.

<figure><img src="https://blog.kakaocdn.net/dn/K7YVb/btsDQXqGciD/gvShKaVKbm9ospPMy1JGqK/img.png" alt=""><figcaption><p>Llama-2-ko/en 개발기</p></figcaption></figure>

요 내용들은 백문이 불여일견이라고 직접 봐보시는걸 강력하게 추천드려요!! \
그 밖에도 다양한 세션이 많은데 개인적으로 흥미롭게 봤던것은 아래 몇가지 좌표를 남겨놓을테니 한번 보시면 좋을것 같습니다 :)

[https://youtu.be/Iy\_jHC1JMtg?si=aptWFupYtmLyFa8m](https://youtu.be/Iy_jHC1JMtg?si=aptWFupYtmLyFa8m)

<figure><img src="https://scrap.kakaocdn.net/dn/EPNHv/hyU8S6Mcgv/xH0CH2eHGIbgGedXNB8yDk/img.jpg?width=1280&#x26;height=720&#x26;face=894_336_1042_496" alt="" height="484" width="860"><figcaption></figcaption></figure>

[https://youtu.be/Xyg8RK3Pc8g?si=kSeHnU29P8YoWVxx](https://youtu.be/Xyg8RK3Pc8g?si=kSeHnU29P8YoWVxx)

<figure><img src="https://scrap.kakaocdn.net/dn/DHu9E/hyU80KuKF8/dgKpQwPcvZ55Pu4i35ZUIK/img.jpg?width=1280&#x26;height=720&#x26;face=900_302_1028_442" alt="" height="484" width="860"><figcaption></figcaption></figure>

[https://youtu.be/Nfwm2Zkw-gE?si=rEEcu3Pg9dejxLqW](https://youtu.be/Nfwm2Zkw-gE?si=rEEcu3Pg9dejxLqW)

<figure><img src="https://scrap.kakaocdn.net/dn/ZspKV/hyU813JvVt/hIx72R71f0MnsPZnWoLjXk/img.jpg?width=1280&#x26;height=720&#x26;face=904_280_1044_432" alt="" height="484" width="860"><figcaption></figcaption></figure>

[https://youtu.be/6VogP1B1UJw?si=OVX77KkG0zbtl7Uu](https://youtu.be/6VogP1B1UJw?si=OVX77KkG0zbtl7Uu)

<figure><img src="https://scrap.kakaocdn.net/dn/blZG7b/hyU8VvEVuM/dXMXYdozKTh6BJyDFSg9x0/img.jpg?width=1280&#x26;height=720&#x26;face=0_0_1280_720" alt="" height="484" width="860"><figcaption></figcaption></figure>

또한, 보면서 메모한 노션도 올려둘테니 참고해보세요@@\
[https://different-uranium-6e0.notion.site/MODUCON-2023-80774f697aae45b6abf85bf344d0b19d?pvs=4](https://different-uranium-6e0.notion.site/MODUCON-2023-80774f697aae45b6abf85bf344d0b19d?pvs=4)

[\
MODUCON 2023 | Notion ※ 다시보기 링크 👉 https://www.youtube.com/@modulabs\_/videosdifferent-uranium-6e0.notion.site](https://different-uranium-6e0.notion.site/MODUCON-2023-80774f697aae45b6abf85bf344d0b19d?pvs=4)

\
