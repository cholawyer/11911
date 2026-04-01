# 조장현 변호사 긴급대응 웹페이지 — 프로젝트 지침서

## 프로젝트 개요
- **URL**: https://cholawyer.github.io/11911
- **레포**: `cholawyer/11911` (GitHub Pages)
- **파일**: 단일 `index.html` (모든 CSS/JS 인라인)
- **로컬 경로**: `/Users/limkipyo/Desktop/자료/drunk_web/index.html`
- **용도**: 법적 긴급상황(음주운전, 성범죄 등) 즉각 대응 모바일 웹앱

---

## 현재 구조 (2026.03.30 기준)

### 화면 흐름
```
스플래시 (달빛 디자인, 2초 자동전환 또는 "천천히 시작하기" 버튼)
  ↓
홈 (카테고리 선택: 금주운전 / 성범죄)
  ↓
카테고리 메인 → 세부 패널 → 뒤로가기
```

### 하단 네비게이션 (4탭, 고정)
| 탭 | 동작 | 활성색 |
|---|---|---|
| 🏠 홈 | 스플래시 생략, 홈으로 이동 | #2563eb |
| ⚖️ AI판사봇 | 봇 패널 열림 | #2563eb |
| 💬 상담 | 상담 패널 (카카오 오픈채팅) | #2563eb |
| 👤 프로필 | 프로필 패널 | #2563eb |
- 누른 탭: 파란색(#2563eb), 나머지: 회색(#9ca3af)

### 패널 구조
```
#splash          ← 달빛 스플래시 (최초 접속 시만)
#home            ← 카테고리 선택 홈
#mv              ← DUI 패널 컨테이너 (display:none 기본)
  #dui-main      ← 금주운전 메인 (.panel)
  #p1~p4         ← 금주운전 세부 패널 (.panel)
#sc-main         ← 성범죄 메인 (.panel, #mv 밖)
#sc-victim       ← 성범죄 세부 (.panel)
#sc-accused      ← 성범죄 세부 (.panel)
#sc-threat       ← 성범죄 세부 (.panel)
#sc-arrest       ← 성범죄 세부 (.panel)
#bot-panel       ← AI판사봇 (fixed overlay)
#consult-panel   ← 상담 (fixed overlay)
#profile-panel   ← 프로필 (fixed overlay)
```

### 외부 링크
| 용도 | URL |
|---|---|
| 변호사 상담 | https://cholawyer.github.io/divine-justice/ |
| 전화 | tel:01025020208 |
| AI판사봇 | https://t.me/Choilawyer_bot |
| 카카오 상담 | https://open.kakao.com/o/sxtohZni |
| 프로필 상세 | https://limkipyo.github.io/divine-justice |

---

## 핵심 개발 규칙

### JS 규칙 (절대 지킬 것)
1. **모든 함수는 `<head>` 안 `<script>` 블록에 선언** — 아티팩트/iframe 환경에서 `<body>` 끝 스크립트는 onclick 시점에 미정의
2. **화살표 함수(`=>`) 금지** — `function(){}` 만 사용
3. **`window.addEventListener('load')` 금지** — iframe에서 안 먹음
4. **CSS animation/transition으로 화면 전환 금지** — `display` 토글만
5. **DOM 접근 코드는 `<body>` 끝 `<script>`에** — 스플래시 타이머만

### 화면 전환 패턴
```js
// 카테고리 진입
function showCategory(id){
  document.getElementById('home').style.display='none';
  document.getElementById('mv').style.display='block';
  document.querySelectorAll('.panel').forEach(function(p){p.classList.remove('active')});
  document.getElementById(id).classList.add('active');
  window.scrollTo(0,0);
}
// 세부 패널
function showPanel(id){ ... }
// 홈으로
function goHome(){ ... }
// DUI 메인으로
function goBack(){ ... }
// 하단 네비
function navTo(tab){ ... }
```

### 네비게이션 함수 정리
| 함수 | 용도 | 사용처 |
|---|---|---|
| `showCategory(id)` | 홈 → 카테고리 메인 | 홈 카드 onclick |
| `showPanel(id)` | 패널 간 이동 | 카테고리 내 카드, 성범죄 뒤로가기 |
| `goHome()` | → 홈 | 카테고리 메인 ‹ 버튼 |
| `goBack()` | → dui-main | DUI 세부 ‹ 버튼 |
| `navTo(tab)` | 하단 네비 전환 | 하단 4탭 |
| `showR(id)` | 접이식 콘텐츠 토글 | DUI 상황 선택 버튼 |
| `ck(el)` | 체크리스트 토글 | P4 체크리스트 |

---

## 디자인 시스템

### 테마: Stitch 라이트
- 배경: `#f0f4f8` 그라데이션
- 카드: `#fff` + 소프트 그림자
- 경고박스: `#fef2f2` (빨간 배경)
- 팁박스: `#eff6ff` (파란 배경)
- 펼침 콘텐츠: 왼쪽 4px 파란 세로 바
- 단계 번호: 파란 원형 `#2563eb`
- 체크리스트: 파란 체크 원형

### 스플래시: 달빛 안정형
- 배경: `#0f172a` (다크)
- 달: `#fef3c7 → #fde68a` 그라데이션
- 별: 흰색 2px 반짝임 애니메이션
- 파형 바: `rgba(253,230,138,.3)`
- 버튼: 따뜻한 옐로우 톤

### CSS 변수
```css
--bg:#f0f4f8; --card:#fff; --border:rgba(0,0,0,.06);
--text:#1a1a2e; --sub:#6b7280; --sub2:#9ca3af;
--blue:#2563eb; --blue-light:#eff6ff;
--green:#10b981; --green-bg:#ecfdf5;
--red:#ef4444; --red-bg:#fef2f2;
--orange:#f59e0b; --orange-bg:#fff7ed;
--r:16px; --rs:12px;
```

### 주요 CSS 클래스
| 클래스 | 용도 |
|---|---|
| `.panel` / `.active` | 화면 패널 (display:none ↔ block) |
| `.pc` | 클릭 카드 |
| `.pi .g/.y/.r/.b` | 카드 아이콘 색상 |
| `.ab` / `.at` / `.ai` | 경고 박스 / 제목 / 항목 |
| `.rc` / `.show` | 접이식 콘텐츠 |
| `.rs` / `.sn` / `.st` | 단계 행 / 번호 / 텍스트 |
| `.cb1` / `.cb2` | CTA 버튼 (주요/보조) |
| `.tip` | 팁 박스 |

---

## 카테고리 추가 절차

### Step 1: 홈에 카테고리 카드 추가
`#home` 안 `.phase-list`에 `.pc` 카드 추가, `onclick="showCategory('NEW-main')"`

### Step 2: 카테고리 메인 패널 생성
`</div><!-- close #mv -->` 뒤에 `.panel#NEW-main` 배치, 뒤로가기는 `goHome()`

### Step 3: 세부 패널 생성
카테고리 메인 뒤에 배치, 뒤로가기는 `showPanel('NEW-main')`

### Step 4: CTA 패턴 (통일)
```html
<div class="ca">
  <a href="https://cholawyer.github.io/divine-justice/" target="_blank" class="cb1">도움이 필요하다면</a>
  <a href="tel:01025020208" class="cb2">📞 010-2502-0208 즉시 전화</a>
  <div class="cn">수습할 수 없을 만큼 일이 커졌다면 상담 신청하세요</div>
</div>
```

### Step 5: 검증
- div 균형: `grep -c "</div>" index.html` == `grep -c "<div" index.html`
- 모든 onclick 함수가 `<head>`에 정의되어 있는지
- 패널 ID 고유성

### Step 6: 배포
```bash
cd /Users/limkipyo/Desktop/자료/drunk_web
git add index.html
git commit -m "커밋 메시지"
git push -f origin main
```
반영 확인: https://cholawyer.github.io/11911 (1~2분 소요)

---

## 변호사 정보
- **이름**: 조장현 변호사
- **소속**: 이작 법무법인 대표변호사
- **경력**: 前 부장판사 (영장 전담)
- **주소**: 서울 서초구 서초대로49길 12, 201호
- **전화**: 010-2502-0208
- **전문**: 형사 (음주운전·성범죄)

---

## 금지 사항 체크리스트
- [ ] `<body>` 끝에 함수 선언하지 않았는가?
- [ ] CSS transition/animation으로 화면 전환하지 않았는가?
- [ ] 화살표 함수(`=>`) 사용하지 않았는가?
- [ ] `window.addEventListener('load')` 사용하지 않았는가?
- [ ] 모든 패널 ID가 고유한가?
- [ ] div 태그 개수가 맞는가?
- [ ] CTA가 "도움이 필요하다면" 패턴인가?
- [ ] 뒤로가기가 올바른 상위 패널로 연결되는가?
- [ ] 하단 네비 navTo 함수에 새 패널 처리가 추가되었는가?
