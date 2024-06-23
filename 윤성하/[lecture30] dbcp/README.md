# DBCP

매 요청 시 connection을 열고 닫는것은 성능에 좋지 않다 (3 way handshake)

그래서 나온게 Connection Pool  
처음에 커넥션을 미리 만들어두었다가 요청 시 커넥션 풀에서 커넥션을 가져와 사용  
→ 커넥션을 재사용하기때문에 열고 닫는 시간을 절약할 수 있다

# DB 서버 설정(Mysql 기준)

- max_connections: client와 맺을 수 있는 최대 커넥션 수
- wait_timeout: connection이 inactive 할 때 다시 요청이 오기까지 얼마의 시간을 기다린 뒤에 close 할 것인지

# DBCP 설정 (HikariCP 기준)

- minimulIdle: pool에서 유지하는 최소한의 idle connection 수
- maximumPoolSize: pool이 가질 수 있는 최대 connection 수
	- idle과 in-use connection 합친 최대 수
	- 권장은 minimumIdle == maximumPoolSize (pool size 고정)
- maxLifetime: pool에서 connection의 최대 수명
	- maxLifetime을 넘기면 idle인 경우 바로 제거, in-use인 경우 pool로 반환 후 제거
	- **pool로 반환이 안되면 maxLifetime이 동작을 하지 않는다!**
	- DB의 connection time limit보다 몇 초 짧게 설정해야 한다
		- 만약 동일하다면 요청이 날아가는 중에 DB에서 커넥션을 끊어버릴 수 도 있다
- connectionTimeout: pool에서 connection을 받기 위한 대기 시간
	- 시간을 길게 잡아도 과연 그 시간동안 사용자가 기다려줄까?

# 적절한 Connection 수를 찾아보기

부하테스트!

- 백엔드 서버, DB 서버의 CPU, MEM 등등 리소스 사용률 확인
- thread per request 모델이라면 active thread 수 확인
- DBCP의 active connection 수 확인
- 사용할 백엔드 서버 수를 고려하여 DBCP의 max pool size 결정