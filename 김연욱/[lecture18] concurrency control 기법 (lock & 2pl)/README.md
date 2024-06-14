> # [Lecture 18](https://www.youtube.com/watch?v=0PScmeO3Fig&list=PLcXyemr8ZeoREWGhhZi5FZs6cvymjIBVe&index=18)

## 주요 내용

- write-lock
- read-lock
- 2PL (Two-Phase Locking) protocol

## write-lock

- exclusive lock
- read / write(insert, modify, delete) 할 때 사용
- 다른 트랜잭션이 같은 데이터를 read / write 하는 것을 허용하지 않음

## read-lock

- shared lock
- read 할 때 사용
- 다른 트랜잭션이 같은 데이터를 read 하는 것을 허용 함

## lock 호환성

|  | read-lock | write-lock |
| --- | --- | --- |
| read-lock | O | X |
| write-lock | X | X |

## 2PL Protocol

- Two-Phase Locking Protocol
- 트랜잭션에서 모든 locking operation이 최초의 unlock operation보다 먼저 수행되도록 하는 것
- Expanding phase (growing phase) : lock을 취득하기만 하고 반환하지는 않는 phase
- Shrinking phase (contracting phase) : lock을 반환만 하고 취득하지는 않는 phase
- 2PL Protocol은 Serializability 보장
- 특정 상황에서 Deadlock이 발생할 수도 있음

## 2PL Protocol 종류

- conservative 2PL
    - 모든 lock을 취득한 뒤 트랜잭션을 시작
    - deadlock-free (deadlock이 발생하지 않음)
    - 실용적이지 않음
- strict 2PL (S2PL)
    - strict schedule을 보장하는 2PL
    - recoverability 보장
    - write-lock을 commit / rollback 될 때 반환(unlock)
- strong strict 2PL (SS2PL)
    - strict schedule을 보장하는 2PL
    - recoverability 보장
    - read-lock / write-lock 모두 commit / rollback 될 때 반환(unlock)
    - S2PL보다 구현이 쉽다
    - 다만 lock하는 시간이 S2PL보다 길어질 수 있다는 단점이 존재 (read를 계속 lock하고 있다가 나중에 unlock하기 때문)

## MVCC

- multiversion concurrency control
- read와 write가 서로를 block 하는 것이라도 해결하기 위한 해결책
- 오늘날 많은 RDBMS가 lock과 MVCC를 같이 혼용해서 많이 사용함
- 다음 강의에서 설명!
