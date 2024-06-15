## [lecture22] db 정규화의 근본 (functional dependency)

### 바른 DB schema 설계

1. 의미적으로 관련있는 속성들끼리 테이블을 구성
2. 중복 데이터를 최대한 허용하지 않도록 설계
3. join 수행 시 가짜 데이터가 생기지 않도록 설계
4. 되도록이면 null값을 줄일 수 있는 방향으로 설계

cf. 성능향상을 위해 일부러 역정규화하는 경우 있음

<hr>

- **functional dependency(FD)**
  한 테이블에 있는 두개의 attributes 집합 사이의 제약
  - X에 따라 Y값이 유일하게 결정될 때 'X가 Y를 함수적으로 결정/의존한다'
  - X -> Y
  - X -> Y 일 때 Y -> X는 존재할수도 있고 아닐 수도 있음
  - DB 내에 테이블 스키마를 보고 의미적으로 파악해야 함
  - 테이블의 특정 상태만 보고 FD를 파악해서는 안됨
  - ex)
    {stu_id} -> {stu_name, birth_date, address}
    {class_id} -> {class_name, year, semester, credit}
    {stu_id, class_id} -> {grade}
- **Trival functional dependency**
  - X -> Y 일때, 만약 Y가 X의 부분집합이면, X -> Y는 trivial FD
  - ex)
    {a,b,c} -> {c}
    {a,b,c} -> {a,c}
    {a,b,c} -> {a,b,c}
- **Non-trival functional dependency**
  - X -> Y 일때, 만약 Y가 X의 부분집합이 아니면, X -> Y는 Non-trivial FD
  - ex)
    {a,b,c} -> {b,c,d}
    {a,b,c} -> {d,e} (completely non-trivial FD)
- **Partial functional dependency**
  - proper subset
    - 집합 X의 proper subset은 X의 부분집합이지만 X와 동일하지는 않은 집합 = 자기자신을 제외한 부분집합(공집합 포함)
  - X -> Y일때, X의 어떠한 proper subset이 Y를 결정할 수 있다면 X->Y는 partial FD
  - ex)
    {empl_id, empl_name} -> {birth_date} :: empl_id만으로 birth_date 결정가능 (즉 partial FD)
- **full functional dependency**
  - X -> Y일때, X의 어떠한 proper subset도 Y를 결정할 수 없을 때 X->Y는 full FD
  - ex)
    {stu_id, class_id} -> {grade}
