> # [Lecture 29](https://www.youtube.com/watch?v=P7LqaEO-nGU&list=PLcXyemr8ZeoREWGhhZi5FZs6cvymjIBVe&index=29)

## 주요 내용

- partitioning
- sharding
- replication

## partitioning

- 파티셔닝
- database table을 더 작은 table로 나누는 것
- partitioning 종류
    - vertical partitioning : column을 기준으로 table을 나누는 방식
    - horizontal partitioning : row를 기준으로 table을 나누는 방식

## vertical partitioning

- board 테이블이 있다고 했을 때 게시글 목록을 보여주는데 content까지 가져올 필요가 없음!!
- 이럴 경우 무거운 데이터인 content같은 것을 따로 분리해서 테이블로 나누는 것

## horizontal partitioning

- 테이블의 크기가 커질수록 인덱스의 크기도 커짐
- 테이블에 read / write가 있을 때마다 인덱스에서 처리되는 시간도 조금씩 늘어남
- 한 테이블에만 데이터를 쌓지 않고 테이블을 나누어서 데이터를 분산하여 쌓는 방법을 사용하여 문제를 해결함
- 즉, hash function을 이용하여 horizontal partitioning을 하여 해결 가능(hash based horizontal partitioning)
- hash function에서 기준이 되는 키(테이블에서 기준이 되는 키)를 partition key라고 함
- 가장 많이 사용될 패턴에 따라 partition key를 정하는 것이 중요함!!!
- 데이터가 균등하게 분배될 수 있도록 hash function을 잘 정의하는 것도 중요함!!!
- hash-based horizontal partitioning은 한번 partition이 나뉘어져서 사용되면 이후에 partition을 추가하기 까다로움
- partition들을 같은 DB 서버에 저장함 (HW 자원에 한정될 수 있음)

## sharding

- horizontal partitioning처럼 동작
- horizontal partitioning과 차이는 각 partition이 독립된 DB 서버에 저장됨 (HW 자원에 한정되지 않음)
- 그래서 부하(load)를 분산 시킬 수 있음
- 즉, 데이터가 많이 쌓이는 테이블에는 샤딩을 사용하여 각 파티션마다 독립된 DB 서버를 할당하고 트래픽을 분산 시켜 DB 서버의 부하를 낮추는 방식을 사용함

## replication

- DB 서버의 데이터를 copy하여 sync를 맞춰 나가 하나의 DB 서버에 문제가 생기더라도 다른 DB 서버(보조 서버)를 통해 문제를 해결하는 방법
- 기존 DB 서버 : master / primary / leader
- 복사 DB 서버 : slave / secondary / replica
- 즉, 한 서버에 장애가 발생하여도 서비스에 타격이 없도록 하는 고가용성(High availability)을 보장하는 것
- 또는 계속 되는 트래픽에 대한 부하를 분산시킬 수도 있음!!

## 정리

- partitioning : table을 목적에 따라 작은 table들로 나누는 방식
- sharding : horizontal partitioning으로 나누어진 table들을 각각의(독립된) DB 서버에 저장하는 방식
- replication : DB를 복제해서 여러 대의 DB 서버에 저장하는 방식