Lock은 처리량이 줄어들어 문제 -> MVCC 등장

MVCC는 write-write를 제외하면 동시에 처리 가능

write lock을 사용하지 않고도 MVCC를 구현 가능하지만, 오늘날 대부분의 RDB는 write lock을 사용하여 구현한다

- MVCC는 commit된 데이터만 읽는다 <- dirty read발생 x
- Recoverability를 위해 commit 할 때 write lock 반환 <- Strict 2PL과 유사
- 데이터 변화 이력을 관리

## Isolation Level에 따른 read 시점
- read uncommitted: MVCC 자체가 commit 된 데이터를 읽도록 하므로 보통 해당 레벨은 MVCC 자체가 적용되지 않는다
- read committed: read하는 시간을 기준으로 commit 되어 있는 데이터 읽기 가능  
- repeatable read: tx 시작 시간 기준(db마다 다를 수 있음)으로 commit 되어 있는 데이터만 읽기 가능
- serializable: repeatable read와 동일


Consistent read: 데이터를 읽을 때 특정 시점을 기준으로 commit 된 데이터 읽기  
> [The query sees the changes made by transactions that committed before that point in time, and no changes made by later or uncommitted transactions.](https://dev.mysql.com/doc/refman/8.0/en/innodb-consistent-read.html)

MVCC가 적용되었더라도 read committed level에서 LOST UPDATE 발생 가능
-> repeatable read로 바꾸면?
1. 하나의 tx만 repeatable read로 변경: 여전히 다른 schedule에서 LOST UPDATE 발생 가능
2. 모든 tx repeatable read로 변경: first committer win의 원리에 따라서 나중에 write하는 tx는 rollback됨 (단, 이건 postgresql만 해당. mysql은 여전히 LOST UPDATE 존재)