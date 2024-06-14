> # [Lecture 16](https://www.youtube.com/watch?v=89TZbhmo8zk&list=PLcXyemr8ZeoREWGhhZi5FZs6cvymjIBVe&index=16)

## 주요 내용

- Recoverable Schedule
- Cascadeless Schedule
- Strict Schedule

## Unrecoverable Schedule

- schedule내에서 commit된 트랜잭션이 rollback된 트랜잭션의 write했었던 데이터를 읽은 경우를 의미
- rollback을 해도 이전 상태로 회복 불가능할 수 있기 때문에 이런 schedule은 DBMS가 허용하면 안됨

## Recoverable Schedule

- 회복 가능한 스케쥴
- schedule내에서 모든 트랜잭션은 자신이 읽은 데이터를 write한 트랜잭션이 먼저 commit/rollback 전까지는 commit을 하지 않는 경우를 의미
- rollback할 때 이전 상태로 온전히 돌아갈 수 있기 때문에 DBMS는 이런 schedule만 허용해야 함

## Cascading Rollback

- 하나의 트랜잭션이 rollback하면 의존성이 있는 다른 트랜잭션도 rollback 해야 함
- 문제는 여러 트랜잭션의 rollback이 연쇄적으로 일어나면 처리하는 비용이 많이 듦
- 해결책으로는 데이터를 write한 트랜잭션이 commit/rollback 한 뒤에 데이터를 읽는 schedule만 허용하도록 하자!!

## Cascadeless Schedule

- avoid cascading rollback 라고 봐도 됨
- schedule 내에서 모든 트랜잭션은 commit 되지 않은 트랜잭션들이 write한 데이터는 읽지 않는 경우를 의미 → 트랜잭션들이 write한 뒤 commit이 되지 않은 데이터를 스케쥴 내에서 다른 트랜잭션이 읽으면 안됨!!

## Strict Schedule

- 트랜잭션들이 write한 뒤 commit이 되지 않은 데이터를 스케쥴 내에서 다른 트랜잭션이 읽거나 쓰지도 않는 경우 → Cascadeless Schedule은 읽지만 않는 경우
- rollback 할 때 recovery가 쉬움
- 트랜잭션 이전 상태로 돌려놓기만 하면 됨

## 최종 정리

- unrecoverable schedule : DBMS가 허용하면 안됨
- recoverable schedule : DBMS가 허용해야 함
    - cascadeless schedule
        - strict schedule
- concurrency control은 serializability 와 recoverability를 제공함
    - 이와 관련된 트랜잭션 속성이 Isolation