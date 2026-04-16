---
name: push
allowed-tools: Bash(git status *), Bash(git log *), Bash(git push *), Bash(git branch *)
description: Git 푸시 도우미 스킬. 사용자가 "푸시", "푸쉬", "push", "푸시해줘", "푸쉬해줘", "올려줘", "원격에 올려줘", "반영해줘" 등을 요청할 때 사용한다. 현재 브랜치 상태를 확인하고 안전하게 원격 저장소에 푸시한다. 커밋하고 푸시를 동시에 요청할 때도 사용한다. 푸시 없이 커밋만 요청할 때는 사용하지 않는다.
---

# Git 푸시 도우미

항상 한국어로 응답한다.

## 실행 프로세스

### 1. 현재 상태 확인

```bash
git status
git log origin/{현재 브랜치}..HEAD --oneline
```

- 커밋되지 않은 변경사항이 있으면 먼저 `/commit` 스킬 실행을 안내한다
- 푸시할 커밋이 없으면 안내 후 종료한다

### 2. 푸시 실행

확인 없이 바로 실행한다:

```bash
git push origin {현재 브랜치}
```

## 규칙

- `--force` / `--force-with-lease` 는 절대 사용하지 않는다
- `master` 또는 `main` 브랜치 직접 푸시 시 경고를 표시한 뒤 푸시한다
- 커밋이 없으면 푸시하지 않고 안내한다
- 커밋하고 푸시를 동시에 요청한 경우, `/commit` 스킬로 커밋 먼저 완료 후 푸시한다
