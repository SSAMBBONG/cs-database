## 16.concurrency control 기초 : recoverability

- unrecoverable schedule : rollback해도 이전 상태로 회복 불가능할 수 있기 때문에 이런 schedule은 DBMS가 허용하면 안된다
- recoverable schedule
    - cascadeless schedule : schedule 내에서 어떤 transaction도 commit 되지 않은 transaction들이 write한 데이터는 읽지 않는 경우
        - strict schedule : schedule 내에서 어떤 transaction도 commit 되지 않은 transaction들이 write한 데이터는 **쓰지도** 읽지도 않는 경우