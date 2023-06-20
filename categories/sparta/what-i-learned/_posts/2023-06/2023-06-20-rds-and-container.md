---
title: "EC2 RDS vs 도커 컨테이너, DB 환경으로 무엇이 더 적합할까?"
tags: [DB, RDS, Docker]
sidebar:
  nav: categories
permalink: /categories/sparta/what-i-learned/rds-and-container/
---

<div class="article__content" markdown="1">

## ✅ Ephemeral

&ensp; 현재 스파르타 최종 프로젝트에서 postgreSQL 도커 컨테이너를 DB 환경으로 사용하고 있습니다. RDS로 구성할까 했습니다만, 둘 중 무엇이 더 DB 환경으로 적합한지를 모르거니와, 심적 여유도 없어, 도커 컨테이너를 DB 환경으로 정했습니다.

&ensp; 한 가지 걸리는 것은 도커 컨테이너의 비영구적(Ephemeral) 특성입니다. 도커 컨테이너는 존재 자체가 언제든 종료되고, 재실행될 수 있는 것이므로, 볼륨을 지정하여 로컬과의 싱크를 통해 데이터의 손실을 예방합니다. 그런데 '그렇게 할 바에 차라리 AWS RDS를 사용하는 편이 더 낫지 않을까?'하는 의문이 드는 것도 사실입니다. 그래서 둘의 특성을 비교하고 어떤 프로젝트에서 무엇을 DB 환경으로 사용하는 것이 바람직한지를 비교해보고자 합니다.

---

## ✅ RDS(Relational Database Service)

### 장점

- AWS의 DB 관리 서비스
- 설정 및 관리 용이 - AWS 콘솔에서 손쉽게 RDB를 관리할 수 있음
- 보안, 데이터 백업, DB 스케일링, 고가용성 등 자동화된 다양한 기능들을 제공

### 단점

- 사용자 설정이 제한됨 - 확장 기능 설치나 시스템 수준에서의 사용자 설정에 제약이 있음
- 앱 환경이 AWS 인프라에 구속될 수 있음

<br/>

## ✅ 도커 컨테이너

### 장점

- 독립성(Isolation), 재현성(Reproducibility) 있는 컨테이너를 사용함으로써 배포 환경으로부터의 제약이 덜함

- 컨테이너에는 앱 실행에 필요한 모든 의존성과 환경 설정이 캡슐화되어 있기 때문에 실행 환경으로부터의 제약이 덜함

### 단점

- 리소스 제약 - 호스트 머신의 리소스에 직접적으로 영향을 받음
- 비영구적

<br/>

## ✅ 결론

&ensp; 굉장히 간추려서 비교하기는 했습니다만, 이렇게 놓고 보니 RDS를 사용하지 않을 이유가 없는 것 같습니다. 아직 실력이 미흡하여 시스템 수준에서 사용자 설정할 것도 없으니... 여러모로 RDS로 연결하는 것이 더 득 되어 보입니다. 시간을 내서 RDS에 연결하기를 시도해야겠습니다.

<br/>

## +ɑ 장고 백엔드를 RDS상의 postgreSQL 연결하는 과정

1.  RDS에 postgreSQL 인스터스 설정 및 생성
2.  장고 앱에 postgreSQL 어댑터 `psycopg2` 설치
3.  장고 앱의 `settings.py`에서 `DATABASES` 설정 변경

    ```python
    # 예시
    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.postgresql_psycopg2',
            'NAME': POSTGRES_DB_NAME,
            'USER': POSTGRES_USER,
            'PASSWORD': POSTGRES_PASSWORD,
            'HOST': POSTGRES_HOST,
            'PORT': POSTGRES_PORT,
         }
    }
    ```

4.  Migrate 후에 `python manage.py dbshell`을 실행하여 DB 연결 확인하기

---

## 📚 출처

[AWS RDS Docs](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Welcome.html)

</div>
