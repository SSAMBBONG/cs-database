> # [Lecture 21](https://www.youtube.com/watch?v=JwfQ8ouhAzA&list=PLcXyemr8ZeoREWGhhZi5FZs6cvymjIBVe&index=21)

## 주요 내용

- DB Schema 설계의 중요성

## Insertion Anomaly

- 삽입 이상 현상
- 문제점<br>
<img src="./img/lecture21-1.png">
    
    - 중복된 데이터로 인한 저장 공간 낭비 (empl_id = 1, 2)
    - 실수로 인한 데이터 불일치 가능성 존재
    - null 값은 적게 쓰는 것이 좋음 (empl_id = 3)
- 해결<br>
<img src="./img/lecture21-2.png">
    

## Deletion Anomaly

- 삭제 이상 현상
- 문제점<br>
<img src="./img/lecture21-1.png">
    
    - empl_id = 5를 삭제하면 QA부서에 대한 정보가 다 없어짐
    - empl_id = 5인 데이터의 부서 정보를 제외한 값을 null로 하여 관리하면 QA부서에 대한 정보는 존재하나 null이 많아짐, pk인 empl_id의 의미와도 맞지 않는 정보
- 해결<br>
<img src="./img/lecture21-3.png">
    

## Modification Anomaly

- 갱신 이상 현상
- 문제점<br>
<img src="./img/lecture21-1.png">
    
    - DEV부서가 DEV1로 변경되었는데 JINHO의 부서명만 DEV1로 변경된 경우 (MESSI의 부서명과의 데이터 불일치 발생)
- 해결<br>
<img src="./img/lecture21-4.png">
    

## 바른 DB Schema 설계

1. 의미적으로 관련있는 속성들끼리 테이블을 구성
2. 중복 데이터를 최대한 허용하지 않도록 설계
3. join 수행 시 가짜 데이터가 생기지 않도록 설계 (매우 중요)
4. 되도록이면 null 값을 줄일 수 있는 방향으로 설계