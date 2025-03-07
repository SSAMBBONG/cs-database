# Functional Dependency

한 테이블에 있는 두 개의 attribute set 사이의 제약

X값에 따라 Y값이 유일하게 결정될 때
> X가 Y를 함수적으로 결정한다
> Y가 X에 함수적으로 의존한다
> 
> 표기: X(left hand side) → Y(right hand side)

이러한 관계를 Functional Dependency라고 부른다.

> [!NOTE]
> 
> X → Y 라고 해서 Y → X는 아니다  
> {} → Y의 의미는 언제나 항상 Y라는 값만 나온다

### Functional Dependency를 파악할 때 주의할 점

테이블의 스키마를 보고 의미적으로 파악해야 한다.  
→ 테이블의 특정 순간의 특정 상태(state)만 보고 파악하면 안된다.

ex) 사람의 이름으로 생일이 결정되는가?  
사람의 이름은 중복될 수 있기 때문에 사람의 이름으로는 unique한 값이 결정될 수 없다

구축하려는 DB의 attributes가 관계적으로 어떤 의미를 지니는지에 따라 FD가 달라진다.  
(같은 테이블이더라도 정책에 따라 FD가 될 수도 안될 수도)

### FD의 예

{stu_id} → {stu_name, birth_date, address}

{class_id} → {class_name, year, semester, credit}

{stu_id, class_id} → {grade}

{bank_name, bank_account} → {balance, open_date}

{user_id, location_id, visit_date} → {comment, picture_url}

### FD의 종류

- Trivial Functional Dependency
	- X → Y 일 때, Y가 X의 subset인 경우
	- {a, b, c} → { c }: trivial FD
- Non-trivial functional Dependency
	- X → Y 일 때, Y가 X의 subset이 아닌 경우
	- {a, b, c} → {b, c, d}: non-trivial FD
	- 공통된 attribute가 단 하나도 없는 경우는 completely non-trivial FD라고 부른다
- Partial Functional Dependency
	- X → Y 일 때 X의 진부분집합이 Y를 결정지을 수 있는 경우
	- {empl_id, empl_name} → {birth_date}, {empl_id} → {birth_date}
- Full Functional Dependency
	- X → Y 일 때 X의 진부분집합이 Y를 결정지을 수 없는 경우