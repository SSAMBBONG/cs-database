# Git Convention for Database Study

## 스터디 정보

- 시작 날짜: 2024-06-10
- 종료 날짜: 미정
- 강의: [쉬운코드-데이터베이스](https://youtube.com/playlist?list=PLcXyemr8ZeoREWGhhZi5FZs6cvymjIBVe&si=6qCWRhJ1jPhBDTLw)

## 벌금

- 과제를 기간내에 완료하지 못한 경우 벌금을 내야 합니다.
- 5000원 (특별한 사유가 있는 경우 제외)

## 브랜치 명명 규칙

- 각자의 브랜치는 본인의 이름과 공부하는 강의 번호를 포함하여 생성합니다. 이는 다른 사람들이 브랜치를 보았을 때 누가 어떤 내용을 작업 중인지 쉽게 파악할 수 있게 합니다.
- **구조**: `lecture<start>~<end>-<initial>-`
  - `<start>`: 시작 강의 번호
  - `<end>`: 마지막 강의 번호
  - `<initial>`: 이름 이니셜
- **예시**:
  - `lecture01~04-hgd`
  - `lecture12~15-ysh`

## 폴더 구조 규칙

- 각자의 이름으로 최상위 폴더를 만들고, 그 안에 강의별 폴더를 생성합니다. 각 챕터 폴더 안에는 README.md 파일이 위치해야 하며, 이 파일에는 해당 강의의 학습 내용이 정리됩니다.
- **구조**: `<your-name>/[lecture<number>] <lecture-description>/README.md`
  - `<your-name>`: 본인의 이름 (ex. 홍길동, 김철수...)
  - `<number>`: 강의 번호 (두 자릿수로 표현, 예: 01, 02)
  - `<lecture-description>`: 사전에 정한 강의 주제명
- **예시**:
  - `홍길동/[lecture14] transaction (ACID)/README.md`
  - `김철수/[lecture15] concurrency control 기초 (serializability)/README.md`

## README.md 작성 규칙

- 각 `README.md` 파일은 다음과 같은 정보를 포함해야 합니다:
  - 챕터 제목
  - 주요 학습 내용 요약
  - 중요 개념 설명
  - 필요한 경우, 추가 자료 링크

## Pull Request (PR) 규칙

- 모든 PR은 해당 챕터의 제목을 명확하게 설명하는 타이틀과 과제 수행 여부, 본인 이름의 태그를 포함해야 합니다.
- **PR 타이틀 양식**: `lecture<start>~<end>`
  - `<start>`: 시작 강의 번호
  - `<end>`: 마지막 강의 번호
- **예시**:
  - `lecture14~18`
  - `lecture19~23`
- **PR 설명**: 다음 정보를 포함해야 합니다:
  - 과제 수행 여부
  - 사유 (과제를 수행하지 못한 경우)
  - 관련 문서나 이슈 (있는 경우)
- 모든 PR은 적어도 한 명의 다른 팀원에 의해 리뷰되어야 하며, 필요에 따라 적절한 피드백이나 수정 후 **Squash Merge** 합니다.

## Commit Message 작성 규칙

- 네트워크 스터디 이후 스터디원의 피드백을 반영하여 PR 타이틀을 커밋 메시지로 자동 기입합니다.
- **귀찮게 커밋 메시지 따로 작성하지 않아도 됩니다**
- **예시**
  - `lecture14~18 (#2)`
  - `lecture19~23 (#3)`
