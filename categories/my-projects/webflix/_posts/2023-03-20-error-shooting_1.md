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

<br/>

### 오류 발생

> Property 'movies' does not exist on type 'IntrinsicAttributes & { slot?: string &#124; undefined; style?: MotionStyle &#124; undefined; title?: string &#124; undefined; animate?: boolean &#124; TargetAndTransition &#124; AnimationControls &#124; VariantLabels &#124; undefined; ... 307 more ...; ignoreStrict?: boolean &#124; undefined; } & { ...; } & { ...; }'.

<br/>

&nbsp;&nbsp; IntrinsicAttributes Error가 발생했습니다. 이 Error는 부모 컴포넌트로부터 자식 컴포넌트에 Props로 데이터를 보낼 때, 자식 컴포넌트에서 해당 데이터에 대한 Type을 제대로 정의하지 않을 때 발생합니다. 근데 문제는... 무슨 에러인지 알면서도 어디가 잘못됐는 지를 모르겠다는 것입니다. 구글링해서 비슷한 사례를 여럿 찾아 비교해봤음에도, 제 코드의 어디가 잘못됐는 지를 찾을 수 없었습닏다. 최후의 보루로 GPT에게 코드 전문을 올려 물었으나, *"I don't see any other issues in the code snippets you provided."*라는 답변을 받았습니다. GPT도 답변을 해주지 못해 정말 안타까운 상황이나 (속이 타나) ,해결 방법은 좀 더 궁구해 봐야 하니, 우선 현재 상황부터 재검토하겠습니다.

---

&nbsp;&nbsp; 현재 상황은 이렇습니다. 밑의 이미지처럼 개별 영화들이 나열된 슬라이더 하나를 구현했고, 넷플릭스 홈 화면처럼 슬라이더를 여럿 생성하기 위해, 해당 슬라이더를 컴포넌트화하고자 합니다.

<br/>

<div align="center">
<img src="/imgs/my_projects/webflix/webflix_slider.png" alt="webflix slider capture" width="100%"/>
</div>

<br/>

&nbsp;&nbsp; 그래서 Slider라는 이름의 tsx 파일을 생성하고, 거기에 필요한 코드를 복사-붙여넣기 했습니다. 그리고 부모 컴포넌트에서 컴포넌트화한 Slider를 렌더링하고자 했으나 Type 오류가 났습니다. 제 코드는 다음과 같습니다.

<br/>

```javascript
// 부모 컴포넌트
export interface IMovies {
  page: number;
  results: IMovie[];
  total_pages: number;
  total_results: number;
}

export interface IMovie {
  adult: boolean;
  backdrop_path: string;
  id: number;
  title: string;
  overview: string;
  poster_path: string;
}

function Home() {
  // ...codes
  return (
    <>
      <SomeJSXElements />

      <Slider movies={movies} />

      <SomeJSXElements />
    </>
  );
}

// 자식 컴포넌트 (Slider)

interface ISliderProps {
  movies: IMovies;
}

function Slider({ movies }: ISliderProps) {
  // ...codes
  return <SomeJSXElements />;
}
```

<br/>

### 오류 해결

&nbsp;&nbsp; 제 Slider.tsx에는 코드 오류가 없었습니다. 인터페이스도 잘 정의했구요. Props를 데이터 구조 분해한 것도, 인터페이스를 부여한 것도 잘못되지 않았습니다. **부모 컴포넌트가 있는 tsx 파일(이하 부모 tsx)에 제 Slider가 import 되지 않았었다는 사실만 빼면 말입니다.**
&nbsp;&nbsp; 정말 말도 안 되는 실수를 했었습니다. 부모 tsx에 제가 Slider를 컴포넌트화 하기 전에 생성해 놓은 같은 이름의 styled 요소가 있었습니다...! 그걸 지우지 않고서 부모 컴포넌트에 아무리 Slider를 렌더링해봤자, 렌더링 되는 것은 컴포넌트화 한 Slider가 아닌, 같은 이름의 styled 요소입니다. 실수를 깨닫고, 우선 Slider 컴포넌트의 export 이름을 MySlider로 변경했습니다

<br/>

<div align="center">
<img src="/imgs/my_projects/webflix/type_error.png" alt="a type error" width="600px"/>
</div>

<br/>

&nbsp;&nbsp; 오류 내용이 바뀌었습니다. IMovies 타입에 undefined를 할당할 수 없다는 내용입니다. 그렇다면 movies라는 data가 undefined일 때는 렌더링하지 않게 바꿔보겠습니다.

<br/>

<div align="center">
<img src="/imgs/my_projects/webflix/shoot_type_error.png" alt="type error shooted" width="600px"/>
</div>

<br/>

&nbsp;&nbsp; 오류가 사라졌습니다.

<br/>

<div align="center">
<img src="/imgs/my_projects/webflix/webflix_homepage.png" alt="type error shooted" width="100%"/>
</div>

<br/>

&nbsp;&nbsp; 홈 화면도 문제가 없습니다.

<br/>

<div align="center">
<img src="/imgs/my_projects/webflix/home_code.png" alt="type error shooted" width="600px"/>
</div>

<br/>

&nbsp;&nbsp; 코드도 깔끔해졌습니다.

&nbsp;&nbsp; 정말 말도 안 되는, 해서는 안 되는 실수였습니다만, 어찌 됐든 해결해서 정말 기쁘네요. 슬라이더를 컴포넌트화 했으니, 슬라이더를 여럿 더 추가하고, 기능도 확장해나가겠습니다.
