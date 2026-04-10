# CLAUDE.md — 11911 프로젝트 루트 규칙

## 프로젝트 개요
- 단일 `index.html` (CSS/JS 전부 인라인)
- 로컬: `/Users/limkipyo/Desktop/자료/drunk_web/`
- 배포: `cholawyer.github.io/11911` (GitHub Pages, `cholawyer/11911`)
- 하위 규칙 파일: `CLAUDE_web.md` (웹 패턴 전용)

---

## ✅ 절대 규칙

- 모든 함수는 `<head>` 안 `<script>`에 선언
- `function(){}` 문법만 사용 (화살표 함수 `=>` 금지)
- DOM 조작은 `display` 토글만 (화면 전환)
- 함수/모듈당 60줄 이하 유지
- 커밋 단위: 기능 하나 = 커밋 하나
- 법률 로직 수정 시 관련 조항 번호 주석 필수 (예: `// 학교폭력예방법 제17조`)
- Hallucination 절대 금지: 실재하지 않는 판례/법조항 생성 금지, 제공된 데이터 범위 내에서만 참조

### 검증 & 배포

- 검증 루프: `CLAUDE_plan.md` 공통 원칙 적용
- push 전 검증 3종: **`체크` 커맨드** 사용
- 배포: **`배포` 커맨드** 사용 (검증 3종 + git push 자동)

## 🚫 금지

- `<body>` 끝 함수 선언
- `window.addEventListener('load')`
- CSS transition/animation 화면 전환
- 기존 DUI(#mv), 성범죄(#sc-main) 패널 수정 금지 (학폭 작업 중)

---

## 변호사 정보 (CTA 고정값)

- 전화: `tel:01025020208` / `010-2502-0208`
- 상담: `https://cholawyer.github.io/divine-justice/`
- 카카오: `https://open.kakao.com/o/sCNwh0ni`

---

# CLAUDE_web.md — 11911 웹앱 패턴 규칙

> 루트 규칙을 상속. 이 섹션은 웹 UI 패턴 전용.

---

## 패널 구조

```
#splash → #home → 카테고리메인 → 세부패널
```

| ID | 역할 |
|---|---|
| `#splash` | 달빛 스플래시 (최초 1회) |
| `#home` | 카테고리 선택 |
| `#mv` | DUI 컨테이너 |
| `#sc-main` | 성범죄 메인 |
| `#hp-main` | 학폭 메인 |
| `#bot/consult/profile-panel` | fixed overlay |

---

## ✅ 웹 패턴 규칙

- 새 카테고리 추가 시 반드시 `navTo()` hideAll 목록 업데이트
- CTA 패턴 통일: "도움이 필요하다면" 버튼 + 전화 버튼
- API Key는 파일 상단 `var KEY = ''` 상수로만 선언
- 패널 ID는 카테고리 접두사로 구분 (`dui-` / `sc-` / `hp-`)

## 🚫 웹 금지

- 패널 ID 중복
- `navTo()` 업데이트 누락 시 카테고리 추가 금지
- CTA 패턴 임의 변경

---

## 화면 전환 함수

```js
showCategory(id)  // 홈 → 카테고리메인
showPanel(id)     // 패널 간 이동
showHp(id)        // 학폭 패널 진입
goHome()          // → #home
goBack()          // → dui-main
navTo(tab)        // 하단 4탭
hpBack(parent)    // 학폭 하위 → 상위
```

---

## 카테고리 추가 체크리스트

```
[ ] #home .phase-list에 .pc 카드 추가
[ ] .panel#NEW-main 패널 생성 (#mv 닫는태그 뒤)
[ ] 세부 패널 생성 (뒤로가기 → NEW-main)
[ ] navTo() hideAll 목록에 새 ID 추가
[ ] CTA "도움이 필요하다면" 패턴 적용
[ ] `체크` 커맨드로 검증 통과 확인
```

---

## 디자인 토큰

```css
--blue:#2563eb; --blue-light:#eff6ff;
--red:#ef4444;  --red-bg:#fef2f2;
--green:#10b981;--green-bg:#ecfdf5;
--orange:#f59e0b;--orange-bg:#fff7ed;
--text:#1a1a2e; --sub:#6b7280; --bg:#f0f4f8;
```

## API 상수 위치 (head script 최상단)

```js
var CLAUDE_KEY = '';  // Anthropic — 진술서 정리기
```

---

## 📌 현재 작업: 학폭 카테고리 삽입

### 삽입 소스 파일

`hp-statement-snippet.html` — 이 파일에 삽입할 코드가 전부 들어있음.

### 삽입 순서 (README.md 기준)

1. **PART 1 JS** → `index.html` `<head>` 안 `<script>` 블록에 추가
   - `showHp()`, `hpBack()`, `hpSubmit()`, `hpAnalyze()`, `hpRenderResult()` 등
   - `var CLAUDE_KEY = ''` 설정 확인
2. **PART 2 카드** → `#home .phase-list` 마지막에 학폭 카드 추가
3. **PART 3 패널** → `#mv` 닫는태그 뒤에 패널 5개 추가
   - `#hp-main` (학폭 메인 — 피해자/가해자/쌍방 선택)
   - `#hp-victim` (피해자 대응 가이드)
   - `#hp-accused` (가해자 대응 가이드)
   - `#hp-both` (쌍방 대응 가이드)
   - `#hp-statement` (AI 진술서 정리기)
4. **navTo() 업데이트** → hideAll 목록에 추가:
   `'hp-main','hp-victim','hp-accused','hp-both','hp-statement'`

### 학폭 패널 구조

```
#hp-main (학폭 메인)
  ├── #hp-victim (피해자 → 즉시/오늘/내일/이번주 단계별)
  ├── #hp-accused (가해자 → 하지말것/오늘/사안조사전)
  ├── #hp-both (쌍방 → 알아야할것/피해확보/전략)
  └── #hp-statement (AI 진술서 정리기 — Anthropic API 호출)
```

### 필수 체크리스트

- [ ] PART 1 JS가 `<head>` `<script>` 안에 들어갔는가
- [ ] PART 2 학폭 카드가 `#home .phase-list`에 추가됐는가
- [ ] PART 3 패널 5개가 `#mv` 닫는태그 뒤에 있는가
- [ ] navTo() hideAll에 hp- ID 5개 추가됐는가
- [ ] CTA 패턴 (도움이 필요하다면 + 전화 버튼) 모든 패널에 적용됐는가
- [ ] `var CLAUDE_KEY` 설정됐는가
- [ ] 기존 DUI/성범죄 패널이 깨지지 않았는가
- [ ] `체크` 커맨드 검증 3종 통과

### 참고: 텔레그램 봇 (hakpok_bot.py)

별도 프로젝트. 이 작업에서는 건드리지 않음.
오라클 배포 시 `oracle_setup_guide.md` 참조.

### ⚠️ API 키 보안

- `CLAUDE_KEY`는 index.html에 노출됨 (MVP 단계)
- Anthropic 콘솔에서 월 사용량 한도(spending limit) 반드시 설정
- 향후 Cloudflare Workers 프록시 전환 권장
