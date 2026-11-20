# Troubleshooting Log

## 시나리오: amend (`git commit --amend`)

### 참여자

- 김동탁 (팀원 A, @Dong-tak): 실습 수행과 기록 작성
- Claude Code (AI 페어 프로그래머): 명령 실행 보조와 결과 정리

### 상황

[PR #9](https://github.com/Codyssey-Nov-2-2-3/B2-2/pull/9)의 `docs/CONTRIBUTING.md`를 수정해 커밋하면서, 커밋 메시지의 타입을 `docs`가 아닌 `dcos`로 잘못 입력했습니다. 실습을 위해 일부러 낸 오타입니다.

- 우리 팀 컨벤션은 `타입: 구체적인 대상과 변경 내용`이고, 허용 타입은 `feat`, `docs`, `fix`, `refactor`뿐이라 `dcos:`는 규칙을 어깁니다.
- 이 커밋은 아직 push 하지 않은 로컬 커밋이었고, 브랜치의 마지막 커밋이었습니다.
- 목표는 **파일 내용은 그대로 두고, 마지막 커밋의 메시지만 올바른 컨벤션으로 고치는 것**입니다.

### 시도한 명령/절차

```bash
git add docs/CONTRIBUTING.md
git commit -m "dcos: CONTRIBUTING 네이밍 규칙 제목을 브랜치 네이밍 규칙으로 변경"   # 오타 커밋
git log -n 1                                                                          # 수정 전 확인
git commit --amend -m "docs: CONTRIBUTING 네이밍 규칙 제목을 브랜치 네이밍 규칙으로 변경"
git log -n 1                                                                          # 수정 후 확인
git push -u origin feature/dongtak-array-note                                           # amend를 끝낸 뒤에 push
```

수정 전 `git log -n 1` 결과입니다. (이메일은 마스킹했습니다.)

```text
commit 213f19c489f546b7e29e8f482e40074a13ffd9a4
Author: Kim dong tak <이메일 마스킹>
Date:   Mon Nov 16 22:30:25 2026 +0900

    dcos: CONTRIBUTING 네이밍 규칙 제목을 브랜치 네이밍 규칙으로 변경
    
    Co-Authored-By: Claude Sonnet 5.5 <이메일 마스킹>
```

수정 후 `git log -n 1` 결과입니다.

```text
commit acdb43629cb3fe40f8da453eb91acff6d18c9e66
Author: Kim dong tak <이메일 마스킹>
Date:   Mon Nov 16 22:30:25 2026 +0900

    docs: CONTRIBUTING 네이밍 규칙 제목을 브랜치 네이밍 규칙으로 변경
    
    Co-Authored-By: Claude Sonnet 5.5 <이메일 마스킹>
```

### 결과

- 마지막 커밋의 메시지가 `dcos:`에서 `docs:`로 바뀌었습니다.
- 커밋 해시가 `213f19c`에서 `acdb436`으로 **바뀌었습니다.** 메시지도 해시 계산에 들어가므로, 메시지만 고쳐도 새 커밋 객체가 됩니다.
- 파일 내용(tree `1496c0d`)과 `Date`는 그대로이고, 브랜치의 커밋 개수도 4개로 그대로입니다. `--amend`는 새 커밋을 더하지 않고 마지막 커밋을 **교체**합니다.
- 오타 커밋 `213f19c`는 push 전에 고쳤기 때문에 **원격에 올라간 적이 없습니다.** GitHub에서 이 해시를 조회하면 존재하지 않고, 원격에는 수정된 [`acdb436`](https://github.com/Codyssey-Nov-2-2-3/B2-2/commit/acdb43629cb3fe40f8da453eb91acff6d18c9e66)만 PR #9에 포함됐습니다.

### 주의할 점 (원격 히스토리와 협업 영향)

- `--amend`는 커밋을 교체해 해시를 바꿉니다. **이미 push 한 커밋에 쓰면** 로컬과 원격 히스토리가 갈라져 `git push --force`가 필요해지는데, 우리 팀 규칙은 공유 브랜치의 강제 push와 히스토리 재작성을 금지합니다. 그래서 **push 전의 마지막 커밋에만** 사용합니다.
- 다른 사람이 이미 내 브랜치를 받아 갔거나 리뷰가 시작된 뒤에도 쓰지 않습니다. 실제로 PR #9에서는 리뷰를 받은 뒤의 수정을 amend가 아니라 **새 커밋**(`537231b`)으로 추가했습니다.
- push 이후에 이미 공유된 커밋을 취소해야 한다면 히스토리를 고쳐 쓰는 대신 `git revert`로 되돌리는 커밋을 추가합니다.
- amend 직후에도 이전 커밋은 로컬 `git reflog`에 한동안 남아 있어서, 잘못 고쳤다면 `git reflog`로 이전 해시를 찾아 되돌릴 수 있습니다. (이 실습에서도 `213f19c`가 로컬에 남아 있는 것을 `git cat-file -t 213f19c`로 확인했습니다.)

### 왜 이 방법을 선택했는가 (Why)

| 대안 | 선택하지 않은 이유 |
|---|---|
| `git reset --soft HEAD~1` 후 다시 커밋 | 결과는 같지만 단계가 둘이고, 메시지만 고치는 데는 과합니다. 변경을 다시 스테이징하다 실수할 수 있습니다. (이 방법은 팀원 B가 별도 시나리오로 다룹니다.) |
| `git rebase -i`의 `reword` | 마지막 커밋이 아니어도 메시지를 고칠 수 있지만, 히스토리를 고쳐 쓰는 범위가 커서 팀 규칙상 부담이 크고 절차도 복잡합니다. |
| `fix:` 새 커밋으로 오타 정정 | 이력에 오타 커밋이 그대로 남습니다. 아직 push 전이라 깔끔하게 교체할 수 있었습니다. |

**push 전의 마지막 커밋 메시지만 고치는 상황**에서는 `git commit --amend`가 단계가 가장 적고, 원격 히스토리에 흔적도 남기지 않는 가장 작은 방법입니다.

### 재현 방법

임시 저장소에서 그대로 따라 할 수 있습니다. (처음이라면 `git config user.name`과 `git config user.email` 설정이 먼저 필요합니다.)

```bash
git init amend-practice && cd amend-practice
echo "# practice" > note.md
git add note.md
git commit -m "dcos: 연습 노트 파일 추가"          # 오타 커밋
git log -n 1                                         # 해시와 메시지 기록
git commit --amend -m "docs: 연습 노트 파일 추가"   # 메시지만 수정
git log -n 1                                         # 해시가 바뀌고 메시지가 고쳐졌는지 확인
git reflog                                           # 이전 커밋이 남아 있는지 확인
```

위 절차를 임시 저장소에서 실행했을 때 해시는 `d90956f`에서 `e704b93`으로 바뀌었고, 파일 내용(tree)과 커밋 개수(1개)는 그대로였으며, `git reflog`에는 `commit (initial)`과 `commit (amend)`가 모두 남았습니다.
