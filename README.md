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

## 쓰는 법

작업 폴더에서 아래를 실행한다.

```bash
git clone --depth 1 <이 저장소 URL> /tmp/kit
cp -r /tmp/kit/AGENTS.md /tmp/kit/.agents /tmp/kit/docs .
```

Codex를 다시 시작한 뒤 `/status`에 `AGENTS.md`가 보이는지, `/skills`에 스킬 5개가 보이는지 확인한다. 첫 지시는 다음과 같다.

```
과제를 읽고 grill-me로 인터뷰해 줘.
```

## 구성

```
AGENTS.md                 ELI20 말투, 일하는 순서, 꼭 지킬 것
docs/WRITING.md           문서 문체와 용어
.agents/skills/
  grill-me/  adr/  prd/  handoff/  quiz-me/
```

## 참고

- `grill-me`는 Matt Pocock의 공개 스킬 grill-me(질문을 하나씩 던져 계획을 다듬는 방식)에서 아이디어를 얻어 새로 썼다.
- `adr`·`prd`의 틀은 내가 실무에서 쓰는 ADR·PRD 운영 방식(결정 하나에 파일 하나, 기능 하나에 파일 하나, 테스트 태그로 AC 연결)을 일반화했다.
