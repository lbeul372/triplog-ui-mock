# TripLog AI Mock UI — Build Brief

> **목적**: 5/7 발표 PPT 에 박을 화면 캡처용 정적 HTML mockup 빌드 (실제 구현 X, 비주얼만)
> **타겟**: 모바일 first (max-width 420px, iPhone 14 Pro 기준)
> **결과물**: 화면 8종, **각각 별도 HTML 파일**
> **시간 budget**: 30~40분 안에 완성 (너무 깊이 X, mockup 수준)
> **출력 디렉토리**: `ui_mock/` (clodcomp 프로젝트 루트 직속)

---

## 1. 프로젝트 콘텍스트

- **서비스명**: TripLog AI
- **슬로건**: 흩어진 정보를 한 줄로
- **한 줄 설명**: 20-30대 자유여행자를 위한 개인 맞춤 여행 일정 추천 서비스
- **핵심 차별화**: RAG 2-Layer (fine-tuning 없이 정확도 + 개인화)
- **사용 목적**: PPT 슬라이드 안에 화면 캡처 박기 — Triple 식 신뢰감 ↑

---

## 2. 디자인 참조

### Triple 앱 — 디자인 톤 reference

`Lim/triple_refs/` 디렉토리 캡처 참조:
- `Ai 추천 맞춤일정 누름/` — wizard 흐름 + 결과 화면 패턴
- `모바일 캡처/` — 일반 모바일 UI 톤
- `칭다오_장소결과)참고/` — 일자별 결과 카드 형태 (가장 중요)

**Triple 패턴 차용:**
- 부드러운 둥근 카드 (border-radius 12-16px)
- 슬롯 카드 = 사진 placeholder + 이름 + "분류 · 권역" + "추천: <한 줄 코멘트>"
- 일자 탭 (Day 1 / Day 2 / Day 3) — 가로 스크롤 또는 가로 버튼
- 하단 fixed CTA 버튼 (예: "내 일정으로 담기")
- 지도 영역은 정적 placeholder (실제 인터랙티브 지도 X)
- 한글 sans-serif (Pretendard)

**Triple 과 차이점:**
- Triple 메인 컬러 = 연두색 → **TripLog AI = 하늘 (메인) + 코랄 (액센트)**

---

## 3. 컬러 팔레트 (CSS 변수)

```css
:root {
  --primary: #5DA8C5;        /* 하늘 — 메인 액센트 (Triple 의 녹색 자리) */
  --primary-soft: #DCEBF5;
  --primary-deep: #3D8AAA;
  --accent: #EA8775;          /* 코랄 — 보조 액센트 (TripLog 로고 톤) */
  --accent-soft: #FCE9E4;
  --success: #7DC97D;         /* 연두 — 검증 성공 표시 */
  --bg: #ffffff;
  --bg-soft: #F8FAFC;
  --text: #1f2937;
  --text-soft: #6B7280;
  --border: #E5E7EB;
  --shadow: 0 4px 12px rgba(0,0,0,0.05);
}
```

**폰트:**
```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/orioncactus/pretendard@v1.3.9/dist/web/static/pretendard.min.css">
<style>body { font-family: 'Pretendard', -apple-system, sans-serif; }</style>
```

---

## 4. Common Layout

### 모든 화면 공통

- **viewport**: max-width 420px, 가운데 정렬, 흰 배경, 부드러운 회색 border (`box-shadow`)
- **상단 header** (height 56px):
  - 좌: 뒤로가기 ← 또는 햄버거 메뉴
  - 중: 화면 제목
  - 우: 프로필 아이콘
- **하단 nav** (fixed, height 64px) — 4 탭:
  - 🏠 홈 / 🔍 검색 / 📔 내 일정 / 👤 프로필
- **본문 padding**: 20px

### 카드 (슬롯 / 여행기 / 코스)
- border-radius: 14px
- background: #ffffff
- border: 1px solid var(--border)
- padding: 16px
- box-shadow: var(--shadow)
- 16px gap between cards

### CTA 버튼 (primary)
- background: var(--primary)
- color: white
- border-radius: 12px
- height: 48-56px
- font-weight: 600

---

## 5. Required Screens (8화면 — 모두 필수)

### Screen 1: 홈 (`01_home.html`)
**용도**: PPT Slide 4 (라이프사이클 비전), Slide 8 (여행기 공유 영역), Slide 19 (Closing)

**구성:**
- 헤더: "TripLog AI" 로고 + 우상단 프로필
- 메인 hero 영역:
  - 큰 그라데이션 배경 (하늘 → 코랄)
  - 타이틀: "어디로 떠나시나요?"
  - 부제: "5분 입력 → 30초 안에 일정 받기"
  - 큰 CTA 버튼: "AI 맞춤 일정 만들기 ✨"
- 섹션 1: **추천 코스 (featured)** — 가로 스크롤 카드 3개
  - 도쿄 4박5일 (jiwon, 좋아요 234)
  - 발리 5박6일 (yuri, 좋아요 189)
  - 제주 2박3일 (minho, 좋아요 156)
- 섹션 2: **YouTube 큐레이션 코스** — 카드 2개
  - 정제형 vlog 미리보기 (썸네일 placeholder + 제목 + chapter 표시)
- 하단 nav 탭

---

### Screen 2: Wizard Step 1 - 도시 선택 (`02_wizard_city.html`)
**용도**: PPT Slide 6 (PATH 2 AI 맞춤 일정 — 5가지 질문 보여주기)

**구성:**
- 헤더: ← 뒤로가기 + "AI 맞춤 일정 만들기" + (1/5)
- 진행 바 (1/5 = 20% 채움, 하늘색)
- 본문 타이틀: "**떠나고 싶은 도시는?**"
- 부제: "도시 1곳을 선택해 주세요"
- 도시 카드 grid (3 cols × 4 rows = 12개):
  - 일본: 도쿄 ★, 오사카, 교토, 후쿠오카
  - 동남아: 발리, 다낭, 방콕, 싱가포르
  - 한국: 서울, 부산, 제주, 강릉
  - **도쿄 = 선택됨 (하늘색 border + 체크 아이콘)**
- 하단 fixed CTA: "다음 →"

---

### Screen 3: Wizard Step 5 - 테마 선택 (`03_wizard_theme.html`)
**용도**: PPT Slide 6 (PATH 2 AI — 마지막 단계 + 일정 만들기 버튼)

**구성:**
- 헤더: ← + "AI 맞춤 일정 만들기" + (5/5)
- 진행 바 (5/5 = 100%)
- 본문 타이틀: "**선호하는 스타일은?**"
- 부제: "다중 선택 가능"
- 테마 카드 grid (3 cols × 3 rows):
  - 🍜 먹방 (선택됨 ★)
  - 🏛️ 문화·역사 (선택됨 ★)
  - 🌿 자연
  - 🛍️ 쇼핑
  - 🛌 여유롭게 힐링
  - 📷 SNS 핫플
  - 🎵 음악·공연
  - 🎢 액티비티
  - 🏯 전통 체험
- 선택된 항목 요약 (작은 chip 들):
  - 도쿄 · 4박5일 · 부모님과 · 널널하게 · 먹방 + 문화·역사
- 하단 fixed CTA: "AI 일정 만들기 ✨"

---

### Screen 4: AI 일정 결과 — Day 1 (`04_result_day1.html`)
**용도**: PPT Slide 9 (RAG 2-Layer), Slide 14 (검증 시나리오), Slide 15 (Demo Plan)

⭐ **핵심 화면 — 가장 많이 사용됨, 디테일 최고로**

**구성:**
- 헤더: ← + "도쿄 4박5일 일정" + 우상단 공유 아이콘
- 일자 탭 (가로 스크롤): **Day 1** ★ / Day 2 / Day 3 / Day 4
- 지도 영역 (정적 placeholder — 도쿄 지도 SVG 또는 placeholder div, 위에 핀 4-5개)
- 슬롯 카드 시리즈 (Triple 식):

```
┌─────────────────────────────────┐
│ 1️⃣  ⏰ 09:30 MORNING            │
│ 🏛️ 메이지 신궁                   │
│ 관광명소 · 시부야권              │
│ ─────────────                   │
│ 추천: 자연 속 고요한 휴식,         │
│       산책로와 신사 본전 방문      │
│ [🔄 다른 곳 보기] (대안 2개)       │
└─────────────────────────────────┘

┌─────────────────────────────────┐
│ 2️⃣  ⏰ 12:30 LUNCH              │
│ 🍱 규카츠 모토무라 하라주쿠점       │
│ 음식점 · 하라주쿠권              │
│ ─────────────                   │
│ 추천: 바삭한 튀김옷의 돈카츠 정식,  │
│       부모님 모시기 좋은 자리       │
│ [🔄 다른 곳 보기]                │
└─────────────────────────────────┘

┌─────────────────────────────────┐
│ 3️⃣  ⏰ 15:00 AFTERNOON          │
│ 🏛️ 도쿄 국립박물관                │
│ 관광명소 · 우에노권              │
│ ...
└─────────────────────────────────┘

(Day 1 식사 추천 sub-section)
─────────────────────────────────
4️⃣  ⏰ 18:30 DINNER
🍱 이세 스이요시
음식점 · 신주쿠권
추천: 다양한 해산물 요리, 정갈한 식사

5️⃣  ⏰ NIGHT
🏨 숙소 복귀
```

- 하단 fixed CTA: "내 일정으로 저장 ★" + 좋아요 / Deep Copy 아이콘

---

### Screen 5: AI 일정 결과 — Day 2 (다른 권역) (`05_result_day2.html`)
**용도**: PPT Slide 9 (지리 클러스터링 효과 시각화), Slide 14

**구성:**
- Screen 4 와 동일 layout
- 일자 탭: Day 1 / **Day 2** ★ / Day 3 / Day 4
- 지도: **다른 권역 표시** (Day 1 시부야권 → Day 2 아사쿠사권 시각적 차이)
- 슬롯 (예시):
  - 1️⃣ 아침 — 죠몬 롯폰기점 (음식점 · 롯폰기권)
  - 2️⃣ 오전 — teamLab Borderless (관광명소 · 오다이바권)
  - 3️⃣ 점심 — Tsukishima Monja Tamatoya (음식점 · 츠키시마권)
  - 4️⃣ 오후 — 미쓰비시1호관미술관 (관광명소 · 마루노우치권)
  - 5️⃣ 저녁 — 교파오 교자 (음식점 · 신주쿠권)
  - 6️⃣ 숙소 복귀

→ Day 2 = 같은 도쿄지만 **다른 권역 묶음** (지리 클러스터링 효과 명시)

---

### Screen 6: 슬롯 상세 + Swap (`06_slot_swap.html`)
**용도**: PPT Slide 5 (PATH 1 직접 짜기), Slide 9 (RAG 추천 근거)

**구성:**
- 헤더: ← + "장소 상세"
- 메인 슬롯 큰 카드:
  - 사진 placeholder (큰 그레이 박스, 이모지 🏛️)
  - 이름: 메이지 신궁
  - 카테고리: 관광명소 · 시부야권 · 평점 4.7
  - 영업시간: 09:00 ~ 17:00
  - 추천 근거 영역 (하늘색 배경 박스):
    - 💡 **AI 추천 근거**: "사용자 history 분석 — 자연·산책 선호 패턴, 부모님 동행 고려한 평온한 코스"
- **대안 장소 2개** (탭 또는 카드):
  - 대안 1: 우에노 공원 (관광명소 · 우에노권)
  - 대안 2: 하마리큐 은사정원 (관광명소 · 하마리큐권)
- 액션 버튼:
  - "📍 장소 저장"
  - "🔄 이걸로 교체"

---

### Screen 7: 여행기 발행 form (`07_travelogue_publish.html`)
**용도**: PPT Slide 8 (여행기 + 공유)

**구성:**
- 헤더: ← + "여행기 발행"
- 입력 영역:
  - 제목 input ("도쿄 4박5일 — 부모님과 다녀온 여유로운 일정")
  - 본문 textarea (placeholder: "다녀온 후기를 자유롭게 적어주세요")
  - 사진 업로드 영역 (placeholder 6칸 grid, 각 회색 + 카메라 아이콘)
  - 공개 / 비공개 토글
  - 태그 input (chip 형식: #도쿄 #부모님 #문화·역사 #여유롭게)
- 미리보기 섹션 (요약):
  - 일정: 도쿄 4박5일 · 12 슬롯
  - 동행: 부모님과 · 페이스: 널널하게
- 하단 fixed CTA: "여행기 발행 ★"

---

### Screen 8: 발행된 여행기 detail + Deep Copy (`08_travelogue_detail.html`)
**용도**: PPT Slide 8 (여행기 + 공유 — 다른 사용자가 보는 화면)

**구성:**
- 헤더: ← + "여행기"
- 작성자 영역: jiwon 프로필 + 작성일 + ❤️ 234 / 📥 89 / 👁 1,452
- 커버 이미지 placeholder (큰 그라데이션 박스 + 도쿄 텍스트)
- 제목: "도쿄 4박5일 — 부모님과 다녀온 여유로운 일정"
- 일정 요약: 12 슬롯, RAG 추천 (Layer 2 — 동남아 history personalize)
- 본문 텍스트 (3-4 단락 mock — 여행 후기 스타일)
- 일자별 슬롯 미리보기 (Day 1 - 4 작은 카드)
- 댓글 섹션 (mock 2-3 댓글)
- 하단 fixed action bar:
  - "❤️ 좋아요"
  - "**📥 내 일정으로 가져오기 (Deep Copy)** ★" — 가장 큰 버튼
  - "💬 댓글"

---

## 6. Sample Data (실제 같이 보이게)

### 페르소나
- **jiwon** (28세, 동남아 4회 history: 발리/다낭/코타키나발루/푸켓)
- 입력 시나리오: 도쿄 4박5일 / 부모님과 / 널널하게 / 문화·역사 + 먹방
- 응답: Triple 식 슬롯 (식사 3끼 + 공항 + 숙소 자동) — 12 슬롯 예상

### 도쿄 슬롯 마스터 데이터 (검증 시나리오에서 추출)

**Day 1:**
- 메이지 신궁 (관광명소 · 시부야권)
- 규카츠 모토무라 하라주쿠점 (음식점 · 하라주쿠권)
- 도쿄 국립박물관 (관광명소 · 우에노권)
- 이세 스이요시 (음식점 · 신주쿠권)

**Day 2:**
- 죠몬 롯폰기점 (음식점 · 롯폰기권)
- teamLab Borderless (관광명소 · 오다이바권)
- Tsukishima Monja Tamatoya (음식점 · 츠키시마권)
- 미쓰비시1호관미술관 (관광명소 · 마루노우치권)
- 교파오 교자 (음식점 · 신주쿠권)

### 추천 코멘트 톤 (Triple 식 "<매력>+<포인트>")
- "자연 속 고요한 휴식, 산책로와 신사 본전 방문"
- "바삭한 튀김옷의 돈카츠 정식, 부모님 모시기 좋은 자리"
- "디지털 아트 체험, 부모님께도 신선한 경험"
- "정갈한 일식 코스, 격조있는 식사"

---

## 7. Technical Spec

- **Static HTML** — 단일 page 구조, JavaScript 최소 (탭 전환 정도)
- **CDN 의존성**: Pretendard (한글 폰트)
- **이미지**: 사용 X — 모두 placeholder div (이모지 + 회색 배경)
- **지도**: 정적 SVG / div placeholder (실제 지도 라이브러리 X)
- **반응형**: max-width 420px 고정 (데스크탑에서도 모바일 화면)
- **파일 명**:
  - `01_home.html`
  - `02_wizard_city.html`
  - `03_wizard_theme.html`
  - `04_result_day1.html`
  - `05_result_day2.html`
  - `06_slot_swap.html`
  - `07_travelogue_publish.html`
  - `08_travelogue_detail.html`
- **공통 CSS**: 인라인 또는 `common.css` 파일 1개
- **링크 (페이지 간 navigation)**: 작동해도 안 해도 OK (캡처 용이라)

---

## 8. 캡처 가이드 (robse 용)

1. 브라우저로 각 HTML 파일 열기
2. **DevTools → Toggle device toolbar → iPhone 14 Pro** 선택
3. 풀 스크린샷 (Ctrl+Shift+P → "Capture full size screenshot")
4. PNG 저장 → PPT 슬라이드에 삽입

---

## 9. 우선순위 (시간 부족 시)

1. **Screen 4 (Day 1 결과)** ⭐⭐⭐ — 가장 많이 쓰임, 가장 높은 디테일
2. **Screen 1 (홈)** ⭐⭐ — Slide 4, 8, 19 다 쓰임
3. **Screen 5 (Day 2)** ⭐⭐ — 지리 클러스터링 셀링
4. **Screen 8 (여행기 detail)** ⭐⭐ — Deep Copy 핵심
5. **Screen 2 / 3 (Wizard)** ⭐ — 5가지 질문 시연
6. **Screen 6 (Swap)** ⭐ — 슬롯 swap 시연
7. **Screen 7 (발행 form)** — 여행기 흐름 마무리

→ **Screen 1, 4, 5, 8 은 무조건 완성**, 시간 남으면 나머지.

---

## 10. 출력 디렉토리 구조 (예상)

```
ui_mock/
├── BRIEF.md (이 파일)
├── common.css (공통 스타일, 선택)
├── 01_home.html
├── 02_wizard_city.html
├── 03_wizard_theme.html
├── 04_result_day1.html
├── 05_result_day2.html
├── 06_slot_swap.html
├── 07_travelogue_publish.html
└── 08_travelogue_detail.html
```
