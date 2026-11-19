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
1. `release` 브랜치로 제출 전 검수 단계를 분리해 문서 품질을 점검할 수 있다.
2. `hotfix` 브랜치로 제출 후 발견된 오류를 다른 작업과 분리해 수정할 수 있다.

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
