---
title: 콘솔로 즐기는 턴제 RPG (1)
tags: [Sparta, Python]
sidebar:
  nav: categories
permalink: "/categories/sparta/team-projects/turn-based-game_1"
article_header:
  type: cover
  image:
    src:
---

<!-- more -->

<br/>

&nbsp;&nbsp; 두 번째 팀 과제가 할당됐습니다. 이번 팀 과제는 지난 개인 과제로 할당된 "콘솔 바탕의 턴제 RPG"를 팀 단위로 고도화하는 것입니다. 제 개인 과제를 기준으로, 기존 게임은 레벨 업이나 몬스터를 무찔렀을 때의 보상 시스템 등이 없었거니와, 전투가 종료되면 게임도 같이 종료되는 정말 단순한 형태의 전투 시스템만이 구현되어 있었습니다. 이번에는 이것을 고도화하여 레벨업과 보상 시스템을 구현하고, 원한다면 여러 번 전투가 가능하도록 만들 예정입니다.

&nbsp;&nbsp; 우선 코드 구현에 앞서 우리 팀은 두 가지 원칙을 정했고, 다음과 같습니다.

1. **객체의 내부 변수에는 해당 객체의 내부 메소드로만 접근할 수 있게 하기**
2. **내부 변수는 protected로 선언하기**

&nbsp;&nbsp; 이에 대해 팀원들을 납득시키기는 했습니다만, 사실 제가 멋대로 정한 것이기도 합니다. 클래스는 독립적이어야 하고... 캡슐화되어야 하고... 등등 객체 지향 언어의 특징을 지키면서 코딩을 하면 좋겠으나, 저도 남한테 요령 있게 설명할 만큼 자세하고 정확하게 알고 있는 실정은 아닌지라, 저 두 원칙만은 지키면서 가면 좋을 것 같다고 제안했습니다. 비록 파이썬에서는 클래스 내 protected 변수에 대한 강제성이 없다 하더라도 말입니다. 한편으로, 저렇게 해야 1 대 다, 또는 다 대 다 전투 시스템을 구현할 때에 편리할 것이라 생각했습니다.

&nbsp;&nbsp; 스파르타 캠프에서 개인 과제 힌트로 알려준 메소드를 사용하게 되면, 다음과 같이 공격시에 공격 대상이 되는 객체 데이터에 직접 접근하게 됩니다.

```python
class Player:
    def __init__(self, hp, mp, power):
        # ...codes

    def attack(self, monster_instance):
        # ...codes

```

<br/>

&nbsp;&nbsp; 만일 몬스터 수가 여럿이면, 그 수에 따라 메소드를 달리 정의해야만 하는 불편함이 생깁니다. 따라서, 그 대신으로 attack 메소드는 공격 정보만을 반환하고, 공격 대상은 그 공격 정보를 받아 반응하게 했습니다. 대략 코드로 구현하면 다음과 같이 됩니다.

```python
# 플레이어 클래스
class Player:
    def __init__(self, hp, mp, power):
        # ...codes

    def attack(self):
        # ...codes

        return attack_info

    def attacked(self, attack_info)

# 몬스터 클래스
class Monster:
    def __init__(self, hp, mp, power):
        # ...codes

    def attack(self):
        # ...codes

        return attack_info

    def attacked(self, attack_info)
        # ...codes

# 실행 코드
player = Player(100, 100, 10) # 플레이어 객체 생성
monster = Monster(80, 0, 15) # 몬스터 객체 생성

attack_info = player.attack() # 플레이어가 공격
monster.attacked(attack_info) # 몬스터가 피격
```

<br/>

&nbsp;&nbsp; 이렇게 하면, attack_info에 어떤 데이터를 넣을 것인지만 결정하면 전투 시스템 구현이 쉬워질 것입니다.

&nbsp;&nbsp;사실 이번 프로젝트에서 제가 맡은 역할을 전투 시스템을 구현하는 것입니다. 이번 팀 프로젝트에서 사용할 꽤 많은 코드들(플레이어 클래스나 몬스터 클래스 등)이 제 개인 과제를 바탕으로 하고 있기 때문에, 똑같은 일보다 다른 일을 하는 것이 도움이 될 것이라 생각하여 도전했습니다. 여기까지가 서론으로, 구체적으로 어떻게 전투 시스템을 구현할 것인지에 대해서는 다음 포스트에서 이어가겠습니다.
