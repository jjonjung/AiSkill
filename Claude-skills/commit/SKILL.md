---
name: commit
allowed-tools: Bash(git status *), Bash(git diff *), Bash(git add *), Bash(git commit *)
description: Git 커밋 도우미 스킬. 사용자가 "커밋", "커밋해줘", "commit", "저장해줘", "변경사항 저장" 등을 요청할 때 사용한다. 변경사항을 분석해 [FEAT]/[WIP]/[DOCS]/[FIX] prefix와 한국어 커밋 메시지를 자동 생성하고 사용자 확인 후 커밋을 실행한다. 푸시 없이 커밋만 요청할 때 사용하며, 커밋 메시지 형식 질문이나 git 개념 설명 요청에는 사용하지 않는다.
---

# Git 커밋 도우미

항상 한국어로 응답한다.

## PREFIX 규칙

| PREFIX | 사용 조건 |
|--------|-----------|
| `[FEAT]` | 완성된 새 기능 (게임 플레이 흐름 통합) |
| `[WIP]` | 구현 중인 기능 (빌드 가능 여부 무관) |
| `[DOCS]` | 문서만 수정 (코드 변경 없음) |
| `[FIX]` | 기존 `[FEAT]` 기능 수정/보완 |

## 실행 프로세스

### 1. 변경사항 파악

다음 명령으로 현재 상태를 확인한다:

```bash
git status
git diff
git diff --staged
```

### 2. 커밋 메시지 생성 및 확인

변경 내용을 분석해 PREFIX를 자동 추천하고, 커밋 전에 반드시 사용자에게 확인을 받는다:

```
Summary    : [PREFIX] {설명}
Description:
- {변경 항목 1}
- {변경 항목 2}
- ...

진행할까요? (수정이 필요하면 말씀해주세요)
```

스테이징되지 않은 파일이 있으면 포함 여부도 함께 확인한다.

### 3. 스테이징 & 커밋

사용자가 확인하면 실행한다:

```bash
git add {파일 또는 -A}
git commit -m "[PREFIX] 설명" -m "- 변경 항목 1\n- 변경 항목 2"
```

## 규칙

- 커밋 메시지는 반드시 한국어로 작성
- Summary와 Description 모두 매 커밋마다 작성 (Description 생략 불가)
- 커밋 후 푸시가 필요하면 `/push` 스킬을 사용하도록 안내
