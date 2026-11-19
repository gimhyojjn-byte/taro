# 오늘의 타로 디자인 가이드

하얀 전시 공간과 여백, 절제된 타이포그래피를 바탕으로 타로 카드 한 장에 시선을 모은다. 한국어 서비스명은 **오늘의 타로**로 통일한다. 아이보리 배경과 짙은 자두색 버튼, 먹색에 가까운 보라 카드 뒷면, 카드에만 쓰는 옅은 황동색으로 서비스의 분위기를 만든다.

기획 기준은 [PRD](TARO_01_PRD.md), [기능명세서](TARO_02_기능명세서.md), [유저플로우](TARO_03_유저플로우.md), [와이어프레임](TARO_04_와이어프레임.md)이다.

## 색상과 Tailwind CSS v4 토큰

| 역할 | Tailwind 유틸리티 | 값 | 사용 위치 |
|---|---|---|---|
| 배경 · 기본 글씨 | `bg-background text-foreground` | `#FCFAF7` · `#241D2D` | 페이지와 본문 |
| 카드 · 카드 글씨 | `bg-card text-card-foreground` | `#FFFFFF` · `#241D2D` | 결과 카드와 대화상자 |
| 보조 영역 · 보조 글씨 | `bg-muted text-muted-foreground` | `#F2EDF4` · `#62586B` | 개인정보 안내와 보조 설명 |
| 핵심 행동 · 버튼 글씨 | `bg-primary text-primary-foreground` | `#563575` · `#FFFFFF` | 시작, 뽑기, 저장 |
| 강조 · 강조 글씨 | `bg-accent text-accent-foreground` | `#EEE3F6` · `#46265F` | 선택된 주제, 보조 버튼 호버 |
| 구분선 · 입력 테두리 | `border-border` · `border-input` | `#E6DFE9` · `#897994` | 섹션 구분선과 입력 경계 |
| 포커스 · 오류 | `ring-ring` · `text-destructive` | `#7549A1` · `#A3253C` | 키보드 포커스와 날짜 오류 |
| 카드 뒷면 · 장식 | `bg-tarot-back` · `text-tarot-gold` | `#30213E` · `#E2C68B` | 뒷면의 달, 별, 테두리 |

전역 CSS 예시에서 shadcn의 의미 기반 변수와 Tailwind 유틸리티를 연결한다. 현재 디자인은 라이트 테마로 정의한다.

```css
@import "tailwindcss";

:root {
  --background: #fcfaf7;
  --foreground: #241d2d;
  --card: #fff;
  --card-foreground: var(--foreground);
  --popover: var(--card);
  --popover-foreground: var(--foreground);
  --primary: #563575;
  --primary-foreground: #fff;
  --secondary: #f2edf4;
  --secondary-foreground: var(--foreground);
  --muted: #f2edf4;
  --muted-foreground: #62586b;
  --accent: #eee3f6;
  --accent-foreground: #46265f;
  --border: #e6dfe9;
  --input: #897994;
  --ring: #7549a1;
  --destructive: #a3253c;
  --radius: 1rem;
}

@theme inline {
  --color-background: var(--background);
  --color-foreground: var(--foreground);
  --color-card: var(--card);
  --color-card-foreground: var(--card-foreground);
  --color-popover: var(--popover);
  --color-popover-foreground: var(--popover-foreground);
  --color-primary: var(--primary);
  --color-primary-foreground: var(--primary-foreground);
  --color-secondary: var(--secondary);
  --color-secondary-foreground: var(--secondary-foreground);
  --color-muted: var(--muted);
  --color-muted-foreground: var(--muted-foreground);
  --color-accent: var(--accent);
  --color-accent-foreground: var(--accent-foreground);
  --color-border: var(--border);
  --color-input: var(--input);
  --color-ring: var(--ring);
  --color-destructive: var(--destructive);
  --radius-md: calc(var(--radius) - 4px);
  --radius-lg: var(--radius);
}

@theme {
  --color-tarot-back: #30213e;
  --color-tarot-gold: #e2c68b;
  --font-sans: "Noto Sans KR", "Malgun Gothic", system-ui, sans-serif;
  --font-display: "Noto Serif KR", "Batang", serif;
  --radius-tarot: 1.5rem;
  --text-hero: 2.25rem;
  --text-hero--line-height: 1.2;
  --text-reading: 1.0625rem;
  --text-reading--line-height: 1.6;
}
```

## 글씨 · 간격 · 레이아웃

| 역할 | 크기 / 줄 높이 | 굵기 |
|---|---|---|
| 시작 화면 제목 | 36px / 1.2, 넓은 화면 48px | 700, 명조 |
| 입력 화면 제목 | 28px / 1.3 | 700, 고딕 |
| 결과 카드 이름 | 24px / 1.3 | 700, 명조 |
| 맞춤 해석 | 17px / 1.6 | 400, 고딕 |
| 입력 · 버튼 · 본문 | 16px / 1.5 | 400, 버튼 600 |
| 라벨 · 안내 · 상태 | 14px / 1.5 | 400, 라벨 600 |

- 간격은 4px 단위를 쓴다. 아이콘과 글씨는 8px, 라벨과 입력은 8px, 필드 사이는 16px, 영역 사이는 24~32px로 둔다. 스택은 `flex flex-col gap-*`로 구성한다.
- 화면 폭 320px부터 가로 스크롤 없이 사용할 수 있게 한다. 좌우 여백은 모바일 16px, 넓은 화면 24px로 두고, 입력과 결과는 최대 480px에서 가운데 정렬한다.
- 카드와 전체 패널은 24px, 입력과 주제 선택은 12px, 버튼은 알약형으로 둔다. 카드 패딩은 24px, 좁은 화면은 16px로 조정한다. 얇은 경계선으로 표면을 구분한다.
- 버튼과 주제 선택의 터치 영역은 최소 44×44px, 주 행동 높이는 48px로 둔다. 결과 버튼은 2열로 시작하고, 글자 확대 시 한 열로 흐르게 한다.

## shadcn/ui 컴포넌트

| 컴포넌트 | 구성과 상태 |
|---|---|
| `Button` | `default`는 [타로 시작], [카드 한 장 뽑기], [이미지 저장]. `outline`은 [결과 공유], [다시 뽑기]. `ghost`는 [처음으로], 뒤로가기. 색과 크기는 공통 variant에서 관리한다. 호버는 색을 짙게, 누름은 즉시 피드백, 포커스는 2px 링과 2px 간격으로 표시한다. 비활성은 `disabled`와 보조 영역 색으로 표시한다. 처리 중은 `Spinner`와 행동 문구를 함께 표시한다. |
| `FieldGroup`, `Field`, `Input` | 이름·생년월일·전공에 항상 보이는 `FieldLabel`을 연결한다. 이름은 최대 10자, 닉네임 가능, 전공은 선택이다. 날짜는 실제 날짜를 검증하고 오류 시 `Field`의 `data-invalid`, 입력의 `aria-invalid`, `FieldError`와 `aria-describedby`를 연결한다. |
| `ToggleGroup`, `ToggleGroupItem` | 단일 선택으로 커리어·재물·연애·건강·학업·종합을 3열 2행에 배치한다. 좁은 화면이나 글자 확대 시 2열로 줄인다. 선택 상태는 강조 배경, 테두리, 체크 표시를 함께 사용한다. |
| `Card` | `CardHeader`는 이름·주제, `CardContent`는 그림·카드 이름·별자리·해석, `CardFooter`는 오락용 안내를 담는다. 행동 버튼과 개인정보 안내는 카드 바깥에 둔다. |
| `Alert`, `Dialog` | 개인정보 문구는 보조 표면의 `Alert`로 표시한다. 다운로드·복사 대안은 제목이 있는 `Dialog`에 이미지 또는 선택 가능한 메시지로 표시한다. 닫으면 실행한 버튼으로 포커스를 돌린다. 성공 메시지는 `role="status"`로 알린다. |

여백과 배치만 화면의 `className`에서 조정한다. 아이콘이 들어간 버튼은 아이콘에 `data-icon`을 지정하고 크기는 컴포넌트가 관리한다. 아이콘만 보이는 뒤로가기 버튼에는 접근 가능한 이름을 지정한다.

## 화면과 전환

| 화면 | 화면 구성 및 동작 |
|---|---|
| **S-01 시작 · F-1** | 제목, 카드 뒷면, 한 장으로 맞춤 해석을 받아 본다는 설명, [타로 시작], 오락용·개인정보 안내 순서다. 뒷면의 기준 비율은 2:3이며 시작 화면에서 약 160~200px 너비로 표시한다. |
| **S-02 내 정보 · 주제 · F-2·F-3** | 뒤로가기와 제목 아래 필드를 표시한다. 이름·유효한 생년월일·주제가 모두 채워지면 [카드 한 장 뽑기]를 활성화한다. 주제 선택과 뽑기 버튼 사이에 개인정보 안내를 표시한다. 잘못된 날짜에는 “날짜를 다시 확인해 주세요”를 표시한다. |
| **S-03 맞춤 해석 · F-4~F-7** | 카드 한 장과 이름·주제·별자리, 맞춤 해석을 우선 표시한다. 기본 문장, 별자리 문장, 분류된 전공 문장 순으로 최대 3문장·90자다. 좁은 화면과 확대 상태에서는 문장 줄바꿈을 허용한다. 결과 아래에 이름이 이미지와 공유 글에 들어간다는 안내, 저장·공유 버튼, 다시 뽑기·처음으로 버튼을 둔다. |

- 22장의 메이저 아르카나 중 정방향 한 장을 공개한다. 카드 뒤집기는 약 600ms로 설정하고 1초를 넘기지 않는다. 애니메이션 중 중복 뽑기를 방지하고 `prefers-reduced-motion: reduce`에서는 결과를 즉시 표시한다. 화면 전환 후에는 새 화면 제목에 포커스를 옮긴다.
- [다시 뽑기]는 입력값을 유지한 채 주제 선택으로 돌아가고, [처음으로]는 입력과 결과를 비우고 시작 화면으로 돌아간다.
- 카드에는 직접 제작하거나 사용 허가가 확인된 그림을 쓴다. 앞면 그림의 크롭, 프레임, 비율은 22장에 일관되게 적용한다. 카드 이름이 그림 바로 옆에 있을 때는 중복 낭독을 피하도록 그림의 대체 텍스트를 비워 둔다.

## 개인정보 · 저장 · 공유

- 입력과 결과 화면에는 “입력한 이름 · 생년월일 · 전공은 해석을 만드는 데만 쓰고, 서버로 보내거나 저장하지 않아요. 창을 닫거나 [처음으로]를 누르면 바로 지워져요.”를 표시한다. 이름 입력에 닉네임을 권장한다.
- 결과와 개인정보는 현재 화면의 메모리에서 처리한다. 창 닫기·새로 고침·[처음으로]에서 비운다. 서버 전송, 브라우저 저장소, 분석 도구 전송 없이 미리 준비한 해석 문구를 사용한다.
- 내보내는 **1080×1350 PNG**는 결과 카드 영역을 사용한다. 최소 72px 안쪽 여백을 두고 이름·주제·카드 그림·카드 이름·별자리·해석·“※ 오락용 타로 해석입니다”·서비스명을 넣는다. 생년월일과 전공 원문은 제외한다. 가장 긴 이름과 해석도 잘리지 않는지 확인한다.
- 공유는 기기 공유 메뉴로 이미지와 글을 전달한다. 글에는 이름·주제·카드 이름·별자리·해석·오락용 안내·시작 링크를 담는다. 생년월일과 전공 원문은 제외하고 링크에는 입력이나 결과를 붙이지 않는다. 공유 취소 시 결과 화면을 유지한다.
- 저장 성공은 “이미지를 저장했어요”로 알린다. 내려받기 어려운 환경에서는 이미지를 표시하고 “이미지를 길게 눌러 저장해 주세요”를 보여 준다.
- 공유 메뉴가 없으면 공유 글을 복사하고 “결과를 복사했어요. 원하는 앱에 붙여 넣어 주세요”를 보여 준다. 복사도 실패하면 글과 “아래 글을 길게 눌러 복사해 주세요”를 표시한다.
- 건강·재물 해석은 돌아볼 관점으로 작성한다. 결과 화면과 내보내는 콘텐츠에 오락용 안내를 유지한다.

## 구현 확인

- 일반 텍스트 대비 4.5:1 이상, 입력 경계와 포커스 대비 3:1 이상을 확인한다. 주제 선택, 오류, 저장 상태는 색 외에 문구 또는 상태 표시로도 전달한다.
- 320px·375px·768px 화면과 200% 글자 확대에서 입력, 버튼, 긴 이름과 해석이 잘리지 않는지 확인한다.
- 키보드 탐색, 날짜 오류, 애니메이션 감소, 저장·공유 대안, 처음으로 돌아간 뒤 입력값 초기화를 확인한다.
