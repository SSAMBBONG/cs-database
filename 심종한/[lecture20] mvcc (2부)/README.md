# lecture20 - mvcc (2부)

## MySQL에서 lost update 해결

### locking read

![alt text](<locking read1.png>)

MySQL에서는 select for update 구문을 사용할 수 있다.
단, `tx1` , `tx2` 모두 `locking read` 를 사용해야만 한다.
`tx1` 이 `locking read` 를 사용하지 db에 커밋되어 있는 값을 읽게 된다.

![alt text](<locking read2.png>)

또한 MySQL에서 locking read는 가장 최근에 커밋된 데이터를 읽는다.
따라서 repeatable read 레벨이더라도 트랜잭션의 시작 시점의 값을 읽지 않고 가장 최근에 커밋된 데이터를 읽도록 동작한다.

![alt text](<locking read3.png>)

### wrtie skew 문제

정상적으로 동작하면 결과는 다음과 같다.

![alt text](<repeatable read 정상.png>)

그러나 아래처럼 locking read를 사용하지 않으면 문제가 생긴다.

![alt text](<locking read 사용하지 않음.png>)

![alt text](<wrtie skew.png>)

#### MySQL에서의 해결 - select for update

![alt text](<mysql solution.png>)

MySQL에서 locking read를 사용하면 repeatable read 격리 수준이더라도 가장 최근에 커밋된 데이터를 읽게 되므로 문제가 해결된다.

#### PostgreSQL

그러나 PostgreSQL에서 repeatable read 격리 수준에서는 동작하는 방식이 달라 먼저 update한 tx가 커밋되면 이후 tx는 롤백된다.

![alt text](<postgresql rollback1.png>)

이번엔 select for share 로 읽기 잠금을 획득하더라도 동일한 이유로 롤백된다.

### 정리

DBMS의 종류에 따라 동작 방식이 다르므로 상황에 알맞은 방법을 적용해야 한다.
