# Transaction
- 단일한 `논리적인 작업 단위`
- 논리적인 이유로 여러 SQL문들을 단일 작업으로 묶어서 나눠질 수 없게 만든다
- Transaction의 일부만 DB에 반영될 수 없다

## 관련 명령어
### START TRANSACTION

트랜잭션 시작  
**autocommit은 자동으로 비활성화** (commit/rollback 이후 기본 설정값으로 돌아간다)
```mysql
START TRANSACTION;
```
### COMMIT

작업 내용을 영구적으로 저장  
트랜잭션 종료

```mysql
START TRANSACTION;
UPDATE account SET balance = balance - 200000 WHERE id = 'J';
UPDATE account SET balance = balance + 200000 WHERE id = 'H';
COMMIT;
```

### ROLLBACK

지금까지의 작업들을 모두 취소하고 트랜잭션 이전 상태로 되돌린다  
트랜잭션 종료

```mysql
START TRANSACTION;
UPDATE account SET balance = balance - 200000 WHERE id = 'J';
ROLLBACK;
```

### AUTOCOMMIT

각각의 SQL문을 자동으로 transaction처리
SQL문이 성공적으로 실행되었다면 자동으로 commit, 문제 발생 시 자동 rollback

```mysql
SELECT @@AUTOCOMMIT;
SET autocommit=0; // autocommit 비활성화
SET autocommit=1; // autocommit 활성화
```

## JAVA에서 transaction 처리

```java
public void transfer(String[] args) {
	Connection connection = null;
	Statement statement = null;
	String sql = "SOME_SQL_QUERY";

	try {
		// JDBC 드라이버 로드
		Class.forName("com.mysql.jdbc.Driver");

		// 데이터베이스 연결
		connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/mydatabase", "username", "password");

		// 트랜잭션 시작
		connection.setAutoCommit(false);

		// SQL 쿼리 실행
		statement = connection.createStatement();
		statement.executeUpdate(sql);

		// 트랜잭션 커밋
		connection.commit();
		System.out.println("Transaction committed successfully!");
	} catch (SQLException | ClassNotFoundException e) {
		e.printStackTrace();
		try {
			// 롤백
			if (connection != null) {
				connection.rollback();
				System.out.println("Transaction rolled back successfully!");
			}
		} catch (SQLException ex) {
			ex.printStackTrace();
		}
	} finally {
		// 자원 해제
		try {
			if (statement != null)
				statement.close();
			if (connection != null)
				connection.close();
		} catch (SQLException e) {
			e.printStackTrace();
		}
	}
}
```

spring에서는 [@Transactional](https://docs.spring.io/spring-framework/reference/data-access/transaction/declarative/annotations.html)을 통해 트랜잭션 관련 코드들을 없앨 수 있다. (AOP!)

# ACID

## Atomicity - 원자성
> ALL or NOTHING

- transaction은 논리적으로 쪼갤 수 없는 작업 단위이기에 일부만 반영되어서는 안된다  
- SQL문 실패 시 모든 작업 취소 (Rollback)

개발자는 언제 commit 또는 rollback 할지를 결정하기만 하면 된다

[@Transactional의 예외 발생 시 rollback 전략](https://engineerinsight.tistory.com/282)

## Consistency - 일관성

- Transaction 시작, 종료 시 consistent한 상태여야 한다  
- constraints, trigger 등을 통해 DB에 정의된 규칙 위반 시 rollback (DBMS가 commit 전 확인)

개발자는 application 관점에서 consistent 한지 확인해야 한다. (캐시, 로그 등)

## Isolation - 독립성

- 여러 transaction들이 동시에 실행될 때도 혼자 실행되는 것처럼 동작해야 한다
- 너무 엄격하게 isolation을 구현하면 성능이 좋지 않기에 DBMS는 여러 종류의 isolation level을 제공

개발자는 어떤 isolation level로 transaction을 동작시킬 지 정해야 한다.

동시성 문제!

## Durability - 지속성

- commit된 transaction은 영구적으로 저장된다(HDD, SSD등 비 휘발성 메모리)
- DB system에 문제(power fail, DB crash …)가 생겨도 commit된 transaction은 저장되어야 함