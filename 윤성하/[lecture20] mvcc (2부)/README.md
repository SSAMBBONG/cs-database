Mysql의 locking read는 가장 최근의 commit된 데이터를 읽는다. (repeatable read level이여도 동일하게 적용)

## Locking Read의 종류

SELECT ... FOR UPDATE;
> 이건 write lock

SELECT ... FOR SHARE;
> 이건 read lock

## 그럼에도 해결되지 않은 문제

MVCC + repeatable read를 사용하였는데도 발생하는 문제가 남아있다.

WRITE SKEW! <- 이건 write lock을 적용하면 해결 가능하다  
다만, mysql과 postgresql의 결과는 다르다.  

mysql: 두 tx 모두 정상적으로 commit
postgresql: 먼저 commit된 tx만 commit. 다른건 rollback (write lock 뿐 아니라 read lock이여도 repeatable read level에서는 먼저 update tx가 commit하면 나중 tx는 rollback 된다)


## Serializable은 어떻게 동작할까?

MYSQL
- repeatable read와 유사
- tx의 모든 평범한 select 문은 암묵적으로 select ... for share 처럼 동작한다

> [!NOTE]
> 왜 for share 일까?
> 
> - for update는 일종의 write lock이기 때문에 성능상 좋지 않음
> 
> 하지만 shared lock은 exclusive lock에 비해 deadlock 발생 가능성이 높다

POSTGRESQL
- SSL(Serializable Snapshot Isolation)[^1]로 구현
- first-committer-winner

[^1]: MVCC로 동작하면서도 모든 이상현상을 방지하는 기법이다.