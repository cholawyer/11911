# 카카오톡 OG & 배포 가이드

---

## 프로젝트별 경로 & URL

| 프로젝트 | 로컬 경로 | 레포 | GitHub Pages URL |
|---------|----------|------|-----------------|
| 11911 변호사 | `~/Desktop/자료/drunk_web` | cholawyer/11911 | https://cholawyer.github.io/11911 |
| LimPossible 명함 | `~/Desktop/포트폴리오/Limpossible` | limkipyo/Limpossible | https://limkipyo.github.io/Limpossible/ |
| Dash 대시보드 | `~/Desktop/포트폴리오/dash` | limkipyo/dash | https://limkipyo.github.io/dash |

---

## 1. OG 메타태그 (index.html `<head>` 안에 필수)

```html
<meta property="og:title" content="페이지 제목">
<meta property="og:description" content="설명">
<meta property="og:image" content="https://[GitHub Pages URL]/og-v1.png">
<meta property="og:url" content="https://[GitHub Pages URL]/">
<meta property="og:type" content="website">
<meta property="og:site_name" content="사이트명">
<meta property="og:locale" content="ko_KR">
```

**주의:** `og:image` URL은 해당 레포의 GitHub Pages 주소와 일치해야 함. 다른 레포 URL 쓰면 404.

---

## 2. OG 이미지 규격

- 1200 x 630px (카카오/스레드/페북 공통)
- PNG 포맷, Pillow로 생성

---

## 3. GitHub Pages 활성화

레포 → Settings → Pages → Source: `Deploy from a branch` → Branch: `main` / `/ (root)` → Save

**필수:** 메인 파일은 반드시 `index.html`이어야 함. 다른 이름이면 Pages에서 안 뜸.

---

## 4. 배포 (공통)

```bash
git add -A
git commit -m "커밋 메시지"
git push origin main
```

---

## 5. 카카오톡 OG 캐시 초기화

1. https://developers.kakao.com/tool/clear/og 접속
2. 해당 URL 입력 → 캐시 초기화

---

## 6. OG 이미지 캐시 우회

카카오톡은 OG 이미지 URL의 `?v=날짜` 파라미터를 **무시**함.

**⚠️ 반드시 파일명을 바꿔야 캐시가 깨짐**

```bash
# 1. 새 파일명으로 복사
cp og-v1.png og-v2.png

# 2. index.html URL 변경
sed -i '' 's|og-v1.png|og-v2.png|g' index.html

# 3. 커밋 & 푸시
git add og-v2.png index.html
git commit -m "OG 이미지 파일명 교체 - 캐시 우회"
git push origin main

# 4. 카카오 캐시 초기화
```

**❌ 효과 없는 방법**
- `og-image.png?v=20260401` — 카카오는 파라미터 무시
- 같은 파일명으로 덮어쓰기 후 push

---

## 7. Threads / Facebook OG 캐시 초기화

1. https://developers.facebook.com/tools/debug/ 접속
2. URL 입력 → **"다시 스크랩" 2번** 클릭

---

## 8. 11911 오픈채팅 링크 교체

```bash
cd ~/Desktop/자료/drunk_web
sed -i '' 's|이전링크|새링크|g' index.html
grep -n "open.kakao.com" index.html
```

---

## 트러블슈팅 기록

### Limpossible OG 카드 안 뜨던 문제 (2025.04.08)

**원인:** 3가지가 겹침
1. `index.html`에 OG 메타태그가 없었음
2. OG 이미지 파일이 레포에 없었음
3. `dash` 레포에서 작업했는데 OG URL은 `Limpossible` 레포를 가리킴 (레포 불일치)

**해결:**
1. `~/Desktop/포트폴리오/Limpossible/` 레포에서 작업
2. `index.html`에 OG 메타태그 7줄 추가 (`og:image` URL을 `https://limkipyo.github.io/Limpossible/og-v1.png`으로)
3. OG 이미지(`og-v1.png`)를 같은 레포에 넣고 push
4. GitHub Pages 활성화 (Settings → Pages → main 브랜치)
5. 카카오 OG 캐시 초기화

**교훈:**
- OG 이미지 URL과 실제 파일이 같은 레포에 있어야 함
- GitHub Pages는 수동으로 활성화해야 함 (레포 생성만으로 안 됨)
- 메인 파일명은 반드시 `index.html`

---

## 체크리스트 (OG 설정 시)

```
[ ] index.html에 og:title, og:description, og:image, og:url 태그 있는가
[ ] og:image URL이 해당 레포의 GitHub Pages 주소와 일치하는가
[ ] OG 이미지 파일이 레포에 push 되었는가
[ ] GitHub Pages가 활성화되어 있는가 (Settings → Pages → main)
[ ] 파일명 index.html인가
[ ] 카카오 OG 캐시 초기화 했는가
[ ] 스레드 캐시 초기화 했는가 (필요 시)
```
