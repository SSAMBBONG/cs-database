# lecture25 - db 인덱스

## first_name에 인덱스가 걸려있다면?

```sql
SELECT * FROM customer WHERE first_name = 'Minsoo';
```

- 풀 스캔보다 더 빠르게 찾을 수 있다.

## 인덱스

### 인덱스를 사용하는 이유

- 조건을 만족하는 튜플을 빠르게 조회하기 위해
- 빠르게 정렬(order by)하거나 그룹핑(group by)하기 위헤

### 예제

| id  | name | team_id | backnumber |
| --- | ---- | ------- | ---------- |
| ... | ...  | ...     | ...        |

- 이 쿼리는 name 컬럼에 player_name_idx라는 이름의 인덱스를 생성한다.
- 인덱스를 사용하면 name 컬럼을 기준으로 한 검색이 더 빠르게 수행될 수 있다.

```sql
SELECT * FROM player WHERE name = 'Sonny';

CREATE INDEX player_name_idx ON player (name);
```

<br/>

- team_id, backnumber는 player 테이블 각 선수 정보를 유니크하게 식별할 수 있다
- 유니크 인덱스를 사용하면 team_id와 backnumber의 조합이 테이블에서 유일하도록 보장된다.

```sql
SELECT * FROM player WHERE team_id = 105 and backnumber = 7;

CREATE UNIQUE INDEX team_id_backnumber_idx ON player (team_id, backnumber);
```

**즉 사용하는 쿼리에 맞춰 적절하게 인덱스를 걸어줘야 빠르게 처리할 수 있다.**

### 쿼리문이 사용하는 인덱스를 알아보는 방법

```sql
EXPLAIN SELECT * FROM player WHERE team_id = 105 and backnumber = 7;
```

쿼리문에서 사용하는 인덱스는 DBMS의 opimizer가 알아서 적절하게 인덱스를 선택한다.

만약 직접 쿼리문에서 사용될 인덱스를 지정하고 싶은 경우 아래 쿼리를 실행하면 된다.

```sql
# 권장 사항(반드시 사용한다는 것을 보장하지 않음)
SELECT * FROM player USE INDEX (backnumber_idx) WHERE backnumber = 7;

# 더욱 강제(하지만 해당 인덱스를 사용해서 조회가 불가능하면 `optimezer` 가 다른 방법을 선택한다)
SELECT * FROM player FORCE USE INDEX (backnumber_idx) WHERE backnumber = 7;
```

### Covering index

만약 team_id, backnumber 로 인덱스를 생성한 상태에서 아래 쿼리를 실행한다고 가정해보자.

```sql
SELECT team_id, backnumber FROM player WHERE team_id = 5;
```

`team_id` 를 기준으로 인덱스 테이블을 탐색하게 되고, 이후 실제 테이블을 찾아가 데이터를 조회하게 된다. 그러나 지금은 인덱스 정보에 `backnumber` 가 함께 저장되어 있어 굳이 실제 테이블에 접근할 필요가 없다.

이처럼 조회하는 속성을 인덱스가 모두 커버할 때 `커버링 인덱스`라고 부른다. 일종의 테크닉이다.

![alt text](<covering index.png>)

### Hash index

- hash table을 사용하여 인덱스를 구현
- O(1) 시간 복잡도
- rehashing에 대한 부담
  - hash 테이블이 꽉 찬 경우 테이블의 크기를 늘리는 과정이 부담될 수 있음
- range 비교 불가능
  - 정렬 정보가 없어 범위 검색이 불가능
- 인덱스 정보의 일부 칼럼 정보만으로 인덱스를 사용할 수 없음

### 풀 스캔이 더 좋은 경우

- 테이블에 데이터가 조금 있는 경우 (몇 십, 몇 백건 정도)
- 조회하려는 데이터가 테이블의 상당 부분을 차지하는 경우

### 주의사항

> [!NOTE]
>
> 1. 두 개 이상의 칼럼으로 구성된 인덱스를 `multicolumn index` 또는 `composite index` 라고 부른다.
> 2. MySQL에서 primary key에는 인덱스가 자동으로 생성된다.
> 3. MySQL에서 인덱스는 항상 정렬된 상태로 유지된다. 그리고 새로운 튜플이 추가되면 인덱스를 재구성하게 되며 저장 공간을 차지하는 등 인덱스 유지 비용이 발생한다.
> 4. 외래키는 인덱스가 자동으로 생성되지 않을 수 있다.
> 5. 이미 데이터가 몇 백만건 이상 있는 테이블에 인덱스를 생성하는 경우 해당 서버에 트래픽이 적은 시간대에 작업을 하는 것이 좋다. 작업이 오래 걸려 DB 성능이 저하될 수 있다.
