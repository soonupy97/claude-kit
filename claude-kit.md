# Claude Code, 업무별로 쓸 만한 스킬·플러그인

기획·디자인·퍼블리싱·프론트엔드·문서 업무 기준. 여러 곳에서 반복 추천되고 평가가 좋은 것 위주. (2026-09 기준 · 공식 문서: https://code.claude.com/docs)

---

## 0. 용어 5개

스킬·플러그인·MCP·훅·컨텍스트. 이 다섯만 알면 문서가 읽힙니다.

| 용어 | 뜻 | 팁 |
|---|---|---|
| **스킬** | Claude에게 일하는 방법을 적어 둔 지침. 필요할 때 자동으로 읽힘 | 가볍고 토큰을 거의 안 씀 |
| **플러그인** | 스킬·명령어·훅·MCP를 한 번에 설치하는 패키지 | `/plugin install 이름@마켓` |
| **MCP** | Claude를 외부 서비스(Figma·Notion·브라우저 등)와 연결하는 서버 | 켜 두는 것만으로 토큰을 차지. 꼭 필요한 것만 |
| **Hooks** | 특정 시점에 자동 실행되는 스크립트 | 규칙을 강제하는 유일한 방법 |
| **컨텍스트·토큰** | Claude가 한 번에 기억하는 양 | 도구가 많을수록 줄어들고 비용이 늘어남 |

---

## 1. 시작하기 A–Z

유료 플랜 필요. Node.js는 Claude 자체엔 불필요하나 스킬(`npx`)·MCP 설치에 쓰이므로 함께 설치.  
`PS C:\…>`는 PowerShell 창, `>`는 내가 치는 말, `⏺`는 Claude의 답.

### A. 설치와 확인

설치는 명령 한 줄이면 끝납니다. 순서대로 따라 하세요.

1. **PowerShell 열기** — 시작 메뉴에서 `PowerShell`을 검색해 실행. 검은(파란) 글자 창이 뜨면 됩니다.
2. **설치 명령 붙여넣기** — 아래 첫 줄(`irm … | iex`)을 복사해 창에 붙여넣고 Enter. 인터넷에서 Claude를 받아 자동으로 설치합니다.
3. **설치 확인** — `claude --version`을 치고 Enter. 숫자 버전이 찍히면 성공.
4. **Node.js 설치** — [nodejs.org 다운로드](https://nodejs.org/ko/download)에서 LTS 버전을 받아 "다음"만 눌러 설치. Claude 자체에는 필요 없지만 이 문서의 스킬·MCP 설치 명령(`npx`)이 사용합니다.

이 방식은 이후 업데이트도 자동입니다. 뭔가 이상하면 `claude doctor`가 원인을 알려 줍니다.

**설치 링크 모음**

- [Claude Code 공식 설치 안내](https://code.claude.com/docs/en/setup) — 설치·문제 해결
- [데스크톱 앱](https://claude.com/download) — 터미널 없이 사용
- [Node.js LTS](https://nodejs.org/ko/download) — 스킬·MCP 설치용
- [claude.ai](https://claude.ai) — 로그인·플랜

**가장 쉬운 길 (터미널이 낯설 때)** — 데스크톱 앱 설치 → 로그인 → 폴더 열기. 이것만으로 대화·코드 수정이 됩니다.  
PowerShell 설치와 스킬·MCP는 익숙해진 뒤에.

```text
# Windows PowerShell — 설치와 확인
PS C:\Users\<이름>> irm https://claude.ai/install.ps1 | iex
Installing Claude Code...
✔ Installed to C:\Users\<이름>\.local\bin\claude.exe
PS C:\Users\<이름>> claude --version
2.1.263 (Claude Code)
```

### B. 로그인 · 첫 실행 · 프로젝트 설정

작업 폴더에서 `claude`. 첫 실행은 브라우저 로그인, `/init`은 Claude가 매번 읽는 메모(CLAUDE.md) 생성.

```text
# Windows PowerShell — 로그인 · 첫 실행 · /init
PS C:\Users\<이름>> cd C:\work\my-project
PS C:\work\my-project> claude
Opening browser to log in...   (처음 한 번만)
> 이 프로젝트가 뭐 하는 건지 설명해줘
⏺ React로 만든 관리자 화면입니다. 주요 폴더는 src/pages, src/components ...
> /init
⏺ Write(CLAUDE.md)
⏺ CLAUDE.md를 만들었어요. 빌드 명령 · 폴더 구조 · 코딩 규칙을 적어 두었습니다.
```

### C. 플러그인 · MCP · 스킬 붙이기

플러그인은 Claude 안에서, MCP·스킬은 PowerShell 창에서.

```text
# Windows PowerShell — 플러그인 · MCP · 스킬
> /plugin install skill-creator@claude-plugins-official
✔ Installed skill-creator
> /plugin install mattpocock-skills@claude-plugins-official
✔ Installed mattpocock-skills   (/grill-me, /to-spec, /to-tickets)
# MCP와 스킬은 Claude를 나와서(Ctrl+D) PowerShell 창에서
PS C:\work\my-project> npx ctx7 setup
✔ Context7 connected   (브라우저에서 API 키 발급)
PS C:\work\my-project> npm i -g agent-browser && agent-browser install
PS C:\work\my-project> npx skills add vercel-labs/agent-browser
✔ Installed skill agent-browser
```


### 자주 쓰는 명령 한눈에

| 명령 | 무엇 |
|---|---|
| `claude "작업"` · `claude -p "질문"` | 첫 지시와 함께 시작 · 한 번 답하고 종료 |
| `claude -c` · `claude -r` | 직전 대화 이어가기 · 이전 대화 고르기 |
| `/help` · `/model` · `/exit` | 명령 목록 · 모델 변경 · 종료 |
| `/plugin` · `/mcp` · `/permissions` | 플러그인 탐색 · MCP 상태 · 권한 설정 |

---

## 2. 터미널 쉽게 쓰기

제목을 누르면 펼쳐집니다. Windows 기준(맥은 마지막). `필수`만 익혀도 충분.

<details>
<summary><b>A. Windows Terminal</b> — 새 탭 `Ctrl+Shift+T` · 화면 나누기 `Alt+Shift+D` · 복사 · 붙여넣기</summary>

| 하는 일 | 키 | 메모 |
|---|---|---|
| 새 탭 | `Ctrl+Shift+T` | 프로젝트마다 탭 하나 |
| 탭 전환 | `Ctrl+Tab` |  |
| 탭 닫기 | `Ctrl+Shift+W` | 나눈 창에서는 포커스된 창만 닫힘 |
| 화면 나누기 | `Alt+Shift+D` | 지금 창을 복제해 넓은 쪽으로 나눔 |
| 나눈 창 이동 | `Alt+←→↑↓` |  |
| 나눈 창 크기 조절 | `Alt+Shift+←→↑↓` |  |
| 검색 | `Ctrl+Shift+F` |  |
| 화면 맨 위로 | `Ctrl+Shift+Home` | 지난 출력 처음까지 |
| 화면 맨 아래로 | `Ctrl+Shift+End` | 입력줄로 복귀 |
| 한 화면 위로 | `Ctrl+Shift+PgUp` |  |
| 한 화면 아래로 | `Ctrl+Shift+PgDn` |  |
| 글자 키우기 | `Ctrl+=` |  |
| 글자 줄이기 | `Ctrl+-` |  |
| 복사 | 드래그 후 `Ctrl+C` |  |
| 붙여넣기 | `Ctrl+V` |  |
| 명령 팔레트 | `Ctrl+Shift+P` | 모든 동작 검색 |
| 퀵 창 | `Win+`` | 화면 위에서 내려오는 터미널 |
| 키 바꾸기 | `Ctrl+,` | 설정 → 동작에서 변경 |

> 추천 배치: 화면을 나눠(`Alt+Shift+D`) 왼쪽은 작업, 오른쪽은 질문용 `claude`. 프로젝트가 여럿이면 탭.

</details>

```text
# Windows PowerShell — 왼쪽 창: 작업하는 세션
PS C:\work\my-project> claude
> 가입 화면에 카카오 로그인 추가해줘
⏺ Update(src/LoginForm.tsx)
⏺ Update(src/auth.ts)
⏺ Write(src/kakao.ts)
```

```text
# Windows PowerShell — 오른쪽 창: 물어보는 세션 (Alt+Shift+D 로 열기)
PS C:\work\design-system> claude
> Figma MCP는 어떻게 연결해?
⏺ /plugin install figma@claude-plugins-official 한 줄이면 됩니다.
```

<details>
<summary><b>B. PowerShell 창에서</b> — `Tab` 자동 완성 · `Esc` 지우기 · `Ctrl+R` 검색 · 자주 쓰는 명령 5</summary>

| 하는 일 | 키 | 메모 |
|---|---|---|
| 자동 완성 | `Tab` | 여러 번 누르면 후보 순환 |
| 완성 목록 보기 | `Ctrl+Space` | 후보를 메뉴로 보고 고르기 |
| 이전 명령 검색 | `Ctrl+R` | 일부만 쳐도 찾아 줌 |
| 앞글자 맞는 이전 명령 | `F8` | `cd`까지 치고 누르면 cd로 시작한 명령만 |
| 입력 줄 지우기 | `Esc` |  |
| 줄 처음으로 | `Home` |  |
| 줄 끝으로 | `End` |  |
| 단어 왼쪽으로 | `Ctrl+←` |  |
| 단어 오른쪽으로 | `Ctrl+→` |  |
| 단어 하나 지우기 | `Ctrl+Backspace` |  |
| 화면 지우기 | `Ctrl+L` | `cls`와 같음 |
| 되돌리기 | `Ctrl+Z` | 입력 편집 취소 |
| 여러 줄 명령 | `Shift+Enter` | PowerShell 창에서는 됨 (Claude 안에서는 Ctrl+J) |
| 경로 넣기 | 탐색기에서 폴더를 창으로 드래그 | 경로가 그대로 붙음 |

**자주 쓰는 명령 다섯 개**

| 명령 | 무엇 |
|---|---|
| `cd 폴더` · `cd ..` | 폴더로 이동 · 상위로 |
| `ls` · `pwd` | 목록 보기 · 현재 위치 |
| `code .` · `explorer .` | 현재 폴더를 VS Code · 탐색기로 열기 |
| `cls` | 화면 지우기 |
| `claude` | 여기서 Claude Code 시작 |

</details>

<details>
<summary><b>C. VS Code 안에서</b> — `Ctrl+`` 터미널 열기 · `Ctrl+Shift+5` 나누기</summary>

| 하는 일 | 키 | 메모 |
|---|---|---|
| 터미널 열기 · 닫기 | `Ctrl+`` | 같은 키로 토글 |
| 새 터미널 | `Ctrl+Shift+`` |  |
| 터미널 나누기 | `Ctrl+Shift+5` | 나눈 창 이동은 `Alt+←` `Alt+→` |
| Claude Code 패널 | Spark 아이콘 클릭 | 확장 설치 후 오른쪽 패널 |

</details>

<details open>
<summary><b>D. Claude Code 세션 안에서</b> — `필수` 6개 포함 · `Esc` · `Shift+Tab` · `Ctrl+J` · `Alt+V`</summary>

| 하는 일 | 키 | 메모 |
|---|---|---|
| 단축키 도움말 | `?` | 빈 입력창에서 누르면 모든 단축키가 뜸 |
| 답변 멈추기 `필수` | `Esc` |  |
| 되감기 `필수` | `Esc` 두 번 | 이전 지점으로 돌아가 다시 시도 |
| 입력 지우기 · 중단 | `Ctrl+C` | 입력 중이면 지우기, 실행 중이면 중단. PowerShell 창에서도 같음 |
| 종료 | `Ctrl+D` |  |
| 여러 줄 입력 `필수` | `Ctrl+J` | 안 되면 줄 끝에 `\` 치고 Enter |
| 이전 입력 | `↑` | PowerShell 창에서도 같음 |
| 권한 모드 전환 `필수` | `Shift+Tab` | Manual → acceptEdits → Plan → auto 순환. [모드 설명은 05장](#5-누구나--권한-모드와-습관) |
| 이미지 붙이기 `필수` | `Alt+V` | 스크린샷 복사 후 바로. 파일 드래그도 됨 |
| 대화 맨 위로 | `Ctrl+Home` | 전체화면 모드에서. 마우스 휠도 됨 |
| 대화 맨 아래로 | `Ctrl+End` | 최신 답변으로 복귀 |
| 반 화면 위로 | `PgUp` |  |
| 반 화면 아래로 | `PgDn` |  |
| 모델 전환 | `Alt+P` |  |
| 명령 실행 `필수` | `/` | 명령 · 스킬 목록 |
| 셸 명령 그대로 실행 | `!` | 예: `!git status` |
| 파일 언급 | `@` | 예: `@src/App.tsx` |

</details>

```text
# 세션 안에서 자주 쓰는 동작
# Shift+Tab → 아래 안내줄이 바뀜
▸▸ plan mode on (shift+tab to cycle) · ? for shortcuts
# Ctrl+J → 여러 줄 입력
> 결제 화면 개편 계획 세워줘
  - 카드 · 카카오페이 지원
  - 기존 주문 데이터 유지
⏺ 계획만 정리했습니다. 파일은 수정하지 않았어요. 진행할까요?
# Alt+V → 스크린샷 붙이기
> [Image #1] 이 화면 왜 깨졌어?
⏺ flex-wrap 이 없어 좁은 폭에서 넘칩니다. 고칠까요?
```

<details>
<summary><b>E. 맥이라면</b> — iTerm2 나누기 `Cmd+D` · Option을 Meta로</summary>

| 하는 일 | 키 | 메모 |
|---|---|---|
| iTerm2 화면 나누기 | `Cmd+D` | 가로는 `Cmd+Shift+D`. 새 탭은 `Cmd+T` |
| Alt 단축키 쓰기 | 터미널 설정에서 Option → Meta | `Alt+P` 같은 키를 쓰려면 필요 |
| 이미지 붙이기 | `Ctrl+V` | iTerm2는 `Cmd+V` |


</details>

---

## 3. 활용 예시

하루 업무를 네 장면으로. `>`가 내가 치는 말, `⏺`가 Claude의 답입니다.

설치한 플러그인·스킬은 `/이름`으로 직접 부르거나(장면 1), 말만 하면 알아서 쓰입니다(장면 2~4).

### 1. 요구사항 확정 → 스펙 → 티켓

만들 것을 한 줄로 말하면 Claude가 질문으로 빈 곳을 채운 뒤 스펙 문서와 작업 티켓을 만듭니다. `/grill-me`로 시작해 답만 하면 됩니다(플러그인 [mattpocock-skills](#4-핵심-요약--이것만-알아도-됨)).

```text
# Windows PowerShell — 요구사항 → 스펙 → 티켓
> /grill-me 회원가입 화면을 개편하려고 해
⏺ 확인할게요. 1) 소셜 로그인 포함?  2) 기존 데이터 유지?  3) 완료 기준은?
> 1) 카카오만  2) 유지  3) 가입 완료율 측정
⏺ 범위 정리: 카카오 로그인 추가 · 데이터 유지 · 완료율 이벤트 로깅
> /to-spec
⏺ Write(docs/spec.md)
> /to-tickets
⏺ 티켓 9개로 나눴습니다. 각 티켓에 수용 기준 포함.
```

### 2. 디자인 → 코드

Figma에서 컴포넌트 링크를 복사(우클릭 → 링크 복사)해 붙여넣으면 코드로 만듭니다(플러그인 [figma](#디자인)). 지킬 규칙은 "우리 변수로"처럼 한 문장만 덧붙입니다.

```text
# Windows PowerShell — 디자인 → 코드
> https://www.figma.com/design/xxxx?node-id=12-345 이 컴포넌트를 우리 색·간격 변수로 코드화해줘
⏺ figma - get_design_context(node 12-345)
⏺ Write(src/components/Button.tsx)
⏺ Write(src/components/button.scss)
⏺ 색·간격을 변수(--color-primary, --space-3)에 맞췄습니다.
```

### 3. 화면 검증

Claude가 브라우저를 열어 화면을 눌러 보고 스크린샷으로 보여 줍니다(스킬 [agent-browser](#퍼블리싱--프론트엔드)). "접근성 기준으로"처럼 관점을 말하면 그 기준으로 검토합니다(스킬 [web-design-guidelines](#퍼블리싱--프론트엔드)).

```text
# Windows PowerShell — 화면 검증
> localhost:3000 열어서 가입 버튼 눌러보고 스크린샷 찍어줘
⏺ Bash(agent-browser open http://localhost:3000 && click "가입")
⏺ 클릭 후 완료 화면이 뜹니다. [Image] signup-done.png
> 접근성 기준으로 검토해줘
⏺ 3가지: 버튼 대비 부족(4.2:1) · 입력창 라벨 누락 · 포커스 표시 없음. 고칠까요?
```

### 4. 문서 업무와 마무리

PDF·엑셀·PPT는 파일 이름과 원하는 결과만 말하면 됩니다(플러그인 [document-skills](#문서--업무), 준비물은 [06장 문서 탭](#문서--업무)). 끝나면 "커밋해줘"로 변경을 기록합니다(git 프로젝트일 때).

```text
# Windows PowerShell — 문서 업무와 마무리
> report.pdf 요약해서 엑셀 표로 만들어줘
⏺ Read(report.pdf)
⏺ Write(summary.xlsx)
⏺ 핵심 항목 24개를 summary.xlsx 로 저장했습니다.
> 변경한 파일 커밋해줘
⏺ Bash(git add -A && git commit -m "feat: 카카오 로그인 추가")
⏺ 커밋했습니다.
```

> 팁: "로그인 버그 고쳐줘"보다 "비밀번호 틀리면 빈 화면 뜨는 버그 고쳐줘"처럼 구체적으로. 마음에 안 들면 "왜 그렇게 했어?" 또는 `Esc` 두 번으로 되감기.

---

## 4. 핵심 요약 — 이것만 알아도 됨

업무와 상관없이 꼭 설치할 7개. 업무별 도구는 [06장](#6-업무별-도구).

`필수` = 누구나 · `핵심` = 업무별 기본 · `/이름` = 직접 호출 · 자동 = 설치만.

| 항목 | 한 줄 | 호출 · 사용법 | 설치 |
|---|---|---|---|
| **[skill-creator](https://claude.com/plugins/skill-creator)** `필수` · 플러그인 | 반복 지시를 스킬로 | **`/skill-creator`** 지시를 설명하면 파일 생성 | `/plugin install skill-creator@claude-plugins-official` |
| **[grill-me](https://github.com/mattpocock/skills/blob/main/skills/productivity/grill-me/SKILL.md)** `필수` · 스킬 | 시작 전 요구사항 검증 | **`/grill-me`** 하고 싶은 일을 말하면 질문으로 검증 | `/plugin install mattpocock-skills@claude-plugins-official` |
| **[find-skills](https://github.com/vercel-labs/skills/blob/main/skills/find-skills/SKILL.md)** `필수` · 스킬 | 필요한 스킬 검색·설치 | **자동** "PDF 다루는 스킬 있어?" | `npx skills add vercel-labs/skills --skill find-skills` |
| **[security-guidance](https://claude.com/plugins/security-guidance)** `필수` · 플러그인 | 비밀키·취약 코드 경고 | **자동** Python 설치 후. 편집 시 경고 | `/plugin install security-guidance@claude-plugins-official` |
| **[Hooks](https://claude.com/plugins/hookify)** `필수` · 설정 | 위험 명령 차단·종료 전 검사 | **설정** `/hookify`에 "삭제 명령 막아줘"처럼 말하면 훅 생성 | `/plugin install hookify@claude-plugins-official` |
| **[humanizer](https://github.com/blader/humanizer)** `필수` · 스킬 | AI 문체 제거 | **`/humanizer`** 글을 붙여넣고 실행 | `npx skills add blader/humanizer` |
| **[claude-md-management](https://claude.com/plugins/claude-md-management)** `필수` (공식) · 플러그인 | 세션 학습을 CLAUDE.md에 반영 | **`/revise-claude-md`** 세션 끝에 실행. 점검은 "CLAUDE.md 감사해줘" | `/plugin install claude-md-management@claude-plugins-official` |

> 원칙: 적게 설치(MCP 3~6, 스킬 8~12). 명령어로 되면 MCP 대신 스킬·CLI.

---

## 5. 누구나 — 권한 모드와 습관

매일 쓰는 권한 모드 6가지와 습관 4개.

### 권한 모드 6가지 (주로 `Shift+Tab`으로 전환)

| 모드 | 동작 | 언제 |
|---|---|---|
| **Manual** (default) | 도구별 첫 사용 시 확인 | 하나씩 확인받고 싶을 때 |
| **acceptEdits** | 파일 편집 자동 승인 | 평소 작업. 확인 창 줄이기 |
| **Plan** | 파일 수정 없이 읽기·계획 | 시작 전 계획 세울 때 |
| **auto** | 안전 검사 후 자동 승인 | Pro·Max·Team 기본. 긴 작업 맡길 때 |
| **dontAsk** | 허용 목록 외 자동 거부 | 명령줄 옵션으로만 켬. Shift+Tab엔 없음 |
| **bypassPermissions** (`claude --dangerously-skip-permissions`) | 확인 없이 전부 실행 | 격리 환경(컨테이너·VM)에서만. 차단 훅 먼저 |

### 습관 4개

| 항목 | 무엇 |
|---|---|
| `/context` | 도구가 쓰는 토큰 확인. MCP가 많이 차지하면 끄기 |
| `/statusline` | 아래에 모델·컨텍스트·폴더 표시. 입력하면 자동 설정. 더 풍부하게는 [claude-hud](https://github.com/jarrodwatts/claude-hud) |
| `/clear` · `/compact` | 작업 바뀌면 clear, 길어지면 compact |
| CLAUDE.md 짧게 | `/init`으로 만들고 200줄 미만 |

---

## 6. 업무별 도구

내 업무 탭만 보면 됩니다. 공통 도구는 [04장](#4-핵심-요약--이것만-알아도-됨).

### 기획

요구사항을 질문으로 확정하고 스펙·티켓·기획서로 만드는 도구.

| 항목 | 용도 | 호출 · 사용법 | 설치 |
|---|---|---|---|
| **[grill-me](https://github.com/mattpocock/skills/blob/main/skills/productivity/grill-me/SKILL.md)** `필수` · 스킬 | 질문으로 요구사항 확정 | **`/grill-me`** 하고 싶은 일을 말하면 질문으로 검증 | [핵심 요약 참고](#4-핵심-요약--이것만-알아도-됨) |
| **[to-spec · to-tickets](https://github.com/mattpocock/skills/blob/main/skills/engineering/to-spec/SKILL.md)** · 스킬 | 요구사항 → 스펙·티켓 | **`/to-spec` · `/to-tickets`** grill 뒤에 실행. 티켓 게시는 `/setup-matt-pocock-skills` 먼저 | [grill-me와 같은 플러그인](#4-핵심-요약--이것만-알아도-됨) |
| **[문서 스킬](https://github.com/anthropics/skills#readme)** `핵심` (pptx·docx) · 스킬 | 기획서·제안서 생성 | **자동** "이 내용으로 PPT 만들어줘" | `/plugin marketplace add anthropics/skills` 후 `/plugin install document-skills@anthropic-agent-skills` |
| **[Notion MCP](https://claude.com/plugins/notion)** · MCP | Notion 읽기·쓰기 | **자동** 첫 사용 전 `/mcp`에서 로그인. "Notion 기획 페이지 요약해줘" | `/plugin install notion@claude-plugins-official` |

### 디자인

Figma에서 코드까지. 결과물이 평범해지지 않게 잡아 주는 도구.

| 항목 | 용도 | 호출 · 사용법 | 설치 |
|---|---|---|---|
| **[frontend-design](https://claude.com/plugins/frontend-design)** `핵심` (공식) · 플러그인 | "AI 티" 나는 화면 방지 | **자동** UI 작업이면 자동 적용 | `/plugin install frontend-design@claude-plugins-official` |
| **[Figma MCP](https://claude.com/plugins/figma)** `핵심` (공식) · MCP | Figma → 코드·스타일 변수 | **자동** 첫 사용 전 `/mcp`에서 로그인. 링크 붙여넣고 "코드로 만들어줘" | `/plugin install figma@claude-plugins-official` |
| **[design-taste-frontend](https://github.com/Leonxlnx/taste-skill/blob/main/skills/taste-skill/SKILL.md)** · 스킬 | 디자인 취향 보정 | **자동** 디자인 작업 시 자동 | `npx skills add Leonxlnx/taste-skill --skill design-taste-frontend` |
| **[animate](https://github.com/emilkowalski/skills/blob/main/skills/animate/SKILL.md)** · 스킬 | 애니메이션 설계 | **자동** "호버 애니메이션 넣어줘" | `npx skills add emilkowalski/skills` |
| **[apple-design](https://github.com/emilkowalski/skills/blob/main/skills/apple-design/SKILL.md)** · 스킬 | Apple식 모션·타이포 | **자동** "Apple 느낌 모션으로" | 위와 같은 저장소 |
| **[ui-ux-pro-max](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill)** · 플러그인 | 팔레트·서체·UX 규칙 검색 | **자동** 디자인 작업 시 자동. frontend-design과 둘 중 하나만 | `/plugin marketplace add nextlevelbuilder/ui-ux-pro-max-skill` 후 `/plugin install ui-ux-pro-max@ui-ux-pro-max-skill` |

> 회사 디자인 시스템이 있으면 그쪽이 우선.

### 퍼블리싱 · 프론트엔드

최신 문서 참조·화면 검증·타입과 성능 규칙.

| 항목 | 용도 | 호출 · 사용법 | 설치 |
|---|---|---|---|
| **[Context7](https://github.com/upstash/context7)** `핵심` · MCP | 최신 문서 참조 | **자동** 라이브러리 질문 시 자동. "use context7"로 강제 | `npx ctx7 setup` (API 키 발급 · MCP 등록 자동) |
| **[agent-browser](https://github.com/vercel-labs/agent-browser)** `핵심` · 스킬 · CLI | 브라우저로 화면 직접 확인 | **자동** "localhost:3000 열어서 확인해줘" | `npm i -g agent-browser && agent-browser install` 후 `npx skills add vercel-labs/agent-browser` |
| **[web-design-guidelines](https://github.com/vercel-labs/agent-skills/blob/main/skills/web-design-guidelines/SKILL.md)** (Vercel) · 스킬 | 접근성·UX 감사 | **자동** "접근성 기준으로 검토해줘" | `npx skills add vercel-labs/agent-skills` |
| **[react-best-practices](https://github.com/vercel-labs/agent-skills/blob/main/skills/react-best-practices/SKILL.md)** (Vercel) · 스킬 | React 성능 규칙 | **자동** React 작성 시 자동 | 위와 같은 저장소 |
| **[pick-ui-library](https://github.com/emilkowalski/skills/blob/main/skills/pick-ui-library/SKILL.md)** · 스킬 | UI 라이브러리 추천 | **`/pick-ui-library`** 요구를 말하면 라이브러리 1개 추천 | `npx skills add emilkowalski/skills` |
| **[typescript-lsp](https://claude.com/plugins/typescript-lsp)** (공식) · 플러그인 | 타입 오류 자동 확인 | **자동** 편집할 때마다 자동 | `npm i -g typescript-language-server typescript` 후 `/plugin install typescript-lsp@claude-plugins-official` |

### 문서 · 업무

워드·PPT·엑셀·PDF와 Notion, 문체 정리.

| 항목 | 용도 | 호출 · 사용법 | 설치 |
|---|---|---|---|
| **[문서 스킬](https://github.com/anthropics/skills#readme)** `핵심` (docx·pptx·xlsx·pdf) · 스킬 | 워드·PPT·엑셀·PDF | **자동** "이 내용으로 PPT 만들어줘" | `/plugin marketplace add anthropics/skills` 후 `/plugin install document-skills@anthropic-agent-skills` |
| **[humanizer](https://github.com/blader/humanizer)** `필수` · 스킬 | AI 문체 제거 | **`/humanizer`** 글을 붙여넣고 실행 | [핵심 요약 참고](#4-핵심-요약--이것만-알아도-됨) |
| **[Notion MCP](https://claude.com/plugins/notion)** · MCP | Notion 연동 | **자동** "Notion 기획 페이지 요약해줘" | [기획 탭과 동일](#기획) |
| **[marketingskills](https://github.com/coreyhaines31/marketingskills)** · 스킬 | 마케팅·SEO 스킬 묶음 | **자동** "이 페이지 SEO 감사해줘" | `npx skills add coreyhaines31/marketingskills --skill seo-audit` (필요한 것만) |

> 준비물: 문서 스킬은 Node.js·Python·LibreOffice·Poppler가 있어야 파일이 만들어집니다.

---

## 7. 작업 방식 · 세팅

세팅·워크플로·토큰 절약·리뷰.

| 항목 | 용도 | 호출 · 사용법 | 설치 |
|---|---|---|---|
| **[Hooks](https://claude.com/plugins/hookify)** `필수` · 설정 | 위험 명령 차단·종료 검사 | **설정** `/hookify`에 규칙을 말하면 훅 생성 | [핵심 요약 참고](#4-핵심-요약--이것만-알아도-됨) |
| **[claude-code-setup](https://claude.com/plugins/claude-code-setup)** (공식) · 플러그인 | 맞는 훅·스킬·MCP 추천 | **자동** 프로젝트 폴더에서 "이 프로젝트에 맞는 자동화 추천해줘" | `/plugin install claude-code-setup@claude-plugins-official` |
| **[claude-md-management](https://claude.com/plugins/claude-md-management)** `필수` (공식) · 플러그인 | 세션 학습을 CLAUDE.md에 반영 | **`/revise-claude-md`** 세션 끝에 실행. 점검은 "CLAUDE.md 감사해줘" | [핵심 요약 참고](#4-핵심-요약--이것만-알아도-됨) |
| **[find-skills](https://github.com/vercel-labs/skills/blob/main/skills/find-skills/SKILL.md)** `필수` · 스킬 | 필요한 스킬 검색·설치 | **자동** "PDF 다루는 스킬 있어?" | [핵심 요약 참고](#4-핵심-요약--이것만-알아도-됨) |
| **[systematic-debugging](https://github.com/obra/superpowers/blob/main/skills/systematic-debugging/SKILL.md)** · 스킬 | 원인부터 찾는 디버깅 | **자동** 버그 보고 시 자동 | `npx skills add obra/superpowers --skill systematic-debugging` (전체는 아래 superpowers) |
| **[Ponytail](https://github.com/DietrichGebert/ponytail)** · 스킬 | 코드 최소화 | **자동** 코드 작성 시 자동. `/ponytail off`로 해제 | `/plugin marketplace add DietrichGebert/ponytail` 후 `/plugin install ponytail@ponytail` |
| **[context-mode](https://github.com/mksglu/context-mode)** · 플러그인 | 툴 출력 격리로 토큰 절약 | **자동** 설치하면 자동 | `/plugin marketplace add mksglu/context-mode` 후 `/plugin install context-mode@context-mode` |
| **[RTK](https://github.com/rtk-ai/rtk)** · 도구 | 터미널 출력 압축으로 토큰 절약 | **자동** `rtk init -g` 한 번 실행 | Releases의 Windows zip을 PATH에 두기 |
| **[graphify](https://github.com/Graphify-Labs/graphify)** · 스킬 | 코드·문서를 지식 그래프로 파악 | **`/graphify .`** 프로젝트나 문서 폴더에서 실행 | `pip install graphifyy && graphify install` (Python 3.10+) |
| **[superpowers](https://github.com/obra/superpowers)** · 플러그인 | 브레인스토밍 → 계획 → TDD → 검증 절차 강제 | **자동** 설치하면 작업마다 절차를 밟음. 토큰·시간을 더 씀 | `/plugin install superpowers@claude-plugins-official` |
| **[gstack](https://github.com/garrytan/gstack)** · 스킬 묶음 | 리뷰·QA·배포까지 40개+ 스킬 | **`/review` `/qa` `/ship` 등** 설치 후 `/`로 목록 확인 | `git clone --depth 1 https://github.com/garrytan/gstack.git ~/.claude/skills/gstack && cd ~/.claude/skills/gstack && ./setup` (Git·Bun 필요) |
| **[open-gsd](https://github.com/open-gsd/gsd-core)** · 도구 | 며칠 걸리는 프로젝트를 계획 → 실행 → 검증으로 | **`/gsd-new-project` · `/gsd-onboard`** 새 프로젝트·기존 코드 | `npx @opengsd/gsd-core@latest` |
| **[code-review](https://claude.com/plugins/code-review)** (공식) · 플러그인 | PR을 에이전트 4개가 병렬로 심층 리뷰 | **`/code-review`** PR 올린 뒤 실행. 내장 명령보다 깊게 봄 | `/plugin install code-review@claude-plugins-official` |

> 끄기·지우기: 플러그인 `/plugin uninstall 이름` · MCP `claude mcp remove 이름` · 스킬은 `.claude/skills/이름` 폴더 삭제. 항목 이름을 누르면 원본으로 이동. 설치는 이 표의 명령으로.

---

## 8. 스킬 직접 만들기

스킬은 SKILL.md 한 파일. [`/skill-creator`](#4-핵심-요약--이것만-알아도-됨)에게 말로 시키거나 아래처럼 직접 만듭니다.

```text
# Windows PowerShell — SKILL.md 만들기
PS C:\Users\<이름>> mkdir .claude\skills\meeting-notes
PS C:\Users\<이름>> notepad .claude\skills\meeting-notes\SKILL.md
# 메모장에 아래 내용을 붙여넣고 저장
---
name: meeting-notes
description: 회의 메모를 결정 사항 · 할 일 · 다음 안건 형식의 회의록으로 정리한다
---
결정 사항 → 할 일(담당자 · 기한) → 다음 안건 순으로 개조식 정리. 한국어.
```

> 저장하면 `/meeting-notes`로 바로 사용. `C:\Users\<이름>\.claude\skills\`는 내 PC 전체, `프로젝트\.claude\skills\`는 그 프로젝트만(git 공유). description에는 "언제 쓰는지"를 적습니다. Claude가 스킬을 고르는 기준.

---

## 9. Anthropic 공식 스킬·플러그인

공식 마켓·저장소 중 이 문서 업무에 맞는 것. 앞 장 항목 제외.

| 항목 | 용도 | 호출 · 사용법 | 설치 |
|---|---|---|---|
| **[security-guidance](https://claude.com/plugins/security-guidance)** `필수` · 플러그인 · 누구나 | 비밀키·취약 코드 경고 | **자동** Python 설치 후. 편집 시 경고 | [핵심 요약 참고](#4-핵심-요약--이것만-알아도-됨) |
| **[commit-commands](https://claude.com/plugins/commit-commands)** · 플러그인 · 퍼블리싱 · 프론트엔드 | 커밋·푸시·PR 한 번에 | **`/commit` · `/commit-push-pr`** 작업 끝에 실행. PR은 `gh` 로그인 필요 | `/plugin install commit-commands@claude-plugins-official` |
| **[webapp-testing](https://github.com/anthropics/skills/blob/main/skills/webapp-testing/SKILL.md)** · 스킬 · 퍼블리싱 · 프론트엔드 | 로컬 웹앱 Playwright 테스트 | **자동** Python·Playwright 설치 후 "이 페이지 테스트해줘" | example-skills (아래 설치) |
| **[web-artifacts-builder](https://github.com/anthropics/skills/blob/main/skills/web-artifacts-builder/SKILL.md)** · 스킬 · 퍼블리싱 · 프론트엔드 | React 앱을 HTML 한 파일로 | **자동** "React로 만들어서 HTML 하나로 묶어줘" | example-skills |
| **[brand-guidelines](https://github.com/anthropics/skills/blob/main/skills/brand-guidelines/SKILL.md)** · 스킬 · 디자인 | Anthropic 브랜드 적용 예제 | **자동** 자사 브랜드는 SKILL.md 복사 후 색·서체 교체 | example-skills |
| **[doc-coauthoring](https://github.com/anthropics/skills/blob/main/skills/doc-coauthoring/SKILL.md)** · 스킬 · 문서 · 업무 | 제안서·스펙 공동 작성 | **자동** "제안서 같이 써 줘" | example-skills |
| **[internal-comms](https://github.com/anthropics/skills/blob/main/skills/internal-comms/SKILL.md)** · 스킬 · 문서 · 업무 | 사내 공지·보고 형식 | **자동** 예제 양식 기준. 자사 양식은 SKILL.md 수정 | example-skills |

> example-skills 설치: `/plugin marketplace add anthropics/skills` 후 `/plugin install example-skills@anthropic-agent-skills`

---

## 10. 비권장 · 비효율

부정 평가 우세 · 토큰 대비 효과 낮음 · 내장 기능과 중복.

| 항목 | 판정 | 이유 | 대신 |
|---|---|---|---|
| **[GitHub MCP](https://github.com/github/github-mcp-server)** · MCP | 비효율 | 툴 85개+. `gh`로 대부분 대체 | `gh` 명령어 |
| **[Playwright MCP](https://github.com/microsoft/playwright-mcp)** · MCP | 비효율 | 툴 50개+. MS도 코딩 에이전트엔 CLI 권장 | agent-browser · Playwright CLI |
| **[Sequential Thinking MCP](https://github.com/modelcontextprotocol/servers/tree/main/src/sequentialthinking)** · MCP | 비권장 | 내장 기능으로 충분 | Plan 모드 |
| **[Serena MCP](https://github.com/oraios/serena)** · MCP | 비권장 | 공식 LSP 플러그인 등장. 보안 이슈 제기 이력 | typescript-lsp |
| **[caveman](https://github.com/JuliusBrussee/caveman)** · 스킬 | 비효율 | 벤치마크에서 "간단히 답해"와 차이 없음 | 적게 설치 · `/context` |
| **[mcproxy](https://github.com/team-attention/mcproxy)** · 도구 | 비권장 | 2026-01 이후 업데이트 없음. 내장 Tool Search로 대체 | MCP Tool Search |
| **[claude-mem](https://github.com/thedotmack/claude-mem)** · 플러그인 | 비효율 | 리밋 소진 빠르고 자주 깨짐 | CLAUDE.md · git 기록 |
| **[code-simplifier](https://claude.com/plugins/code-simplifier)** · 플러그인 | 비효율 | 내장 명령과 중복 | 내장 `/simplify` |
| **[headroom](https://github.com/headroomlabs-ai/headroom)** · 도구 | 비효율 | 프록시·라이브러리 방식이라 설정이 무거움 | [RTK(7장)](#7-작업-방식--세팅) |

---

## 11. 기타 — 터미널 대체·보조

기본은 [Windows Terminal(2장)](#2-터미널-쉽게-쓰기)·데스크톱 앱(1장). 세션을 여럿 돌리거나 미리보기가 필요할 때만 선택.  
터미널을 바꿔도 `~/.claude` 설정(스킬·MCP·로그인)은 그대로.

| 항목 | 용도 | 호출 · 사용법 | 설치 |
|---|---|---|---|
| **[Orca](https://www.onorca.dev)** · 도구 | Claude Code 전용 작업 창. 프로젝트마다 worktree를 나눠 세션 여러 개 병렬, 내장 브라우저·에디터, 휴대폰 앱으로 진행 확인 | **앱** 프로젝트 열기 → 에이전트로 Claude Code 선택. 기존 로그인을 자동 인식 | [onorca.dev/download](https://www.onorca.dev/download). Claude Code는 미리 설치·로그인(1장). 무료·오픈소스 · Win·Mac·Linux |
| **[Warp](https://www.warp.dev)** · 도구 | AI 터미널. 명령·출력이 블록으로 나뉘고 자동완성·오류 설명 | **터미널** 열고 `claude` 그대로 실행 | [warp.dev/download](https://www.warp.dev/download). 계정 가입 필요 · Windows 지원 |
| **[Wave Terminal](https://www.waveterm.dev)** · 도구 | 오픈소스 블록형 터미널. 옆 칸에 파일 미리보기·브라우저를 띄워 놓고 작업 | **터미널** 열고 `claude` 실행. 옆 칸에 파일·URL 열어 두기 | [waveterm.dev](https://www.waveterm.dev). 무료·오픈소스 · Windows 지원 |
| **[Ghostty](https://ghostty.org)** · 도구 | 빠르고 가벼운 터미널. Mac·Linux에서 인기 | **터미널** Windows는 공식 미지원(2026-09 기준) | Mac·Linux만. Windows는 [Windows Terminal(2장)](#2-터미널-쉽게-쓰기) |

> 고르는 기준: 세션 하나면 Windows Terminal. 여러 세션·원격 확인은 Orca. 출력만 보기 좋게 하려면 Warp·Wave. 어느 쪽이든 설치 명령은 이 문서 그대로.

---

© 2026 soonupy. All rights reserved. · [공식 문서 code.claude.com/docs](https://code.claude.com/docs/en/overview) · 2026-09 기준
