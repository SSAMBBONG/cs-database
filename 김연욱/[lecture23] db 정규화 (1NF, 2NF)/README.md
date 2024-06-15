> # [Lecture 23](https://www.youtube.com/watch?v=EdkjkifH-m8&list=PLcXyemr8ZeoREWGhhZi5FZs6cvymjIBVe&index=23)

## 주요 내용

- DB 정규화
- 1NF
- 2NF

## DB Normalization

- DB 정규화
- 데이터 중복과 insertion, deletion, modification anomaly를 최소화하기 위해 일련의 NF(Normal Forms)에 따라 Relation DB를 구성하는 과정

## Normal Forms

- 정규화 되기 위해 준수해야 하는 몇 가지 규칙들이 있는데 이 각각의 규칙을 NF라고 부름

## DB 정규화 과정

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/df3be3e2-8b43-4d4f-8a98-a4b36f42e3d8/c57f674e-a5db-4d63-b429-4d0f0d929ba8/Untitled.png)

- 1NF ~ BCNF
    - FD와 Key만으로 정의되는 NF
    - 3NF까지 도달하면 정규화 됐다고 말하기도 함
    - 보통 실무에서는 3NF 혹은 BCNF까지 진행 (많이 해도 4NF 정도까지만 진행)

## 1NF

- attribute의 value는 반드시 나눠질 수 없는 단일한 값이어야 한다

## 2NF

- 모든 non-prime attribute는 모든 key에 fully functionally dependent 해야 한다
- non-prime attribute : 어떠한 key에도 속하지 않는 attribute