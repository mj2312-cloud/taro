# 오늘의 타로 (Taro) — Design System & Style Guide

> **Warm paper, quiet serif, personal ritual.**  
> Steep의 에디토리얼 미니멀리즘(따뜻한 종이 질감의 화이트 캔버스, Regular 웨이트 세리프 헤드라인, 97% 무채색 위 단 하나의 블러시 피치 포인트, 절제된 플로팅 아티팩트)을 **모바일 퍼스트 타로카드 웹서비스**(`TARO_01~04`)에 맞게 재해석한 디자인 가이드입니다.

**Theme:** Light (단일 라이트 테마 — 종이 카드와 타로 리딩의 집중감을 위한 밝은 아티팩트 중심 시스템)  
**Target Stack:** Tailwind CSS v4 (`@theme` CSS-first) · shadcn/ui (React 19 + CVA + Radix Primitives)

---

## 1. 디자인 철학 및 서비스 맥락

### 에디토리얼 타로 아티팩트 (Editorial Tarot Artifact)
기존 타로 서비스의 어두운 신비주의(보라·검정·네온 그라데이션)를 따르지 않습니다. **오늘의 타로**는 쉬는 시간 1분 안에 링크 하나로 열어보는 가벼운 개인적 리추얼이자, 친구에게 공유하고 싶은 **한 장의 종이 인쇄물**처럼 느껴져야 합니다.

- **종이 위의 세리프:** 굵은 볼드체로 외치지 않고, Weight `400`(Regular) 세리프 헤드라인과 정제된 산세리프 본문의 대비로 조용한 권위와 여운을 만듭니다.
- **절제된 단일 온기(Warmth Rationing):** 전체 UI의 97%는 `#ffffff`·`#f2f2f3`·`#17191c` 무채색으로 비우고, 오직 **블러시 피치(`#fbe1d1`)와 시에나 브라운(`#5d2a1a`)** 한 쌍만 화면당 최대 1회(타로 결과 핵심 강조 또는 카드 봉인 포인트) 사용해 종이 위 압인(letterpress) 같은 온기를 줍니다.
- **떠 있는 타로 카드 아티팩트:** 대시보드나 장식적 프레임 대신, 타로 카드 뒷면(S-01)과 맞춤 해석 결과 카드(S-03)가 화이트 캔버스 위에 **둥근 모서리(`20px`)와 10% 이하의 미세한 그림자**를 지닌 독립 오브제로 떠 있습니다.
- **신뢰와 투명성:** 개인정보를 서버·브라우저 저장소에 남기지 않는다는 원칙(PRD 원칙 1)을 불안한 경고창이 아닌 **차분한 미스트 그레이(`#f2f2f3`) 인라인 안내 블록**으로 자연스럽게 전달합니다.

---

## 2. 디자인 토큰 계층 (Token Hierarchy)

`tailwind-design-system` 패턴에 따라 토큰을 **3단계 계층**으로 분리합니다. 컴포넌트 내부에서는 원시 색상명(`#17191c`) 대신 시맨틱 토큰(`bg-primary`, `text-muted-foreground`)을 사용합니다.

```text
Brand Tokens (Steep 원시 팔레트 · 서체 · 간격)
  └── Semantic Tokens (목적 기반: --color-background, --color-primary, --color-accent ...)
        └── Component Tokens (CVA variants: Pill Button, Tarot Artifact Card, Topic Chip ...)
```

---

## 3. 색상 시스템 (Tokens — Colors)

### 3.1 브랜드 컬러 팔레트

| Name | Hex | OKLCH (근사치) | Token | 타로 서비스 내 역할 |
|---|---|---|---|---|
| **Ink Black** | `#17191c` | `oklch(21% 0.006 264)` | `--color-ink-black` | 기본 본문 텍스트, 채움(Filled) Pill CTA, 선택된 주제 칩 배경, 서비스 타이틀 |
| **Paper White** | `#ffffff` | `oklch(100% 0 0)` | `--color-paper-white` | 기본 페이지 캔버스, 플로팅 타로 결과 카드 표면, 입력창 배경, 저장 PNG 배경 |
| **Mist Gray** | `#f2f2f3` | `oklch(96% 0.002 264)` | `--color-mist-gray` | 중립 카드(Neutral Card), 개인정보 비저장 안내 블록, 비선택 주제 칩 배경 |
| **Fog White** | `#fafafb` | `oklch(98.5% 0.002 264)` | `--color-fog-white` | S-01/S-03 섹션 배경 밴드, 데스크톱 뷰포트 외곽 배경, 미세 호버 표면 |
| **Slate Gray** | `#777b86` | `oklch(58% 0.018 264)` | `--color-slate-gray` | 보조 설명 텍스트(S-01 서브 카피), 오락용 안내 및 개인정보 폐기 보조 문구 |
| **Ash Gray** | `#979799` | `oklch(68% 0.003 264)` | `--color-ash-gray` | 카테고리·메타 태그(`XIX 태양 · ♌ 사자자리` 보조 구분, `※ 오락용 타로 해석입니다`) |
| **Smoke Gray** | `#a3a6af` | `oklch(72% 0.015 264)` | `--color-smoke-gray` | 입력창 Placeholder(`지민`, `2004.08.03`, `경영학과`), 비활성 버튼 텍스트 |
| **Hairline Border** | `#ececec` | `oklch(94% 0 0)` | `--color-hairline` | 입력창 테두리, 아티팩트 카드 내부 1px 구분선, 주제 칩 미세 경계선 |
| **Blush Peach** | `#fbe1d1` | `oklch(91.5% 0.042 42)` | `--color-blush-peach` | **유일한 유채색 표면.** 별자리·해석 강조 뱃지/띠, S-03 맞춤 해석 포인트 카드(화면당 최대 1회) |
| **Sienna Brown** | `#5d2a1a` | `oklch(34% 0.088 34)` | `--color-sienna-brown` | Blush Peach 위의 텍스트 및 라인 아트워크/별자리 심볼 잉크 (화이트 배경 본문에는 미사용) |
| **Error Terracotta** | `#b42318` | `oklch(48% 0.175 27)` | `--color-error-terracotta` | 생년월일 검증 오류(`날짜를 다시 확인해 주세요`) 등 최소한의 오류 피드백 전용 |

### 3.2 shadcn/ui 시맨틱 컬러 매핑

| Semantic Token | Light Value | 용도 |
|---|---|---|
| `--color-background` | `var(--color-paper-white)` (`#ffffff`) | 페이지 기본 배경 |
| `--color-foreground` | `var(--color-ink-black)` (`#17191c`) | 기본 텍스트 |
| `--color-card` | `var(--color-paper-white)` (`#ffffff`) | 플로팅 타로 결과 카드 (`TarotArtifactCard`) |
| `--color-card-foreground` | `var(--color-ink-black)` (`#17191c`) | 카드 내부 기본 텍스트 |
| `--color-secondary` | `var(--color-mist-gray)` (`#f2f2f3`) | 중립 정보 카드, 비선택 칩, 개인정보 안내 박스 |
| `--color-secondary-foreground` | `var(--color-ink-black)` (`#17191c`) | 세컨더리 표면 위 텍스트 |
| `--color-muted` | `var(--color-fog-white)` (`#fafafb`) | 보조 영역, 비활성/여백 밴드 |
| `--color-muted-foreground` | `var(--color-slate-gray)` (`#777b86`) | 안내 문구, 부가 설명 |
| `--color-primary` | `var(--color-ink-black)` (`#17191c`) | 주요 액션 버튼(`타로 시작`, `카드 한 장 뽑기`, `이미지 저장`) |
| `--color-primary-foreground` | `var(--color-paper-white)` (`#ffffff`) | 주요 버튼 텍스트 |
| `--color-accent` | `var(--color-blush-peach)` (`#fbe1d1`) | 타로 에디토리얼 포인트 영역 |
| `--color-accent-foreground` | `var(--color-sienna-brown)` (`#5d2a1a`) | 포인트 영역 위 텍스트 및 심볼 |
| `--color-destructive` | `var(--color-error-terracotta)` (`#b42318`) | 날짜 형식 오류 등 폼 유효성 에러 |
| `--color-destructive-foreground` | `var(--color-paper-white)` (`#ffffff`) | 에러 배지/토스트 전경색 |
| `--color-border` | `var(--color-hairline)` (`#ececec`) | 1px 헤어라인 경계 |
| `--color-input` | `var(--color-hairline)` (`#ececec`) | Input 필드 기본 테두리 |
| `--color-ring` | `var(--color-ink-black)` (`#17191c`) | 키보드 포커스 링 (`focus-visible`) |

---

## 4. 타이포그래피 (Tokens — Typography)

Steep의 핵심 문법은 **"Regular(`400`) 세리프 헤드라인 + 미세한 웨이트 단계의 산세리프 본문"**입니다.  
한국어 모바일 환경에서 `Signifier`와 `Sohne`의 라틴/로마 숫자 조형을 유지하면서 한글 가독성을 확보하기 위해 한글 폴백 스택을 지정합니다.

### 4.1 서체 스택 (Font Families)

- **Display / Headline Serif (`--font-serif` / `--font-signifier`):**
  `'Signifier', 'Noto Serif KR', 'Nanum Myeongjo', Georgia, ui-serif, serif`
  - **역할:** S-01 메인 타이틀(`오늘의 타로`), S-02 페이지 질문 헤드라인, S-03 카드명(`XIX 태양`)
  - **시그니처 규칙:** 모든 크기에서 **Weight `400` 고정**. 절대 `600`이나 `700`을 쓰지 않습니다.
- **Body / UI Sans (`--font-sans` / `--font-sohne`):**
  `'Sohne', 'Pretendard Variable', 'Pretendard', -apple-system, BlinkMacSystemFont, system-ui, sans-serif`
  - **역할:** 입력 라벨, 6종 주제 칩, **맞춤 해석 3줄(90자 이내)**, 버튼, 개인정보·오락용 안내문
  - **웨이트 규칙:** `400`(기본), `430~450`(리딩 본문/라벨 강조), `500`(카드 타이틀/버튼/메트릭 강조)의 좁은 간격으로 굵기 위계를 나눕니다. (한글 폰트 폴백 시 `430`/`450`은 `400` 또는 variable `font-variation-settings: "wght" 450`으로 대응)

### 4.2 모바일 퍼스트 타입 스케일 (Mobile-First Type Scale)

레퍼런스(데스크톱 90px/64px/44px)를 모바일 375px–430px 화면 및 1080×1350 저장 이미지에 맞춰 정규화했습니다.

| Role | Family | Weight | Size (Mobile / Export) | Line Height | Letter Spacing | Token | 타로 사용처 |
|---|---|---|---|---|---|---|---|
| **micro** | Sans | 400 | `13px` / `28px` | `1.45` | `0` | `--text-micro` | `※ 오락용 타로 해석입니다`, 개인정보 폐기 문구 |
| **caption** | Sans | 400 | `14px` / `32px` | `1.50` | `0` | `--text-caption` | 입력 도움말, 에러 문구, `{이름}님의 타로 · {주제}` |
| **ui** | Sans | 400–450 | `16px` / `36px` | `1.35` | `0` | `--text-ui` | 버튼 텍스트, 입력창 텍스트, 주제 칩 라벨 |
| **body** | Sans | 430 | `17px` / `40px` | `1.50` | `-0.01em` | `--text-body` | **S-03 맞춤 해석 3줄(90자 이내)**, S-01 서브 카피 |
| **subheading** | Sans | 500 | `20px` / `46px` | `1.35` | `-0.012em` | `--text-subheading` | 카드 섹션 소제목, S-02 `나에 대해 알려 주세요` |
| **heading** | Serif | **400** | `28px` / `60px` | `1.30` | `-0.015em` | `--text-heading` | S-03 타로 카드명 (`XIX 태양 · ♌ 사자자리`) |
| **heading-lg** | Serif | **400** | `36px` / — | `1.28` | `-0.02em` | `--text-heading-lg` | S-02 메인 인트로, 결과 강조 타이틀 |
| **display** | Serif | **400** | `44px` (`md: 64px`) | `1.25` | `-0.025em` | `--text-display` | S-01 히어로 타이틀 (`오늘의 타로`) |

> **한글 조판 규칙:** 해석 3줄과 헤드라인에는 `word-break: keep-all; overflow-wrap: break-word;`를 기본 적용해 어절 중간 줄바꿈을 방지하고, 로마 숫자(`XIX`)와 생년월일(`2004.08.03`)에는 `font-variant-numeric: tabular-nums`를 적용합니다.

---

## 5. 간격 · 형태 · 그림자 (Spacing, Shapes & Elevation)

### 5.1 간격 체계 (4px Base Unit)

| Token | Value | 주요 사용처 |
|---|---|---|
| `--spacing-4` | `4px` | 라벨-도움말 간격, 아이콘-텍스트 미세 여백 |
| `--spacing-8` | `8px` | 버튼 그룹(2×2 그리드) 열 간격, 칩 내부 간격 |
| `--spacing-12` | `12px` | 주제 칩 그리드 간격, 입력 필드 간 세로 간격 |
| `--spacing-16` | `16px` | 입력창 내부 패딩, 개인정보 안내 박스 패딩 |
| `--spacing-20` | `20px` | 모바일 화면 좌우 여백(`px-5`), 카드 기본 패딩 |
| `--spacing-24` | `24px` | 폼 섹션 간격, S-03 카드 내부 요소 블록 간격 |
| `--spacing-32` | `32px` | S-01 히어로 카드와 CTA 사이 여백, 카드 내부 상하 여백 |
| `--spacing-40` | `40px` | S-01 상단/하단 호흡 여백 |
| `--spacing-64` | `64px` | 섹션 분리 여백 (모바일 기준) |
| `--spacing-80` | `80px` | 데스크톱 뷰포트 섹션 간격 |

### 5.2 모서리 곡률 (Border Radius)

Steep의 두 가지 구조적 곡률인 **Pill(`9999px`)**과 **Soft Card(`24px`)**를 엄격히 유지합니다.

| 요소 | Radius | Tailwind Class | 비고 |
|---|---|---|---|
| **모든 버튼 (CTA / 보조 액션)** | `9999px` | `rounded-full` | 9999px 미만 곡률 사용 금지 |
| **콘텐츠 / 개인정보 / 강조 카드** | `24px` | `rounded-3xl` | Neutral Card, Accent Peach Card, Dialog |
| **플로팅 타로 카드 / 결과 카드** | `20px` | `rounded-[20px]` | `Elevated White` 아티팩트 전용 |
| **입력창 / 안내 박스 / 주제 칩** | `16px` | `rounded-2xl` | 6종 주제 선택 타일, Text Input |
| **카드 내부 아트워크 프레임** | `12px` | `rounded-xl` | 22장 메이저 아르카나 일러스트 영역 |

### 5.3 고도 및 표면 계층 (Surfaces & Elevation)

| Level | Name | Background | Shadow | 타로 화면 적용 예 |
|---|---|---|---|---|
| **0** | Canvas | `#ffffff` | 없음 | S-01, S-02, S-03 기본 바닥면 |
| **1** | Card Mist | `#f2f2f3` | 없음 | 개인정보 비저장 안내 박스, 비선택 주제 칩 |
| **2** | Section Fog | `#fafafb` | 없음 | 데스크톱 외곽 배경, S-03 하단 액션 영역 미세 밴드 |
| **3** | Accent Blush | `#fbe1d1` | 없음 | S-03 별자리·맞춤 포인트 강조 배지/카드 (페이지당 최대 1회) |
| **4** | Elevated Artifact | `#ffffff` | `--shadow-subtle-3` | **S-01 카드 뒷면 아티팩트**, **S-03 타로 결과 카드** |
| **5** | Overlay / Modal | `#ffffff` | `--shadow-subtle-2` | 인앱 브라우저 저장 폴백 모달, 복사 폴백 시트, 토스트 |

```css
--shadow-subtle: oklab(0 0 0 / 0.05) 0px 0px 0px 1px, rgba(0, 0, 0, 0.08) 0px 4px 24px 0px;
--shadow-subtle-2: oklab(0 0 0 / 0.05) 0px 0px 0px 1px, rgba(0, 0, 0, 0.1) 0px 8px 40px 0px;
--shadow-subtle-3: rgba(4, 23, 43, 0.05) 0px 0px 0px 1px, rgba(0, 0, 0, 0.1) 0px 20px 25px -5px, rgba(0, 0, 0, 0.1) 0px 8px 10px -6px;
```

---

## 6. 레이아웃 & 화면별 규격 (S-01 ~ S-03)

### 6.1 모바일 퍼스트 프레임
- **앱 컨테이너:** `mx-auto min-h-dvh w-full max-w-[440px] bg-background px-5 pt-6 pb-[calc(24px+env(safe-area-inset-bottom))]`
- **데스크톱 대응:** `440px` 초과 뷰포트에서는 외부 배경을 `--color-fog-white`(`#fafafb`)로 깔고 중앙 `440px` 종이 캔버스만 `--color-paper-white`(`#ffffff`) 및 미세 1px 헤어라인(`border-border/60`)으로 띄워 잡지 지면처럼 정돈합니다.
- **터치 타겟:** 모바일 한 손 조작을 고려해 모든 클릭 가능한 버튼·칩·입력창 높이는 최소 **`44px` (`h-11` ~ `h-12`)** 이상을 유지합니다.

### 6.2 화면별 시각 구조 (TARO_04 와이어프레임 연동)

1. **S-01 시작 (`F-1`)**
   - **상단:** 로고/서비스명 `오늘의 타로` (Display/Heading Serif `400`, 중앙 정렬)
   - **중앙 플로팅 아티팩트:** 카드 뒷면 일러스트 카드 (`20px` radius, `--shadow-subtle-3`, 2:3 비율 약 `180px × 270px`, 중앙에 `#5d2a1a` 시에나 브라운 또는 `#17191c` 미세 선형 천체 문양 + 작은 `#fbe1d1` 블러시 피치 인장 포인트)
   - **카피:** `"내 정보를 넣고 카드 한 장으로 나만의 해석을 받아 봐요"` (`17px` Sans, `#777b86`)
   - **하단 CTA:** `[타로 시작]` Filled Pill (`w-full h-12`, `#17191c` 배경) + 하단 마이크로 안내 `"※ 오락용 타로 · 정보는 저장 안 해요"` (`13px`, `#979799`)

2. **S-02 내 정보 · 주제 (`F-2`)**
   - **상단 내비게이션:** 배경·테두리 없는 투명 바 (`h-12`), 좌측 `← 나에 대해 알려 주세요` (뒤로가기 + Sans `16px` weight `450`)
   - **입력 폼 (`space-y-5`):**
     - `이름 (닉네임 OK)` — 최대 10자 (`maxLength={10}`), placeholder `"지민"`
     - `생년월일` — `YYYY.MM.DD` 숫자 입력/포맷팅 (`inputMode="numeric"`), 유효하지 않은 날짜 입력 시 하단에 `"날짜를 다시 확인해 주세요"` (`role="alert"`)
     - `전공 (선택)` — 선택 입력, placeholder `"경영학과"`
   - **주제 선택 (`어떤 주제가 궁금한가요?`):**
     - 6종 칩 3×2 또는 2×3 그리드 (`grid-cols-2 sm:grid-cols-3 gap-2.5`):
       `💼 커리어` · `💰 재물` · `💕 연애` · `🌿 건강` · `📚 학업` · `✨ 종합`
   - **개인정보 안내 Callout:** `bg-secondary` (`#f2f2f3`), `rounded-2xl`, `p-4`, 자물쇠 아이콘(`16px`, `#777b86`) + `"입력한 정보는 해석을 만드는 데만 쓰고 저장하지 않아요. 창을 닫거나 [처음으로]를 누르면 바로 지워져요."`
   - **하단 CTA:** `[카드 한 장 뽑기]` Filled Pill (`w-full h-12`). 이름·유효한 생년월일·주제가 모두 채워지기 전에는 `disabled` 상태(opacity 0.35 또는 `#f2f2f3` 배경 / `#a3a6af` 텍스트).

3. **S-03 맞춤 해석 결과 (`F-3 ~ F-7`)**
   - **카드 뽑기 전환 (`F-3`):** `[카드 한 장 뽑기]` 클릭 후 **700ms**(`1초 이내`) 3D Y축 뒤집기 모션으로 정방향 메이저 아르카나 카드 공개. (`prefers-reduced-motion: reduce` 시 200ms 크로스페이드)
   - **플로팅 결과 카드 (저장 이미지 원본 영역):**
     - Surface: `Elevated White` (`#ffffff`, `rounded-[20px]`, `--shadow-subtle-3`, `p-6`)
     - 상단 메타: `{이름}님의 타로 · {주제}` (`14px` Sans, `#777b86`)
     - 중앙 카드 그림: `12px` radius 아트워크 (`aspect-[3/4]`, max-w `168px` centered, `#fafafb` 또는 `#fbe1d1`의 15% 워시 배경 위 정교한 라인/에디토리얼 일러스트)
     - 카드명 & 별자리 라인: `XIX 태양` (Serif `400` `28px`, `#17191c`) + `♌ 사자자리` (**화면 내 유일한 Blush Peach `#fbe1d1` 배경 / Sienna Brown `#5d2a1a` 텍스트 필 배지**, `rounded-full px-3 py-1 text-[13px]`)
     - **맞춤 해석 3줄 (90자 이내):** Sans `17px`, weight `430`, line-height `1.55`, `#17191c`, 중앙 또는 단정한 에디토리얼 좌측 정렬
     - 하단 각주: `※ 오락용 타로 해석입니다` (`13px`, `#979799`)
   - **액션 버튼 페어링 (`gap-2.5`):**
     - Primary Pair (`grid-cols-2 gap-2.5`): `[이미지 저장]` (Filled Pill) + `[결과 공유]` (Ghost Pill)
     - Secondary Pair (`grid-cols-2 gap-2.5`): `[다시 뽑기]` (Secondary Mist Pill) + `[처음으로]` (Text Link `처음으로 →` 또는 Ghost Subtle)
   - **하단 개인정보 폐기 안내:** `"🔒 정보는 저장되지 않아요. [처음으로]를 누르면 바로 지워져요."` (`13px`, `#777b86`)

4. **저장용 PNG 이미지 (`1080 × 1350px`, 4:5 세로 비율, `F-5`)**
   - S-03 플로팅 결과 카드를 `#ffffff` 종이 캔버스 여백(`padding: 72px`) 위에 인쇄물처럼 렌더링합니다.
   - **필수 포함:** `{이름}님의 타로 · {주제}`, 카드 그림, `{카드 이름} · {별자리}`, 해석 3줄, `※ 오락용 타로 해석입니다`, 최하단 서비스명(`오늘의 타로`).
   - **절대 제외 (원칙 2):** 생년월일 원문(`2004.08.03`)과 전공 원문(`경영학과`)은 이미지와 공유 문구에 절대 넣지 않습니다.

---

## 7. 컴포넌트 규칙 (Tailwind v4 + shadcn/ui + CVA)

모든 UI 컴포넌트는 **React 19** 규약(`forwardRef` 미사용, `ref`를 일반 prop으로 전달), **`class-variance-authority` (CVA)** 변형 정의, **`cn()` 유틸리티**, 그리고 **WCAG 접근성 속성(`aria-invalid`, `aria-pressed`, `focus-visible`)**을 따릅니다.

### 7.1 공통 유틸리티 (`lib/utils.ts`)

```ts
import { type ClassValue, clsx } from 'clsx'
import { twMerge } from 'tailwind-merge'

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs))
}

export const focusRing = cn(
  'focus-visible:outline-none focus-visible:ring-2',
  'focus-visible:ring-ring focus-visible:ring-offset-2 focus-visible:ring-offset-background'
)

export const disabledState = 'disabled:pointer-events-none disabled:opacity-40'
```

### 7.2 Pill Button (`components/ui/button.tsx`)

Steep의 핵심 액션 형태인 `rounded-full`(`9999px`)을 모든 버튼에 강제합니다. Filled 버튼은 항상 Ghost 버튼과 같은 베이스라인에 짝지어 배치합니다.

| Variant | 스타일 | 타로 서비스 사용처 |
|---|---|---|
| `default` (Filled Pill) | `bg-primary text-primary-foreground border border-transparent hover:bg-primary/90` | `[타로 시작]`, `[카드 한 장 뽑기]`, `[이미지 저장]` |
| `outline` (Ghost Pill) | `bg-transparent text-foreground border border-foreground hover:bg-muted` | `[결과 공유]` (이미지 저장과 페어링) |
| `secondary` (Mist Pill) | `bg-secondary text-secondary-foreground hover:bg-secondary/80` | `[다시 뽑기]` |
| `accent` (Blush Pill) | `bg-accent text-accent-foreground hover:bg-accent/85` | (선택적) 특별 강조 뱃지형 버튼 |
| `link` (Text Link + `→`) | `bg-transparent text-foreground rounded-none px-0 hover:underline underline-offset-4` | `[처음으로 →]`, 상단 `← 뒤로` |

```tsx
import { Slot } from '@radix-ui/react-slot'
import { cva, type VariantProps } from 'class-variance-authority'
import { cn, disabledState, focusRing } from '@/lib/utils'

const buttonVariants = cva(
  cn(
    'inline-flex items-center justify-center gap-2 whitespace-nowrap rounded-full text-[16px] font-normal leading-none transition-colors cursor-pointer select-none',
    focusRing,
    disabledState
  ),
  {
    variants: {
      variant: {
        default: 'bg-primary text-primary-foreground border border-primary hover:bg-primary/90',
        outline: 'bg-transparent text-foreground border border-foreground hover:bg-muted',
        secondary: 'bg-secondary text-secondary-foreground border border-transparent hover:bg-secondary/80',
        accent: 'bg-accent text-accent-foreground border border-transparent hover:bg-accent/85',
        ghost: 'bg-transparent text-foreground hover:bg-muted',
        link: 'rounded-none px-0 text-foreground underline-offset-4 hover:underline',
      },
      size: {
        sm: 'h-9 px-4 text-[14px]',
        default: 'h-11 px-5',
        lg: 'h-12 px-6',
        full: 'h-12 w-full px-6',
        icon: 'size-10',
      },
    },
    defaultVariants: {
      variant: 'default',
      size: 'default',
    },
  }
)

export interface ButtonProps
  extends React.ButtonHTMLAttributes<HTMLButtonElement>,
    VariantProps<typeof buttonVariants> {
  asChild?: boolean
  ref?: React.Ref<HTMLButtonElement>
}

export function Button({
  className,
  variant,
  size,
  asChild = false,
  ref,
  ...props
}: ButtonProps) {
  const Comp = asChild ? Slot : 'button'
  return (
    <Comp
      ref={ref}
      className={cn(buttonVariants({ variant, size }), className)}
      {...props}
    />
  )
}
```

### 7.3 Card (`components/ui/card.tsx`)

Steep의 카드 3원칙을 `variant`로 분리합니다:
- **`neutral` (`#f2f2f3`, `rounded-3xl`, 그림자 없음):** 개인정보 안내, 부가 설명 블록
- **`accent` (`#fbe1d1`/`#5d2a1a`, `rounded-3xl`, 그림자 없음):** 에디토리얼 포인트 카드 (**화면당 최대 1회**, 반드시 `#ffffff` 또는 `#fafafb` 위에만 배치)
- **`artifact` (`#ffffff`, `rounded-[20px]`, `--shadow-subtle-3`):** **유일하게 그림자를 갖는 표면.** 타로 카드 뒷면(S-01) 및 맞춤 해석 결과 카드(S-03) 전용

```tsx
import { cva, type VariantProps } from 'class-variance-authority'
import { cn } from '@/lib/utils'

const cardVariants = cva('transition-colors', {
  variants: {
    variant: {
      neutral: 'rounded-3xl bg-secondary text-secondary-foreground',
      accent: 'rounded-3xl bg-accent text-accent-foreground',
      artifact: 'rounded-[20px] bg-card text-card-foreground shadow-subtle-3',
    },
    padding: {
      none: 'p-0',
      sm: 'p-4',
      md: 'p-5',
      lg: 'p-6',
    },
  },
  defaultVariants: {
    variant: 'neutral',
    padding: 'md',
  },
})

export interface CardProps
  extends React.HTMLAttributes<HTMLDivElement>,
    VariantProps<typeof cardVariants> {
  ref?: React.Ref<HTMLDivElement>
}

export function Card({ className, variant, padding, ref, ...props }: CardProps) {
  return (
    <div
      ref={ref}
      className={cn(cardVariants({ variant, padding }), className)}
      {...props}
    />
  )
}

export function CardHeader({
  className,
  ref,
  ...props
}: React.HTMLAttributes<HTMLDivElement> & { ref?: React.Ref<HTMLDivElement> }) {
  return <div ref={ref} className={cn('flex flex-col space-y-1.5', className)} {...props} />
}

export function CardTitle({
  className,
  ref,
  ...props
}: React.HTMLAttributes<HTMLHeadingElement> & { ref?: React.Ref<HTMLHeadingElement> }) {
  return (
    <h2
      ref={ref}
      className={cn('font-serif text-[28px] font-normal leading-[1.3] tracking-[-0.015em]', className)}
      {...props}
    />
  )
}

export function CardDescription({
  className,
  ref,
  ...props
}: React.HTMLAttributes<HTMLParagraphElement> & { ref?: React.Ref<HTMLParagraphElement> }) {
  return <p ref={ref} className={cn('text-[14px] text-muted-foreground', className)} {...props} />
}

export function CardContent({
  className,
  ref,
  ...props
}: React.HTMLAttributes<HTMLDivElement> & { ref?: React.Ref<HTMLDivElement> }) {
  return <div ref={ref} className={cn('pt-4', className)} {...props} />
}

export function CardFooter({
  className,
  ref,
  ...props
}: React.HTMLAttributes<HTMLDivElement> & { ref?: React.Ref<HTMLDivElement> }) {
  return <div ref={ref} className={cn('flex items-center pt-4', className)} {...props} />
}
```

### 7.4 Form Input & Label (`components/ui/input.tsx`)

- `rounded-2xl`(`16px`), 높이 `h-12`(`48px`), `1px solid #ececec` 헤어라인 테두리, placeholder `#a3a6af`
- 에러 발생 시(`날짜를 다시 확인해 주세요`) `aria-invalid="true"`와 `role="alert"`를 연결하고 테두리를 `--color-destructive`로 전환합니다.

```tsx
import { cn, disabledState, focusRing } from '@/lib/utils'

export interface InputProps extends React.InputHTMLAttributes<HTMLInputElement> {
  error?: string
  ref?: React.Ref<HTMLInputElement>
}

export function Input({ className, type = 'text', error, id, ref, ...props }: InputProps) {
  return (
    <div className="w-full space-y-1.5">
      <input
        id={id}
        type={type}
        ref={ref}
        aria-invalid={!!error}
        aria-describedby={error && id ? `${id}-error` : undefined}
        className={cn(
          'flex h-12 w-full rounded-2xl border border-input bg-background px-4 text-[16px] text-foreground placeholder:text-smoke-gray transition-colors',
          focusRing,
          disabledState,
          error && 'border-destructive focus-visible:ring-destructive',
          className
        )}
        {...props}
      />
      {error && (
        <p id={id ? `${id}-error` : undefined} className="px-1 text-[13px] text-destructive" role="alert">
          {error}
        </p>
      )}
    </div>
  )
}
```

### 7.5 주제 선택 칩 (`components/tarot/topic-chip.tsx`)

- 6종 주제(`커리어`, `재물`, `연애`, `건강`, `학업`, `종합`) 중 단일 선택 (`role="radiogroup"` / `role="radio"`, `aria-checked`).
- **비선택:** `bg-secondary text-foreground border border-transparent` (`#f2f2f3`)
- **선택:** `bg-primary text-primary-foreground border border-primary` (`#17191c` 솔리드 로젠지) — 97% 무채색 원칙에 따라 칩 선택에도 파랑/보라 대신 Ink Black을 사용합니다.

```tsx
import { cn, focusRing } from '@/lib/utils'

export function TopicChip({
  selected,
  icon,
  label,
  className,
  ref,
  ...props
}: React.ButtonHTMLAttributes<HTMLButtonElement> & {
  selected: boolean
  icon: string
  label: string
  ref?: React.Ref<HTMLButtonElement>
}) {
  return (
    <button
      ref={ref}
      type="button"
      role="radio"
      aria-checked={selected}
      className={cn(
        'flex h-12 w-full items-center justify-center gap-1.5 rounded-2xl px-3 text-[15px] font-normal transition-colors cursor-pointer',
        focusRing,
        selected
          ? 'bg-primary text-primary-foreground'
          : 'bg-secondary text-foreground hover:bg-mist-gray/80',
        className
      )}
      {...props}
    >
      <span aria-hidden="true">{icon}</span>
      <span>{label}</span>
    </button>
  )
}
```

### 7.6 개인정보 안내 블록 (`PrivacyNotice`) & 태그

- **개인정보 안내 (`F-2`, `F-4`):**
  - `bg-secondary` (`#f2f2f3`), `rounded-2xl`, `px-4 py-3.5`, `text-[13px] leading-[1.5] text-muted-foreground`
  - 자물쇠 아이콘(`size-4 shrink-0 text-slate-gray`)을 좌측 상단에 정렬하고, **절대 과장된 방패·경고 색상(초록/빨강/노랑 배너)을 쓰지 않습니다.**
- **카테고리/메타 태그:**
  - 별자리 배지 1개에만 Blush Peach(`bg-accent text-accent-foreground rounded-full px-3 py-1 text-[13px]`)를 허용하고, 나머지 라벨(`커리어`, `※ 오락용 타로 해석입니다`)은 배경과 테두리가 없는 순수 텍스트(`text-ash-gray text-[13px]`)로 처리합니다.

### 7.7 카드 뒤집기 모션 & 폴백 다이얼로그 / 토스트

- **카드 뒤집기 (`F-3`):**
  - 요구사항인 **"1초 이내"**를 준수해 **`700ms cubic-bezier(0.22, 1, 0.36, 1)`** 3D Y-축 플립을 적용합니다.
  - `perspective: 1000px`, 앞면/뒷면 모두 `backface-visibility: hidden` 적용.
- **인앱 브라우저 폴백 모달 (`F-5`, `F-6`):**
  - `Dialog` 오버레이: `bg-ink-black/40 backdrop-blur-[2px]`
  - `DialogContent`: `rounded-3xl bg-background p-6 shadow-subtle-2 max-w-[380px] w-[calc(100%-32px)]`
  - **이미지 저장 실패 폴백:** 생성된 `1080×1350` PNG를 `rounded-xl border border-border`로 띄우고 상/하단에 `"길게 눌러 저장해 주세요"` 안내.
  - **복사 실패 폴백:** 공유 메시지 전문을 `select-all bg-secondary rounded-2xl p-4 text-[14px]` 박스로 띄우고 `"아래 글을 길게 눌러 복사해 주세요"` 안내.
- **상태 토스트 (`이미지를 저장했어요`, `결과를 복사했어요. 원하는 앱에 붙여 넣어 주세요`):**
  - 하단 플로팅 Pill (`rounded-full bg-primary text-primary-foreground px-5 py-3 text-[14px] shadow-subtle`), 2.5초 후 자동 소멸 (`role="status"`).

---

## 8. Do's and Don'ts (타로 서비스 전용 수칙)

### Do
- **세리프 Regular(`400`) 고수:** 카드명(`XIX 태양`)과 히어로 타이틀(`오늘의 타로`)에는 반드시 Regular `400` 세리프를 사용하세요.
- **Blush Peach(`#fbe1d1`) 1회 제한:** `#fbe1d1`과 `#5d2a1a` 조합은 S-03의 별자리/포인트 요소처럼 **화면당 최대 1곳**에만 써서 에디토리얼 온기를 보존하세요.
- **모바일 버튼·카드 곡률 규칙:** 모든 버튼은 `rounded-full`(`9999px`), 콘텐츠·모달 카드는 `rounded-3xl`(`24px`), 플로팅 타로 결과 카드는 `rounded-[20px]`를 지키세요.
- **액션 버튼 페어링:** S-03의 `[이미지 저장]`(Filled)과 `[결과 공유]`(Ghost)처럼 주 액션과 보조 액션을 같은 행에 짝지어 배치하세요.
- **개인정보 노출 차단:** 결과 카드 DOM 및 `1080×1350` Export 캔버스에 생년월일과 전공 원문이 렌더링되지 않도록 컴포넌트 props 단계에서 분리하세요.

### Don't
- **신비주의 다크·네온 컬러 금지:** 보라색(`purple`), 금색 그라데이션(`gold gradient`), 밤하늘 별빛 배경 등을 도입하지 마세요.
- **중립 카드에 그림자 금지:** 개인정보 안내 박스(`#f2f2f3`)나 비선택 칩에는 그림자를 넣지 마세요. 그림자(`--shadow-subtle-3`)는 오직 플로팅 타로 카드와 모달만 가집니다.
- **세리프 볼드(`500+`) 사용 금지:** 한글 세리프라도 헤드라인에 굵은 폰트 웨이트를 주지 마세요.
- **Sienna Brown(`#5d2a1a`) 오용 금지:** `#5d2a1a`는 `#fbe1d1` 위에서만 쓰고, 흰색(`#ffffff`) 배경 위의 본문 텍스트로는 `#17191c`를 사용하세요.
- **인라인 링크 밑줄 기본 표시 금지:** `[처음으로 →]` 같은 텍스트 링크는 평상시 밑줄 없이 `→` 화살표로 동작을 암시하고 `:hover` 시에만 밑줄을 표시하세요.

---

## 9. Quick Start — CSS Tokens & Tailwind CSS v4 설정

프로젝트의 `src/app/globals.css` (또는 `app.css`)에 그대로 복사해 사용할 수 있는 **Tailwind CSS v4 CSS-first** 정의입니다.

```css
@import "tailwindcss";

/* 1. Brand & Semantic Tokens (@theme) */
@theme {
  /* --- Brand Palette (Steep Editorial Warm Paper) --- */
  --color-ink-black: #17191c;
  --color-paper-white: #ffffff;
  --color-mist-gray: #f2f2f3;
  --color-fog-white: #fafafb;
  --color-slate-gray: #777b86;
  --color-ash-gray: #979799;
  --color-smoke-gray: #a3a6af;
  --color-hairline: #ececec;
  --color-blush-peach: #fbe1d1;
  --color-sienna-brown: #5d2a1a;
  --color-error-terracotta: #b42318;

  /* --- Semantic Tokens (shadcn/ui v4) --- */
  --color-background: var(--color-paper-white);
  --color-foreground: var(--color-ink-black);

  --color-card: var(--color-paper-white);
  --color-card-foreground: var(--color-ink-black);

  --color-primary: var(--color-ink-black);
  --color-primary-foreground: var(--color-paper-white);

  --color-secondary: var(--color-mist-gray);
  --color-secondary-foreground: var(--color-ink-black);

  --color-muted: var(--color-fog-white);
  --color-muted-foreground: var(--color-slate-gray);

  --color-accent: var(--color-blush-peach);
  --color-accent-foreground: var(--color-sienna-brown);

  --color-destructive: var(--color-error-terracotta);
  --color-destructive-foreground: var(--color-paper-white);

  --color-border: var(--color-hairline);
  --color-input: var(--color-hairline);
  --color-ring: var(--color-ink-black);

  /* Semi-transparent wash for card artwork frame */
  --color-blush-wash: color-mix(in oklab, var(--color-blush-peach) 22%, var(--color-paper-white));

  /* --- Typography --- */
  --font-serif: 'Signifier', 'Noto Serif KR', 'Nanum Myeongjo', Georgia, ui-serif, serif;
  --font-sans: 'Sohne', 'Pretendard Variable', 'Pretendard', -apple-system, BlinkMacSystemFont, system-ui, sans-serif;

  /* Type Scale */
  --text-micro: 0.8125rem;      /* 13px */
  --text-caption: 0.875rem;     /* 14px */
  --text-ui: 1rem;              /* 16px */
  --text-body: 1.0625rem;       /* 17px */
  --text-subheading: 1.25rem;   /* 20px */
  --text-heading: 1.75rem;      /* 28px */
  --text-heading-lg: 2.25rem;   /* 36px */
  --text-display: 2.75rem;      /* 44px */

  /* --- Spacing (4px base unit) --- */
  --spacing-unit: 4px;
  --spacing-page-x: 20px;
  --spacing-section: 64px;

  /* --- Border Radius --- */
  --radius-xl: 12px;            /* 타로 카드 아트워크 */
  --radius-2xl: 16px;           /* 입력창 · 주제 칩 · 개인정보 박스 */
  --radius-artifact: 20px;      /* 플로팅 타로 결과 카드 */
  --radius-3xl: 24px;           /* 콘텐츠 카드 · 모달 */
  --radius-full: 9999px;        /* 모든 Pill 버튼 · 배지 */

  /* --- Shadows (Only for Floating Artifacts & Overlays) --- */
  --shadow-subtle: oklab(0 0 0 / 0.05) 0px 0px 0px 1px, rgba(0, 0, 0, 0.08) 0px 4px 24px 0px;
  --shadow-subtle-2: oklab(0 0 0 / 0.05) 0px 0px 0px 1px, rgba(0, 0, 0, 0.1) 0px 8px 40px 0px;
  --shadow-subtle-3: rgba(4, 23, 43, 0.05) 0px 0px 0px 1px, rgba(0, 0, 0, 0.1) 0px 20px 25px -5px, rgba(0, 0, 0, 0.1) 0px 8px 10px -6px;

  /* --- Animations (F-3 카드 뒤집기 1초 이내 & 모달/토스트) --- */
  --animate-card-flip: tarot-flip 0.7s cubic-bezier(0.22, 1, 0.36, 1) forwards;
  --animate-fade-in: fade-in 0.2s ease-out;
  --animate-dialog-in: dialog-fade-in 0.22s cubic-bezier(0.22, 1, 0.36, 1);

  @keyframes tarot-flip {
    0% {
      transform: perspective(1000px) rotateY(180deg) scale(0.96);
    }
    100% {
      transform: perspective(1000px) rotateY(0deg) scale(1);
    }
  }

  @keyframes fade-in {
    from { opacity: 0; }
    to { opacity: 1; }
  }

  @keyframes dialog-fade-in {
    from {
      opacity: 0;
      transform: scale(0.96) translateY(8px);
    }
    to {
      opacity: 1;
      transform: scale(1) translateY(0);
    }
  }
}

/* 2. Base Layer */
@layer base {
  * {
    @apply border-border;
  }

  html {
    font-family: var(--font-sans);
    background-color: var(--color-fog-white);
    color: var(--color-foreground);
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }

  body {
    @apply bg-fog-white text-foreground antialiased;
    word-break: keep-all;
    overflow-wrap: break-word;
  }

  h1, h2 {
    font-family: var(--font-serif);
    font-weight: 400;
  }
}

/* 3. Custom Utilities (@utility) */
@utility tabular-reading {
  font-variant-numeric: tabular-nums;
}

@utility card-perspective {
  perspective: 1000px;
  transform-style: preserve-3d;
}

@utility backface-hidden {
  backface-visibility: hidden;
  -webkit-backface-visibility: hidden;
}

@media (prefers-reduced-motion: reduce) {
  .animate-card-flip,
  .animate-dialog-in {
    animation-duration: 0.15s !important;
    transform: none !important;
  }
}
```
