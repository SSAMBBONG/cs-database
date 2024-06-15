> # [Lecture 24](https://www.youtube.com/watch?v=5QhkZkrqFL4&list=PLcXyemr8ZeoREWGhhZi5FZs6cvymjIBVe&index=24)

## 주요 내용

- DB 정규화
- 3NF
- BCNF

## 3NF

- 모든 non-prime attribute는 어떤 key에도 transitively dependent 하면 안 된다
- non-prime attribute와 non-prime attribute 사이에는 FD가 있으면 안 된다

## BCNF

- 모든 유효한 non-trivial FD X → Y 는 X가 super key여야 한다

## 2NF 참고 사항

- 2NF는 key가 composite key(두 개 이상의 attribute로 이루어진 키)가 아니라면 2NF는 자동적으로 만족한다??
    - 2NF : 모든 non-prime attribute는 모든 key에 fully FD 해야 한다
    - 2NF : 모든 non-prime attribute는 어떤 key에도 partially FD 하면 안 된다
- 항상 그런 것은 아님!!

## Denormalization

- 역 정규화
- 테이블을 너무 쪼개게 되면 여러 테이블을 조인을 해야하는 성능 이슈가 발생
- 테이블을 너무 쪼개게 되면 관리가 힘듬
- 그래서 전략적으로 너무 쪼개지 않겠다 라는 의미
- DB를 설계할 때 과도한 조인과 중복 데이터 최소화 사이에서 적정 수준을 잘 선택할 필요가 있음