## [lecture31] NoSQL

## NoSQL

- flexible schema
- 중복 허용(join 회피)
- RDB의 특징을 가지고 있으며 RDB의 단점을 보완하기 위해 나옴
- application 레벨에서 중복된 데이터들이 모두 최신 데이터를 유지할 수 있도록 관리해줘야함
- scale-out
  - 서버 여러대로 하나의 클러스터를 구성하여 사용
- ACID의 일부를 포기하고 high-throughput, low-latency 추구
  - 금융시스템처럼 consistency가 중요한 환경에서는 사용하기 조심스러움

## MongoDB

## Redis

- key-value형태로 저장
- memory cache라서 DB에 비해 빠름

```
SET name easy code
# easy : key code : value
```

참고 :https://hostingdata.co.uk/nosql-database/
