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
- `main`: 실제 배포된 버전만 존재하는 브랜치로, 릴리스 시점마다 태그를 붙인다.
- `feature/*`: `main`에서 분기하여 기능 단위로 작업한 뒤 다시 `main`으로 병합한다.
- `release/*`: 배포 직전 `main`에서 분기해 QA와 버전 정리를 거친 뒤 `main`에 병합한다.
- `hotfix/*`: 배포된 `main`에서 긴급 수정이 필요할 때 분기해 `main`에 병합한다.

### 우리 팀이 GitHub Flow를 선택한 이유
1. 브랜치가 `main`과 `feature/*` 두 종류뿐이라 Git 협업이 처음인 4인 팀도 규칙을 쉽게 익히고 지킬 수 있다.
2. 모든 변경이 PR과 리뷰를 거쳐 `main`에 들어가므로 `main`을 항상 올바른 상태로 유지하고 변경 이력을 추적할 수 있다.
3. 별도 배포 주기가 없는 학습 노트 프로젝트라 `develop`·`release` 브랜치를 두는 Git Flow보다 작업을 작게 나눠 빠르게 병합하는 방식이 적합하다.

## 학습 노트 목차
- 배열과 연결 리스트
- 스택과 큐
- 트리와 힙
- 해시와 그래프

## 협업 문서
- [기여 가이드](docs/CONTRIBUTING.md)
- [충돌 해결 기록](docs/conflict-resolution.md)
- [트러블슈팅 기록](docs/troubleshooting-log.md)
- [제출물 인덱스](SUBMISSION.md)
