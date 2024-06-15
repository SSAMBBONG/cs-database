## [lecture24] db 정규화 (3NF, BCNF, 역정규화)

- **3NF** : 모든 non-prime-attribute는 어떤 key에도 transitively dependent 하면 안됨
  `3NF 위반`

| StudentID | CourseID | Instructor | InstructorEmail |
| --------- | -------- | ---------- | --------------- |
| 1         | 101      | Smith      | smith@uni.edu   |
| 2         | 102      | Johnson    | johnson@uni.edu |

`3NF 만족`

| StudentID | CourseID | Instructor |
| --------- | -------- | ---------- |
| 1         | 101      | Smith      |
| 2         | 102      | Johnson    |

| Instructor | InstructorEmail |
| ---------- | --------------- |
| Smith      | smith@uni.edu   |
| Johnson    | johnson@uni.edu |

non-prime attribute와 non-prime attribute 사이에는 FD가 있으면 안됨

- **BCNF** : 모든 유효한 non-trivial FD X -> Y 는 X가 super key여야 함

`BCNF 위반`

| StudentID | CourseID | Instructor |
| --------- | -------- | ---------- |
| 1         | 101      | Smith      |
| 1         | 102      | Johnson    |
| 2         | 101      | Smith      |
| 2         | 103      | Johnson    |

`BCNF 만족`

| StudentID | CourseID |
| --------- | -------- |
| 1         | 101      |
| 1         | 102      |
| 2         | 101      |
| 2         | 103      |

| CourseID | Instructor |
| -------- | ---------- |
| 101      | Smith      |
| 102      | Johnson    |
| 103      | Johnson    |

<span style="color:gray">(보통 여기까지만 정규화시킴) </span>

- **4NF** : 다치 종속성(Multi-valued Dependency)을 제거한 정규형
- **5NF** : 조인 종속성(Join Dependency)을 제거한 정규형
- **6NF** : 데이터베이스의 모든 관계가 5NF를 만족하고, 시간에 따른 변화를 추적할 수 있도록 분해된 정규형

> DB 설계 시 과도한 조인과 중복 데이터 최소화 사이에서 적정 수준을 잘 선택할 필요가 있음
> **Denormalization을 적절히 활용하자!!**
