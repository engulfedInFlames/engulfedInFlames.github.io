---
title: Github Pages에 SPA 배포하기
tags: [SPA, Deploy, gh-pages]
sidebar:
  nav: categories
permalink: "/categories/docs/deploy/spa-gh-pages"
# article_header:
#   type: cover
#   image:
#     src: /imgs/SPA_Lifecycle.png
---

<!--more-->

<br/>

&ensp; 몇 날 며칠에 걸려, 겨우 React 기반 SPA를 배포하는 데 성공했습니다...
AWS와 Heroku 배포는 여전히 애 먹고 있다는 사실이 저를 애석하게
합니다만, React 프로젝트를 Github에 배포하고나니 다시 자신감이 붙네요.
배포에 성공했으니, 복습 겸 실패 원인들을 다시 살피도록 하겠습니다. 저를 골치
아프게 한 것은 (1) **유니콘**, (2) **Github Pages의 SPA 미지원** 크게 두
가지였습니다.

<br/>

## (1) 유니콘

<br/>

<div align="center">
<img src="https://unicornpro-web-static.s3.ap-northeast-2.amazonaws.com/static/img/logo/logo.svg" alt="unicorn app logo" width="200px"/>
</div>

<br/>

&ensp; 첫 번째 원인은 이 유니콘 앱이었습니다. 사실 이 유니콘 앱이 왜 프로젝트를
배포하는 데 걸림돌이 되는지, 그 메커니즘은 아직도 잘 모릅니다. 다만,
유니콘을 사용하면 리눅스 환경에서 프로그래밍할 때에 호스팅이 끊기다든지 하는 문제가 있다는 것을 최근에 알게 되어, 유니콘 앱을 끄고 프로그래밍을 해봤는데 그 결과... 제 컴퓨터가 문제가 아니라 이 유니콘이 문제였더군요. 자잘한 호스팅 문제는 집어치우고, 가장 심각한 문제는 SSH Key를 생성할 때 발생했습니다.

<br/>

<div align="center">
<img src="/assets/images/docs/deploy/gh-pages/ssh-key-random-art.png" alt="A random art of the SSH key generated in the terminal" width="500px"/>
</div>

<br/>

&ensp; 유니콘이 켜져 있을 때 SSH Key를 생성하면, 저 랜덤 이미지가 생성된 뒤에
터미널이 멈춰버립니다. 위 이미지 속 경로를 따라가면 SSH Key가 생성이 되어
있기는 한데, 그 Key가 Github Pages, AWS, Heroku 등 배포 서비스를 제공하는
클라우드들과 잘 연결이 됐는지는 확인할 수가 없었습니다. 특히, AWS 배포시에
계속해서 Runtime Error를 만났고, 아무리 구글링해서 해결책을 찾아본들, 해결될
리가 없었습니다. 그 사람들은 유니콘 앱을 사용하지 않았을테니까요. 문제는
바로 유니콘이었고, 유니콘을 끄고 난 뒤에는 정상적으로 SSH Key가
생성됐습니다. (이제 SSH Key를 생성했고, AWS에서 클라우드 컴퓨터를 만드는 과정까지도 해봤기
때문에 다음 포스트 주제는 AWS와 Heroku 배포 방법이 될 것입니다!)

<br/>

## (2) Github Pages의 SPA 미지원

<br/>

&ensp; 두 번째 원인은 Github Pages의 SPA 미지원입니다. Github Pages가 SPA를 미지원하기에 조금 꼼수(?)를 부려야 SPA를 배포할 수 있습니다.

<br/>

<div align="center">
<img src="/assets/images/docs/deploy/gh-pages//SPA_Lifecycle.png" alt="SPA lifecycle" />
</div>

<div align="center">
<a href="http://fewgoodengineers.com/SinglePage_app.html">[출처]
</a>
</div>

<br/>

&ensp; 일반적으로 어플리케이션은 새로운 페이지를 요청할 때마다 페이지에 맞는 정적E
자원들(HTML, CSS, JS 등)을 다운로드 합니다. 즉, 페이지를 요청할 때마다
서버로부터 새로운 HTML 파일 등을 가지고 와서 페이지 전E체를 렌더링하는
것이죠. 이를 MPA(Multi Page Application)라고 합니다. 반면, SPA는 최초 요청시
필요한 정적 자원들을 한 번에 다운로드합니다. 그리고 새로운 요청이 있을 때는
필요한 데이터만 가지고 와서 상태가 변화한 컴포넌트만을 다시 렌더링합니다.
React의 장점이 바로 이 컴포넌터 상태 관리의 유용성입니다. 페이지 전체를 다시
렌더링하는 것보다는 변화한 컴포넌트만을 다시 렌더링하는 것이 컴퓨터 연산
측면에서 득이 되기 때문입니다.

&ensp; 다만, Github Pages가 SPA를 지원하지 않기에, 최초 요청 이후에 새롭게 정적 자원들을 가지고 오려해도 (가령, 새로고침을 한다든지) 서버에서는 제공할 정적 자원이 없습니다. 그래서 클라이언트는 404 Not Found 오류를 만납니다. Github Pages에서 SPA를 배포하려면 꼼수를 부려야 합니다. 그 꼼수란 바로 이 404 Not Found 오류를 이용하는 것입니다.

<br/>

<div align="center">
<img
  src="/assets/images/docs/deploy/gh-pages/react-routing-tree.png"
  alt="routing tree by createBrowerRouter(react)" width="400px"
/>
</div>

<br/>

&ensp; 위 이미지는 예시로서 제가 배포한 사이트의 라우팅 트리입니다. URL의 Basename으로 process.env.PUBLIC_URL을 지정했기 때문에, 가령 웹페이지에 처음 접속하면 Root 페이지로 가게 되어, process.env.PUBLIC_URL + "/"라는 경로의 URL로 라우팅됩니다. (Basename을 지정하지 않으면 처음부터 404 Not Found 오류를 만납니다. Github Pages는 단순히 "/" 라는 주소를 이해하지 못하기 때문이죠.) 앞서 언급했듯이, 새로고침이라도 하게 되면 서버는 제공할 HTML이 없기에 대신 404 Not Found 오류를 알리는 404.html을 제공합니다. 이를 이용하여 404.html 내에서 스크립트를 작성하여 유효한 경로로 리다이렉트 되게끔 합니다. 구체적인 방법은 다음과 같습니다. 저는 React를 사용하기 때문에 React를 기준으로 설명하겠습니다. 해결방법은 다음을 참조했습니다. [&rarr;&nbsp;Single Page Apps for Github Pages](https://github.com/rafgraph/spa-github-pages#readme)

### Step 1 : 404 HTML 생성

<div align="center">
<img
  src="/assets/images/docs/deploy/gh-pages/public_404.png"
  alt="routing by react-hook-createBrowerRouter" width="400px"
/>
</div>

<br/>

&ensp; React 앱을 생성했을 때 루트 디렉토리에 public 폴더가 생성됩니다. public 폴더에는 index.html이 기본으로 들어가 있는데, 그곳에 404.html 파일을 생성합니다. 404 오류 발생시 서버 측에서 이 public 폴더 안에 있는 404.html을 가지고 오게끔 하는 것입니다.

<br/>

### Step 2 : 404.HTML에 리다이렉트를 위한 스크립트 작성

&ensp; 다음으로 404.html 파일에 다음 전문을 복사하여 붙여넣기합니다.

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>Single Page Apps for GitHub Pages</title>
    <script type="text/javascript">
      var segmentCount = 0;
      var l = window.location;
      l.replace(
        l.protocol +
          "//" +
          l.hostname +
          (l.port ? ":" + l.port : "") +
          l.pathname
            .split("/")
            .slice(0, 1 + segmentCount)
            .join("/") +
          "/?p=/" +
          l.pathname
            .slice(1)
            .split("/")
            .slice(segmentCount)
            .join("/")
            .replace(/&/g, "~and~") +
          (l.search ? "&q=" + l.search.slice(1).replace(/&/g, "~and~") : "") +
          l.hash
      );
    </script>
  </head>
  <body></body>
</html>
```

<br/>

### Step 3 : index.html에 리다이렉트를 위한 스크립트 작성

&ensp; 마지막으로 본인 프로젝트의 index.html에 다음의 전문을 복사하여 head 태그에 넣어줍니다.

```html
<script type="text/javascript">
  // Single Page Apps for GitHub Pages
  // https://github.com/rafrex/spa-github-pages
  // Copyright (c) 2016 Rafael Pedicini, licensed under the MIT License
  (function (l) {
    if (l.search) {
      var q = {};
      l.search
        .slice(1)
        .split("&")
        .forEach(function (v) {
          var a = v.split("=");
          q[a[0]] = a.slice(1).join("=").replace(/~and~/g, "&");
        });
      if (q.p !== undefined) {
        window.history.replaceState(
          null,
          null,
          l.pathname.slice(0, -1) +
            (q.p || "") +
            (q.q ? "?" + q.q : "") +
            l.hash
        );
      }
    }
  })(window.location);
</script>
```

<br/>

### Step 4 : gh-pages로 배포하기

&ensp; 해당 내용을 다루고 있는 다른 블로그들이 많기 때문에 생략하기로 하겠습니다.

<br/>

### 결론

&ensp; 요컨대 클라이언트가 404.html을 받게 되면 작성된 스크립트가 현재 url의 경로와 쿼리를 변환하여 클라이언트의 브라우저를 리다이렉트시킵니다. 위의 JS 함수들은 이를 위한 코드이구요. 설명을 찬찬히 읽어보기는 했습니다만, 반드시 알아야 할 것은 아닌 것 같습니다.

##### 참고

[https://github.com/rafgraph/spa-github-pages#readme](https://github.com/rafgraph/spa-github-pages#readme)
