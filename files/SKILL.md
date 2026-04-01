---
name: cholawyer-emergency-page
description: 조장현 변호사 긴급대응 웹페이지(cholawyer.github.io/11911) 카테고리 추가·수정·배포 스킬. 사용자가 "카테고리 추가", "섹션 추가", "패널 추가", "11911 페이지 수정", "긴급대응 페이지", "금주운전", "성범죄", "변호사 페이지" 등을 언급하면 반드시 이 스킬을 사용하세요. 기존 페이지의 HTML 구조·네비게이션·스타일 패턴을 정확히 따라야 시행착오 없이 작업할 수 있습니다.
---

# 조장현 변호사 긴급대응 페이지 스킬

## 개요
`cholawyer.github.io/11911` — 법적 긴급상황(음주운전, 성범죄 등) 즉각 대응 모바일 웹페이지.
단일 `index.html` 파일. GitHub Pages로 배포.

## 핵심 규칙 (반드시 지킬 것)

### JS 규칙
1. **모든 함수는 `<head>` 안의 `<script>` 블록에 선언** — 아티팩트/iframe 환경에서 `<body>` 끝 스크립트는 onclick 시점에 정의 안 될 수 있음
2. **화살표 함수 금지** — `function(){}` 만 사용 (호환성)
3. **`window.addEventListener('load')` 금지** — iframe에서 안 먹음
4. **CSS animation/transition으로 화면 전환 금지** — `display` 토글만 사용
5. **DOM 접근 코드(getElementById 등)는 `<body>` 끝 `<script>`에** — 스플래시 타이머만 여기에

### 화면 전환 패턴
모든 화면 전환은 `display:none/block` + `.classList.add/remove('active')` 패턴만 사용:
```js
function showCategory(id){
  document.getElementById('home').style.display='none';
  document.getElementById('mv').style.display='block';
  document.querySelectorAll('.panel').forEach(function(p){p.classList.remove('active')});
  document.getElementById(id).classList.add('active');
  window.scrollTo(0,0);
}
```

### 네트워크
- Claude 환경에서 네트워크 차단됨 → GitHub 직접 푸시 불가
- 파일 생성 후 사용자에게 터미널 명령어 제공

---

## 페이지 구조 (3단계)

```
#splash          ← 스플래시 (2초 후 사라짐, 확인 버튼으로 스킵)
#home            ← 카테고리 선택 홈 (금주운전 / 성범죄 / ...)
#mv              ← 패널 컨테이너 (display:none 기본)
  #dui-main      ← 금주운전 메인 (.panel)
  #p1~p4         ← 금주운전 세부 패널 (.panel)
  #sc-main       ← 성범죄 메인 (.panel)  ※ #mv 밖에 위치
  #sc-victim...  ← 성범죄 세부 패널 (.panel)  ※ #mv 밖에 위치
```

### 네비게이션 흐름
```
스플래시 → [확인/2초] → 홈
홈 → showCategory('xxx-main') → 카테고리 메인
카테고리 메인 → showPanel('xxx-detail') → 세부 패널
세부 패널 → goBack() → 카테고리 메인 (DUI만)
세부 패널 → showPanel('xxx-main') → 카테고리 메인 (성범죄 등)
카테고리 메인 → goHome() → 홈
```

---

## 카테고리 추가 절차

새 카테고리(예: "마약사건")를 추가할 때 아래 순서를 정확히 따르세요.
상세 코드 템플릿은 `references/templates.md`를 참조하세요.

### Step 1: `<head>` JS에 함수 추가/확인
기존 `showCategory`, `showPanel`, `goHome` 함수가 새 ID도 처리하는지 확인.
새 카테고리 전용 goBack이 필요하면 추가.

### Step 2: #home에 카테고리 카드 추가
```html
<div class="pc" onclick="showCategory('NEW-main')" style="padding:22px 20px">
  <div class="pi" style="background:rgba(R,G,B,.15);width:52px;height:52px;border-radius:14px;font-size:24px">EMOJI</div>
  <div style="flex:1"><div class="pn" style="font-size:17px;margin-bottom:5px">카테고리명</div><div class="pd">설명 텍스트</div></div>
  <div class="pa">›</div>
</div>
```

### Step 3: 카테고리 메인 패널 생성
`</div><!-- close #mv -->` 뒤에 배치 (sc-main과 동일 위치).

```html
<div class="panel" id="NEW-main">
  <div class="ph"><button class="bb-btn" onclick="goHome()">‹</button><div class="pt">카테고리명</div><div class="pbg r">뱃지</div></div>
  <div style="padding:0 16px 8px;font-size:12px;color:var(--w3)">지금 어떤 상황인가요</div>
  <div class="phase-list" style="margin-bottom:16px">
    <!-- 상황 카드들 -->
  </div>
  <div class="ca"><!-- CTA --></div>
  <div class="ftr"><!-- 변호사 정보 --></div>
</div>
```

### Step 4: 세부 패널 생성
카테고리 메인 뒤에 배치. 뒤로가기는 `showPanel('NEW-main')`.

```html
<div class="panel" id="NEW-detail1">
  <div class="ph"><button class="bb-btn" onclick="showPanel('NEW-main')">‹</button><div class="pt">상황명</div><div class="pbg r">뱃지</div></div>
  <div class="ab"><!-- 경고 박스 --></div>
  <div class="sec-t">지금 순서대로 따르세요</div>
  <div style="padding:0 16px;margin-bottom:16px">
    <div class="rc show" style="margin:0 0 10px"><!-- 단계별 안내 --></div>
  </div>
  <div class="ca"><!-- CTA --></div>
  <div class="ftr"><!-- 변호사 정보 --></div>
</div>
```

### Step 5: CTA 패턴
모든 CTA는 동일 패턴:
```html
<div class="ca">
  <a href="https://cholawyer.github.io/divine-justice/" target="_blank" class="cb1">도움이 필요하다면</a>
  <a href="tel:01025020208" class="cb2">📱 010-2502-0208 즉시 전화</a>
  <div class="cn">수습할 수 없을 만큼 일이 커졌다면 상담 신청하세요</div>
</div>
```

### Step 6: 푸터 패턴
```html
<div class="ftr">
  <div class="fn">조장현 변호사</div>
  <div class="fi">前 부장판사 (영장 전담)<br>서울 서초구 서초대로49길 12, 201호</div>
</div>
```

### Step 7: 검증
- div 개수 균형 확인: `grep -c "</div>" index.html` == `grep -c "<div" index.html`
- 모든 onclick이 `<head>`에 정의된 함수를 호출하는지 확인
- 모든 패널 ID가 고유한지 확인

### Step 8: 파일 출력 & 배포 명령어 제공
```bash
# 사용자에게 제공할 명령어
cd /Users/limkipyo/Desktop/자료/drunk_web
# (다운로드한 index.html을 이 폴더에 넣은 후)
git add index.html
git commit -m "커밋 메시지"
git push -f origin main
```
GitHub: `cholawyer/11911`, 토큰: 사용자에게 확인.

---

## 컴포넌트 레퍼런스

상세 HTML 템플릿은 `references/templates.md` 파일 참조.

### CSS 클래스 요약
| 클래스 | 용도 |
|--------|------|
| `.panel` | 세부 화면 (display:none, .active로 표시) |
| `.pc` | 클릭 가능 카드 |
| `.pi` | 카드 아이콘 (46x46) |
| `.pi .g/.y/.r/.b` | 아이콘 색상 변형 |
| `.ab` | 경고 박스 (빨간 배경) |
| `.ai` | 경고 항목 (✕ 앞에 붙음) |
| `.rc` | 접이식 콘텐츠 (.show로 표시) |
| `.rs` | 단계 행 |
| `.sn` | 단계 번호 원형 |
| `.cb1` | 주요 CTA 버튼 |
| `.cb2` | 보조 CTA 버튼 (전화) |
| `.sec-t` | 섹션 타이틀 (소문자) |
| `.phase-list` | 카드 리스트 컨테이너 |
| `.ph` | 패널 헤더 |
| `.bb-btn` | 뒤로가기 버튼 |
| `.pbg` | 헤더 뱃지 (.g/.y/.r/.b) |

### 뱃지 색상
- 녹색: `background:rgba(52,199,89,.2);color:#34c759`
- 노랑: `background:rgba(255,159,10,.2);color:#ff9f0a`
- 빨강: `background:rgba(255,69,58,.2);color:#ff453a`
- 파랑: `background:rgba(30,111,207,.2);color:#6ab4ff`

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
