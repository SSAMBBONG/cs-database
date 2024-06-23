> # [Lecture 31](https://www.youtube.com/watch?v=sqVByJ5tbNA&list=PLcXyemr8ZeoREWGhhZi5FZs6cvymjIBVe&index=31)

## 주요 내용

- NoSQL이란?
- RDB vs NoSQL
- MongoDB
- Redis

## RDB 단점

- 경직된 스키마 / 유연한 확장성의 부족 (새로운 기능이 추가될 때마다 컬럼을 추가해야하는 상황이 발생하기도 함)
- 정규화로 인한 과도한 조인과 성능 하락
- scale-out이 쉽지 않음 (여기서의 scale-out은 DB 서버를 추가하는 것)
- ACID를 보장하려다 보니 DB 서버의 performance에 영향을 줌

## NoSQL

- Not only SQL
- mongoDB, redis, DynamoDB …

## NoSQL 특징

- 유연한 스키마 / 스키마 관리를 application 레벨함 (개발자가 부담)
- 데이터 중복 허용 (join 회피)
- scale-out이 쉽고 편함 (서버 여러 대로 하나의 클러스터를 구성하여 사용)
- ACID의 일부를 포기하고 high-throughput, low-latency 추구 (금융 시스템처럼 consistency가 중요한 환경에서는 사용하기가 조심스러움)

## Redis

- in-memory key-value database, cache …
- 메모리로도 사용하기도 하고 캐시로도 사용하기도 하고 …
- data type : string, list, set, hash, sorted set …
- hash-based sharded cluster
- High Availability (replication, automatic failover)
- in-memory로 SSD와 HDD보다 빠르기 때문에 주로 cache처럼 사용함
- 기본 동작 과정
    1. 백앤드에서 redis에 데이터가 있는지 확인 (redis에 데이터 존재하지 않음)
    2. 백앤드에서 DB에 데이터가 있는지 확인 (DB에 데이터 존재)
    3. 백앤드에서 가공하여서 프론트로 보여줌과 동시에 redis에 데이터를 key-value 형태로 저장
    4. 1~3번 반복