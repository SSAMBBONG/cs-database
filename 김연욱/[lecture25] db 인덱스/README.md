> # [Lecture 25](https://www.youtube.com/watch?v=IMDH4iAQ6zM&list=PLcXyemr8ZeoREWGhhZi5FZs6cvymjIBVe&index=25)

## 주요 내용

- index가 중요한 이유
- index 거는 법
- index 동작 방식
- index 사용 시 참고 사항

## index가 중요한 이유

- index가 없다면 full scan(= table scan)으로 데이터를 찾아야 하므로 시간 복잡도는 O(N)
- index가 있다면 O(logN) (B-tree based index)
- index를 쓰는 이유
    - 조건을 만족하는 튜플(들)을 빠르게 조회하기 위해!
    - 빠르게 정렬(order by)하거나 그룹핑(group by) 하기 위해!

## index 거는 법

```sql
create [unique] index 인덱스명 on 테이블명 (컬럼명);
```

- unique 옵션은 중복이 안되는 고유 인덱스를 생성하는 것
    - 생략하면 중복이 허용
- unique 옵션으로 인덱스를 생성하려면 인덱스를 설정하려는 컬럼 값에 중복이 있으면 안 됨

```sql
create table 테이블명 (
	id int primary key,
	name varchar(20) not null,
	team_id int,
	backnumber int,
	index 인덱스명 (컬럼명),
	unique index 인덱스명 (컬럼명들..),
);
```

- primary key에는 index가 자동 생성됨

## index 조회

```sql
show index from 테이블명;
```

## B-tree 기반 인덱스 동작 방식

- members table
    
    
    | a | b | c |
    | --- | --- | --- |
    | 3 | … | … |
    | 7 | … | ... |
    | 1 | … | … |
    | 2 | … | … |
    | … | … | … |
    | 7 | … | … |
    | 9 | … | … |
- index(a) table (members의 a컬럼에 대한 index 테이블)
    
    
    | a | ptr |
    | --- | --- |
    | 1 | … |
    | 2 | … |
    | 3 | … |
    | 4 | … |
    | … | … |
    | 13 | … |
- index테이블에서 ptr은 포인터 데이터로 members테이블의 어떤 튜플(행)과 연관있는지 가리키고 있음
- a = 9 인 튜플을 찾는다고 해보면 index테이블에서 binary search를 통해 a = 9인 것을 찾음
- a = 7 and b = 95 인 튜플을 찾는다고 하면 똑같이 binary search를 통해 a = 7인 것을 찾지만 그 이후 b를 찾아야 하기 때문에 a를 찾는 것은 이진 탐색이지만 b를 찾는 것은 full scan이 되어버림
    - 그래서 index(a, b)를 생성하여 해결
    - 만약 index(a, b) 테이블을 생성했다면 이것을 이용해서 b에 대한 탐색을 한다 해도 b에 대한 정렬이 제대로 되어 있지 않기 때문에 효율이 나오지 않거나 index가 아예 사용되지 않을 수 있음 (index(b)를 생성해서 탐색해야함)

## 쿼리가 어떤 index를 쓰는지 확인하기

```sql
explain select * from player where backnumber = 7;
```

- 만약 index가 여러개 있다고 하자, 그럼 이때 어떤 index를 쓰는 기준은 무엇인가??
    - DBMS에 존재하는 optimizer가 알아서 적절하게 index를 선택함
- optimizer가 항상 최고의 index를 선택하지는 않음!
- 특정 index를 사용하도록 명시하는 법
    - MySQL
        
        ```sql
        select * from player force index (인덱스명)
          where backnumber = 7;
        ```
        
        - use index도 존재하나 force index가 좀 더 강제적임 (use index는 써 주세용~)
        - 만약 해당 인덱스로 데이터를 찾을 수 없다면 full scan을 실시함
        - 특정 인덱스를 제외하고 싶다면 ignore index 사용

## index 주의 사항

- index가 많다고 좋은건 아님!
- table에 write할 때마다 index도 변경 발생
    - B-tree일 경우 정렬(트리의 구조 조정)을 해야하므로 많은 시간이 걸릴 수 있음
- 추가적인 저장 공간 차지
- 즉, 불필요한 index를 만들지 말자

## Covering index

- 조회하는 attribute(s)를 index가 모두 cover할 때를 의미
- index(team_id, backnumber)가 존재할 때 team_id = 5;인 데이터를 가져온다고 하면 굳이 player 테이블에서 데이터를 가져올 필요가 없음 (index테이블에서 바로 가져오면 됨)
- 조회 성능이 더 빠름!!

## Hash index

- hash table을 사용하여 index를 구현
- 시간복잡도 O(1)
- 하지만 단점 존재!!
    - rehashing에 대한 부담 (rehashing : hash table을 더 큰 사이즈로 늘려주는 것)
    - index의 equality 비교(==, ≠)만 가능! (range 비교 불가능)

## full scan이 더 좋은 경우

- table에 데이터가 조금 있을 때 (몇 십, 몇 백건 정도)
- 조회하려는 데이터가 테이블의 상당 부분을 차지할 때
    - 남자만 조회, 여자만 조회 ..
- index를 쓸 지 full scan을 할 지는 optimizer가 판단함!!

## 참고 사항

- order by나 group by에도 index가 사용될 수 있음
- FK에는 index가 자동으로 생성되지 않을 수 있음 (MySQL은 생성되나 다른 RDBMS에는 생성되지 않는 경우가 있음)
- 이미 데이터가 몇 백만 건 이상 있는 테이블에 인덱스를 생성하는 경우 시간이 몇 분 이상 소요될 수 있고 DB 성능에 안 좋은 영향을 줄 수 있음