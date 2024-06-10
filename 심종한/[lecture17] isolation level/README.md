# lecture17 - isolation level

## dirty read

![alt text](<dirty read.png>)

커밋되지 않은 변화를 읽어 문제가 되는 상황

## nonrepeatable read

![alt text](<nonrepeatable read.png>)

같은 데이터의 값이 달라지는 상황

## phantom read

![alt text](<phantom read.png>)

없던 데이터가 생기는 상황

## 문제상황

위 3가지 이상 현상들을 모두 발생하지 않게 만들 수 있지만 제약사항이 많아져 동시 처리 가능한 트랜잭션 수가 줄어들게 된다.

**결국 DB의 전체 처리량이 떨어진다.**

## 해결책

일부 이상한 현상은 허용하는 몇 가지 레벨을 만들어서 사용자가 필요에 따라 적절하게 선택할 수 있도록 하면 된다.

## isolation level

| Isolation Level  | Dirty Read | Non-Repeatable Read | Phantom Read |
| ---------------- | ---------- | ------------------- | ------------ |
| READ UNCOMMITTED | O          | O                   | O            |
| READ COMMITTED   | X          | O                   | O            |
| REPEATABLE READ  | X          | X                   | O            |
| SERIALIZABLE     | X          | X                   | X            |

개발자는 isolation level을 통해 전체 처리량과 데이터 일관성 사이에서 적절한 `trade-off` 를 할 수 있다.

## dirty write

![alt text](<dirty write .png>)

커밋되지 않은 데이터를 write한 상황

## lost update

![alt text](<lost update.png>)

업데이트를 덮어버려서 반영되지 않는 상황

## read skew

![alt text](<read skew.png>)

특정 트랜잭션이 다른 트랜잭션의 변경 사항을 반영한 일관성 없는 데이터를 읽는 상황

## write skew

![alt text](<write skew.png>)

동시에 실행되는 트랜잭션이 각기 다른 조건에서 write하여 일관성 없는 상태를 만드는 상황
