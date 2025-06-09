---
description: 2024년 1월 30일
---

# 모두를 위한 한국어 오픈액세스 언어모델 못다한 이야기 (feat. 모두연) #2

<figure><img src="https://blog.kakaocdn.net/dn/bf576V/btsEJdrB5NG/Q94rwefyHYSAoTek5xkfB0/img.png" alt=""><figcaption><p>모두콘 2023에 이어 모두연에 등장한 "한국어 오픈액세스 언어모델" 강연</p></figcaption></figure>

본격적인 이준범님의 프로젝트 소개 가보자궈\~\~!\~!\~!

> 1\. 회사 및 프로그램 소개\
> 2\. 강연\
> &#x20; a. 한국어 오픈액세스 LM 동향\
> &#x20; b. Llama-2-Ko/Yi-Ko/Solar-Ko 등 프로젝트 소개\
> &#x20; c. 학습 데이터 수집, 전처리 과정, 토크나이저 제작 과정 및 시행착오\
> &#x20; d. 오픈모델의 사용성과 라이센스\
> 3\. Q\&A

***

<figure><img src="https://blog.kakaocdn.net/dn/LZG5o/btsEFkMi4mQ/nVD9yYKMPK0p6dZVPZWjoK/img.png" alt=""><figcaption><p>본격적인 소개 가보자궈~~</p></figcaption></figure>

<figure><img src="https://blog.kakaocdn.net/dn/bfzXth/btsEFgpIMCr/FStskfy936MY4sFqwTOKik/img.png" alt=""><figcaption><p>한국어 데이터~~!</p></figcaption></figure>

그쵸.. 항상 데이터가 중요하죠. 공개되어있는 한국어 데이터의 경우 Ai Hub, 모두의 말뭉치, 위키피디아 등등 많지만 은근 AI Hub가 짱짱하다고 하는데요! 혹시 프로젝트 하시는 분들은 참고하시면 좋겠네요ㅎㅎ

<figure><img src="https://blog.kakaocdn.net/dn/0o9dg/btsEFNOj3Gm/fA7DZiVGxBP6HrQXWW8Mg0/img.png" alt=""><figcaption><p>이거 굉장히 신기하게 봤어요!</p></figcaption></figure>

<figure><img src="https://blog.kakaocdn.net/dn/bTROzP/btsEFd7yfOp/yc2D2OJlqhz0uYjxEWzzmK/img.png" alt=""><figcaption><p>오...!</p></figcaption></figure>

<figure><img src="https://blog.kakaocdn.net/dn/wI0Is/btsEEH2oglk/AHoLoT9S1kWIBYx0lvG1UK/img.png" alt=""><figcaption><p>오호...</p></figcaption></figure>

<figure><img src="https://blog.kakaocdn.net/dn/c9G9sn/btsEGLJclfT/HkKnMuR2IWL7cuSbWpn1v0/img.png" alt=""><figcaption></figcaption></figure>

직접 from scratch 식으로 만들었다고 하시는데, 와우... 대단하군요..! 특히 데보션에 이와 관한 내용이 잘 정리가 되어있다고 하니 함 봐야겠어요! 좌표는 아시죠? 밑에 달아놓겠습니다ㅎㅎ

사실 토크나이저를 스크래치 방식으로 만드는게 저도 생소한지라.. 설명해주신것이 온전히 다 이해가 되진 않네요.. 특히 mean embedding은 조금 더 공부해봐야겠어요..ㅎㅎ&#x20;

<figure><img src="https://blog.kakaocdn.net/dn/dyNGCc/btsEHJjZR72/G4cTJKXhRSDKzWPePMRKsk/img.png" alt=""><figcaption><p>와... 이거 비용이...ㄷㄷ</p></figcaption></figure>

<figure><img src="https://blog.kakaocdn.net/dn/bVS9rj/btsEEyxSTI3/hd9VfkxzHYVCiHO0KYB3M0/img.png" alt=""><figcaption><p>음.. 역시</p></figcaption></figure>

<figure><img src="https://blog.kakaocdn.net/dn/cb7KPH/btsEE2SDHap/jhCXhaX0tMTYWJqFxKhcZ0/img.png" alt=""><figcaption></figcaption></figure>

음.. 역시 다양한 데이터셋으로 평가하고 이것을 가지고 모델의 task 목적에 맞는것을 중점적으로 봐야한다고 하는군요.

이번에 진행했던 프로젝트 중 'WelSSiSKo'라고 하는 Welfare Domain에 DAPT한 Domain Specific한 LM을 만들었었는데요, 얼마안되서 요런 컨퍼런스를 보니 아주 감회가 새롭네요! 사실 Llama-2-ko를 이용해서 새롭게 업데이트 시키고 있는중이긴 해요 😆😆 이번에 알게 된 내용을 참고해서 더 좋은 모델을 한번 만들어 볼게요!!!&#x20;

***

👉 [Youtube 모두의 연구소 : 모두를 위한 한국어 오픈 액세스 언어모델\_못다한 이야기 - 이준범님 (데이터드리븐, AI 연구원)](https://youtu.be/ZoMWvu4RsGc?si=3T-Zkvpk6PzkIPCw)

👉 [SentencePiece를 활용한 효율적인 한국어 토크나이저 만들기](https://devocean.sk.com/blog/techBoardDetail.do?ID=164570\&boardType=techBlog#none)

[\
SentencePiece를 활용한 효과적인 한국어 토크나이저 만들기\
devocean.sk.com](https://devocean.sk.com/blog/techBoardDetail.do?ID=164570\&boardType=techBlog#none)

\
