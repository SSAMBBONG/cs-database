# DB 정규화

데이터 중복과 insertion, update, deletion anomaly를 최소화하기 위해 일련의 normal form(NF)에 따라 relational DB를 구성하는 과정

### Normal Form

정규화 되기 위해 준수해야 하는 몇 가지의 rule들

- 처음부터 순차적으로 진행
- 앞 단계를 만족해야 다음 단계로 진행 가능
- BCNF 단계까지는 FD와 key만으로 정의된다
- 일반적으로 3NF 또는 BCNF 까지만 정규화 진행

| 정규형     | 설명                                                                |
| ------- | ----------------------------------------------------------------- |
| **1NF** | 모든 테이블은 원자값(나눌 수 없는 값)만을 포함해야 한다                                  |
| **2NF** | 모든 non-prime attribute는 모든 key에 fully functionally dependent해야 한다 |


> [!IMPORTANT]
> 
> ### key가 composite key가 아니라면 2NF는 자동적으로 만족한다?
> 
> 2NF는 모든 non-prime attribute는 어떤 key에도 partially dependent 하면 안된다  
> → composite key가 아니라면 partially dependent 할 수 없으므로 일반적으로는 맞다.
> 
> 하지만 앞에서 {} → Y인 FD가 있다고 배웠었다.  
> 공집합도 key의 부분집합이므로 이러한 경우 2NF를 위반한다.

***
super key: table에서 tuple들을 unique하게 식별할 수 있는 attribute set
(cantidate) key: 어느 한 attribute라도 제거하면 unique하게 튜플들을 식별할 수 없는 super key
primary key: table에서 튜플들을 unique하게 식별하기 위해 선택한 키
prime attribute: 임의의 key에 속하는 attribute
non-prime attribute: 어떠한 key에도 속하지 않는 attribute