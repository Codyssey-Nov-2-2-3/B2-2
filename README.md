# 자료구조 학습 노트 (B2-2 Git 협업 과제)

4인 팀이 Git/GitHub 협업 워크플로우(브랜치, PR, 코드 리뷰, 충돌 해결)를 연습하며 자료구조 학습 노트를 함께 작성하는 저장소입니다.

## 팀원
| 역할 | GitHub | 담당 노트 |
|---|---|---|
| A | @Dong-tak | 배열과 연결 리스트 |
| B | @ted1117 | 스택과 큐 |
| C | @yoo040501 | 트리와 힙 |
| D | @ChoMyeongHwan | 해시와 그래프 |

## 브랜치 전략: GitHub Flow

### GitHub Flow란?
GitHub Flow는 GitHub에서 제안한 가벼운 브랜치 기반 워크플로우로, 항상 배포 가능한 상태인 `main` 브랜치 하나를 기준으로 모든 변경을 짧게 쓰는 작업 브랜치에서 진행한 뒤 Pull Request와 리뷰를 거쳐 `main`에 병합하는 방식이다.

### 우리 팀이 GitHub Flow를 선택한 이유
1. 기준 브랜치 `main`과 작업 브랜치 `feature/*` 두 가지만 운영하기로 합의하여, GitHub Flow를 처음 접하는 팀원도 규칙을 쉽게 익히고 지킬 수 있다.
2. 모든 변경이 PR과 리뷰를 거쳐 `main`에 들어가므로 `main`을 항상 올바른 상태로 유지하고 변경 이력을 추적할 수 있다.
3. 별도 배포 주기가 없는 학습 노트 프로젝트라 `develop`·`release` 브랜치를 두는 Git Flow보다 작업을 작게 나눠 빠르게 병합하는 방식이 적합하다.

## 학습 노트 목차
- 배열과 연결 리스트
- 스택과 큐
- [트리와 힙](src/03-binary-tree-heap.md)
- [해시와 그래프](src/04-hash-graph.md)

## 협업 문서
- [기여 가이드](docs/CONTRIBUTING.md)
- [충돌 해결 기록](docs/conflict-resolution.md)
- [트러블슈팅 기록](docs/troubleshooting-log.md)
- [제출물 인덱스](SUBMISSION.md)

## 자료구조 시간복잡도 요약표

배열(파이썬 `list`)과 단일 연결 리스트의 연산별 시간복잡도입니다. 자세한 설명과 구현 예제는 [배열과 연결 리스트 노트](src/01-array-linkedlist.md)에 있습니다.

| 연산 | 배열 (`list`) | 단일 연결 리스트 |
|---|---|---|
| 인덱스 접근 | O(1) | O(n) |
| 값 탐색 | O(n) | O(n) |
| 앞쪽 삽입 | O(n) | O(1) |
| 뒤쪽 삽입 | 분할 상환 O(1) (리사이징 순간만 O(n)) | `tail` 없으면 O(n), 있으면 O(1) |
| 중간 삽입 (위치를 알 때) | O(n) | O(1) (앞 노드를 알 때) |
| 앞쪽 삭제 | O(n) | O(1) |
| 뒤쪽 삭제 | O(1) | O(n) (`tail`이 있어도 앞 노드를 찾아야 함) |
| 중간 삭제 (위치를 알 때) | O(n) | O(1) (앞 노드를 알 때) |

- 연결 리스트에서 "위치를 안다"는 것은 앞 노드를 이미 가리키고 있다는 뜻이며, 그 위치를 값으로 찾는 비용(O(n))은 포함하지 않았습니다.
- 이중 연결 리스트는 `prev`가 있어 뒤쪽 삭제도 O(1)입니다.

### 배열과 연결 리스트 노트 목차

- [2. 배열 (Array)](src/01-array-linkedlist.md#2-배열-array)
- [3. 연결 리스트 (Linked List)](src/01-array-linkedlist.md#3-연결-리스트-linked-list)
- [4. 배열과 연결 리스트 비교](src/01-array-linkedlist.md#4-배열과-연결-리스트-비교)
- [6. 파이썬 구현 예제](src/01-array-linkedlist.md#6-파이썬-구현-예제)
