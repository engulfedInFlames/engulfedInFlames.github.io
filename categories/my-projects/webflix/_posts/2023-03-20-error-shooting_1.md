---
title: WEBFLIX Error Shooting (1)
tags: [React, TS, Error]
sidebar:
  nav: categories
permalink: "/categories/my-projects/webflix/error-shooting_1"
article_header:
  type: cover
  image:
    src: imgs/my_projects/webflix/webflix_logo.png
---

<!--more-->

&nbsp;&nbsp; IntrinsicAttributes Error가 발생했습니다. 이 Error는 자식 컴포넌트에 Props로 데이터를 보낼 때, 자식 컴포넌트에서 해당 데이터에 대한 Type의 제대로 정의하지 않을 때 발생합니다. 근데 문제는... 무슨 에러인지 알면서도 어디가 잘못됐는 지를 모르겠다는 것입니다. 구글링해서 비슷한 사례를 여럿 찾아 봤음에도, 제 코드의 어디가 잘못됐는 지 못 찾겠습니다. 최후의 보루로 GPT에게 코드 전문을 올려 물었으나, *"I don't see any other issues in the code snippets you provided."*라는 답변을 받았습니다. GPT도 답변을 해주지 못해 정말 안타까우나 (속이 타나) ,해결 방법은 좀 더 궁구해 봐야 하니, 우선 현재 상황부터 재검토하겠습니다.

---

&nbsp;&nbsp; 현재 상황은 이렇습니다. 밑의 이미지처럼 개별 영화들이 나열된 슬라이더 하나를 구현했고, 넷플릭스 홈 화면처럼 슬라이더를 여럿 생성하기 위해, 해당 슬라이더를 컴포넌트로 만들려고 하는 중입니다.

<br/>

<div align="center">
<img src="/imgs/my_projects/webflix/webflix_slider.png" alt="webflix slider capture" width="100%"/>
</div>

<br/>

&nbsp;&nbsp; 그래서 Slider라는 이름의 tsx 파일을 생성하고, 거기에 필요한 코드를 복사-붙여넣기를 완료했습니다. 단 한 가지 빼구요. 바로 앞서 언급한 자식 컴포넌트의 Props 문제입니다.
