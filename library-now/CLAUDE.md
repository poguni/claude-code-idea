# CLAUDE.md — 지금 열린 도서관 (Library Now)

서울 공공도서관의 운영 상태와 "지금 가면 몇 시간 이용 가능한지"를 지도로 보여 주는 개인용 반응형 웹앱입니다.
1차 범위는 구로구·영등포구·양천구 공공도서관이며, 이후 서울 전체로 넓힙니다.

## 먼저 읽을 문서
작업 전에 관련 부분을 반드시 확인합니다. 문서와 코드가 다르면 문서를 기준으로 하고, 문서가 틀렸다고 판단되면 고치기 전에 사용자에게 묻습니다.

| 문서 | 내용 |
|---|---|
| `docs/PRD.md` | 기능 요구사항(F1~F17), 시간 계산 로직(5장), 데이터 구조(6장), 화면 구성(8장), 마일스톤(11장) |
| `design/DESIGN.md` | 디자인 토큰, 테마 3종, 컴포넌트 명세, 반응형 규칙, 접근성 체크리스트 |
| `design/design-*.html` | 시안 원본(서재·말랑·모노). 화면을 만들 때 구조·크기·문구의 기준 |

화면 작업을 할 때는 해당 시안 HTML을 열어 마크업과 CSS를 참고합니다. 디자인 문서와 PRD가 다르면 화면 모양은 `design/`을, 기능은 `docs/PRD.md`를 따릅니다.

## 기술 스택
- Vite + React + TypeScript (strict)
- 지도: Leaflet + react-leaflet, OpenStreetMap 타일 (키 없음). 지도 코드는 `src/map/`의 `MapProvider` 인터페이스 뒤에 둡니다(나중에 카카오맵 추가).
- 스타일: CSS Modules + CSS 변수(디자인 토큰). UI 라이브러리(MUI, Tailwind 등)는 쓰지 않습니다.
- 아이콘: lucide-react
- 테스트: Vitest(도메인 로직), Playwright(화면)
- 데이터 스크립트: Node + TypeScript(tsx)
- 배포: GitHub Actions → GitHub Pages

## 명령어
(M0에서 `package.json`을 만든 뒤 실제 스크립트와 맞는지 확인하고 이 목록을 갱신합니다.)

```bash
npm install
npm run dev          # 개발 서버 (http://localhost:5173)
npm run build        # 타입 검사 + 프로덕션 빌드
npm run test         # Vitest 단위 테스트
npm run test:e2e     # Playwright 화면 테스트 (--trace off 기본)
npm run lint         # ESLint + Prettier 검사
npm run data:build   # data/raw + data/overrides → public/data/*.json
```

## 폴더 구조
```
CLAUDE.md
docs/          PRD (기획)
design/        디자인 가이드와 시안 HTML (수정은 사용자 요청이 있을 때만)
data/raw/      공공데이터 원본 CSV (내려받은 그대로, 수정 금지)
data/overrides/ 도서관별 수기 입력(자료실·열람실 시간, 휴관 규칙 검수값)
scripts/       데이터 빌드 스크립트
public/data/   앱이 읽는 JSON (스크립트가 생성, 직접 수정 금지)
src/domain/    시간 계산·휴관 판정·거리 계산 (순수 함수 + 테스트)
src/map/       MapProvider 인터페이스와 Leaflet 구현
src/components/ 화면 컴포넌트
src/state/     설정·즐겨찾기·출발지·테마 저장 (localStorage)
src/styles/    themes.css(테마 토큰·장식), global.css
tests/e2e/     Playwright 테스트
```

## 핵심 규칙

### 시간 계산
- 모든 시간 계산은 **Asia/Seoul** 기준입니다. 기기 시간대에 의존하지 말고 `Intl.DateTimeFormat`으로 서울 시각을 구합니다.
- 상태 판정 순서(임시 휴관 → 정기 휴관 → 공휴일 → 요일·계절 운영시간)는 PRD 5.2를 그대로 따릅니다.
- `src/domain/`의 함수는 순수 함수로 만들고(현재 시각은 인자로 받음), 바꿀 때마다 단위 테스트를 함께 고칩니다. PRD 5.6의 테스트 사례는 모두 있어야 합니다.
- 이동 시간 추정 상수(우회 계수 1.3, 도보 4.2km/h, 대중교통 10분 + 18km/h, 기준 1.5km)는 `src/domain/config.ts` 한 곳에만 둡니다.

### 데이터
- 앱은 실행 중에 외부 API를 부르지 않습니다. `public/data/*.json`만 읽습니다(정적 방식).
- `public/data/`는 `npm run data:build`로만 만듭니다. 값을 고칠 때는 `data/overrides/`를 고치고 다시 빌드합니다.
- 도서관 운영시간이나 휴관 규칙을 **추측해서 채우지 않습니다.** 확인하지 못한 값은 비워 두고(`null`) 화면에 "운영시간 확인 필요"로 표시합니다.
- 자료실(`lending`)과 열람실(`reading`)은 항상 따로 다룹니다. 열람실이 없으면 `reading: null`.

### 디자인과 테마
- 테마는 `library`(서재, **기본**), `pastel`(말랑), `mono`(모노) 세 가지입니다. `<html data-theme="…">`로 바꿉니다.
- 컴포넌트 CSS에 색·글꼴·모서리 값을 직접 쓰지 않고 **반드시 토큰(`var(--…)`)**을 씁니다. 새 토큰이 필요하면 세 테마에 모두 값을 넣고 `design/DESIGN.md` 2.3에도 추가합니다.
- 테마별 장식은 `src/styles/themes.css`의 `[data-theme="…"]` 범위에만 둡니다. 장식이 크기나 배치를 바꾸면 안 됩니다.
- 데스크탑(1200px 이상)은 화면 전체 폭을 씁니다. 가운데 좁은 기둥 배치와 `max-width`로 내용 전체를 가두는 방식은 쓰지 않습니다.
- 상태는 색 + 기호 + 글자로 함께 표시합니다(●, ◔, ○, ◐, ✕). 이모지는 쓰지 않습니다.

### 접근성
- 버튼은 `<button>`, 링크는 `<a href>`, 입력은 `<label>`과 연결합니다. `div`에 클릭 이벤트를 달지 않습니다.
- 아이콘만 있는 버튼에는 `aria-label`을 붙입니다. 터치 영역은 최소 44×44px.
- 글자 대비 4.5:1 이상. 새 색을 넣으면 세 테마 모두에서 확인합니다.

### 저장과 개인정보
- `localStorage` 읽기·쓰기는 모두 `try/catch`로 감쌉니다. 저장이 안 되어도 앱은 동작해야 합니다.
- 키는 `ln.settings`, `ln.places`, `ln.favorites`, `ln.notes`이며 저장 형식에 버전 번호를 둡니다.
- 위치와 출발지 좌표는 브라우저 밖으로 보내지 않습니다(길찾기 링크를 누를 때만 예외).

### 지도
- OSM 타일 정책을 지킵니다: 출처 표기 필수, 타일 일괄 다운로드·오프라인 캐시 금지, 서비스 워커에서 타일 캐시 금지.
- 테마별 타일 필터는 `.leaflet-tile-pane`에만 적용합니다.
- 길찾기 링크 생성 코드는 `src/domain/directions.ts` 한 곳에 모읍니다.

## 작업 방식
- 작업은 PRD 11장의 마일스톤 순서(M0 → M5)로 진행합니다. 한 번에 한 기능씩 작게 만듭니다.
- 기능을 만들기 전에 관련 PRD 항목 번호(예: F5, 5.5)를 확인하고, 커밋 메시지나 PR 설명에 적습니다.
- 화면을 바꾼 뒤에는 개발 서버에서 **1440×900과 390×844**, **테마 3종**으로 확인합니다.
- 새 라이브러리를 추가하기 전에 사용자에게 먼저 묻습니다.
- `design/`과 `docs/`의 문서는 사용자가 요청할 때만 고칩니다. 구현하다가 문서와 다르게 해야 할 이유가 생기면 먼저 알립니다.

## 완료 기준 (작업마다)
- [ ] `npm run build` 통과 (타입 오류 없음)
- [ ] `npm run test` 통과, 바꾼 도메인 로직에 테스트 추가
- [ ] `npm run lint` 통과
- [ ] 화면 변경 시 1440×900·390×844, 테마 3종 확인
- [ ] 가로 스크롤이 생기지 않음 (스마트폰)

## 커뮤니케이션
- 사용자에게는 한국어 존댓말로 답하고, 맞춤법과 띄어쓰기를 지킵니다.
- 화면에 나오는 모든 문구는 한국어입니다. 코드의 변수·함수 이름은 영어로 씁니다.
- 코드 주석은 필요한 곳에만 한국어로 짧게 씁니다.

## 하지 않을 것
- 로그인, 서버, DB 추가
- 유료 API나 키가 필요한 서비스 추가 (카카오맵은 PRD F17·부록 A 단계에서만)
- 도서관 정보를 지어내기 (목업 이름·시간을 실제 데이터에 섞지 않기)
- 시험을 통과시키려고 테스트를 지우거나 건너뛰기
