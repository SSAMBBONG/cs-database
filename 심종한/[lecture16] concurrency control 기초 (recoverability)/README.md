# lecture16 - concurrency control 기초 (recoverability)

## unrecoverable schedule

![alt text](<unrecoverable schedule.png>)

schedule 내에서 commit된 트랜잭션이 롤백된 트랜잭션이 write했었던 데이터를 읽은 경우를 `unrecoverable schedule` 라고 한다.

롤백을 하더라고 `tx1` 을 복구할 수 없다. 이미 커밋된 트랜잭션은 `durability` 속성 때문에 롤백 불가능하다.

이런 schedule은 DBMS가 허용하면 안된다.

## recoverable schedule

![alt text](<recoverable schedule.png>)

### cascading rollback

하나의 트랜잭션이 롤백하면 거기에 의존성이 있는 다른 트랜잭션도 롤백해야 한다.

그러나 여러 트랜잭션의 롤백이 연쇄적으로 일어나므로 처리 비용이 많이 드는 문제가 있다.

**따라서 데이터를 write한 트랜잭션이 커밋 또는 롤백을 한 뒤에 데이터를 읽는 schedule만 허용하면 해결할 수 있다.**

![alt text](<cascading rollback 문제 해결책.png>)

![alt text](<cascading rollback 문제 해결 롤백 예시.png>)

`tx2` 를 커밋 또는 롤백후 데이터를 읽으면 문제가 생기지 않는 것을 확인할 수 있다.

## cascadeless schedule (avoid cascading schedule)

**schedule 내에서 어떤 트랜잭션도 커밋되지 않은 트랜잭션들이 write한 데이터는 읽지 안는 경우를 의미한다.**

그러나 이 스케줄에서도 문제가 생기는 상황이 존재한다.

```
H 사장님이 3만원이던 피자 가격을 2만원으로 낮추려는데, K 직원도 동일한 피자의 가격을 실수로 1만원으로 낮추려 한다.
```

![alt text](<cascadeless schedule 문제 상황.png>)

이 경우 `tx2` 의 결과가 사라지는 문제가 발생한다.

## strict schedule

**schedule 내에서 어떤 트랜잭션도 커밋되지 않은 트랜잭션들이 write한 데이터는 쓰지도, 읽지도 안는 경우를 의미한다.**

![alt text](<strict schedule.png>)

트랜잭션이 서로 겹치지 않으므로 복구가 쉽다.

## 정리

동시성 제어는 **serializability & recoverability** 속성을 제공하는 것이며 이와 관련된 트랜잭션의 속성이 `isolation` 이다.
