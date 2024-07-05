Insertion anomalies
- 중복된 데이터가 많다
- 저장 공간 낭비
- 실수로 인한 데이터 불일치 가능성 존재 (오타, ...)
- null값이 많다. (null은 적게 쓰는 것이 좋다.)
	- SQL에서 NULL과 비교 연산을 하게 되면 그 결과는 UNKNOWN이다(TRUE, FALSE 아님!)
	- three-valued logic: 비교/논리 연산의 결과로 TRUE, FALSE, UNKNOWN을 가진다
	- WHERE 절은 condition의 결과가 TRUE인 tuple만 선택된다.
	- v NOT IN (v1, v2, v3)를 살펴보자
		- 3 NOT IN (1, 2, NULL) -> UNKNOWN!!!!!!!!
		- 서브쿼리를 작성 할 때 주의해야한다 (WHERE D.id NOT IN (some subquery...))
- 강제적인 불필요한 데이터 입력
- 두 개 이상의 관심사가 하나의 테이블에 존재한다

해결: 관심사별로 테이블을 분리하자

Deletion anomalies
- 특정 데이터를 삭제할 때 관련된 다른 유용한 정보까지 함께 삭제

Update anomalies
- 하나의 값을 수정해야 할 때, 동일한 데이터가 여러 레코드에 존재하면 모든 곳에서 동일하게 업데이트 필요

Spurious Tuples
- 조인 연산을 수행할 때 잘못된 설계로 인해 발생하는, 의미가 없는 불필요한 튜플들

많은 null
이게 왜 문제?
- null값이 있는 column으로 join 하는 경우 상황에 따라 예상과 다른 결과 발생
- null 값이 있는 column에 aggremate function을 사용했을 때 주의 필요
- 불필요한 storage 낭비

그럼 어떻게 DB를 설계해야 하나?
- 의미적으로 관련있는 속성들끼리 테이블을 구성
- 중복 데이터를 최대한 허용하지 않도록 설계
- join 수행 시 가짜 데이터가 생기지 않도록 설계 (PK, FK!)
- 되도록이면 null 값을 줄일 수 있는 방향으로 설계
- 다만, 성능 향상을 위해 일부러 테이블을 나누지 않을 수 도 있다 (비정규화, 반정규화)