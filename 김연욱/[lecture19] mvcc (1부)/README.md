> # [Lecture 19](https://www.youtube.com/watch?v=wiVvVanI3p4&list=PLcXyemr8ZeoREWGhhZi5FZs6cvymjIBVe&index=19)

## 주요 내용

- MVCC (MultiVersion Concurrency Control)
- isolation level과 함께 MVCC case study (MySQL, postgreSQL)
- postgreSQL의 MVCC Lost Update

## MVCC

- MultiVersino Concurrency Control
- Lock
    
    
    |  | Read | Write |
    | --- | --- | --- |
    | Read | O | O |
    | Write | O | X |
- MVCC는 데이터를 읽을 때 특정 시점 기준으로 가장 최근에 commit된 데이터만 읽음 (중요한 특징!)
- 데이터 변화(write) 이력을 관리함 (공간이 더 필요해서 단점이 될 수 있음)
- read와 write는 서로 block하지 않음!!
- MySQL에서는 특정 시점 기준을 Consistent read라고 함

## postgreSQL의 MVCC Lost Update

- 변경된 값에 대해서 덮어 쓰게 되는 경우(lost update)가 발생할 수 있음
- postgreSQL에서 repeatable read 레벨은 같은 데이터에 먼저 update한 tx이 commit되면 나중 tx는 rollback된다는 특징이 있음 (first-updater-win)
- 이 경우 한 트랜잭션의 레벨만 repeatable read로 두는 것이 아닌 연관된 트랜잭션을 모두 repeatable read로 둬야함!! (트랜잭션마다 서로 다른 isolation level을 줄 수 있음)
- 즉 postgreSQL의 MVCC는 lost update를 repeatable read 만으로 해결 가능