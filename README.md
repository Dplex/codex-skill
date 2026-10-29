# codex-contest-kit

Codex로 과제를 풀 때 쓰는 작업 규칙(`AGENTS.md`)과 스킬 5개다. 결과물 코드는 들어 있지 않다. **인터뷰로 요구사항을 정하고, 결정과 요구사항을 문서로 남기고, 내 말로 설명하는 과정**을 돕는 도구만 담았다.

## 흐름

```
grill-me ──▶ adr ──▶ prd ──▶ 구현 ──▶ handoff ──▶ quiz-me
 인터뷰       결정 기록   요구사항·AC        세션 인계     서술형·인터뷰 대비
```

| 스킬 | 언제 | 남기는 것 |
|---|---|---|
| `grill-me` | 과제를 받은 직후 | `docs/GRILL.md` — 질문·답·종류(결정·가정·열림) |
| `adr` | 길이 둘 이상이었고 하나를 골랐을 때 | `docs/adr/NNNN-제목.md` — 맥락·결정·다른 길·되돌리는 법 |
| `prd` | 인터뷰 뒤, 구현 전 | `docs/prd/F-nn.md` — 요구사항·수용 기준(AC), `questions.md` |
| `handoff` | 세션을 넘기기 전 | `docs/HANDOFF.md` |
| `quiz-me` | 제출 직전 | 막힌 질문 목록(파일은 고치지 않음) |

말투는 대화와 문서 모두 ELI20(대학생이 한 번 읽고 이해하는 수준)이다. 기준은 `AGENTS.md`와 `docs/WRITING.md`에 있다.

## 설치 — 어떤 방법을 고를까

| 방법 | 설치되는 것 | 적용 범위 | 이럴 때 |
|---|---|---|---|
| **A. 프로젝트에 kit 전체 넣기 (추천)** | `AGENTS.md` + 스킬 5개 + `docs/WRITING.md` | 그 프로젝트만 | 과제 하나를 이 흐름대로 끝까지 풀 때 |
| B. `$skill-installer`로 스킬만 | 스킬 5개 | 내 계정의 모든 프로젝트 | Codex 안에서 말로 빠르게 깔고 싶을 때 |
| C. 스킬만 직접 복사 | 스킬 5개 | 내 계정의 모든 프로젝트 | B가 안 될 때(사내망·권한 문제 등) |

B·C는 `AGENTS.md`를 깔지 않는다. 스킬은 동작하지만 ELI20 말투, 일하는 순서, "합의 전에는 `docs/` 밖을 고치지 않는다" 같은 규칙은 적용되지 않는다. 규칙까지 쓰려면 A를 고르거나, B·C 뒤에 `AGENTS.md`만 따로 프로젝트에 복사한다.

### A. 프로젝트에 kit 전체 넣기 (추천)

**과제 작업 폴더 맨 위에서** 실행한다. 작업 폴더에 `AGENTS.md`가 이미 있으면 덮어쓰므로, 먼저 아래 [주의](#주의)를 읽는다.

macOS · Linux · Git Bash:

```bash
KIT=$(mktemp -d)
git clone --depth 1 https://github.com/Dplex/codex-skill.git "$KIT"
cp -r "$KIT"/AGENTS.md "$KIT"/.agents "$KIT"/docs .
rm -rf "${KIT:?}"
```

Windows PowerShell:

```powershell
$KIT = Join-Path $env:TEMP "codex-kit"
git clone --depth 1 https://github.com/Dplex/codex-skill.git $KIT
Copy-Item "$KIT\AGENTS.md", "$KIT\.agents", "$KIT\docs" -Destination . -Recurse -Force
Remove-Item $KIT -Recurse -Force
```

복사가 끝나면 작업 폴더가 이렇게 된다.

```
내-과제/
├─ AGENTS.md
├─ docs/WRITING.md
└─ .agents/skills/  grill-me/  adr/  prd/  handoff/  quiz-me/
```

Codex는 **지금 폴더부터 git 저장소 맨 위까지** 올라가며 `.agents/skills`를 찾는다. 그래서 Codex는 이 작업 폴더(또는 그 아래 폴더)에서 켠다. 작업 폴더가 아직 git 저장소가 아니면 정확히 이 폴더에서 켠다.

### B. `$skill-installer`로 스킬만 설치

Codex 안에서 아래처럼 말한다. 스킬 폴더 경로를 적어 주어야 확실하다. 저장소 이름만 주면 Codex가 경로를 찾다가 헤맬 수 있다.

```
$skill-installer Dplex/codex-skill 저장소에서
.agents/skills/grill-me, .agents/skills/adr, .agents/skills/prd,
.agents/skills/handoff, .agents/skills/quiz-me 를 설치해 줘
```

- 인터넷에서 내려받으므로 중간에 권한 승인 창이 뜬다. 승인한다.
- 스킬은 `~/.codex/skills/<스킬 이름>`에 들어간다(`CODEX_HOME`을 바꿨다면 그 아래 `skills/`).
- 같은 이름의 폴더가 이미 있으면 설치를 멈춘다. 다시 깔려면 아래 [업데이트](#업데이트와-삭제)를 본다.

### C. 스킬만 직접 복사

내 계정 전체에서 쓰는 스킬 폴더(`~/.agents/skills`)에 복사한다.

macOS · Linux · Git Bash:

```bash
KIT=$(mktemp -d)
git clone --depth 1 https://github.com/Dplex/codex-skill.git "$KIT"
mkdir -p ~/.agents/skills
cp -r "$KIT"/.agents/skills/* ~/.agents/skills/
rm -rf "${KIT:?}"
```

Windows PowerShell:

```powershell
$KIT = Join-Path $env:TEMP "codex-kit"
git clone --depth 1 https://github.com/Dplex/codex-skill.git $KIT
New-Item -ItemType Directory -Force "$HOME\.agents\skills" | Out-Null
Copy-Item "$KIT\.agents\skills\*" -Destination "$HOME\.agents\skills" -Recurse -Force
Remove-Item $KIT -Recurse -Force
```

## 설치 확인

1. Codex를 켠다(이미 켜져 있었다면 다시 시작한다).
2. `/skills`에 `grill-me`·`adr`·`prd`·`handoff`·`quiz-me` 5개가 보이는지 본다.
3. A로 설치했다면 `/status`에 `AGENTS.md`가 잡혀 있는지도 본다.

둘 다 보이면 첫 지시를 보낸다.

```
과제를 읽고 grill-me로 인터뷰해 줘.
```

## 스킬 부르는 법

스킬은 상황이 맞으면 Codex가 알아서 고른다. 직접 부르려면 `$스킬이름`을 쓰거나 이름을 말한다.

| 하고 싶은 것 | 이렇게 말한다 |
|---|---|
| 요구사항 인터뷰 | `과제를 읽고 grill-me로 인터뷰해 줘.` |
| 결정 기록 | `방금 정한 저장 방식을 adr로 남겨 줘.` |
| 요구사항·AC 정리 | `인터뷰 결과로 prd 써 줘.` |
| 세션 넘기기 | `handoff 정리해 줘. 새 세션에서 이어갈 거야.` |
| 제출 전 점검 | `$quiz-me 서술형 대비로 질문해 줘.` |

## 주의

- **`AGENTS.md` 덮어쓰기**: A는 작업 폴더의 `AGENTS.md`를 바꾼다. 이미 쓰던 규칙이 있으면 먼저 `AGENTS.md.bak`처럼 이름을 바꿔 두고, 설치 뒤 필요한 줄을 새 `AGENTS.md`에 옮긴다.
- **`docs/WRITING.md`**: A는 같은 이름의 파일이 있으면 덮어쓴다. `docs/` 안의 다른 파일은 건드리지 않는다.
- **같은 이름 스킬**: 다른 곳에 `grill-me` 같은 이름의 스킬이 이미 있으면 Codex는 둘을 합치지 않고 둘 다 보여 준다. 헷갈리면 하나를 지운다.
- **B·C로 깔았을 때 문체 기준**: `adr`·`prd`는 프로젝트의 `docs/WRITING.md`를 먼저 찾고, 없으면 스킬 폴더 안의 `references/WRITING.md`를 읽는다. 따로 복사하지 않아도 된다.

## 업데이트와 삭제

- **A**: 설치 명령을 다시 실행한다. `AGENTS.md`·`docs/WRITING.md`·스킬 5개가 최신으로 바뀐다. 내가 고친 내용이 있으면 먼저 백업한다.
- **B**: 설치된 스킬 폴더를 지운 뒤 같은 요청을 다시 한다.
  - macOS · Linux · Git Bash: `rm -rf ~/.codex/skills/{grill-me,adr,prd,handoff,quiz-me}`
  - PowerShell: `"grill-me","adr","prd","handoff","quiz-me" | % { Remove-Item "$HOME\.codex\skills\$_" -Recurse -Force }`
- **C**: 설치 명령을 다시 실행하면 덮어쓴다. 지우려면 `~/.agents/skills/` 아래 5개 폴더를 지운다.

## 잘 안 될 때

| 증상 | 확인할 것 |
|---|---|
| `/skills`에 스킬이 안 보인다 | Codex를 다시 시작한다. A라면 Codex를 켠 폴더가 `.agents/`가 있는 폴더(또는 그 아래)인지 본다 |
| `/status`에 `AGENTS.md`가 안 보인다 | `AGENTS.md`가 Codex를 켠 폴더나 git 저장소 맨 위에 있는지 본다 |
| PowerShell에서 `cp`가 실패한다 | bash용 명령을 쓴 것이다. 위의 PowerShell 명령을 쓴다 |
| `git clone`이 실패한다 | 사내망이면 프록시 설정을 확인하거나, GitHub에서 ZIP으로 받아 압축을 푼 뒤 복사 단계만 따라 한다 |
| `$skill-installer`가 "already exists"로 멈춘다 | 같은 이름의 폴더가 있다. [업데이트와 삭제](#업데이트와-삭제)의 B를 따른다 |

## 구성

```
AGENTS.md                 ELI20 말투, 일하는 순서, 꼭 지킬 것
docs/WRITING.md           문서 문체와 용어
.agents/skills/
  grill-me/  handoff/  quiz-me/
  adr/  prd/              references/WRITING.md (docs/WRITING.md 사본)
CLAUDE.md                 Claude Code로 리허설할 때만 쓴다. 설치하지 않아도 된다
```

## 참고

- `grill-me`는 Matt Pocock의 공개 스킬 grill-me(질문을 하나씩 던져 계획을 다듬는 방식)에서 아이디어를 얻어 새로 썼다.
- `adr`·`prd`의 틀은 내가 실무에서 쓰는 ADR·PRD 운영 방식(결정 하나에 파일 하나, 기능 하나에 파일 하나, 테스트 태그로 AC 연결)을 일반화했다.
- 스킬 위치와 설치 방식은 [Codex 스킬 문서](https://learn.chatgpt.com/docs/build-skills)와 [skill-installer](https://github.com/openai/skills/tree/main/skills/.system/skill-installer)를 따랐다.
