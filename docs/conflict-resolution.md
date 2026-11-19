# Conflict Resolution Log

## 충돌 기록 #1: README 브랜치 전략 섹션 (비자명 충돌: 같은 hunk를 서로 다르게 수정)

### 참여자
- 해결 담당: @ted1117 (B)
- 상대 작성자: @yoo040501 (C)

### 상황(What happened)
- #14에서 README `브랜치 전략: GitHub Flow` 섹션이 Git Flow(develop/release/hotfix) 개념으로 잘못 병합됨 → #15로 이슈 등록
- 두 사람이 같은 커밋(`18e90c4`)에서 각자 브랜치를 분기해 README 15~25줄을 서로 다르게 수정
  - @yoo040501 `feature/dongeun-readme-github-flow` (#16): `develop` 설명 줄 삭제, `develop` → `main` 치환, 선택 이유를 2줄로 축소
  - @ted1117 `feature/yang-readme-github-flow` (#17): 개념 설명을 GitHub Flow 6단계로, 선택 이유를 3줄로 전면 재작성
- #16이 먼저 `main`에 병합된 뒤(`5e5a224`), #17 브랜치에서 `origin/main`을 병합하자 `README.md`에서 충돌 발생
- 과제 기준 "같은 파일의 같은 hunk(인접 라인)를 서로 다르게 수정"에 해당하는 비자명 충돌

### 충돌 내용(Conflict markers)
```txt
$ git merge origin/main
자동 병합: README.md
충돌 (내용): README.md에 병합 충돌
자동 병합이 실패했습니다. 충돌을 바로잡고 결과물을 커밋하십시오.
```

```txt
<<<<<<< HEAD
`main` 브랜치 하나와 짧게 쓰고 버리는 `feature` 브랜치만으로 운영하는 단순한 브랜치 전략이다.

1. `main`은 항상 배포 가능한(깨지지 않은) 상태를 유지한다.
2. 새 작업은 최신 `main`에서 `feature/{이름}-{작업요약}` 브랜치를 분기해 진행한다.
3. 작업 내용을 커밋하고 원격 저장소의 같은 이름 브랜치에 push한다.
4. Pull Request를 열어 변경 사항(What/Why/How)과 연결 이슈(`Closes #번호`)를 공유한다.
5. 팀원이 코드 리뷰를 남기고, 작성자는 피드백을 반영한다.
6. 최소 1명의 승인(Approve)을 받으면 `main`에 병합하고, 작업 브랜치는 삭제한다.

### 우리 팀이 GitHub Flow를 선택한 이유
1. 브랜치가 `main`과 `feature/*` 두 종류뿐이라 Git 협업이 처음인 4인 팀도 규칙을 쉽게 익히고 지킬 수 있다.
2. 모든 변경이 PR과 리뷰를 거쳐 `main`에 들어가므로 `main`을 항상 올바른 상태로 유지하고 변경 이력을 추적할 수 있다.
3. 별도 배포 주기가 없는 학습 노트 프로젝트라 `develop`·`release` 브랜치를 두는 Git Flow보다 작업을 작게 나눠 빠르게 병합하는 방식이 적합하다.
=======
- `main`: 실제 배포된 버전만 존재하는 브랜치로, 릴리스 시점마다 태그를 붙인다.
- `feature/*`: `main`에서 분기하여 기능 단위로 작업한 뒤 다시 `main`으로 병합한다.
- `release/*`: 배포 직전 `main`에서 분기해 QA와 버전 정리를 거친 뒤 `main`에 병합한다.
- `hotfix/*`: 배포된 `main`에서 긴급 수정이 필요할 때 분기해 `main`에 병합한다.

### 우리 팀이 GitHub Flow를 선택한 이유
1. `release` 브랜치로 제출 전 검수 단계를 분리해 문서 품질을 점검할 수 있다.
2. `hotfix` 브랜치로 제출 후 발견된 오류를 다른 작업과 분리해 수정할 수 있다.
>>>>>>> origin/main
```

- `<<<<<<< HEAD` ~ `=======`: 현재 브랜치(`feature/yang-readme-github-flow`)의 내용
- `=======` ~ `>>>>>>> origin/main`: 병합하려는 `origin/main`(#16 반영본)의 내용

![VSCode에서 확인한 README 충돌 마커](images/conflict-1-vscode-markers.png)

### 해결 과정(How)
- 전략: 구간별 선택
  - `GitHub Flow란?` 개념 설명 → `origin/main`(#16) 쪽 채택
  - `선택한 이유` → `HEAD`(#17) 쪽 3줄 채택
- 선택 이유: 먼저 병합된 #16의 개념 설명과 #17의 선택 이유 3줄을 구간별로 채택해 두 사람의 작업을 모두 살렸다. 개념 설명에 남은 Git Flow 요소(`release/*`, `hotfix/*`)는 이번 충돌 해결 범위를 넓히지 않고 후속 PR에서 정리하기로 했다.
- 실행한 명령/절차

  ```bash
  git fetch origin
  git merge origin/main          # README.md 충돌 발생
  # VSCode에서 충돌 구간별로 채택할 쪽 선택, 충돌 마커 제거 확인
  git add README.md
  git commit -m "docs: main의 README 수정 사항 병합 및 브랜치 전략 섹션 충돌 해결"
  git push
  ```

### 결과(Outcome)
- 해결 커밋: `697fe0b`
- 관련 PR: #16(먼저 병합), #17(충돌 해결 후 병합, `999b36b`)
- 관련 이슈: #15 (#17 병합으로 자동 종료)
- 후속 조치: README `GitHub Flow란?`에 남은 `release/*`·`hotfix/*` 설명과 릴리스 태그 문구를 후속 이슈·PR에서 정리 예정

![충돌 해결 후 git log](images/conflict-1-git-log.png)

### 배운 점(Learnings)
- 같은 이슈를 두 사람이 동시에 작업하면 같은 구간을 다르게 고치게 되어 충돌이 거의 확실하다. 이슈 담당자를 한 명으로 정하고, 같이 고칠 때는 수정 범위를 먼저 나눠야 한다.
- 문자열을 기계적으로 치환(`develop` → `main`)하는 수정은 문장은 고쳐도 개념(Git Flow 구조)은 고치지 못한다. 충돌을 해결할 때는 마커만 지우지 말고 결과 문서 전체의 일관성을 다시 읽어야 한다.
- 충돌 해결 커밋을 push한 뒤에 리뷰를 받아야 리뷰어가 해결 결과까지 검증할 수 있다.
