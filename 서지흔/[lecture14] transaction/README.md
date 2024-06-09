## 14. ACID

- transaction
  - 단일한 논리적인 작업 단위
  - 논리적인 이유로 여러 SQL문들을 단일 작업으로 묶어서 나눠질 수 없게 만든 것이 transaction
- START TRANSACTION; 실행과 동시에 autocommit off됨(commit/rollback해줘야만함)
- @Transactional annotation (알아서 ACID 지켜준다)
- Atomicity : 원자성 (논리적으로 쪼개질 수 없는 작업단위)
- Consistency
  - DB상태를 consistent 상태에서 또 다른 consistent 상태로 바꿔야함
  - constraints, trigger 등 통해 DB에 정의된 rules을 트랜잭션이 위반했다면 롤백
- Isolation
  - 동시에 실행될때도 혼자 실행되는것처럼 동작
  - DBMS는 여러종류의 isolation level 제공(어떤 level transaction 동작시킬지 설정해야함)(낮을수록 유연함)
- Duability
  - commit된 transaction은 DB에 영구저장
  - DBsystem에 문제(power fail or DB crash) 생겨도 commit된 트랜잭션은 DB에 남아있음
  - 영구저장 → 비휘발성 메모리(HDD, SSD)에 저장함을 의미
  - transaction의 duability는 DBMS가 보장한다
