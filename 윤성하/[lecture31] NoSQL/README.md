# RDB의 단점

- 유연한 확정성의 부족
- 중복 제거를 위해 정규화 진행
	- 여러 join을 해야 하는 경우도 있다 → 성능 하락
- scale-out이 어렵다 (서비스 중인데 scale out을 하려면 데이터를 전부 복제해야하니 부담)
- ACID를 보장하려다 보니 퍼포먼스에 영향이 있다

# NoSQL 등장 배경

- high-throughput 요구
- low-latency 요구
- 비정형 데이터의 증가

# NoSQL의 특징

- 유연한 스키마 → application 레벨에서 스키마 관리가 필요
- 중복 허용 (join 회피) → application 레벨에서 중복된 데이터들이 최신 데이터로 유지되도록 관리 필요
- scale-out이 쉽다 → 일반적으로 여러 서버들로 클러스터를 구성하여 사용한다
	- join할 필요 없이 데이터를 읽어오면 되니까 데이터를 나눠서 저장해도 문제가 적다
- ACID의 일부를 포기
	- high-thoughput, low-latency 추구

