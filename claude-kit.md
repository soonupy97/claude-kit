# Claude Code, 업무별로 쓸 만한 스킬·플러그인

기획·디자인·퍼블리싱·프론트엔드·문서 업무 기준. 여러 곳에서 반복 추천되고 평가가 좋은 것 위주. (2026-09 기준)

---

## 0. 용어 5개

| 용어 | 뜻 | 팁 |
|---|---|---|
| **스킬** | Claude에게 일하는 방법을 적어 둔 지침. 필요할 때 자동으로 읽힘 | 가볍고 토큰을 거의 안 씀 |
| **플러그인** | 스킬·명령어·훅·MCP를 한 번에 설치하는 패키지 | `/plugin install 이름@마켓` |
| **MCP** | Claude를 외부 서비스(Figma·Notion·브라우저 등)와 연결하는 서버 | 켜 두는 것만으로 토큰을 차지. 꼭 필요한 것만 |
| **Hooks** | 특정 시점에 자동 실행되는 스크립트 | 규칙을 강제하는 유일한 방법 |
| **컨텍스트·토큰** | Claude가 한 번에 기억하는 양 | 도구가 많을수록 줄어들고 비용이 늘어남 |

---

## 1. 시작하기 A–Z

세 단계면 끝납니다. 코드 블록은 Windows Terminal(PowerShell) 화면입니다. `PS C:\…>` 줄은 프롬프트에서 치는 명령, `>` 줄은 Claude 안에서 치는 말, `⏺`는 Claude의 답이나 도구 실행입니다. 유료 플랜(Pro·Max·Team) 계정이 필요합니다.

### A. 설치와 확인

터미널에 한 줄 붙여넣고 버전이 찍히면 끝. 이후 업데이트는 자동입니다. 문제가 있으면 `claude doctor`.

```text
# Windows PowerShell — 설치와 확인
PS C:\Users\soonu> irm https://claude.ai/install.ps1 | iex
Installing Claude Code...
✔ Installed to C:\Users\soonu\.local\bin\claude.exe
PS C:\Users\soonu> claude --version
2.1.263 (Claude Code)
```

### B. 로그인 · 첫 실행 · 프로젝트 설정

작업 폴더에서 `claude`를 실행하면 처음 한 번 브라우저 로그인이 열립니다. `/init`은 Claude가 매번 읽는 메모(CLAUDE.md)를 만듭니다.

```text
# Windows PowerShell — 로그인 · 첫 실행 · /init
PS C:\Users\soonu> cd C:\work\my-project
PS C:\work\my-project> claude
╭────────────────────────────────────────────╮
│ ✻ Welcome to Claude Code!                  │
│   /help for help · cwd: C:\work\my-project │
╰────────────────────────────────────────────╯
Opening browser to log in...   (처음 한 번만)
> 이 프로젝트가 뭐 하는 건지 설명해줘
⏺ Read(package.json)
  ⎿  Read 42 lines
⏺ React로 만든 관리자 화면입니다. 주요 폴더는 src/pages, src/components ...
> /init
⏺ Write(CLAUDE.md)
  ⎿  Wrote 38 lines
⏺ CLAUDE.md를 만들었어요. 빌드 명령 · 폴더 구조 · 코딩 규칙을 적어 두었습니다.
Opus · ctx 12k/1M 1% · my-project
▸▸ accept edits on (shift+tab to cycle) · ? for shortcuts
```

### C. 플러그인 · MCP · 스킬 붙이기

플러그인은 `/plugin install 이름@claude-plugins-official`, MCP는 셸에서 `claude mcp add`, 스킬은 `npx skills add`. MCP는 3~6개 이내로.

```text
# Windows PowerShell — 플러그인 · MCP · 스킬
> /plugin install skill-creator@claude-plugins-official
✔ Installed skill-creator
> /plugin install mattpocock-skills@claude-plugins-official
✔ Installed mattpocock-skills   (/grill-me, /to-spec, /to-tickets)
# MCP와 npx 스킬은 Claude를 나와서(Ctrl+D) 프롬프트에서
PS C:\work\my-project> claude mcp add context7 -- npx -y @upstash/context7-mcp
Added stdio MCP server context7 to local config
PS C:\work\my-project> npx skills add vercel-labs/agent-browser
✔ Installed skill agent-browser
```

### D. 에디터에서 쓰기

VS Code·Cursor는 확장 보기(`Ctrl+Shift+X`)에서 "Claude Code" 설치, JetBrains는 플러그인 마켓에서 같은 이름. 터미널이 낯설면 데스크톱 앱(claude.com/download).

### 자주 쓰는 명령 한눈에

| 명령 | 무엇 |
|---|---|
| `claude "작업"` · `claude -p "질문"` | 첫 지시와 함께 시작 · 한 번 답하고 종료 |
| `claude -c` · `claude -r` | 직전 대화 이어가기 · 이전 대화 고르기 |
| `/help` · `/model` · `/exit` | 명령 목록 · 모델 변경 · 종료 |
| `/plugin` · `/mcp` · `/permissions` | 플러그인 탐색 · MCP 상태 · 권한 설정 |

---

## 2. 터미널 쉽게 쓰기## 2. 터미널 쉽게 쓰기

Claude Code는 터미널 안에서 돌아가므로 터미널 자체를 편하게 쓰면 절반은 끝입니다. Windows 기준이고, 맥은 아래에 따로 적었습니다. `필수`는 Claude 안에서 작업할 때 꼭 쓰는 키입니다.

### A. Windows Terminal (Windows 11 기본 터미널)

| 하는 일 | 키 | 메모 |
|---|---|---|
| 새 탭 | `Ctrl+Shift+T` | 프로젝트마다 탭 하나 |
| 탭 전환 | `Ctrl+Tab` |  |
| 탭 닫기 | `Ctrl+Shift+W` | 나눈 창에서는 포커스된 창만 닫힘 |
| 화면 나누기 | `Alt+Shift+D` | 지금 창을 복제해 넓은 쪽으로 나눔. 오른쪽에 질문용 `claude` 띄우기 좋음 |
| 나눈 창 이동 | Alt ←→↑↓ |  |
| 나눈 창 크기 조절 | Alt Shift ←→↑↓ |  |
| 검색 | `Ctrl+Shift+F` |  |
| 화면 맨 위로 | `Ctrl+Shift+Home` | 지난 출력 처음까지 |
| 화면 맨 아래로 | `Ctrl+Shift+End` | 입력줄로 복귀 |
| 한 화면 위로 | `Ctrl+Shift+PgUp` |  |
| 한 화면 아래로 | `Ctrl+Shift+PgDn` |  |
| 글자 키우기 | `Ctrl+=` |  |
| 글자 줄이기 | `Ctrl+-` |  |
| 복사 | 드래그 후 Ctrl+C |  |
| 붙여넣기 | `Ctrl+V` |  |
| 명령 팔레트 | `Ctrl+Shift+P` | 모든 동작 검색 |
| 퀵 창 | `Win+`` | 화면 위에서 내려오는 터미널 |
| 키 바꾸기 | `Ctrl+,` | 설정 → 동작에서 변경. 예: 창 분할을 `Ctrl+Shift++`로 하려면 settings.json에 `{"command":{"action":"splitPane","split":"vertical"},"keys":"ctrl+shift+plus"}` |

> 추천 배치: 화면을 나눠(`Alt+Shift+D`) 왼쪽에서 작업하는 `claude`, 오른쪽에 질문·문서 확인용 `claude`를 하나 더 띄우면 흐름이 끊기지 않습니다. 프로젝트가 여러 개면 탭으로 나눕니다.

```text
# Windows PowerShell — 왼쪽 창: 작업하는 세션
PS C:\work\my-project> claude
> 가입 화면에 카카오 로그인 추가해줘
⏺ Update(src/LoginForm.tsx)
⏺ Update(src/auth.ts)
⏺ Write(src/kakao.ts)
  ⎿  작성 중...
Opus · ctx 58k/1M 6% · my-project
▸▸ accept edits on (shift+tab to cycle) · ? for shortcuts
```

```text
# Windows PowerShell — 오른쪽 창: 물어보는 세션 (Alt+Shift+D 로 열기)
PS C:\work\design-system> claude
> Figma MCP는 어떻게 연결해?
⏺ /plugin install figma@claude-plugins-official 한 줄이면 됩니다.
> 고마워, 다음엔 접근성 검토 방법도
Opus · ctx 3k/1M 0% · design-system
▸▸ accept edits on (shift+tab to cycle) · ? for shortcuts
```

### B. PowerShell 프롬프트에서 (이 PC에서 확인한 기본값)

| 하는 일 | 키 | 메모 |
|---|---|---|
| 자동 완성 | `Tab` | 파일 · 폴더 · 명령 이름을 채워 줌. 여러 번 누르면 순환 |
| 완성 목록 보기 | `Ctrl+Space` | 후보를 메뉴로 보고 고르기 |
| 이전 명령 | `↑` |  |
| 이전 명령 검색 | `Ctrl+R` | 일부만 쳐도 찾아 줌 |
| 앞글자 맞는 이전 명령 | `F8` | `cd`까지 치고 누르면 cd로 시작한 명령만 |
| 입력 줄 지우기 | `Esc` |  |
| 줄 처음으로 | `Home` |  |
| 줄 끝으로 | `End` |  |
| 단어 왼쪽으로 | `Ctrl+←` |  |
| 단어 오른쪽으로 | `Ctrl+→` |  |
| 단어 하나 지우기 | `Ctrl+Backspace` |  |
| 실행 중단 | `Ctrl+C` | 입력 중일 땐 복사가 아니라 중단 |
| 화면 지우기 | `Ctrl+L` | `cls`와 같음 |
| 되돌리기 | `Ctrl+Z` | 입력 편집 취소 |
| 여러 줄 명령 | `Shift+Enter` | PowerShell 프롬프트에서는 됨 (Claude 안에서는 Ctrl+J) |
| 경로 넣기 | 탐색기에서 폴더를 창으로 드래그 | 경로가 그대로 붙음 |

**자주 쓰는 명령 다섯 개**

| 명령 | 무엇 |
|---|---|
| `cd 폴더` · `cd ..` | 폴더로 이동 · 상위로 |
| `ls` · `pwd` | 목록 보기 · 현재 위치 |
| `code .` · `explorer .` | 현재 폴더를 VS Code · 탐색기로 열기 |
| `cls` | 화면 지우기 |
| `claude` | 여기서 Claude Code 시작 |

### C. VS Code 안에서

| 하는 일 | 키 | 메모 |
|---|---|---|
| 터미널 열기 · 닫기 | `Ctrl+`` | 같은 키로 토글 |
| 새 터미널 | `Ctrl+Shift+`` |  |
| 터미널 나누기 | `Ctrl+Shift+5` | 나눈 창 이동은 `Alt+←` `Alt+→` |
| Claude Code 패널 | Spark 아이콘 클릭 | 확장 설치 후 오른쪽 패널 |

### D. Claude Code 세션 안에서

| 하는 일 | 키 | 메모 |
|---|---|---|
| 단축키 도움말 | `?` | 입력창이 비어 있을 때 누르면 모든 단축키가 화면에 뜸. 이것만 기억해도 됨 |
| 답변 멈추기 `필수` | `Esc` |  |
| 되감기 `필수` | `Esc+두+번` | 이전 지점으로 돌아가 다시 시도 |
| 입력 지우기 · 중단 | `Ctrl+C` | 입력 중이면 지우기, 실행 중이면 중단 |
| 종료 | `Ctrl+D` |  |
| 여러 줄 입력 `필수` | `Ctrl+J` | 한/영 전환 없이 바로 줄바꿈. 안 되는 터미널이면 줄 끝에 역슬래시(\) 치고 Enter |
| 이전 입력 | `↑` |  |
| 권한 모드 전환 `필수` | `Shift+Tab` | Manual → acceptEdits → Plan 순환 |
| 이미지 붙이기 `필수` | `Alt+V` | Windows·WSL 기준. 스크린샷 복사 후 바로. 파일 드래그도 됨 |
| 대화 맨 위로 | `Ctrl+Home` | 전체화면 모드에서. 마우스 휠도 됨 |
| 대화 맨 아래로 | `Ctrl+End` | 최신 답변으로 복귀 |
| 반 화면 위로 | `PgUp` |  |
| 반 화면 아래로 | `PgDn` |  |
| 모델 전환 | `Alt+P` |  |
| 명령 실행 `필수` | `/` | 명령 · 스킬 목록 |
| 셸 명령 그대로 실행 | `!` | 예: `!git status` |
| 파일 언급 | `@` | 예: `@src/App.tsx` |

```text
# 세션 안에서 자주 쓰는 동작
# Shift+Tab 을 누를 때마다 맨 아래 안내줄이 바뀜
▸▸ plan mode on (shift+tab to cycle) · ? for shortcuts
# Ctrl+J 로 줄을 바꿔 여러 줄 입력
> 결제 화면 개편 계획 세워줘
  - 카드 · 카카오페이 지원
  - 기존 주문 데이터 유지
⏺ 계획만 정리했습니다. 파일은 수정하지 않았어요. 진행할까요?
# Alt+V 로 스크린샷 붙이기 · Esc 두 번으로 되감기
> [Image #1] 이 화면 왜 깨졌어?
⏺ flex-wrap 이 없어 좁은 폭에서 넘칩니다. 고칠까요?
```

### E. 맥이라면

| 하는 일 | 키 | 메모 |
|---|---|---|
| iTerm2 세로로 나누기 | `Cmd+D` | 새 탭은 `Cmd+T` |
| iTerm2 가로로 나누기 | `Cmd+Shift+D` |  |
| Alt 단축키 쓰기 | 터미널 설정에서 Option → Meta | `Alt+P` 같은 키를 쓰려면 필요 |
| 이미지 붙이기 | `Ctrl+V` | iTerm2는 `Cmd+V` |

> 습관 세 가지: ① 긴 로그나 코드는 그대로 붙여넣기(자동으로 여러 줄 모드). ② 이어서 할 일은 `claude -c`로 직전 대화 복귀. ③ 스크린샷은 캡처 후 `Alt+V`로 바로 붙여 "이 화면 왜 깨졌어?"처럼 묻기.

---

## 3. 활용 예시

실제 대화 흐름 네 가지. 이렇게 말하면 됩니다.

### 1. 요구사항 확정 → 스펙 → 티켓

`/grill-me`가 질문으로 범위를 좁히고, `/to-spec` · `/to-tickets`가 문서와 티켓으로 만듭니다. 모두 mattpocock-skills.

```text
# Windows PowerShell — 요구사항 → 스펙 → 티켓
> /grill-me 회원가입 화면을 개편하려고 해
⏺ 확인할게요. 1) 소셜 로그인 포함?  2) 기존 데이터 유지?  3) 완료 기준은?
> 1) 카카오만  2) 유지  3) 가입 완료율 측정
⏺ 범위 정리: 카카오 로그인 추가 · 데이터 유지 · 완료율 이벤트 로깅
> /to-spec
⏺ Write(docs/spec.md)
  ⎿  Wrote 96 lines
> /to-tickets
⏺ 티켓 9개로 나눴습니다. 각 티켓에 수용 기준 포함.
```

### 2. 디자인 → 코드

Figma 링크(node-id 포함)를 붙여넣기만 하면 Figma MCP가 읽습니다.

```text
# Windows PowerShell — 디자인 → 코드
> https://www.figma.com/design/xxxx?node-id=12-345 이 컴포넌트를 우리 토큰으로 코드화해줘
⏺ figma - get_design_context(node 12-345)
  ⎿  Button · 3 variants · 2 states
⏺ Write(src/components/Button.tsx)
⏺ Write(src/components/button.scss)
⏺ 색·간격을 토큰(--color-primary, --space-3)에 매핑했습니다. 스토리북에서 확인해 보세요.
```

### 3. 화면 검증

agent-browser가 브라우저를 직접 열어 확인하고, web-design-guidelines가 접근성을 검토합니다.

```text
# Windows PowerShell — 화면 검증
> localhost:3000 열어서 가입 버튼 눌러보고 스크린샷 찍어줘
⏺ Bash(agent-browser open http://localhost:3000 && click "가입")
  ⎿  screenshot saved: signup-done.png
⏺ 클릭 후 완료 화면이 뜹니다. [Image] signup-done.png
> 접근성 기준으로 검토해줘
⏺ 3가지: 버튼 대비 부족(4.2:1) · 입력창 라벨 누락 · 포커스 표시 없음. 고칠까요?
```

### 4. 문서 업무와 마무리

문서 스킬 · humanizer는 설치만 해 두면 자동으로 동작합니다. 커밋은 말로 하면 됩니다.

```text
# Windows PowerShell — 문서 업무와 마무리
> report.pdf 요약해서 엑셀 표로 만들어줘
⏺ Read(report.pdf)
  ⎿  24 pages
⏺ Write(summary.xlsx)
⏺ 핵심 항목 24개를 summary.xlsx 로 저장했습니다.
> 변경한 파일 커밋해줘
⏺ Bash(git add -A && git commit -m "feat: 카카오 로그인 추가")
  ⎿  3 files changed
⏺ 커밋했습니다.
```

> 팁: 큰 작업은 `Shift+Tab`으로 Plan 모드에 두고 계획을 먼저 받으세요. 길어지면 `/context`로 확인하고 다른 일을 시작할 때 `/clear`. 요청은 "로그인 버그 고쳐줘"보다 "잘못된 비밀번호 입력 시 빈 화면이 뜨는 로그인 버그 고쳐줘"처럼 구체적으로.

---

## 4. 핵심 요약## 4. 핵심 요약## 3. 핵심 요약 — 이것만 알아도 됨

`필수` = 누구에게나 · `핵심` = 해당 업무의 기본 · `/이름` = 직접 호출 · 자동 = 설치만 하면 됨.

| 항목 | 한 줄 | 호출 · 사용법 | 설치 |
|---|---|---|---|
| **Hooks** `필수` · 설정 | 위험 명령 차단, 작업 끝내기 전 검사 | **설정** `/hookify`에 규칙을 말하면 훅 생성 | `/plugin install hookify@claude-plugins-official` |
| **skill-creator** `필수` · 플러그인 | 반복 지시를 스킬로 | **`/skill-creator`** 반복 지시를 설명하면 스킬 파일 생성 | `/plugin install skill-creator@claude-plugins-official` |
| `grill-me` `필수` · 스킬 | 시작 전 "정말 필요한가" 검증 | **`/grill-me`** 하고 싶은 일을 말하면 질문으로 검증 | `/plugin install mattpocock-skills@claude-plugins-official` |
| **Context7** `핵심` · MCP | 라이브러리 최신 문서 참조 | **자동** 라이브러리 질문 시 자동. "use context7"로 강제 | `claude mcp add context7 -- npx -y @upstash/context7-mcp` |
| `agent-browser` `핵심` · 스킬 · CLI | Claude가 브라우저로 화면 확인 | **자동** "localhost:3000 열어서 확인해줘" | `npx skills add vercel-labs/agent-browser` |
| **frontend-design** `핵심` · 플러그인 | "AI 티" 나는 화면 방지 | **자동** UI 작업이면 자동 적용 | `/plugin install frontend-design@claude-plugins-official` |
| **Figma MCP** `핵심` · MCP | Figma 디자인 → 코드·토큰 | **자동** Figma 링크 붙여넣고 "코드로 만들어줘" | `/plugin install figma@claude-plugins-official` |
| **문서 스킬** `핵심` · 스킬 | 워드·PPT·엑셀·PDF 생성 | **자동** "이 내용으로 PPT 만들어줘" | `/plugin marketplace add anthropics/skills` 후 `/plugin install document-skills@anthropic-agent-skills` |

> 원칙: 적게 설치(MCP 3~6개, 스킬 8~12개). 명령어로 되면 MCP 대신 스킬·CLI.

---

## 5. 누구나 — 권한 모드와 습관

### 권한 모드 6가지 (`Shift+Tab`으로 전환)

| 모드 | 동작 | 언제 |
|---|---|---|
| **Manual** (default) | 도구를 처음 쓸 때마다 확인 | 기본. 처음 익힐 때 |
| **acceptEdits** | 파일 편집·파일 작업은 자동 승인 | 평소 작업. 확인 창 줄이기 |
| **Plan** | 읽기만 하고 파일을 고치지 않음 | 시작 전 계획 세울 때 |
| **auto** | 분류기가 요청과 맞는지 검사한 뒤 자동 승인 | 긴 작업을 맡길 때 |
| **dontAsk** | 미리 허용한 것 외에는 자동 거부 | 허용 목록만으로 돌릴 때 |
| **bypassPermissions** (`claude --dangerously-skip-permissions`) | 확인 없이 전부 실행 | 격리 환경(컨테이너·VM)에서만. 차단 훅 먼저 |

### 습관 5개

| 항목 | 무엇 |
|---|---|
| `/context` | 도구가 쓰는 토큰 확인. MCP 20k 이하 유지 |
| `/statusline` | 화면 아래에 모델·컨텍스트·폴더 표시. 입력하면 자동 설정 |
| `/clear` · `/compact` | 작업 바뀌면 clear, 길어지면 compact |
| CLAUDE.md 짧게 | `/init`으로 만들고 60~200줄. 강제는 훅으로 |
| `Esc` `Esc` | 이전 지점으로 되감기 |

---

## 6. 기획

| 항목 | 용도 | 호출 · 사용법 | 설치 |
|---|---|---|---|
| `grill-me` `필수` · 스킬 | 요구사항을 질문으로 파헤쳐 가정 제거 | **`/grill-me`** 하고 싶은 일을 말하면 질문으로 검증 | 핵심 요약 참고 |
| `to-spec · to-tickets` · 스킬 | 요구사항을 스펙 문서·작업 티켓으로 | **`/to-spec` · `/to-tickets`** grill-me 뒤에 순서대로 실행 | grill-me와 같은 플러그인 |
| **문서 스킬** `핵심` (pptx·docx) · 스킬 | 기획서·제안서 생성 | **자동** "이 내용으로 PPT 만들어줘" | `/plugin marketplace add anthropics/skills` 후 `/plugin install document-skills@anthropic-agent-skills` |
| **Notion MCP** · MCP | Notion 페이지 읽기·쓰기 | **자동** "Notion 기획 페이지 요약해줘" | `/plugin install notion@claude-plugins-official` |

---

## 7. 디자인

| 항목 | 용도 | 호출 · 사용법 | 설치 |
|---|---|---|---|
| **frontend-design** `핵심` (공식) · 플러그인 | "AI 티" 나는 화면을 피하는 기본 규칙 | **자동** UI 작업이면 자동 적용 | 핵심 요약 참고 |
| **Figma MCP** `핵심` (공식) · MCP | Figma 디자인을 읽어 코드·토큰으로 | **자동** Figma 링크 붙여넣고 "코드로 만들어줘" | 핵심 요약 참고 |
| `taste` · 스킬 | 평범한 결과물을 막는 디자인 취향 | **자동** 디자인 작업 시 자동 | `npx skills add Leonxlnx/taste-skill` |
| `animate` · 스킬 | 애니메이션 설계 | **자동** "호버 애니메이션 넣어줘" | `npx skills add emilkowalski/skills` |
| `apple-design` · 스킬 | Apple식 모션·타이포를 웹으로 | **자동** "Apple 느낌 모션으로" | 위와 같은 저장소 |

> 회사 디자인 시스템이 있으면 그 규칙이 우선.

---

## 8. 퍼블리싱 · 프론트엔드

| 항목 | 용도 | 호출 · 사용법 | 설치 |
|---|---|---|---|
| **Context7** `핵심` · MCP | 라이브러리 최신 문서 참조 | **자동** 라이브러리 질문 시 자동. "use context7"로 강제 | 핵심 요약 참고 |
| `agent-browser` `핵심` · 스킬 · CLI | 브라우저로 화면 직접 확인 | **자동** "localhost:3000 열어서 확인해줘" | 핵심 요약 참고 |
| `web-design-guidelines` (Vercel) · 스킬 | 접근성·UX 100+ 규칙으로 화면 감사 | **자동** "접근성 기준으로 검토해줘" | `npx skills add vercel-labs/agent-skills` |
| `react-best-practices` (Vercel) · 스킬 | React 성능 규칙 | **자동** React 작성 시 자동 | 위와 같은 저장소 |
| `pick-ui-library` · 스킬 | 작업에 맞는 UI 라이브러리 추천 | **`/pick-ui-library`** 요구를 말하면 라이브러리 1개 추천 | `npx skills add emilkowalski/skills` |
| **typescript-lsp** (공식) · 플러그인 | 타입 오류를 Claude가 직접 확인 | **자동** 타입 오류를 스스로 확인 | `/plugin install typescript-lsp@claude-plugins-official` |

---

## 9. 문서 · 업무

| 항목 | 용도 | 호출 · 사용법 | 설치 |
|---|---|---|---|
| **문서 스킬** `핵심` (docx·pptx·xlsx·pdf) · 스킬 | 워드·PPT·엑셀·PDF 생성·읽기 | **자동** "이 내용으로 PPT 만들어줘" | 핵심 요약 참고 |
| `humanizer` · 스킬 | AI 문체 제거 | **`/humanizer`** 글을 붙여넣고 실행 | `npx skills add blader/humanizer` |
| **Notion MCP** · MCP | Notion 연동 | **자동** "Notion 기획 페이지 요약해줘" | 기획 표와 동일 |
| `marketingskills` · 스킬 | 마케팅·SEO 스킬 묶음 | **자동** "이 페이지 SEO 감사해줘" | `npx skills add coreyhaines31/marketingskills` |

---

## 10. 작업 방식 · 세팅

| 항목 | 용도 | 호출 · 사용법 | 설치 |
|---|---|---|---|
| **claude-code-setup** (공식) · 플러그인 | 프로젝트에 맞는 훅·스킬·MCP 추천 | **`/claude-code-setup`** 프로젝트 폴더에서 실행 | `/plugin install claude-code-setup@claude-plugins-official` |
| **claude-md-management** (공식) · 플러그인 | CLAUDE.md 점검·정리 | **`/revise-claude-md`** 세션 끝에 실행해 CLAUDE.md 갱신 | `/plugin install claude-md-management@claude-plugins-official` |
| `find-skills` · 스킬 | 필요한 스킬 검색·설치 | **`/find-skills`** "PDF 다루는 스킬 찾아줘" | `npx skills add vercel-labs/skills` |
| `systematic-debugging` · 스킬 | 고치기 전에 원인부터 찾는 4단계 | **자동** 버그 보고 시 자동 | superpowers 안의 스킬 (`obra/superpowers`) |
| `Ponytail` · 스킬 | "안 쓴 코드가 최고". 코드 최소화 | **자동** 코드 작성 시 자동 | `DietrichGebert/ponytail` |
| `mcp-builder` (공식) · 스킬 | MCP 서버 제작 가이드 | **자동** "X 서비스용 MCP 서버 만들어줘" | `anthropics/skills` |

---
| **context-mode** · 플러그인 | 툴 출력을 격리해 컨텍스트 절약 | **자동** 설치하면 자동 | `mksglu/context-mode` |

---

## 11. Anthropic 공식 스킬·플러그인

공식 마켓과 공식 스킬 저장소에서 이 문서의 업무에 맞는 것. 앞 장에 있는 항목은 제외.

| 항목 | 용도 | 호출 · 사용법 | 설치 |
|---|---|---|---|---|
| **security-guidance** · 플러그인 · 누구나 | 편집 시 비밀키 · 취약 코드 경고 | **자동** 설치하면 편집 시 경고 | `/plugin install security-guidance@claude-plugins-official` |
| **learning-output-style** · 플러그인 · 누구나 | 설명을 곁들인 학습형 답변 스타일 | **`/output-style`** 설명형 답변 켜기 | `/plugin install learning-output-style@claude-plugins-official` |
| **commit-commands** · 플러그인 · 퍼블리싱 · 프론트엔드 | 커밋 · 푸시 · PR을 명령 하나로 | **`/commit` · `/commit-push-pr`** 작업 끝에 실행 | `/plugin install commit-commands@claude-plugins-official` |
| `webapp-testing` · 스킬 · 퍼블리싱 · 프론트엔드 | Playwright로 웹 화면을 자동 테스트 | **자동** "이 페이지 테스트해줘" | example-skills (아래 설치) |
| `web-artifacts-builder` · 스킬 · 퍼블리싱 · 프론트엔드 | 단일 HTML 파일로 동작하는 앱 · 프로토타입 제작 | **자동** "HTML 하나로 프로토타입 만들어줘" | example-skills |
| `brand-guidelines` · 스킬 · 디자인 | 브랜드 색 · 서체 규칙 적용 | **자동** "브랜드 가이드 맞춰서 만들어줘" | example-skills |
| `doc-coauthoring` · 스킬 · 문서 · 업무 | 긴 문서를 초안 → 검토 → 수정으로 함께 작성 | **자동** "제안서 같이 써 줘" | example-skills |
| `internal-comms` · 스킬 · 문서 · 업무 | 사내 공지 · 보고 · 회의록 형식으로 작성 | **자동** "이 내용 사내 공지문으로" | example-skills |

> 공식 스킬 설치: `/plugin marketplace add anthropics/skills` 후 `/plugin install example-skills@anthropic-agent-skills`

---

## 12. 비권장 · 비효율

부정 평가가 우세하거나, 토큰 대비 효과가 낮거나, 내장 기능과 겹치는 것.

| 항목 | 판정 | 이유 | 대신 |
|---|---|---|---|
| **superpowers** · 플러그인 | 비효율 | 느리고 토큰을 두 배 씀. 최신 모델에선 불필요하다는 평 | grill-me |
| **gstack** · 플러그인 | 비권장 | 권한 확인 창 과다. 해외 평 부정적 | grill-me · 공식 플러그인 |
| **GitHub MCP** · MCP | 비효율 | 연결만 해도 수만 토큰 | `gh` 명령어 |
| **Playwright MCP** · MCP | 비효율 | 연결만 해도 수만 토큰 | agent-browser · Playwright CLI |
| **Sequential Thinking MCP** · MCP | 비권장 | 내장 기능으로 충분 | Plan 모드 |
| **Serena MCP** · MCP | 비권장 | 공식 LSP 등장 후 하락. 보안 경고 | typescript-lsp |
| `caveman` · 스킬 | 비효율 | 벤치마크에서 "간단히 답해"와 차이 없음 | 적게 설치 · `/context` |
| **mcproxy** · 도구 | 비권장 | 2026년 1월 이후 정지 | MCP Tool Search |
| **get-shit-done** · 도구 | 비권장 | 토큰 과다. 평가가 극단적으로 갈림 | Plan 모드 · `grill-me` |
| **claude-mem** · 플러그인 | 비효율 | 리밋 소진이 빠르고 자주 깨진다는 후기 | CLAUDE.md · git 기록 |
| **ui-ux-pro-max** · 스킬 | 비효율 | 데이터가 커서 컨텍스트를 많이 씀. frontend-design과 겹침 | frontend-design |
| **code-review** · 플러그인 | 비효율 | 내장 명령과 중복 | 내장 `/code-review` |
| **code-simplifier** · 플러그인 | 비효율 | 내장 명령과 중복 | 내장 `/simplify` |
| **graphify** · 스킬 | 비권장 | 개발자용 코드 그래프. 이 문서 업무엔 과함 | 필요 없음 |
| **headroom · RTK** · 도구 | 비효율 | 터미널 출력이 많은 개발 세션 전용 | 적게 설치 · `/context` |
| **ccstatusline · claude-hud** · 도구 | 비효율 | 상태줄 꾸미기용 | 내장 `/statusline` |
