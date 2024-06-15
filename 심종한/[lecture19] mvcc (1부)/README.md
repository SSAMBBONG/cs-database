# lecture19 - mvcc (1부)

## MVCC (Multiversion concurrency control)

MVCC는 커밋된 데이터만 읽는다.

### committed read

이 isolation 레벨로 데이터를 읽으면 read하는 시간을 기준으로 그전에 커밋된 데이터를 읽는다.

![alt text](<read committed.png>)

따라서 이 상태에서는 `tx2` 가 write한 50이 조회된다.

### repeatable read

tx 시작 시간 기준으로 그전에 커밋된 데이터를 읽는다.

![alt text](<repeatable read.png>)

따라서 이 상태에서는 10이 조회된다.

> [!NOTE]
>
> **어떤 isolation 레벨로 데이터를 읽는지는 DBMS마다 다르다.**

### serializable

SSI 기법이 적용된 MVCC로 동작한다.

### read uncommitted

MVCC는 커밋된 데이터를 읽기 때문에 이 레벨에서는 MVCC가 적용되지 않는다.

## MVCC 특징

- 데이터를 읽을 때 `특정 시점 기준` 으로 가장 최근에 `커밋` 된 데이터를 읽는다.
  - _이때 특정 시점은 isolation 레벨에 따라 다르다._
- 데이터 변화 이력을 관리한다.
  - 따라서 추가적인 저장 공간을 사용하게 된다.
- read, write는 서로 block하지 않는다.
  - 동시에 처리할 수 있는 트랜잭션이 늘어나 성능이 더 좋아질 수 있다.

> [!NOTE]
>
> 특정 시점을 기준으로 커밋된 데이터를 읽는 것을 MySQL에서 `consistent read` 라고 부른다.

## MVCC에서 발생 가능한 문제

### read committed 레벨을 사용하는 경우

> 현재 예제는 PostgreSQL을 기반으로 설명한다.

![alt text](<read committed 문제.png>)

`tx1` 의 쓰기 작업의 결과가 사라지는 문제가 있다.

왜냐하면 `tx1` 이 x값애 쓰기 작업을 할때 락을 획득한다.
또한 `tx2` 는 커밋된 시점의 x값을 읽으므로 50을 읽는다.
30을 입금하려 하지만 `tx1` 이 쓰기 락을 가지고 있으므로 대기하게 되고 결과적으로 `lost update` 문제가 발생한다.

### 해결법

tx2 의 isolation 레벨을 repeatable read 로 바꾸면 해결된다.

PostgreSQL 는 `repeatable read` 레벨을 사용하면 같은 데이터에 먼저 update한 tx가 커밋되었다면 이후 tx는 롤백하는 기능이 있다.

![alt text](<repeatable read 해결법.png>)

이런 특성을 `fist-updater-win` 라고도 한다.

### 더 고민해보자

일단 트랜잭션마다 서로 다른 isolation 레벨을 가져갈 수 있다.

그렇다면 tx1 이 read committed 레벨을 사용해도 정말 괜찮은지 보자.

![alt text](<read committed 문제점.png>)

이처럼 트랜잭션의 동작 순서가 바뀐다면 lost update 문제가 발생하게 된다.

**즉 서로 연관되는 트랜잭션의 isoloation 레벨을 동시에 고려해야 문제를 해결할 수 있다는 것이다.**

![alt text](<repeatable read로 바꿔도 문제.png>)

`tx1` 의 isolation 레벨을 바꾸더라도 `tx1` 은 롤백되는 상황이 발생할 수 있고 여전히 `lost update` 문제가 발생한다.
