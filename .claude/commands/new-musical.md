# new-musical

새 뮤지컬 관극 다이어리 앱을 생성한다. `template/`을 복사한 후 공연별 정보로 교체한다.

## 작업 절차

아래 순서로 단계별 정보를 수집한다. 각 단계에서 정보를 확인한 후 다음 단계로 넘어간다.

---

### Step 0 — GitHub 설정 확인

`CLAUDE.md`에서 GitHub 설정을 읽는다.

- `GITHUB_ORG` — 레포를 생성할 GitHub 사용자명 또는 조직명 (예: `my-username`)
- `DEPLOY_BASE_DOMAIN` — GitHub Pages 도메인 (예: `my-username.github.io`)

설정이 비어있으면 다음을 질문한다:
1. GitHub 사용자명 또는 조직명
2. GitHub Pages 커스텀 도메인 여부 (없으면 `{사용자명}.github.io` 자동 사용)

입력받은 값을 `CLAUDE.md`의 `GITHUB_ORG=` 및 `DEPLOY_BASE_DOMAIN=` 뒤에 저장한다. 이후 세션에서는 이 값을 자동으로 사용한다.

---

### Step 1 — 기본 정보

다음을 질문한다:

- **뮤지컬 한글명** (예: 로저, 영웅, 레베카)
- **앱 대표 이모지** (예: ✈ 🔫 🌹 — D-day 카드 배경 및 홈 제목에 사용)
- **localStorage prefix** (예: `roger_`, `hero_` — 같은 브라우저에서 다른 앱과 데이터 분리)
- **테마 색상** — 포스터 또는 공식 이미지를 요청한다. 이미지에서 대표 색상을 추출하여 HEX로 변환한다 (`--accent` 변수에 사용). HEX를 직접 묻지 않는다.
- **레포 이름** (예: `roger2026` → `https://{DEPLOY_BASE_DOMAIN}/{레포명}/` 밑에 신규 레포로 배포)

---

### Step 2 — 역할/배우 구성

다음을 질문한다:

- **역할 수** (2~5개 — 먼저 숫자만 입력받고, 이후 역할별로 상세 정보 수집)
- 역할 수 확인 후 역할마다 순서대로:
  - 역할명 (UI 표시용, 예: 스카일러, 디디, 라울)
  - 역할 대표 색상 HEX (캘린더 칩, 캐스팅 태그, 배우 pill 색상)
  - 배우 목록 (이름 나열)
- **페어 조합** — 가능한 캐스팅 조합 전체 (schedule.json에 없는 조합도 통계에서 표시하려면 명시)

> 역할이 3개 이상이면 record 구조에 필드 추가, 기록 폼 select 추가, 통계 섹션 확장이 필요하다. Step 7 파일 생성 전에 사용자에게 미리 안내한다.

---

### Step 3 — 공연 시간

다음을 질문한다:

- **회차 시간 목록** (예: 14:00 / 16:00 / 18:00 / 20:00)

---

### Step 4 — 좌석 구성

다음을 질문한다:

- **좌석 입력 기능 필요 여부** (없으면 좌석 히트맵 전체 제거)
- 필요한 경우:
  - **좌석 배치도 이미지** 제공 요청 — 이미지를 기준으로 구역/열/번을 정확히 재현한다
  - 구역 수 및 구역명 (예: A/B/C)
  - 각 구역의 시작 열 ~ 끝 열, 번 수 (열에 따라 번 수 다르면 명시) — **행이 아닌 열(1열, 2열...)로 표기**
  - **열 범위** — 기록 폼 `#rec-row` input의 `min`/`max`/`placeholder`에 쓸 값 (예: 1열~10열 → min=1, max=10, placeholder="1-10"). 템플릿 기본값(0-13)이 남아있으면 반드시 교체한다.
  - **번 최대값** — 기록 폼 `#rec-num` input의 `max`에 쓸 값 (예: 최대 11번 → max=11)
  - 특이사항 (예: B구역 1열은 10번으로 1번 적음, 특정 구역에 없는 열 등)
  - **열 사이 시각적 구분(gap)** 이 있으면 명시 (예: 8열과 9열 사이 한 칸 띄움)
  - **구역 간 연결석(통로 없는 열)** 여부 — A구역과 B구역이 특정 열에서 통로 없이 직접 붙어있는 경우 명시 (예: 10열은 A+B 연결)

> **좌석 배치도는 이미지와 픽셀 단위로 일치하지 않아도 되지만, 구역/열/번의 상대적 구조와 시각적 배치가 동일해야 한다.** 구역 간 간격, 열 레이블 위치, 특수 열(번 수 다름) 처리, 열 간 gap까지 이미지 기준으로 구현한다.

---

### Step 5 — 할인권 / 쿠폰

다음을 질문한다:

- **할인권 목록** (기록 폼 select에 표시할 것들, 표시명 + 할인율 포함)
- **쿠폰 연동 할인권** — 잔여 쿠폰과 연동되는 것만 (DISCOUNT_COUPON_MAP에 들어갈 것):
  - 표시명 → 쿠폰 key (예: `'50% 할인권' → 'discount50'`)
- **쿠폰 종류** (COUPON_ITEMS):
  - key명, 표시명, 이력 표 약어
  - 쿠폰 수동 차감 시 **날짜 + comment 입력 필드** 포함 — 사용자가 "교환", "사용" 등 자유 기재
- **쿠폰팩 존재 여부** — 있으면:
  - 구성 (예: 3종 각 ×1)
  - 쿠폰팩 사용 기간 안내 문구 여부
- **차액 지불 가능 여부** — 가능하면:
  - 할인권은 기록에 남기되 쿠폰 잔여 차감은 안 함
  - 기록 폼에 "차액 지불" 체크박스 추가 필요
  - record에 `paidDiff: boolean` 필드 추가, `calcCouponCount()` 차감 로직에서 `paidDiff=true` 건 스킵

---

### Step 6 — 스탬프 기능

다음을 질문한다:

- **기능명** (roger: "교신일지 카드" — 공연마다 다름, 예: 재관람 스탬프, 로열티 카드)
- **총 칸 수** (roger: 7칸)
- **마일스톤** (1~3개):
  - 각 마일스톤마다: 달성 칸 수 / 보상명 / 쿠폰 연동 여부 (연동이면 쿠폰 key명)
  - **폴라로이드 보상은 연뮤 오타쿠에게 최우선 관심사** — 마일스톤에 폴라로이드가 있으면 UI에서 강조 표시 (예: 📸 아이콘, 별도 색상 등)
  - 3개 마일스톤이 있으면 코드를 확장해서 전부 표시한다 (2개로 축소하지 않는다)
- **도장 방식 목록** (기록 폼 "도장 추가 방식" radio, 예: 직접 관람 / 적립권 사용 / 교환)
  - "교환" 방식이 있으면 메모 텍스트 입력란 추가 — 사용자가 교환 내용 자유 기재
- **스탬프 기능 안내 문구** (도장판 생성 패널에 표시, 없으면 생략)

---

### Step 7 — 정보 확인 후 파일 생성

수집한 정보를 요약해서 보여주고 확인을 받는다.

확인 후 다음 순서로 파일을 생성한다:

1. `template/` 폴더 전체를 새 폴더명으로 복사
   - 폴더명은 `{prefix}{연도}` 또는 사용자가 지정한 이름
2. `index.html` 수정 — 아래 항목 순서대로:
   a. `<title>`, `apple-mobile-web-app-title`, 홈 page-title
   b. 배경 이모지 (`::before content`)
   c. `APP_PREFIX` (`'roger_'` → 새 prefix)
   d. `--accent`, `--accent2`, `--accent-light`, `theme-color`
   e. 역할명 레이블 및 JS 내부 필드명 — `skylar`/`didi`를 공연별 역할명으로 일괄 치환 (예: `eoduksini`/`somun`). 기존 데이터 호환을 위해 `migrateData()`에 필드명 변환 로직 추가
   f. 역할별 색상 CSS (`.sky`, `.didi`, `.actor-pill`, `.casting-tag`, `.next-show-pill`)
   g. 배우 목록 (기록 폼 select options, 캘린더 필터 칩, `ACTOR_ROLES`, `sky`/`didi` 배열)
   h. 페어 조합 (`pairs` 배열)
   i. 공연 시간 select options + `resetTimeOptions()`
   j. 좌석 구역 (없으면 좌석 관련 HTML/JS 전체 제거 또는 주석 처리) — **있으면 반드시**: `#rec-row` input의 `placeholder`, `min`, `max`를 Step 4에서 수집한 열 범위로 교체 / `#rec-num` input의 `max`를 Step 4에서 수집한 번 최대값으로 교체 (템플릿 기본값 0-13 방치 금지)
   k. 할인권 select options
   l. `DISCOUNT_COUPON_MAP`
   l2. `DISCOUNT_LABEL` (할인권 내부 key → 한글 표시명 맵) + `openDetail()`에서 `DISCOUNT_LABEL[r.discountType] || r.discountType` 적용 — 누락 시 상세 모달에 `discount40` 같은 raw key가 노출됨
   m. `getCoupons()` 초기값 + `COUPON_ITEMS` + 쿠폰 수동 추가 select
   n. 쿠폰팩 패널 안내 및 `saveCouponPack()` (없으면 제거)
   o. 쿠폰팩 사용 기간 안내 문구 (없으면 제거)
   p. 차액 지불 체크박스 UI + `paidDiff` 로직 (옵션, Step 5에서 YES인 경우)
   q. 스탬프 기능명 (텍스트 전체 치환)
   r. 도장판 칸 수 + 마일스톤 (HTML 안내 패널 텍스트 + `saveStampBoard()` + `migrateStampBoards()` + `applyStampToBoard()` + `renderStampBoards()` statusHtml) — **HTML 안내 패널과 statusHtml 두 군데 모두 수정 필요**. `migrateStampBoards()`는 `if (!b.milestones)` 블록 외에 `else { const at3 = b.milestones.find(m => m.at === 3); if (at3 && at3.qty === undefined) at3.qty = N; }` 패치 블록도 추가
   s. 도장 방식 radio 목록
   t. `migrateData()`에서 쿠폰 key 목록 (`['discount40','discount50','discountPass']`)
   u. `getEvents()` 기본값 제거 (빈 배열로)
   v. 쿠폰 이력 표 헤더 (`<th>40%</th><th>50%</th><th>증빙</th>` → 새 쿠폰 목록)
   w. 캘린더 초기화 — `initCal()`에서 공연 시작월 기준으로 초기 연/월을 설정한다. 현재 날짜가 공연 시작 전이면 공연 첫 달로 이동:
      ```javascript
      function initCal() {
        const n = new Date();
        const start = new Date(YYYY, MM-1, 1); // 공연 시작년월 (0-indexed month)
        calYear = (n >= start ? n : start).getFullYear();
        calMonth = (n >= start ? n : start).getMonth();
      }
      ```
3. `sw.js` 수정:
   - `CACHE_NAME` → `{새앱명}-v1`
   - `BASE` → `/{새 배포 경로}`
   - **이후 index.html 수정 시마다 `CACHE_NAME` 버전을 올려야 한다** (예: `v1` → `v2`). 변경하지 않으면 브라우저가 구 캐시를 계속 사용해 업데이트가 반영되지 않는다.
4. `manifest.json` 수정:
   - `name`, `short_name`, `description`, `start_url`, `scope`, `theme_color`
5. **앱 아이콘** — 사용자에게 아이콘으로 쓸 이미지를 요청한다. 이미지를 받으면 `sips`로 192×192, 512×512로 리사이즈해서 `icon-192.png`, `icon-512.png`로 저장한다. 템플릿 아이콘을 그대로 복사하지 않는다.
   ```bash
   sips -z 192 192 {원본이미지경로} --out icon-192.png
   sips -z 512 512 {원본이미지경로} --out icon-512.png
   ```
6. `schedule.json` → `[]` (빈 배열)
7. `events.json` → `[]` (빈 배열)

---

### Step 8 — GitHub 레포 생성 및 push

검증 통과 후 다음 순서로 진행한다. `{GITHUB_ORG}`와 `{DEPLOY_BASE_DOMAIN}`은 Step 0에서 읽은 값을 사용한다.

1. 새 레포 생성:
```bash
gh repo create {GITHUB_ORG}/{레포명} --public --description "{뮤지컬명} 관극 다이어리"
```

2. 새 앱 폴더에서 git 초기화 및 push:
```bash
cd {새앱폴더}
git init
git add .
git commit -m "Initial commit: {뮤지컬명} 관극 다이어리"
git branch -M main
git remote add origin https://github.com/{GITHUB_ORG}/{레포명}.git
git push -u origin main
```

3. GitHub Pages 활성화:
```bash
gh api repos/{GITHUB_ORG}/{레포명}/pages -X POST -f source[branch]=main -f source[path]=/
```

4. 배포 URL 확인: `https://{DEPLOY_BASE_DOMAIN}/{레포명}/`

5. `CLAUDE.md`의 생성된 앱 목록 표에 새 앱을 추가한다.

---

### Step 9 — 검증

파일 생성 후 다음 검증 스크립트를 실행한다:

```bash
node -e "
const fs = require('fs');
const html = fs.readFileSync('index.html', 'utf8');
const js = html.match(/<script>([\s\S]*?)<\/script>/)[1];
try { new Function(js); console.log('✅ JS OK'); } catch(e) { console.log('❌', e.message); }
const htmlIds = new Set([...html.matchAll(/id=\"([^\"]+)\"/g)].map(m=>m[1]));
const jsIds = [...new Set([...js.matchAll(/getElementById\('([^']+)'\)/g)].map(m=>m[1]))];
const missing = jsIds.filter(id => !htmlIds.has(id));
console.log(missing.length ? '❌ ID 없음: '+missing.join(', ') : '✅ ID OK');
const fns = [...new Set([...html.matchAll(/onclick=\"([a-zA-Z_]\w*)\(/g)].map(m=>m[1]))];
const missFn = fns.filter(f => !js.includes('function '+f+'('));
console.log(missFn.length ? '❌ 함수 없음: '+missFn.join(', ') : '✅ 함수 OK');
"
```

오류 있으면 수정 후 재검증한다.

---

## 주의사항

- JS 내부 필드명(`skylar`/`didi`)은 공연별 역할명 기반으로 변경한다 (예: `eoduksini`/`somun`). index.html 전체에서 일괄 치환하고, 기존 데이터 호환을 위해 `migrateData()`에 필드명 변환 로직을 추가한다
- 역할이 3개 이상이면 record에 `role3` 이상 필드 추가, 기록 폼 select 추가, 통계 섹션 확장 필요 — Step 2에서 미리 안내 후 진행
- 차액 지불 기능은 `paidDiff: true`인 record를 `calcCouponCount()` 차감 로직에서 스킵하는 방식으로 구현
- 쿠폰이 없는 공연이면 쿠폰탭 전체를 제거하거나 스탬프 전용 탭으로 교체 가능 (사용자에게 확인)
- **earned/calcCouponCount 계산 패턴 주의**: `null couponKey`를 기본 쿠폰 key로 폴백(`m.couponKey || 'discount50'`)하면 쿠폰 미연동 마일스톤(예: 폴라로이드)이 잘못된 쿠폰에 산입됨 — 반드시 `m.couponKey === item.key`로만 필터링. qty 필드는 `.length` 대신 `.reduce((t,m)=>t+(m.qty||1),0)`으로 합산
- **배우 이름 색상 ≠ accent 색상**: `.sky/.didi/.role3` text color는 accent와 같은 값이더라도 **CSS class 단위로만 변경**한다. `replace_all`로 hex값을 일괄 치환하면 `:root { --accent }`, `theme-color` 메타태그까지 바뀌어 앱 전체 색조가 달라진다. 배우 이름 색을 accent보다 어둡게 하려면 별도 hex를 class에만 직접 지정한다.
- **`recordId` 필드**: record 저장 시 `id` 값을 stamp history 항목의 `recordId`에 함께 저장한다. 이 값이 있어야 `deleteCurrentRecord()`가 여러 도장판에 흩어진 이력을 정확히 찾아 삭제할 수 있다.
- **`deleteCurrentRecord()` 도장판 이력 삭제는 전체 보드 순회**해야 한다. `target.stampBoardId` 하나만 보면 다중 보드 연동 시 나머지 보드의 이력이 남는다. 올바른 패턴:
  ```javascript
  const boards = getStampBoards();
  let stampChanged = false;
  boards.forEach(board => {
    if (!board.history) return;
    const before = board.history.length;
    board.history = board.history.filter(h => h.recordId !== target.id);
    if (board.id === target.stampBoardId && board.history.length === before) {
      const idx = board.history.map((h,i)=>({h,i})).reverse().find(({h}) => h.date === target.date);
      if (idx !== undefined) board.history.splice(idx.i, 1);
    }
    if (board.history.length !== before) { recalcStampCount(board); revalidateMilestones(board); stampChanged = true; }
  });
  if (stampChanged) saveStampBoards(boards);
  ```
- 완성 검증 후 `CLAUDE.md`의 생성된 앱 목록 표에 추가한다

### 모바일 레이아웃 원칙

**글자가 작아지더라도 잘리거나 넘치는 레이아웃은 절대 허용하지 않는다.**

- **iOS 입력 자동 줌 방지**: 모든 `input`, `select`, `textarea`에 `font-size: 16px` 필수. 미만이면 iOS Safari가 포커스 시 화면을 자동 확대한다. `.form-control { font-size: 16px; line-height: 1.4; -webkit-appearance: none; box-sizing: border-box; }` 로 일괄 적용한다.
- 역할/배우 수가 늘어나도 캘린더 필터 칩은 `flex-wrap: wrap` 유지
- 캐스팅 태그(기록 상세, 홈 D-day 카드)는 `flex-wrap: wrap` + `min-width: 0` 으로 줄바꿈 허용
- 통계 배우/페어 테이블은 역할이 많을수록 행이 늘어나는 구조 — 가로 스크롤 금지
- 기록 폼 역할 select가 3개 이상이면 `form-row`를 2열 이상으로 재배치하거나 세로 배치로 전환
- 좌석 히트맵은 구역이 많거나 열 수가 많으면 `overflow-x: auto` 컨테이너 안에서만 스크롤 허용 (전체 페이지 가로 스크롤 금지)
- 모든 텍스트는 `overflow: hidden; text-overflow: ellipsis; white-space: nowrap` 또는 줄바꿈 허용 중 하나를 명시 — 기본값 방치 금지

#### 예정 일정(upcoming) 레이아웃

역할 수와 무관하게 **1줄 레이아웃**이 기본이다. 배우 이름만 표시(역할 prefix 없음)하면 3역할도 1줄에 충분히 들어간다.

```
[D-3] [03/10 (화) 14:00] [A구역 5열 3번]   [박경호 · 이태이 · 이진우]
```

CSS 패턴:
```css
.upcoming-item { display: flex; align-items: center; gap: 8px; }
.upcoming-dday { min-width: 36px; font-size: 12px; font-weight: 700; color: var(--accent); }
.upcoming-date { font-size: 12px; color: var(--text2); white-space: nowrap; }
.upcoming-seat { font-size: 11px; color: var(--text3); opacity: 0.75; white-space: nowrap; }
.upcoming-cast { font-size: 11px; color: var(--text3); white-space: nowrap; margin-left: auto; }
```

- `.upcoming-cast`의 `margin-left: auto` → 항상 오른쪽 끝 정렬, 좌석 유무와 무관하게 위치 일정
- 좌석은 날짜와 캐스팅 사이에 배치 (date → seat → cast 순서)

### 좌석 배치도 구현 원칙

- 제공된 이미지와 구역/행/열 구조가 동일해야 한다
- 구역 간 시각적 간격, 특수 행(열 수 다름)의 중앙 정렬 방식도 이미지 기준으로 맞춘다
- 미니 좌석도(기록 상세)와 히트맵(통계) 양쪽 모두 동일한 구조로 구현한다
- 구현 후 이미지와 나란히 비교해서 확인을 요청한다

#### 히트맵 레이아웃 — row-centric

두 구역이 좌우로 나란한 극장은 zone-centric(각 구역 독립 컬럼)이 아닌 **row-centric** 레이아웃으로 구현한다.

**구조**: 행마다 `[A구역 cells] [row-label] [B구역 cells]` — row-label이 통로 역할을 한다.

**셀 크기 계산** (모바일에서 가로 스크롤 없이 맞추는 것이 목표):

카드 내부 가용 너비 = 375 - 32 = **343px** (카드 padding 16px × 2 기준)

```
전체 행 너비 = (N_A × c + (N_A-1) × gap) + 2 + label + 2 + (N_B × c + (N_B-1) × gap)
```

gap=2px, label=28px 기준으로 정리하면:
```
(N_A + N_B) × c + (N_A + N_B - 2) × 2 + 70 = 343
c = (343 - (N_A + N_B - 2) × 2 - 70) / (N_A + N_B)
```

예시 (A=11, B=10): `c = (343 - 38 - 70) / 21 = 273 / 21 ≈ 13px`

**구역 헤더 너비** (row-label과 정렬하려면 픽셀 단위 일치 필수):
- `.hm-a-header { width: N_A × c + (N_A - 1) × gap }` — N_A: A구역 최대 번 수
- `.hm-b-header { width: N_B × c + (N_B - 1) × gap }` — N_B: B구역 최대 번 수
- `.hm-label-gap { width: 28px }` — row-label 너비와 동일, gap:2px 환경에서 헤더-셀 정렬 맞춤

**빈 자리(missing seats) 정렬 방향**:
- A구역 (우측 = 통로쪽): 빠진 번은 **왼쪽**에 `visibility:hidden` 셀로 채운다 → 11번이 항상 row-label에 붙음
- B구역 (좌측 = 통로쪽): 빠진 번은 **오른쪽**에 `visibility:hidden` 셀로 채운다 → 1번이 항상 row-label에 붙음

#### 다크모드 D-day 카드

라이트 모드: `background: var(--accent-light); color: var(--accent)` (연한 배경 + 진한 텍스트)
다크 모드: `--accent` 원색 그라디언트를 그대로 쓰면 어두운 배경 대비 너무 밝게 튄다.
→ **accent보다 어두운 그라디언트**를 dark 전용으로 별도 지정한다:

```css
/* 블루 계열 예시 */
[data-theme="dark"] .next-show-card { background: linear-gradient(135deg, #1a3a70 0%, #26508a 100%); color: #fff; }

/* 레드 계열 예시 */
[data-theme="dark"] .next-show-card { background: linear-gradient(135deg, #6a1525 0%, #a02535 100%); color: #fff; }
```

다크모드 히트맵 seat 색상도 채도를 충분히 높여야 `#22222e` 배경 위에서 식별된다:
```css
/* 블루 계열 */
--seat1: #1a3570; --seat2: #1e52a0; --seat3: #2e70cc; --seat4: #4490e0; --seat5: #6ab0ff;

/* 레드 계열 */
--seat1: #5a1a22; --seat2: #8a1e2a; --seat3: #b82840; --seat4: #d94058; --seat5: #f05070;
```

#### 연결석(통로 없는 행) 처리

A구역과 B구역이 특정 열에서 통로 없이 직접 붙어있으면 해당 행을 **merged row**로 구현한다.

**구조**: `[row-label] [.hm-merged: blank cells + A cells + B cells + reserved cells]`
- row-label을 **왼쪽**에 두고 모든 셀을 한 줄로 연결
- blank cells 수 = 해당 행에서 A구역 빠진 번 수 (좌측 정렬 맞춤)
- 전체 너비: `28px(label) + 2px(gap) + 전체셀수 × 18 - 2` ≈ 정상 행 너비와 동일하게 맞춤
