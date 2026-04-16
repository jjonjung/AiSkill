# Portfolio Writing Agent Guide

이 저장소는 Rust 기반 에이전트 하네스 구조를 가지고 있어서, 코어를 크게 수정하지 않고도 프로젝트 로컬 설정만으로 목적형 에이전트를 구성할 수 있다.

이 문서는 현재 저장소를 기준으로 `포트폴리오 작성 전용 에이전트`를 구축하고 사용하는 방법을 정리한다.

## 1. 이번에 추가한 구성

- [portfolio-writer.toml](/C:/Users/EJ/Desktop/SuperLoop/claw-code-main/claw-code-main/.claw/agents/portfolio-writer.toml)
- [SKILL.md](/C:/Users/EJ/Desktop/SuperLoop/claw-code-main/claw-code-main/.claw/skills/portfolio-writer/SKILL.md)
- [settings.json](/C:/Users/EJ/Desktop/SuperLoop/claw-code-main/claw-code-main/.claw/settings.json)

이 세 가지가 각각 하는 일은 다음과 같다.

- `agents/portfolio-writer.toml`
  포트폴리오 작성 전용 에이전트를 목록에 노출한다.
- `skills/portfolio-writer/SKILL.md`
  실제 포트폴리오 문서 작성 절차와 문서 형식을 정의한다.
- `.claw/settings.json`
  기본 모델과 권한 정책을 프로젝트 로컬 기준으로 고정한다.

## 2. 왜 이 방식이 맞는가

이 저장소는 이미 아래 계층으로 분리되어 있다.

- CLI: `rust/crates/claw-cli`
- Runtime loop: `rust/crates/runtime`
- Tools: `rust/crates/tools`
- Commands: `rust/crates/commands`
- Plugins: `rust/crates/plugins`

따라서 포트폴리오 작성 에이전트를 만들 때 코어 런타임을 뜯기보다, `지침`, `허용 권한`, `에이전트 정의`, `스킬 정의`를 프로젝트 단에서 주입하는 것이 더 안전하고 유지보수성이 좋다.

## 3. 사용 방법

Rust 빌드:

```powershell
cd C:\Users\EJ\Desktop\SuperLoop\claw-code-main\claw-code-main\rust
cargo build --release
```

프로젝트 루트에서 에이전트 목록 확인:

```powershell
cd C:\Users\EJ\Desktop\SuperLoop\claw-code-main\claw-code-main
rust\target\release\claw agents
```

스킬 목록 확인:

```powershell
rust\target\release\claw skills
```

직접 프롬프트 실행 예시:

```powershell
rust\target\release\claw prompt "이 저장소를 분석해서 포트폴리오용 프로젝트 소개 문서를 작성해줘"
```

추천 프롬프트 예시:

```text
이 저장소를 분석해서 다음 형식으로 포트폴리오 문서를 작성해줘.
1. 문제의 본질 규명 또는 구현 목적
2. 전략 고도화
3. 실행 및 성과 증명
4. 도식화 및 시각 자료
5. 핵심 구조
6. 사용한 자료구조와 이유
7. 검증 방법과 결과
8. 이력서용 4줄 요약
```

## 4. 이 에이전트가 잘 맞는 작업

- 프로젝트 기술 설명서 작성
- 경력기술서 문장 압축
- 면접 답변용 서술 정리
- 구현 근거 기반 포트폴리오 정리
- "현재 구현"과 "확장 가능 설계" 구분 정리

## 5. 더 강화하려면

현재 구성은 `설정 + agent + skill` 기반의 가벼운 구축이다.
더 강화하려면 다음 단계로 확장하면 된다.

1. `commands` crate 에 `/portfolio` 슬래시 명령 추가
2. `plugins` crate 로 문서 템플릿 생성기 추가
3. 특정 출력 경로에 자동 저장하는 도구 추가
4. 프로젝트 유형별 템플릿(게임, 네트워크, 툴, 그래픽) 분리

## 6. 포트폴리오 관점에서 설명할 수 있는 점

- 범용 에이전트 하네스 위에 목적형 에이전트를 프로젝트 단위로 구축했다.
- 코어 런타임 수정 없이 지침, 권한, 스킬, 에이전트 정의를 분리해 확장 가능하게 구성했다.
- 동일한 런타임 위에서 분석형/문서화형 워크플로우를 재사용할 수 있게 만들었다.

## 7. 다음 단계 추천

실제로 더 완성도 있게 쓰려면 다음 중 하나로 이어가는 것이 좋다.

1. `/portfolio` 명령을 코드로 추가
2. 저장소 분석 결과를 지정 경로에 자동 저장하는 플러그인 추가
3. `portfolio-writer`, `resume-writer`, `interview-writer` 로 에이전트를 분리

## 8. 포트폴리오 문장 작성 기준

이 에이전트는 구현 내용을 단순 나열하지 않고, 사고 과정이 드러나게 재구성하는 것을 목표로 한다.

### 1. 문제의 본질 규명 또는 구현 목적
- 왜 이 기능이 필요했는지 먼저 설명한다.
- 표면적 작업보다 해결 대상이 무엇이었는지 적는다.
- 성능, 유지보수성, 확장성, 검증 가능성 중 어떤 관점이 핵심인지 드러낸다.

### 2. 전략 고도화
- 문제를 어떤 구조로 나눠 풀었는지 설명한다.
- 자료구조 선택, 계층 분리, 소유권 설계, 테스트 전략을 함께 적는다.
- 다른 방식보다 현재 설계가 왜 적합한지 짧게 비교한다.

### 3. 실행 및 성과 증명
- 실제 구현한 파일, 기능, 테스트, 빌드, 실행 결과를 근거로 쓴다.
- 가능하면 수치나 산출물을 붙여서 검증 완료 사실을 보여준다.
- 현재 구현, 검증 완료, 향후 확장을 구분해서 정리한다.

## 9. 시각 자료 구성 기준

포트폴리오 문서는 텍스트 설명만으로 끝내지 않고, 한눈에 구조가 들어오도록 시각 자료를 함께 배치하는 것이 좋다.

우선 넣어야 하는 자료:
- 전체 구조 다이어그램
- 데이터 흐름도
- 계층 책임 표
- 개선 전 / 개선 후 비교표
- 테스트 및 검증 결과 표
- 수식과 방향성을 포함한 개념 도식
- 핵심 코드 하이라이트

추천 표현 방식:
- Markdown 표
- ASCII 구조도
- Before / After 비교
- 입력 -> 처리 -> 출력 흐름도
- 수식 + 도식 결합
- 문제 -> 전략 -> 실행 흐름도

추천 프롬프트 예시:

```text
이 저장소를 분석해서 포트폴리오 문서를 작성해줘.
단, 긴 문단 위주가 아니라 아래를 반드시 포함해줘.
1. 문제의 본질 규명 또는 구현 목적
2. 전략 고도화
3. 실행 및 성과 증명
4. 전체 구조 ASCII 다이어그램
5. 계층 책임 표
6. 전후 비교 표 또는 검증 결과 표
7. 수식 또는 개념 도식
8. 핵심 코드 하이라이트
9. 이력서용 4줄 요약
```
