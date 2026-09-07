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

## 1. 핵심 요약 — 이것만 알아도 됨

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

## 2. 누구나 — 권한 모드와 습관

### 권한 모드 6가지 (`Shift+Tab`으로 전환, `--permission-mode` 플래그, `defaultMode` 설정)

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

## 3. 기획

| 항목 | 용도 | 호출 · 사용법 | 설치 |
|---|---|---|---|
| `grill-me` `필수` · 스킬 | 요구사항을 질문으로 파헤쳐 가정 제거 | **`/grill-me`** 하고 싶은 일을 말하면 질문으로 검증 | 핵심 요약 참고 |
| `to-spec · to-tickets` · 스킬 | 요구사항을 스펙 문서·작업 티켓으로 | **`/to-spec` · `/to-tickets`** grill-me 뒤에 순서대로 실행 | grill-me와 같은 플러그인 |
| **문서 스킬** `핵심` (pptx·docx) · 스킬 | 기획서·제안서 생성 | **자동** "이 내용으로 PPT 만들어줘" | `/plugin marketplace add anthropics/skills` 후 `/plugin install document-skills@anthropic-agent-skills` |
| **Notion MCP** · MCP | Notion 페이지 읽기·쓰기 | **자동** "Notion 기획 페이지 요약해줘" | `/plugin install notion@claude-plugins-official` |

---

## 4. 디자인

| 항목 | 용도 | 호출 · 사용법 | 설치 |
|---|---|---|---|
| **frontend-design** `핵심` (공식) · 플러그인 | "AI 티" 나는 화면을 피하는 기본 규칙 | **자동** UI 작업이면 자동 적용 | 핵심 요약 참고 |
| **Figma MCP** `핵심` (공식) · MCP | Figma 디자인을 읽어 코드·토큰으로 | **자동** Figma 링크 붙여넣고 "코드로 만들어줘" | 핵심 요약 참고 |
| `taste` · 스킬 | 평범한 결과물을 막는 디자인 취향 | **자동** 디자인 작업 시 자동 | `npx skills add Leonxlnx/taste-skill` |
| `animate` · 스킬 | 애니메이션 설계 | **자동** "호버 애니메이션 넣어줘" | `npx skills add emilkowalski/skills` |
| `apple-design` · 스킬 | Apple식 모션·타이포를 웹으로 | **자동** "Apple 느낌 모션으로" | 위와 같은 저장소 |

> 회사 디자인 시스템이 있으면 그 규칙이 우선.

---

## 5. 퍼블리싱 · 프론트엔드

| 항목 | 용도 | 호출 · 사용법 | 설치 |
|---|---|---|---|
| **Context7** `핵심` · MCP | 라이브러리 최신 문서 참조 | **자동** 라이브러리 질문 시 자동. "use context7"로 강제 | 핵심 요약 참고 |
| `agent-browser` `핵심` · 스킬 · CLI | 브라우저로 화면 직접 확인 | **자동** "localhost:3000 열어서 확인해줘" | 핵심 요약 참고 |
| `web-design-guidelines` (Vercel) · 스킬 | 접근성·UX 100+ 규칙으로 화면 감사 | **자동** "접근성 기준으로 검토해줘" | `npx skills add vercel-labs/agent-skills` |
| `react-best-practices` (Vercel) · 스킬 | React 성능 규칙 | **자동** React 작성 시 자동 | 위와 같은 저장소 |
| `pick-ui-library` · 스킬 | 작업에 맞는 UI 라이브러리 추천 | **`/pick-ui-library`** 요구를 말하면 라이브러리 1개 추천 | `npx skills add emilkowalski/skills` |
| **typescript-lsp** (공식) · 플러그인 | 타입 오류를 Claude가 직접 확인 | **자동** 타입 오류를 스스로 확인 | `/plugin install typescript-lsp@claude-plugins-official` |

---

## 6. 문서 · 업무

| 항목 | 용도 | 호출 · 사용법 | 설치 |
|---|---|---|---|
| **문서 스킬** `핵심` (docx·pptx·xlsx·pdf) · 스킬 | 워드·PPT·엑셀·PDF 생성·읽기 | **자동** "이 내용으로 PPT 만들어줘" | 핵심 요약 참고 |
| `humanizer` · 스킬 | AI 문체 제거 | **`/humanizer`** 글을 붙여넣고 실행 | `npx skills add blader/humanizer` |
| **Notion MCP** · MCP | Notion 연동 | **자동** "Notion 기획 페이지 요약해줘" | 3장과 동일 |
| `marketingskills` · 스킬 | 마케팅·SEO 스킬 묶음 | **자동** "이 페이지 SEO 감사해줘" | `npx skills add coreyhaines31/marketingskills` |

---

## 7. 작업 방식 · 세팅

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

## 8. Anthropic 공식 스킬·플러그인

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

## 9. 비권장 · 비효율

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
