> # [Lecture 20](https://www.youtube.com/watch?v=-kJ3fxqFmqA&list=PLcXyemr8ZeoREWGhhZi5FZs6cvymjIBVe&index=20)

## 주요 내용

- MySQL의 MVCC Lost Update

## MySQL에서의 Lost Update

- 변경된 값에 대해서 덮어 쓰게 되는 경우(lost update)가 발생할 수 있음
- MySQL에서의 for update 문을 추가해서 해결!
- Locking read : MySQL에서 for update를 의미 (locking read는 가장 최근의 commit된 데이터를 읽음!!)
    
    ```sql
    select balance 
      from account
      where id = 'x'
      for update;
    ```
    
- 대충 정리 시나리오
    1. tx2가 read(x)를 for update를 하여 lock을 취득함 (이때 x는 50이라 가정)
    2. tx1이 read(x)를 for update로 하는데 이미 tx2에서 점유중이기 때문에 대기
    3. tx2의 이후 로직 write(x = 80)을 실행 후 commit한 뒤 unlock
    4. tx2에서 lock을 반환하였으니 대기중이던 tx1에서 lock을 취득함
    5. 이때 tx1은 가장 최근의 commit된 데이터를 읽음 (즉 80이 읽힘)
    6. 이후 tx1은 write(x = 40)을 수행후 read(y)를 하는데 이때도 locking read인 for update를 수행
    7. 이후 write(y = 50)을 한 뒤 commit후 unlock을 함
- for update : exclusive lock(write-lock)
- for share : shared lock(read-lock)

## repeatable read 레벨에서의 write skew

- 이는 MySQL과 postgreSQL 둘 다 발생함
- 이를 해결하기 위해 MySQL은 locking read를 사용함
- 이를 해결하기 위해 postgreSQL for update와 for share 쓰면 됨 (다만 작동 방식이 좀 다름!)

## postgreSQL의 for update와 for share 작동 방식

- MySQL과 좀 다름
- postgreSQL은 repeatable read 레벨에서는 같은 데이터가 다른 트랜잭션에서 update후 commit 됐다면 나중 트랜잭션은 다 rollback되기 때문에

## serializable 레벨

- 이 레벨을 통해 write skew를 해결할 수도 있음
- MySQL serializable level
    - repeatable read와 유사함
    - tx의 모든 평범한 select문은 암묵적으로 select … for share 처럼 동작
- postgreSQL serializable level
    - SSI(serializable snapshot isolation)로 구현
    - first-commit-winner

## 마무리

- 많은 RDBMS가 MVCC로 동작함
- 이 MVCC는 각 RDBMS마다 내부적으로 동작하는게 좀 다름
- 그래서 사용하고자 하는 RDBMS의 MVCC동작을 찾아보고 잘 알고 있자!