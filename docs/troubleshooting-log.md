## 시나리오: revert (`git revert`)

### 참여자

- 김동은 (팀원 C, @yoo040501): 실습 수행 및 기록 작성

### 상황

트리·힙 노트의 파일명 변경과 정리 문장 보완을 완료한 뒤,
`git revert` 실습을 위해 `heapify`의 시간복잡도를
올바른 `O(n)`에서 잘못된 `O(n log n)`으로 변경했다.

파일명 변경 작업은 유지하면서, 잘못된 설명을 넣은 커밋만
되돌리는 것이 목표였다.

### 실행한 명령과 절차

1. `src/03-binary-tree-heap.md`에서 `heapify`의 시간복잡도를
   실습용으로 잘못 변경했다.
2. 변경 사항을 별도 커밋으로 기록했다.

```bash
git add src/03-binary-tree-heap.md
git commit -m "docs: revert 실습용 heapify 복잡도 변경"
git log -1 --oneline
```

생성된 실습 커밋은 `583c189`이다.

3. 다른 커밋을 추가하지 않은 상태에서 마지막 커밋을 되돌렸다.

```bash
git revert --no-commit HEAD
git --no-pager diff --cached
git commit -m "fix: revert로 heapify 시간복잡도 복구"
```

생성된 복구 커밋은 `09ca0cb`이다.
`--no-commit`으로 되돌린 변경을 먼저 확인한 뒤,
팀의 커밋 메시지 규칙에 맞춰 별도로 커밋했다.

### 결과와 검증

| 커밋 | 내용 |
|---|---|
| `866b029` | 트리·힙 노트 파일명 변경 및 정리 보완 |
| `583c189` | 실습용으로 heapify 시간복잡도를 잘못 변경 |
| `09ca0cb` | revert로 올바른 시간복잡도 복구 |

다음 명령으로 실습 전과 복구 후 파일을 비교했다.

```bash
git diff 866b029 09ca0cb -- src/03-binary-tree-heap.md
```

출력이 없어 두 커밋의 파일 내용이 같음을 확인했다.
`heapify` 설명은 `O(n)`으로 복구됐으며,
파일명 변경과 정리 문장 보완은 유지됐다.

### 실행 증빙

`git revert --no-commit HEAD` 실행 후, 스테이징된 변경에서
heapify의 시간복잡도가 `O(n)`으로 복구되는 것을 확인했다.

![revert 실행과 복구 내용](images/revert-command-diff.png)

실습 변경 커밋과 복구 커밋이 모두 이력에 남아 있다.

![실습 커밋과 복구 커밋 로그](images/revert-commit-log.png)

### revert를 선택한 이유와 주의 사항

- `revert`는 기존 커밋을 삭제하지 않고 반대 변경을 적용한다.
  이를 새 커밋으로 기록하면 변경과 복구 과정을 모두 추적할 수 있다.
- 공유된 커밋을 되돌릴 때도 기존 히스토리를 재작성하지 않는다.
- `HEAD`는 실행 시점의 마지막 커밋이다. 이 실습에서는
  `583c189`가 HEAD인 상태에서 실행했다.
- 특정 커밋을 되돌릴 때는 대상 해시를 먼저 확인해야 한다.
  파일명 변경 커밋까지 되돌리지 않도록 실습 변경을 별도 커밋으로 나눴다.
- `--no-commit` 옵션을 사용하면 자동으로 커밋되지 않으므로,
  결과 확인 후 직접 커밋해야 한다.

---

## 시나리오: stash (`git stash` / `git stash pop`)

### 참여자

- 조명환 (팀원 D, `@ChoMyeongHwan`): 실습 수행 및 기록 작성

### 상황

[PR #20](https://github.com/Codyssey-Nov-2-2-3/B2-2/pull/20) 작업을 위해 `feature/cho-hash-note` 브랜치에서 `src/04-hash-graph.md`의 전반부(해시 테이블 개념, 60줄)를 작성하던 중, 아직 그래프 섹션과 Python 구현 예제가 미완성이라 커밋하기 애매한 상태에서 `main` 브랜치로 전환해 `README.md`의 목차 및 협업 문서 링크 구성을 확인해야 하는 상황이 발생했다.

미완성 상태로 의미 없는 `wip` 커밋을 남기지 않고 작업 내용을 안전하게 보관한 뒤 브랜치를 전환했다가 복귀해 이어서 작업하는 것이 목표였다.

### 시도한 명령/절차

```bash
# 1. 작업 중 변경 상태 확인
git status
git diff --stat
# 출력: modified: src/04-hash-graph.md (60 insertions(+))

# 2. 설명 메시지와 함께 작업 내용을 스택(Stash)에 임시 보관
git stash push -m "docs: 해시 테이블 개념 작성 중 임시 보관"
# 출력: Saved working directory and index state On feature/cho-hash-note: docs: 해시 테이블 개념 작성 중 임시 보관

# 3. 보관 목록 및 깨끗해진 워킹 트리 확인
git stash list
# 출력: stash@{0}: On feature/cho-hash-note: docs: 해시 테이블 개념 작성 중 임시 보관
git status
# 출력: nothing to commit, working tree clean

# 4. 다른 브랜치(main)로 안전하게 전환해 README.md 상태 확인
git switch main
git status
# 출력: On branch main / nothing to commit, working tree clean

# 5. 원래 작업 브랜치로 복귀 후 임시 보관분 복원 및 스택에서 제거
git switch feature/cho-hash-note
git stash pop
# 출력: modified: src/04-hash-graph.md
#       Dropped refs/stash@{0} (c3941136de6f5f7b36bd45cf69dc8334f96254b9)

# 6. 스태시 목록이 비워지고 60줄 내용이 복원되었는지 확인
git stash list
```

### 결과

- `git stash push` 실행 직후 워킹 트리가 깨끗해져 미완성 수정분이 `main` 브랜치로 섞여 들어가지 않고 안전하게 브랜치를 전환할 수 있었다.
- `feature/cho-hash-note`로 복귀해 `git stash pop`을 실행하자 `src/04-hash-graph.md`의 60줄 작성분이 손실 없이 복원되었고 `stash@{0}` 항목도 자동으로 정리(`Dropped`)되었다.
- 이후 그래프 섹션과 Python 3.10+ 예제 3종을 완성해 [PR #20](https://github.com/Codyssey-Nov-2-2-3/B2-2/pull/20)(머지 커밋 `9c3e9d8`)으로 깔끔하게 병합했다.

### 주의할 점 (원격 히스토리와 협업 영향)

- `git stash` 저장소는 **로컬 저장소 전용**이며 `git push`로 원격에 올라가지 않는다. 따라서 다른 PC로 이동하거나 로컬 폴더를 삭제하면 보관한 내용이 유실되므로 단기 전환 용도로만 써야 한다.
- 기본 `git stash push`는 Git이 추적 중인(tracked) 파일과 스테이징된 파일만 보관한다. 새로 만든 미추적(untracked) 파일까지 함께 보관하려면 `-u`(`--include-untracked`) 옵션을 붙여야 한다.
- `git stash pop`을 잘못된 브랜치에서 실행하면 의도치 않은 브랜치에 변경이 적용되거나 충돌이 발생할 수 있으므로, 반드시 `git branch`로 현재 브랜치를 확인한 뒤 복원한다. (충돌이 나면 스태시 항목이 자동 삭제되지 않고 남아 재확인이 가능하다.)

### 왜 이 방법을 선택했는가 (Why)

| 대안 | 선택하지 않은 이유 |
|---|---|
| `wip` 임시 커밋 후 브랜치 전환 | 팀 커밋 컨벤션에서 `wip`, `temp` 등 의미 없는 커밋 메시지를 금지하고 있으며, 나중에 커밋을 다시 되돌리거나 정리해야 하는 번거로움이 생긴다. |
| 변경분을 수동 복사 후 `git restore` | 파일이 여러 개일 때 누락이나 덮어쓰기 실수가 발생하기 쉽고 Git이 변경 이력을 관리해 주지 않는다. |

아직 하나의 완성된 커밋 단위가 아닌 작업 중 상태를 **원격 히스토리에 흔적을 남기지 않고 안전하게 보관·복원**하는 용도로는 `git stash` / `git stash pop`이 가장 적합하다.

---

## 보너스 과제 1: 개인 feature 브랜치 히스토리 정리 (`git rebase -i`)

### 참여자

- 조명환 (팀원 D, `@ChoMyeongHwan`)

### 상황 및 목적

[PR #20](https://github.com/Codyssey-Nov-2-2-3/B2-2/pull/20)을 원격 저장소에 **push하기 전 로컬 전용 상태**의 `feature/cho-hash-note` 브랜치에서 해시 테이블 개념 초안(`d2b3603`), 그래프 개념 추가(`1cbabf8`), Python 예제 추가(`b1328e8`)로 나뉘어 있던 3개의 커밋 중 개념 문서 커밋 2건을 하나의 의미 있는 `docs:` 커밋으로 합치고 메시지를 다듬기 위해 `git rebase -i`(`reword` + `squash`)를 수행했다.

### 정리 전·후 히스토리 비교

**정리 전 (`git log --oneline -n 4`, 원격 push 전 로컬 전용)**

```text
b1328e8 feat: 해시 테이블과 그래프 파이썬 구현 예제 추가
1cbabf8 docs: 그래프 개념과 인접 리스트·행렬·BFS·DFS 설명 추가
d2b3603 docs: 해시 테이블 개념과 충돌 해결 방식 초안 작성
999b36b Merge pull request #17 from Codyssey-Nov-2-2-3/feature/yang-readme-github-flow
```

**실행한 `git rebase -i main` 계획**

```text
reword d2b3603 docs: 해시 테이블 개념과 충돌 해결 방식 초안 작성
squash 1cbabf8 docs: 그래프 개념과 인접 리스트·행렬·BFS·DFS 설명 추가
pick b1328e8 feat: 해시 테이블과 그래프 파이썬 구현 예제 추가
```

**정리 후 (`git log --oneline -n 3`, 원격 push 및 PR #20 반영본)**

```text
5b5de30 feat: 해시 테이블과 그래프 파이썬 구현 예제 추가
d858c8a docs: 해시 테이블과 그래프 개념 노트 작성
999b36b Merge pull request #17 from Codyssey-Nov-2-2-3/feature/yang-readme-github-flow
```

### 협업 안전 수칙

- `git rebase -i`는 기존 커밋의 해시를 새로 생성(히스토리 재작성)하므로 **`main`이나 이미 원격에 공유되어 팀원이 함께 보는 브랜치에서는 절대 사용하지 않는다.**
- 실제로 PR #20을 원격에 push하고 리뷰를 받은 뒤의 수정 사항은 리베이스나 `--amend` 대신 새 커밋(`92027e4`)으로 추가해 협업 히스토리를 안전하게 유지했다.
