---
title: 콘솔로 즐기는 턴제 RPG (0)
tags: [Sparta, Python, Flask]
sidebar:
  nav: categories
permalink: "/categories/sparta/team-projects/turn-based-game_0"
article_header:
  type: cover
  image:
    src:
---

<!-- more -->

<br/>

&nbsp;&nbsp; 개인 과제로 콘솔을 바탕으로 하는 간단한 턴제 RPG 게임을 만들었습니다. 저번 주에 Python 기본 문법(조건문, 함수, 클래스 등)을 배웠기 때문에, 이를 잘 소화했는지를 확인하는 과제라고 할 수 있습니다. 과제는 이미 완성한 상태이므로, 이번 포스트에서는 과제 복습 겸 검토를 하겠습니다.

---

### 1. 파일 구분

<br/>

<div align="center">
<img src="/imgs/sparta/team-projects/turn-based-rpg_1.png" style="height: 160px"/>
</div>

&nbsp;&nbsp; 우선, player 클래스와 monster 클래스를 서로 다른 파일에 작성하고, creator라는 폴더에 둡니다. scripts에는 게임의 인트로나 배경 설명을 위한 script 파일들을 둡니다. 기본적으로 Main dir에 있는 app.py에서 script 출력부터 player 및 monster 생성과 전투 구현까지를 관리합니다. 내일부터 팀 프로젝트로 우리가 만든 게임을 고도화해야 하기 때문에 재사용 가능성을 높이고자 이 같이 했습니다. 다음은 app.py의 코드 일부입니다.

<br/>

<div align="center">
<img src="/imgs/sparta/team-projects/turn-based-rpg_2.png" style="width: 600px"/>
</div>

&nbsp;&nbsp; 위의 코드들은 다음과 같은 기능을 합니다.

- intro() : 게임의 배경 스크립트를 출력
- create_player() : 플레이어를 생성
- get_status() : 플레이어의 상태 정보를 반환
- welcome_player() : 플레이어의 이름을 인자로 하며, 약간의 스크립트를 출력
- Monster() : 몬스터의 이름과 몬스터 등급을 인자로 받으며, 이에 일치하는 몬스터를 생성

<br/>

### 2. 전투 구현

<br/>

<div align="center">
<img src="/imgs/sparta/team-projects/turn-based-rpg_3.png" style="width: 600px"/>
</div>

&nbsp;&nbsp; 전투 구현은 while 문을 사용했습니다. 매턴 마다 플레이아 또는 몬스터의 생사여부(?)를 확인하고, 둘 중 하나라도 죽게 되면 gameover 변수에 참값을 할당하고, 게임은 종료됩니다.

&nbsp;&nbsp; 한편으로, 공격 메소드와 피격 메소드를 분리했습니다. 플레이어의 공격 메소드에 몬스터의 객체 정보를 인자로 담아, 플레이어의 메소드가 몬스터 객체의 내부 변수에 직접 접근하게 할 수도 있었습니다만, 그렇게 하지 않았습니다. 클래스 내부 변수는 되도록 메소드로만 접근할 수 있게끔 하는 것이 바람직하다고 생각했고, 직접 접근하는 대신 attack_info라는 변수 이름에 공격 정보를 담아 간접 전달하는 방식을 채택했습니다. 목적은 각 객체가 서로 다른 객체에 접근할 수 없게끔 하는 것이었습니다.

### 3. 플레이어 클래스 구현

<br/>

<div align="center">
<img src="/imgs/sparta/team-projects/turn-based-rpg_4.png" style="width: 600px"/>
</div>

&nbsp;&nbsp; 플레이어 클래스의 내부 변수는 위와 같습니다. 변수명 앞에 "\_"를 붙이면 protected 변수가 된다고 해서 다 일일이 그렇게 만들었습니다만, 강제성은 없다는 것을 뒤늦게 알았습니다. print() 함수로 직접 접근해도 정보가 잘 출력이 됩니다... 몬스터 클래스도 이와 마찬가지로 설계했습니다. 여하튼 클래스는 일단 독립적이면서, 캡슐화되어 있으면 좋다고 믿기 때문에 내일부터 진행될 팀 프로젝트에서도 이렇게 밀고 나가야겠습니다. 팀원들을 잘 납득시키는 것도 덤으로 따라 올 것입니다.

<br/>

### 4. 결론

&nbsp;&nbsp; 저는 여태까지 클래스를 머리로만 알고 있었고, 기껏해야 개념서의 기본 문제들을 풀 때만 사용해 봤었습니다. 클래스를 가지고 실제 서비스를 구현해 본 적은 없었는 것입니다. 때문에 클래스를 설계하고, 객체를 생성하고, 서로 다른 클래스에 의해 파생된 객체들끼리 데이터를 송수신하는 과정 등등은 처음 해보는 것이었기에 뜻깊었습니다. 내일부터 팀 프로젝트로서 우리가 만든 프로토타입 격의 콘솔 바탕 턴제 RPG를 고도화하는 과제가 주어집니다만, 벌써 흥미진진합니다.
