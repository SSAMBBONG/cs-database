> # [Lecture 17](https://www.youtube.com/watch?v=bLLarZTrebU&list=PLcXyemr8ZeoREWGhhZi5FZs6cvymjIBVe&index=17)

## 주요 내용

- 트랜잭션들이 동시에 실행될 때 발생 가능한 이상 현상들
- Isolation Level

## 초기 이상 현상

- Dirty Read
    - commit 되지 않은 변화를 읽은 경우
- Non-repeatable Read (Fuzzy Read)
    - 같은 데이터의 값이 달라진 경우
- Phantom Read
    - 없던 데이터가 생긴 경우

## Isolation Level

- 표준 SQL 기준

| Isolation Level | Dirty Read | Non-repeatable Read | Phantom Read |
| --- | --- | --- | --- |
| Read uncommited | O | O | O |
| Read commited | X | O | O |
| Repeatable read | X | X | O |
| Serializable | X | X | X |
- Serializable은 위 세 가지 현상 뿐만 아니라 아예 이상한 현상 자체가 발생하지 않는 level을 의미
- 애플리케이션 설계자는 isolation level을 통해 전체 처리량(throughput)과 데이터 일관성 사이에서 어느 정도 거래(trade)를 할 수 있음

## 표준 SQL 기준의 예시를 비판한 논문

- 위의 이상 현상들의 예시를 들으며 표준 SQL 기준을 정했음
- 하지만 그 이후 다른 예시들이 나오면서 이러한 경우에도 이런 이상 현상이 존재한다. 등의 의견이 나옴

## 표준 SQL 기준을 비판한 뒤 추가된 이상 현상

- Dirty write
    - commit 안 된 데이터를 write 하는 경우
    - rollback시 정상적인 recovery는 매우 중요하기 때문에 모든 isolation level에서 dirty write를 허용하면 안 된다!
- Lost update
    - 업데이트를 덮어쓴 경우
- Dirty read 확장판
    - commit 되지 않은 변화를 읽은 경우 + abort가 발생하지 않아도 dirty read가 발생할 수 있음
- Read skew
    - inconsistent(불일치, 일관성 없는..)한 데이터를 읽는 경우
- Write skew
    - inconsistent한 데이터를 쓰는 경우
- Phantom read 확장판
    - 없던 데이터가 생긴 경우 + 중간에 다른 데이터가 추가되었을 때 연관된 데이터를 읽는 경우에 문제가 생기는 경우

## Snapshot Isolation

- 기존 표준에서 정의한 isolation level과 다름 (이상 현상 세 가지를 정의한 뒤에 레벨을 정의했었음)
- concurrency control을 구현하는 것에 기반해서 정의된 isolation level
- 예시로 write-write conflict가 생겼을 때 먼저 commit된 데이터만 인정해주고 뒤에 commit할 데이터는 abort처리 (First-committer win)

## 실무에서의 isolation level

- MySQL(engine → innoDB) : 표준 SQL 에서의 정의한 isolation level
- Oracle : 표준 SQL 에서 정의한 isolation level을 사용하나 read uncommited는 제공하지 않으며, repeatable read와 serializable은 serializable로 통합되어 사용됨. → 즉 read commited와 serializable을 주로 다룸 (추가로 더 있긴 하다고 함 그치만 주요한 것은 이 두 개)
- SQL server : 표준 SQL 에서 정의한 isolation level + Snapshot
- postgreSQL : 표준 SQL 에서 정의한 isolation level + Serialization anomaly라는 이상 현상을 추가하여 사용
- 위 주요 RDBMS에서의 isolation level은 각각 다를 수 있고 같다 하더라도 동작이 다를 수 있음!!
- 사용하는 RDBMS의 isolation level을 잘 파악해서 적절한 isolation level을 사용할 수 있도록 해야 함