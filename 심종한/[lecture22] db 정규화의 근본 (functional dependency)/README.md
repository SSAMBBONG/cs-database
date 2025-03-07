# lecture22 - db 정규화의 근본 (functional dependency)

## functional dependency

`한 테이블에 있는 두 개의 속성 집합 사이의 제약` 을 의미한다.

### EMPLOYEE

| empl_id | empl_name | birth_date | position | salary | dept_id |
| ------- | --------- | ---------- | -------- | ------ | ------- |

#### 집합 X

- empl_id

#### 집합 Y

- empl_name
- birth_date
- position
- salary
- dept_id

X 값에 따라 Y 값이 유일하게 (uniquely) 결정될 때 `X가 Y를 함수적으로 결정한다 (functionally determine)`, `Y가 X에 함수적으로 의존한다 (functionally dependent)` 라고 말할 수 있고, 두 집합 사이의 이러한 제약 관계를 functional dependency (FD)라고 부른다.

> [!IMPORTANT]
>
> 테이블의 스키마를 보고 의미적으로 FD가 존재하는지 파악해야 한다.

## 함수적 종속 종류

### Trivial functional dependency

X -> Y 일 때, Y가 X의 부분집한인 경우

```
{ a, b, c } -> { c }
{ a, b, c } -> { a, c }
{ a, b, c } -> { a, b, c }
```

### Non-trivial functional dependency

X -> Y 일 때, Y가 X의 부분집합이 아닌 경우

```
{ a, b, c } -> { b, c, d}
{ a, b, c } -> { d, e } // 완벽히 겹치는 것이 없으므로 completely non-trivial FD이기도 함
```

### Partial functional dependency

X -> Y 일때, X의 진부분집합도 Y를 함수적으로 결정할 수 있는 경우

```
{ empl_id, empl_name } -> { birth_date } 이 가능하다면
{ empl_id } -> { birth_date } 도 가능하다.
```

### Full functional dependency

X -> Y 일때, 모든 X의 진부분집합으로 Y를 함수적으로 결정할 수 없는 경우

```
{ stu_id, class_id } -> { grade } 이 가능하다면
{ stu_id }, { class_id }, {} 만으로 { grade }를 결정할 수 없다.
```

## FD와 관련된 추가적인 개념

- Armstrong's axioms
- Closure
- minimal cover
