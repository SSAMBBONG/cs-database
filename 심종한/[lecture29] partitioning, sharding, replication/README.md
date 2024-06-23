# lecture29 - partitioning, sharding, replication

## database & DBMS & DB System

### database

- 전자적으로 저장되고 사용되는 관련있는 데이터들의 조직화된 집합이다.

### DBMS

- 사용자에게 DB를 정의하고 만들고 관리하는 기능을 제공하는 소프트웨어 시스템
- DB를 정의하다 보면 부가적인 정보가 저장된다.

#### metadata

- DB를 정의하거나 기술하는 data-catalog 라고도 부름
- 데이터 유형, 구조, 제약 조건, 보안, 저장, 인덱스, 사용자 그룹 등등
- metadata 또한 DBMS를 통해 저장/관리된다.

### DB System

- `database + DBMS + 연관된 applications` 를 줄여서 database라고도 부름

### data model

- DB의 구조를 기술하는데 사용될 수 있는 개념들이 모인 집합
  - DB 구조를 추상화해서 표현할 수 있는 수단을 제공한다.
- data model은 여러 종류가 있으며 추상화 수준과 DB 구조화 방식이 조금씩 다르다.

#### 분류

1. conceptual data models

- 일반 사용자들이 쉽게 이해할 수 있는 개념들로 이뤄진 모델
- 추상화 수준이 가장 높음
- 비즈니스 요구사항을 추상화하여 기술할 때 사용

ex) ER 다이어그램..

2. logical data models

- 이해하기 어렵지 않으면서도 자세히 DB를 구조화 할 수 있는 개념을 제공
- 데이터가 컴퓨터에 저장될 때의 구조와 크게 다르지 않게 DB 구조화를 가능하게 함
- 특정 DBMS나 storage에 종속되지 않는 수준에서 DB를 구조화할 수 있는 모델

ex) 관계형 모델..

3. physical data models

- 컴퓨터에 데이터가 어떻게 파일 형태로 저장되는지를 기술할 수 있는 수단 제공
- data format, data orderings, access path 등등
  - access path: 데이터 검색을 빠르게 하기 위한 구조체 (인덱스..)

## schema & state

### database schema

- data model을 바탕으로 DB의 구조를 기술한 것
- DB를 설계할 때 정해지며 한번 정해진 후로 자주 변경되지 않는다.

### database state

- database에 있는 실제 데이터는 자주 바뀔 수 있다.
- 특정 시점에 DB에 있는 데이터를 database state 혹은 snapshot이라고 한다.

### three-schema level

1. 외부 스키마

- 사용자나 특정 응용 프로그램이 데이터를 보는 방식을 정의한다.

2. 개념 스키마

- 전체 DB의 논리적 구조를 정의한다.
- 모든 엔티티, 속성, 관계 및 제약 조건을 포함하며 DBA에 의해 정의된다.

3. 내부 스키마

- 물리적 저장 구조를 정의한다.
- 디스크에 어떻게 저장되고 인덱싱되는지, 압축된는지 등 물리적 저장소의 세부 사항을 포함한다.
