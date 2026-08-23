# PNU AI College — 상세영역 GNB 규격

이 문서는 `AI대학 조직체계`에서 확정한 상세영역 GNB를 전체 상세 페이지에 동일하게 적용하기 위한 기준이다.

## 1. 기본 원칙

- 상세영역에서는 홈페이지의 로고·검색창 GNB를 사용하지 않는다.
- 상세영역 최상단에는 `대분류 > 현재 부제목`만 표시한다.
- 대분류를 누르면 홈페이지 차례 화면으로 돌아간다.
- 현재 부제목에 마우스를 올리거나 클릭하면 같은 대분류의 전체 부제목 목록이 열린다.
- 브라우저 뒤로가기로도 이전 화면 또는 차례 화면으로 돌아갈 수 있어야 한다.
- 모든 상세영역에서 동일한 높이, 글꼴, 블러, 여백과 인터랙션을 사용한다.

## 2. 정보 구조

```text
[대분류]  >  [현재 부제목⌄]
                 ├─ 01. 부제목
                 ├─ 02. 부제목
                 └─ 03. 부제목
```

예시:

```text
AI대학 조직체계  >  AX 융합·연계전공⌄
```

## 3. 권장 HTML

```html
<nav class="detail-nav" aria-label="현재 위치">
  <button class="category-button" type="button">
    AI대학 조직체계
  </button>

  <img class="detail-nav-separator" src="./assets/chevron-right.svg" alt="" />

  <div class="subnav-wrap">
    <button
      class="subnav-button"
      type="button"
      aria-expanded="false"
      aria-controls="subnav-menu"
    >
      AX 융합·연계전공
    </button>

    <div id="subnav-menu" class="subnav-menu" hidden>
      <button type="button">01. ADP+X 교육체계</button>
      <button type="button">02. AI대학 학부 및 전공</button>
      <button type="button">03. AX 융합·연계전공</button>
    </div>
  </div>
</nav>
```

화살표는 폰트 문자 `⌄`를 사용하지 않는다. CSS border로 만든 chevron 또는 고정 SVG만 사용한다.

## 4. 디자인 토큰

```css
:root {
  --detail-nav-height-desktop: 52px;
  --detail-nav-height-mobile: 48px;
  --detail-nav-gutter: clamp(20px, 4vw, 72px);

  --detail-nav-bg-dark: rgba(3, 11, 19, 0.70);
  --detail-nav-border-dark: rgba(255, 255, 255, 0.10);
  --detail-nav-text-dark: #eef5fb;
  --detail-nav-muted-dark: #8ba5bb;

  --detail-nav-bg-light: rgba(250, 250, 252, 0.78);
  --detail-nav-border-light: rgba(0, 0, 0, 0.08);
  --detail-nav-text-light: #1d1d1f;

  --detail-nav-blur: blur(20px) saturate(160%);
  --detail-nav-font-size: 13px;
  --detail-nav-font-weight: 600;
}
```

현재 AI대학 사이트의 상세영역은 다크 배경을 기본으로 하므로 `dark` 토큰을 우선 적용한다.

## 5. 레이아웃

```css
.detail-nav {
  position: fixed;
  z-index: 30;
  inset: 0 0 auto;
  height: var(--detail-nav-height-desktop);
  padding: 0 var(--detail-nav-gutter);
  display: flex;
  align-items: center;
  gap: 6px;
  color: var(--detail-nav-text-dark);
  background: var(--detail-nav-bg-dark);
  border-bottom: 1px solid var(--detail-nav-border-dark);
  backdrop-filter: var(--detail-nav-blur);
  -webkit-backdrop-filter: var(--detail-nav-blur);
  transition: transform 280ms ease;
}

.detail-nav button {
  padding: 10px 2px;
  border: 0;
  color: inherit;
  background: transparent;
  font-size: var(--detail-nav-font-size);
  font-weight: var(--detail-nav-font-weight);
  line-height: 1;
  letter-spacing: -0.015em;
}
```

상세 콘텐츠는 GNB 아래에서 시작한다.

```css
.detail-view {
  padding-top: var(--detail-nav-height-desktop);
}
```

## 6. 드롭다운

- 데스크톱: 현재 부제목 영역에 `mouseenter` 시 열리고 `mouseleave` 시 닫힌다.
- 터치 기기: 현재 부제목을 탭해 열고 다시 탭하거나 항목을 선택하면 닫힌다.
- 드롭다운은 GNB와 동일한 배경 블러를 사용한다.
- 현재 대분류에 속한 부제목만 표시한다.

```css
.subnav-menu {
  position: absolute;
  top: 38px;
  left: 0;
  min-width: 280px;
  padding: 8px;
  border: 1px solid rgba(255, 255, 255, 0.12);
  border-radius: 12px;
  color: #edf5fb;
  background: rgba(5, 18, 29, 0.72);
  backdrop-filter: blur(20px) saturate(150%);
  -webkit-backdrop-filter: blur(20px) saturate(150%);
  box-shadow: 0 18px 46px rgba(0, 0, 0, 0.34);
}

.subnav-menu button {
  display: block;
  width: 100%;
  padding: 12px;
  border-radius: 9px;
  text-align: left;
  font-size: 15px;
}

.subnav-menu button:hover,
.subnav-menu button:focus-visible {
  background: rgba(103, 184, 242, 0.12);
}
```

## 7. 스크롤 동작

- 아래로 스크롤할 때 GNB를 숨긴다.
- 위로 짧게 스크롤하면 즉시 다시 표시한다.
- 문서 상단에서는 항상 표시한다.
- 숨김은 레이아웃 높이를 바꾸지 않고 `transform`만 사용한다.

```css
body.nav-hidden .detail-nav {
  transform: translateY(-110%);
}
```

권장 기준:

```js
if (scrollY < 40) showNav();
else if (deltaY > 7) hideNav();
else if (deltaY < -4) showNav();
```

## 8. 현재 부제목 동기화

- 사용자가 상세 페이지 안에서 다음 섹션으로 이동하면 GNB의 현재 부제목도 함께 갱신한다.
- 드롭다운 항목을 선택하면 해당 섹션으로 이동하고 메뉴를 닫는다.
- URL history에는 `대분류 index`와 `부제목 index`를 저장한다.
- 뒤로가기 시 저장된 위치를 복원한다.

## 9. 모바일

```css
@media (max-width: 800px) {
  .detail-nav {
    height: var(--detail-nav-height-mobile);
    overflow: visible;
  }

  .detail-view {
    padding-top: var(--detail-nav-height-mobile);
  }

  .detail-nav button {
    font-size: 12px;
    white-space: nowrap;
  }

  .subnav-menu {
    position: fixed;
    top: var(--detail-nav-height-mobile);
    left: 16px;
    right: 16px;
    min-width: 0;
  }
}
```

## 10. 접근성

- GNB에는 `aria-label="현재 위치"`를 지정한다.
- 부제목 버튼은 `aria-expanded`와 `aria-controls`를 사용한다.
- 장식용 구분 화살표는 빈 `alt`를 사용한다.
- 모든 메뉴 항목은 키보드로 접근하고 선택할 수 있어야 한다.
- `Escape` 입력 시 열린 드롭다운을 닫는다.
- 포커스 스타일을 제거하지 않는다. 커스텀 포커스를 사용할 경우 명암 대비를 확보한다.

## 11. 적용 범위

다음 대분류에 동일하게 적용한다.

1. AI대학 소개
2. AI대학 조직체계
3. 신입생모집
4. AI대학 특화 프로그램
5. 함께하는 교수진들

페이지별로 변경할 수 있는 값은 `대분류명`, `현재 부제목`, `부제목 목록`뿐이다. 높이, 간격, 글꼴, 색상, 블러와 동작은 변경하지 않는다.

