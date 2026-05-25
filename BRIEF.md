# TripLog AI Mock UI — Brief (v2, Final 6/11)

> **목적**: Final 발표용 정적 HTML mockup. 실제 구현 X, 비주얼·플로우 전달용.
> **공유**: GitHub Pages → https://lbeul372.github.io/triplog-ui-mock/ (`index.html`이 허브, 카드 클릭으로 각 화면 이동)
> **타겟**: 모바일 first (max-width 420px). **결과물**: 화면 15종 + index 허브.
> 갱신: 2026-05-25 (5/7 중간 버전의 8화면·"RAG 2-Layer 슬로건" 프레임을 새 방향으로 대체)
> 방향성 근거: `../00_master_plan.md`

---

## 1. 프로젝트 콘텍스트 (새 정체성)

- **서비스명**: TripLog AI
- **한 줄 정체성**: "보던 유튜브·블로그·인스타를 **저장만 하고 안 쓰는** 사람을 위해, 그 콘텐츠를 **AI가 진짜 내 일정으로** 정리해주는 모바일 앱."
- **푸는 문제**: Planning Paradox — 콘텐츠를 저장만 하고 일정화하지 못함.
- **핵심**: "내가 모은 진짜 콘텐츠 → 내 일정" = 멀티소스 레퍼런스 엔진.
- **유튜브는 첫 어댑터일 뿐** — 동선/장소 두 granularity로 모든 소스 흡수.

### 핵심 루프 (화면들이 이 순서로 묶임)
```
저장 → 정리 → 기록 → 전파
보관함  골라서   여행기   친구 deep-copy
에 적립  AI 조립  발행     = 지인추천
```

### 소스 & granularity
| 소스 | 상태 | granularity |
|---|---|---|
| 유튜브 | ✅ | 동선 (여러 장소 묶음) |
| 네이버 블로그 | 🔜 | 동선 |
| 스크린샷(Gemini vision, 인스타 우회) | 🔜 | 장소 |
| 텍스트/지도 핀 | ✅ | 장소 |
| 틱톡/인스타 직접연동 | 📋 로드맵 | 장소/동선 |

---

## 2. 디자인 (변동 없음)

- **톤 reference**: 기존 여행앱 디자인 톤 — 둥근 카드, 슬롯 카드(사진+이름+"분류·권역"+추천 한 줄), 일자 탭, 하단 fixed CTA, 정적 지도 placeholder.
- **컬러**: 하늘(메인) + 코랄(액센트).
- **사진 철학**: placeholder 대신 검증된 Unsplash 실사(`onerror`로 실패 시 이모지 fallback) — "디자인의 끝은 실사".

```css
:root {
  --primary: #5DA8C5;  --primary-soft: #DCEBF5;  --primary-deep: #3D8AAA;
  --accent: #EA8775;   --accent-soft: #FCE9E4;   --success: #7DC97D;
  --bg: #ffffff; --bg-soft: #F8FAFC;
  --text: #1f2937; --text-soft: #6B7280; --border: #E5E7EB;
  --shadow: 0 4px 12px rgba(0,0,0,0.05);
}
```
- **폰트**: Pretendard (CDN). **공통 CSS**: `common.css` 1개.

### Common Layout
- viewport max-width 420px 가운데 정렬 · 상단 header 56px · 하단 nav 64px(🏠홈/🔍검색/📔내일정/👤프로필) · 본문 padding 20px
- 카드: radius 14px, border 1px, padding 16px, shadow · CTA: --primary 배경, radius 12px, height 48–56px, weight 600

---

## 3. 화면 15종 (index 허브 기준 4 그룹)

### 🆕 핵심 루프 — 콘텐츠 보관함 (NEW)
| 파일 | 화면 | 핵심 |
|---|---|---|
| `09_reference_add.html` | 콘텐츠 담기 | 멀티소스 입력(유튜브/블로그/스샷/텍스트), 소스·granularity 뱃지 |
| `10_inbox.html` | 보관함 | 적립된 레퍼런스 모음, 동선/장소 혼재 |
| `11_organize.html` | 정리하기 | 여행 설정(도시·일수·동행) + 보관함에서 골라 선택, 정리 미리보기 |
| `13_assemble_loading.html` | 조립 중 | AI 단계 투명 노출(추출→매칭→동선최적화) |
| `12_result_personalized.html` | 개인화 일정 | 출처 뱃지 + 취향 반영 결과 (시그니처) |
| `14_share_receive.html` | 공유 받기 | 인스타/외부 공유 → 앱이 자동 스크랩 |

### ✨ AI 맞춤 경로 (기존 유지)
| 파일 | 화면 |
|---|---|
| `02_wizard_city.html` | 도시 선택 (5스텝 wizard 1) |
| `03_wizard_theme.html` | 테마 선택 (wizard 5/5) |
| `04_result_day1.html` | AI 결과 Day1 (슬롯 카드 핵심 화면) |
| `05_result_day2.html` | AI 결과 Day2 (다른 권역 = 지리 클러스터링) |
| `06_slot_swap.html` | 장소 상세 + 교체(swap) + AI 추천 근거 |

### ✍️ 기록 & 전파 — 커뮤니티 (기존 유지)
| 파일 | 화면 |
|---|---|
| `07_travelogue_publish.html` | 여행기 발행 (제목/본문/사진/태그) |
| `08_travelogue_detail.html` | 여행기 상세 + Deep Copy(내 일정으로 가져오기) |

### 🏠 진입 & 프로필
| 파일 | 화면 |
|---|---|
| `01_home.html` | 홈 (보관함 중심으로 갱신) |
| `15_profile.html` | 프로필 + 내 취향 프로파일 |

> `index.html` = 전체 허브. 상단에 정체성/핵심루프 컨셉 섹션(팀원 이해도용) + 4그룹 카드 그리드(iframe 썸네일, 클릭 시 새 탭).

---

## 4. Sample Data (실제 같이 보이게)

### 페르소나
- **jiwon** (28세, 동남아 4회 history: 발리/다낭/코타키나발루/푸켓)
- 보관함→정리 시나리오: 오사카 3박4일 / 친구와 / 널널하게 — 유튜브 동선 1 + 인스타 스샷 장소 + 텍스트 카페
- AI 폼 시나리오: 도쿄 4박5일 / 부모님과 / 널널하게 / 문화·역사+먹방 → 12 슬롯

### 도쿄 슬롯 (AI 결과 화면용)
- **Day1**: 메이지 신궁(시부야권) · 규카츠 모토무라(하라주쿠권) · 도쿄 국립박물관(우에노권) · 이세 스이요시(신주쿠권)
- **Day2**(다른 권역): 죠몬 롯폰기 · teamLab(오다이바권) · Tsukishima Monja · 미쓰비시1호관미술관 · 교파오 교자

### 추천 코멘트 톤 ("<매력>+<포인트>")
- "자연 속 고요한 휴식, 산책로와 신사 본전 방문"
- "바삭한 튀김옷의 돈카츠 정식, 부모님 모시기 좋은 자리"

---

## 5. Technical Spec
- Static HTML, JS 최소(탭 전환 정도). CDN = Pretendard.
- 이미지 = Unsplash 실사 + `onerror` 이모지 fallback. 지도 = 정적 placeholder.
- 반응형 max-width 420px 고정 (데스크탑에서도 모바일 폭).
- 배포: GitHub Pages (repo `triplog-ui-mock`, main branch root).

## 6. 캡처 가이드 (PPT 삽입용)
1. HTML 열기 → DevTools → device toolbar → iPhone 14 Pro
2. Ctrl+Shift+P → "Capture full size screenshot" → PNG → 슬라이드 삽입
