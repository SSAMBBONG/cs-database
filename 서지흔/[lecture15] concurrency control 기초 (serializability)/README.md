## 15. concurrency control 기초 : schedule과 serializability

- schedule
    - 여러 트랜잭션들이 동시실행될때 각 트랜잭션에 속한 operation의 실행순서
    - 트랜잭션 내의 operations 순서는 바뀌지 않는다
- serial schedule ↔ nonserial schedule
    - transaction들이 겹치지 않고 한 번에 하나씩 실행되는 schedule
- conflict equivalent
    - 두 조건 만족시!!
    1. 두 schedule은 같은 트랜잭션 가진다
    2. 어떤 conflicting operations의 순서도 양쪽 schedule 모두 동일
    - 만약 serial schedule과 conflict equivalent할때 → Conflict serializable(정상결과 나올 것)
- 성능때문에 여러 트랜잭션 겹쳐서 실행할 수 있으면 좋겠는데 결과 이상한건 싫어! → conflict serializable한 nonserial schedule 허용하자
- 오늘날 직접 conflict serializable한지 확인하지는 않고 여러 transaction 동시 실행해도 schedule이 conflict serialiable하도록 보장하는 프로토콜을 적용한다