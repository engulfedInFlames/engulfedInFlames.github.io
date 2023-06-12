---
title: 콘솔로 즐기는 턴제 RPG (2) &#58; 팀 프로젝트 완성
tags: [Sparta, Python]
sidebar:
  nav: categories
permalink: "/categories/sparta/projects/turn-based-game_2"
article_header:
  type: cover
  image:
    src:
---

<div class="article__content" markdown="1">

&ensp; 기존 게임에서는 플레이어 객체가 생성되면 자동으로 몬스터가 생성되고, 곧바로 전투에 돌입하게 되어 있었습니다. 이 일련의 과정들이 한 가지로 동작했고, 몬스터와의 전투에서 승리하거나 패배하면 게임은 자동 종료됐습니다.

&ensp; 이번에는 전투가 종료되면 게임이 곧바로 종료되는 대신, 전투 보상으로 골드와 경험치를 얻고, 마을로 이동하여 (1) 상점에 방문하거나, (2) 던전에 새로 진입하거나, (3) 자신의 스테이터스를 확인할 수 있게끔 비전투시의 환경을 구현하고 선택지를 제공할 예정입니다.

&ensp; 이를 위해서는 플레이어가 머물 두 곳의 공간이 필요합니다. 첫 번째는 마을, 두 번째는 던전입니다. 마을에서 할 수 있는 일은 앞서 말한 3가지입니다만, 스테이터스 확인과 상점 방문은 전투 결과에 따른 스테이터스 변동과 보상 획득이 우선되어야 하므로 던전 진입 및 전투 시스템부터 구현하도록 하겠습니다.

---

### 1. 던전 진입

&ensp; 상점 방문과 스테이터스 확인은 미구현이므로 곧바로 던전에 진입하도록 합니다.

```python
is_in_dungeon = True
while is_in_dungeon:

```

&ensp; 플레이어가 던전에서 나오고 싶을 때 is_in_dungeon이 False가 되도록 하여 dungeon에서 나오게 될 것입니다. 다음은 전투 시스템 구현입니다.

<br/>

### 2. 몬스터 생성

&ensp; 이제 몬스터를 생성합니다. 몬스터 클래스의 기본 형태는 제 개인 과제 때 만든 것과 동일하므로 그대로 사용했습니다.

&ensp; 몬스터의 수는 임의로 결정되므로, 미리 몬스터 딕셔너리를 만듭니다. 그리고 난수를 만들어 이 딕셔너리의 키로 넘기고, 값을 받아 리스트에 추가하여, 리스트에 추가된 몬스터들과 전투하도록 했습니다.

```python
class Monster:
  def __init__(self, name, rank, skill):
    #...codes


def create_monster_list_with_name_list(monster_dict):
    monster_list = []
    monster_name_list = []

    monster_number = random.randint(1, length)

    a_monster = monster_dict[monster_number]
    monster_list.append(a_monster)
    monster_name_list.append(monster_dict[a_monster.get_status("name")])
  # get_status 메소드는 객체의 내부 변수의 값을 반환



monster_dict = [
    {
        "1": Monster("좀비", 1, "저주"), "2": Monster("구울", 1, "광신"), "3": Monster("황혼의 유령", 1, "축복받은 조준"),
        # ...more monsters
    },
]


monster_list, monster_name_list = create_monster_list_with_name_list(
            monster_dict)
```

<br/>

### 3. 전투 여부 선택

&ensp; 이제 몬스터 목록을 출력하고, 플레이어는 전투 여부를 선택합니다. 전투 여부 선택은 단순히 입력이 유효한지만 확인하고, 플레이어가 전투를 그만두면 앞서 선언한 is_in_dungeon에 False를 할당하기만 하면 됩니다. 만일 플레이어가 전투를 원하면 공격을 주고 받기 위한 반복문에 들어가야 합니다. 그래서 "is_battle_or_not"이라는 함수를 정의했습니다.

```python
def is_battle_or_not():
  #...codes


is_battle = is_battle_or_not()

while is_battle:
  # 전투 시작

```

&ensp; 플레이어가 전투를 원하면 해당 함수는 True를, 원하지 않으면 False를 반환합니다. 그리고 is_battle이 True가 되면 전투에 돌입합니다.

<br/>

### 4. 전투 시스템 구현

&ensp; 저에게 주어진 것은 attack_info라는 데이터 뿐입니다. 플레이어 객체와 몬스터 객체는 잘 생성되었을 것이라(?) 가정합니다. 구체적으로 객체가 공격 메소드를 호출하면 attack_info라는 데이터를 반환하고, 이 attack_info는 피격 메소드의 인자로 들어가, 객체의 내부 변수 HP의 값을 변경합니다. 그리고 HP가 0 이하라면 객체는 사망 처리됩니다. attack_info에는 총 4가지의 데이터가 들어있습니다. 순서대로 "is_attack(공격 성공 여부)", "is_area_attack(광역 공격 여부)", "damage(피해량)", "is_critical(치명타 여부)"입니다. 코드로 구현하면 다음과 같습니다.

```python
# 1. 플레이어가 공격
attack_info = player.attack()

# 2-1. 몬스터가 피격 (광역 공격)
if attack_info[1]:
    for monster in monster_list:
        if monster:
            monster.attacked(attack_info)

# 2-2. 몬스터가 피격 (타겟 공격)
else:
    def select_target(_monster_name_list: list):
        target = input("목표를 선택하세요 : ")
        # ...codes

        return target

    monster_list[target].attacked(attack_info)
```

&ensp; 몬스터 객체는 attack_info를 인자로 받고 피해량에 따라 HP가 감소합니다. 만일 HP가 0 이하로 떨어지면 사망 처리 될 것입니다. 사망했는지 유무는 객체 내부 변수 is_alive를 선언하여 알 수 있게 했습니다. HP가 0 이하로 떨어지면 is_alive는 False가 됩니다. 따라서, 몬스터 생사 확인을 하고 보상을 구현할 때 이 내부 변수를 확인하면 됩니다.

```python
# 3. 몬스터 생사 확인
for i, monster in enumerate(monster_list):
    if monster_list[i]:
        monster_is_alive = monster.get_status("is_alive")
        if not monster_is_alive[0]:
            print(f"{monster_name_list[i]}을(를) 무찔렀습니다!")

            # monster_list에서 사망한 몬스터 객체를 제거하는 코드를 추가
            monster_list[i] = False
            monster_name_list[i] = False

            # 보상을 누적하는 코드를 추가

if not any(monster_list):
    print("전투에서 승리했습니다.")
```

&ensp; 마지막 if문에서 monster_list가 모두 False가 되면, 즉, 모든 몬스터가 사망하면 any(monster_list)는 False를 반환하고, 플레이어는 전투에서 승리하게 됩니다. 만약 몬스터가 살아 있다면 플레이어의 턴이 끝나고 몬스터가 공격합니다. 스킬 쿨타임 등 공격에 텀을 두지는 않았기에, 살아 있는 몬스터들이 각각 한 번씩 공격합니다.

```python
# 4. 몬스터가 공격
attack_info_list = []
for monster in monster_list:
    if monster:
        attack_info = monster.attack()
        attack_info_list.append(attack_info)

# 5. 플레이어가 피격
for attack_info in attack_info_list:
    player.attacked(attack_info)

# 6. 플레이어 생사 확인
player_is_alive = player.get_status("is_alive")
if not player_is_alive[0]:
    print("게임이 종료됩니다.")  # ✅ 반복문 탈출 수정
    is_in_dungeon = False
    game_exit = True
    break
```

&ensp; 마찬가지로 attack_info가 오가고 난 뒤에는 플레이어의 생사를 확인하고 사망하면 게임을 종료시킵니다. 아니면 다시 반복문의 처음으로 돌아가, 플레이어의 공격이 다시 시작됩니다.

&ensp; 여기까지 간단하게 전투 시스템을 구현했습니다. 보다 구체적인 기능들이 있어야겠지만, 당장 필요한 기능들은 모두 구현했습니다. 다음 포스트에서는 다른 팀원들이 만든 코드와 함께 시스템을 더 구체화해가도록 하겠습니다.

</div>
