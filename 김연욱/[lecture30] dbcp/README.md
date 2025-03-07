> # [Lecture 30](https://www.youtube.com/watch?v=zowzVqx3MQ4&list=PLcXyemr8ZeoREWGhhZi5FZs6cvymjIBVe&index=30)

## 주요 내용

- DBCP란?
- 성능에 도움되는 이유
- DBCP 튜닝 팁

## DBCP

- DataBase Connection Pool
- connection을 재사용하여 열고 닫는 시간을 절약할 수 있음

## DBCP 설정 방법 (Spring hikariCP - MySQL 기준)

- max_connections (MySQL) : client와 맺을 수 있는 최대 connection 수
- wait_timeout (MySQL) : connection이 inactive 할 때 다시 요청이 오기까지 얼마의 시간을 기다린 뒤에 close할 것인지를 결정
- minimumIdle (hikariCP) : pool에서 유지하는 최소한의 idle connection 수
- maximumPoolSize (hikariCP) : pool이 가질 수 있는 최대 connection 수 (idle과 active(in-use) connection 합쳐서 최대 수)
- idle connection 수가 minimumIdle보다 작고, 전체 connection 수도 maximumPoolSize보다 작다면 신속하게 추가로 connection을 만듦
- 즉, maximumPoolSize는 Pool에 존재할 수 있는 connection의 최대 수이며 minimumIdle은 Pool에 사용하지 않는 connection이 최소한 몇개는 있어야 된다는 것으로 connection이 maximumPoolSize 미만이고 idle connection이 minimumIdle 미만이라면 connection을 만든다!!
- 기본 값은 maximumPoolSize와 minimumIdle이 동일함 (= pool size 고정)
- maxLifetime (hikariCP) : pool에서 connection의 최대 수명
    - idle connection은 maxLifetime을 넘기면 바로 제거
    - active인 경우라면 maxLifetime을 넘기면 pool로 반환된 후 제거
    - 이 값은 DB의 connection time limit(MySQL에서는 wait_timeout)보다 몇 초 짧게 설정해야 함
- connectionTimeout (hikariCP) : pool에서 connection을 받기 위한 대기 시간