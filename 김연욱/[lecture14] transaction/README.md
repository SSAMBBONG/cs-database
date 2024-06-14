> # [Lecture 14](https://www.youtube.com/watch?v=sLJ8ypeHGlM&list=PLcXyemr8ZeoREWGhhZi5FZs6cvymjIBVe&index=14)

## 주요 내용

- Database Transaction
- ACID

## Transaction

- 단일한 논리적인 작업 단위 (a single logical unit of work)
- 트랜잭션의 SQL문들 중에 일부만 성공해서 DB에 반영되는 일은 일어나지 않음

## Commit

- 지금까지 작업한 내용을 DB에 영구적으로(permanently) 저장하라
- 트랜잭션 연산
- 트랜잭션을 종료하는 것

## Rollback

- 지금까지 작업들을 모두 취소하고 트랜잭션 이전 상태로 되돌린다
- 트랜잭션 연산
- 트랜잭션을 종료하는 것

## Autocommit

- 각각의 SQL문을 자동으로 트랜잭션 처리 해주는 개념
- SQL문이 성공적으로 실행 → Commit
- SQL문 실행 중 문제 발생 → Rollback
- MySQL에서는 default로 autocommit enabled(1) → select @@autocommit;
- 다른 DBMS에서도 대부분 같은 기능을 제공함
- start transaction;
    - 실행과 동시에 autocommit은 disabled
    - commit / rollback과 함께 트랜잭션이 종료되면 원래 autocommit상태로 돌아감

## @Transactional

- 트랜잭션과 관련된 부가적인 코드들이 생략되어 있음
- 즉 비지니스 로직에 집중할 수 있음

## ACID

- Atomicity
- Consistency
- Isolation
- Durability

## Atomicity

- 원자성
- ALL or Nothing
- 모두 성공하거나 모두 실패하거나
- 트랜잭션은 논리적으로 쪼개질 수 없는 작업 단위이기 때문에 내부의 SQL문들이 모두 성공해야 함
- 중간에 SQL문이 실패하면 지금까지의 작업을 모두 취소하여 아무 일도 없었던 것처럼 rollback 해야 함
- 개발자는 트랜잭션을 언제 commit할지, rollback할지, 다른 로직을 실행 할지 등.. 을 신경써야함!! (DB에 저장 or 이전 상태로 되돌리기는 DBMS가 담당하는 것)

## Consistency

- 일관성
- 일관성을 유지시켜 주는 것
- 트랜잭션은 DB 상태를 consistent 상태에서 또 다른 consistent 상태로 바꿔줘야 함
- constraints, trigger 등을 통해 DB에 정의된 rules을 트랜잭션이 위반했다면 rollback 해야 함
- 트랜잭션이 DB에 정의된 rule을 위반했는지는 DBMS가 commit 전에 확인하고 알려줌
- 개발자는 application 관점에서 트랜잭션이 consistent하게 동작하는지를 신경써야 함

## Isolation

- 격리성
- 여러 트랜잭션들이 동시에 실행될 때도 혼자 실행되는 것처럼 동작하게 만드는 것
- DBMS는 여러 종류의 isolation level을 제공함 (높을 수록 엄격해서 다른 트랜잭션에 의해 영향을 받지 않음)
- 개발자는 isolation level 중에 어떤 level로 트랜잭션을 동작시킬지 설정할 수 있음
- concurrency control의 주된 목표가 isolation임 (앞으로 배울 내용)

## Durability

- 영속성
- commit된 트랜잭션은 DB에 영구적으로 저장함 (한 번 commit된 트랜잭션은 rollback 될 수 없다!)
- 즉, DB System에 문제(power fail, DB crash)가 생겨도 commit된 트랜잭션은 DB에 남아 있음
- 일반적으로 비휘발성 메모리(HDD, SDD)에 저장함을 의미함
- 기본적으로 트랜잭션의 durability는 DBMS가 보장함

## 주의 사항

1. 트랜잭션을 어떻게 정의해서 쓸지는 개발자가 정하는 것 (구현하려는 기능과 ACID 속성을 이해해야 트랜잭션을 잘 정의할 수 있음)
2. 트랜잭션의 ACID와 관련해서 개발자가 챙겨야 하는 부분들이 있음 (DBMS가 모든 것을 알아서 해주는 것이 아님)