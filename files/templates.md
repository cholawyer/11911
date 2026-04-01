# 컴포넌트 템플릿 레퍼런스

이 문서는 cholawyer 긴급대응 페이지의 모든 HTML 컴포넌트 복붙 템플릿입니다.

---

## 1. 홈 카테고리 카드

`#home` 안 `.phase-list` 내부에 추가:

```html
<div class="pc" onclick="showCategory('카테고리ID-main')" style="padding:22px 20px">
  <div class="pi" style="background:rgba(R,G,B,.15);width:52px;height:52px;border-radius:14px;font-size:24px">EMOJI</div>
  <div style="flex:1">
    <div class="pn" style="font-size:17px;margin-bottom:5px">카테고리명</div>
    <div class="pd">한 줄 설명</div>
  </div>
  <div class="pa">›</div>
</div>
```

---

## 2. 카테고리 메인 패널

`</div><!-- close #mv -->` 뒤, `</div><!-- close .app -->` 앞에 배치:

```html
<!-- ═══ 카테고리명 메인 선택 ═══ -->
<div class="panel" id="카테고리ID-main">
  <div class="ph">
    <button class="bb-btn" onclick="goHome()">‹</button>
    <div class="pt">카테고리명</div>
    <div class="pbg r">뱃지텍스트</div>
  </div>
  <div style="padding:0 16px 8px;font-size:12px;color:var(--w3);letter-spacing:.04em">지금 어떤 상황인가요</div>
  <div class="phase-list" style="margin-bottom:16px">

    <div class="pc" onclick="showPanel('카테고리ID-sub1')">
      <div class="pi r">🔴</div>
      <div style="flex:1"><div class="pn">상황1 제목</div><div class="pd">상황1 설명</div></div>
      <div class="pa">›</div>
    </div>

    <div class="pc" onclick="showPanel('카테고리ID-sub2')">
      <div class="pi y">🟡</div>
      <div style="flex:1"><div class="pn">상황2 제목</div><div class="pd">상황2 설명</div></div>
      <div class="pa">›</div>
    </div>

  </div>
  <div class="ca">
    <a href="https://cholawyer.github.io/divine-justice/" target="_blank" class="cb1">도움이 필요하다면</a>
    <a href="tel:01025020208" class="cb2">📱 010-2502-0208 즉시 전화</a>
    <div class="cn">수습할 수 없을 만큼 일이 커졌다면 상담 신청하세요</div>
  </div>
  <div class="ftr">
    <div class="fn">조장현 변호사</div>
    <div class="fi">前 부장판사 (영장 전담)<br>서울 서초구 서초대로49길 12, 201호</div>
  </div>
</div>
```

---

## 3. 세부 상황 패널 (단계형 — 성범죄 스타일)

카테고리 메인 패널 뒤에 배치:

```html
<!-- ═══ 세부 패널: 상황명 ═══ -->
<div class="panel" id="카테고리ID-sub1">
  <div class="ph">
    <button class="bb-btn" onclick="showPanel('카테고리ID-main')">‹</button>
    <div class="pt">상황명</div>
    <div class="pbg r">뱃지</div>
  </div>

  <!-- 경고 박스 -->
  <div class="ab">
    <div class="at">⚠ 지금 절대 하지 마세요</div>
    <div class="ai">금지사항 1</div>
    <div class="ai">금지사항 2</div>
    <div class="ai">금지사항 3</div>
  </div>

  <!-- 단계 안내 -->
  <div class="sec-t">지금 순서대로 따르세요</div>
  <div style="padding:0 16px;margin-bottom:16px">
    <div class="rc show" style="margin:0 0 10px">
      <div class="rs">
        <div class="sn">1</div>
        <div class="st">
          <strong style="color:var(--w9);display:block;margin-bottom:3px">단계 제목</strong>
          단계 설명
        </div>
      </div>
      <div class="rs">
        <div class="sn">2</div>
        <div class="st">
          <strong style="color:var(--w9);display:block;margin-bottom:3px">단계 제목</strong>
          단계 설명
        </div>
      </div>
      <div class="rs">
        <div class="sn">3</div>
        <div class="st">
          <strong style="color:var(--w9);display:block;margin-bottom:3px">단계 제목</strong>
          단계 설명
        </div>
      </div>
      <div class="rs">
        <div class="sn">4</div>
        <div class="st">
          <strong style="color:var(--w9);display:block;margin-bottom:3px">단계 제목</strong>
          단계 설명
        </div>
      </div>

      <!-- 팁 박스 (선택) -->
      <div style="margin-top:12px;padding:10px 12px;background:rgba(30,111,207,.1);border-radius:8px;font-size:12px;color:#6ab4ff;line-height:1.6">
        💡 <strong>팁 제목</strong> — 팁 내용
      </div>
    </div>
  </div>

  <!-- CTA -->
  <div class="ca">
    <a href="https://cholawyer.github.io/divine-justice/" target="_blank" class="cb1">도움이 필요하다면</a>
    <a href="tel:01025020208" class="cb2">📱 010-2502-0208 즉시 전화</a>
    <div class="cn">수습할 수 없을 만큼 일이 커졌다면 상담 신청하세요</div>
  </div>
  <div class="ftr">
    <div class="fn">조장현 변호사</div>
    <div class="fi">前 부장판사 (영장 전담)<br>서울 서초구 서초대로49길 12, 201호</div>
  </div>
</div>
```

---

## 4. 세부 상황 패널 (선택형 — 금주운전 스타일)

클릭으로 상황을 선택하면 해당 안내가 펼쳐지는 패턴:

```html
<div class="panel" id="카테고리ID-sub1">
  <div class="ph">
    <button class="bb-btn" onclick="goBack()">‹</button>
    <div class="pt">상황명</div>
    <div class="pbg g">PHASE N</div>
  </div>

  <!-- 경고 박스 -->
  <div class="ab">
    <div class="at">⚠ 지금 절대 하지 마세요</div>
    <div class="ai">금지사항 1</div>
    <div class="ai">금지사항 2</div>
  </div>

  <!-- 상황 선택 -->
  <div class="sec-t">지금 상황을 선택하세요</div>
  <div class="sl">
    <button class="si" onclick="showR('rNa')">
      <div class="sd" style="background:#ff9f0a"></div>상황A 제목<div class="sa">›</div>
    </button>
    <button class="si" onclick="showR('rNb')">
      <div class="sd" style="background:#ff453a"></div>상황B 제목<div class="sa">›</div>
    </button>
  </div>

  <!-- 펼침 콘텐츠 -->
  <div class="rc" id="rNa">
    <div class="rct">아이콘 제목</div>
    <div class="rs"><div class="sn">1</div><div class="st">안내 1</div></div>
    <div class="rs"><div class="sn">2</div><div class="st">안내 2</div></div>
    <div class="rw">⚠️ 경고 메시지</div>
  </div>
  <div class="rc" id="rNb">
    <div class="rct">아이콘 제목</div>
    <div class="rs"><div class="sn">1</div><div class="st">안내 1</div></div>
    <div class="rs"><div class="sn">2</div><div class="st">안내 2</div></div>
  </div>

  <!-- CTA -->
  <div class="ca">
    <a href="https://cholawyer.github.io/divine-justice/" target="_blank" class="cb1">도움이 필요하다면</a>
    <a href="tel:01025020208" class="cb2">📱 010-2502-0208 즉시 전화</a>
    <div class="cn">수습할 수 없을 만큼 일이 커졌다면 상담 신청하세요</div>
  </div>
</div>
```

---

## 5. 체크리스트 패널 (사건 이후 대응 스타일)

```html
<div class="cls">
  <div class="cgt">📼 시점 라벨</div>
  <div class="cc">
    <div class="ci2" onclick="ck(this)">
      <div class="cir"></div>
      <div>
        <div class="ctxt">할 일 제목</div>
        <div class="cnote">보조 설명</div>
      </div>
    </div>
    <div class="ci2" onclick="ck(this)">
      <div class="cir"></div>
      <div><div class="ctxt">할 일 제목</div></div>
    </div>
  </div>
</div>
```

---

## 6. 위드마크 계산기 (금주운전 전용)

이미 `<head>` JS에 `selD`, `selA`, `selW`, `selT`, `doCalc` 함수 포함.
새 카테고리에는 계산기 불필요. 금주운전에만 있음.

---

## 7. 배포 명령어 템플릿

파일을 사용자에게 전달한 후:
```bash
cd /Users/limkipyo/Desktop/자료/drunk_web
# 다운로드한 index.html을 이 폴더에 복사/덮어쓰기
git add index.html
git commit -m "커밋 메시지"
git push -f origin main
```

반영 확인: https://cholawyer.github.io/11911 (1~2분 소요)
