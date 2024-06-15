### Transitive FD

X → Y 이고 Y → Z 가 성립한다면 X → Z 는 transitive FD이다
- Y와 Z 모두 어떤 key에 대해서도 부분집합이 아니여야 한다


| 정규형      | 설명                                                                                                                                                      |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **3NF**  | - 기본 키에 의존하지 않는 열을 제거<br>- non-prime attribute와 non-prime attribute 사이에는 FD가 있으면 안된다<br>- 모든 non-prime attribute는 어떤 key에도 transitively dependent하면 안된다 |
| **BCNF** | - 모든 결정자는 후보 키여야 한다<br>- 모든 유효한 non-trivial FD[^1] X → Y는 X가 super key여야 한다                                                                             |


[^1]: Y가  X의 부분 집합이 아닌 FD