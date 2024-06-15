## [lecture20] mvcc (2부)

MVCC(multiversion concurrency control)
ex)
tx1 : x가 y에 40 이체
tx2 : x에 30 입금
초기 x=50, y=10

- (X) MVCC, tx1 ``repeatable read`,tx2 `repeatable read```일때(MySQL기준)
  - 실행순서 : tx2->tx1
  - mysql에서는 `repeatable read`만으로는 올바른 결과 나오지 않음
- (O) MVCC, tx1 ``repeatable read`,tx2 `repeatable read```일때(MySQL기준)

  - 실행순서 : tx2->tx1
  - 각각 tx read query에`Locking read` 걸어주기
  - locking read : 가장 최근의 commit된 데이터를 읽음
  - 따라서 `repeatable read`가 아니더라도 올바른 결과 나옴
    ```sql
    SELECT balance
    FROM account
    WHERE id='x'
    FOR UPDATE -- locking read 걸어주는 부분
    ```
  - 즉 mysql에서는 `lost update` 방지를 위해 `repeatable read`, `locking read` 모두 신경써줘야함

- 모든 tx가 serializable하게 동작한다면? (제일 strict한 isolation level)
  - `lost update`, `write skew` 발생할 일 없음
  - mySQL
    - `repeatable read`와 유사
    - tx의 모든 평범한 select문은 암묵적으로 `select ..for share`처럼 동작함
