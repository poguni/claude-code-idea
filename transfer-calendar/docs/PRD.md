# 편입 캘린더(TExam Calendar): 제품 기획서(PRD)

> 2027학년도 대학 편입 시험 일정을 4개월 달력으로 관리하는 가족용 웹 앱

| 항목 | 내용 |
|---|---|
| 문서 버전 | v0.3 |
| 작성일 | 2026-11-15 |
| 앱 이름 | **편입 캘린더(TExam Calendar)** |
| 형태 | 반응형 웹 앱(PWA). 스마트폰 조회 중심(아이폰 15 Pro 최적화), 데스크톱 입력 지원 |
| 대상 기간 | **2026년 11월 ~ 2027년 2월(4개월)**. 원서 접수부터 최종 발표·등록·충원까지 포함 |
| 사용자 | 편입 준비생(자녀) 1명 + 부모. 같은 데이터를 함께 봄 |
| 테마 | 원고지(기본)·플래너 2종, 설정에서 선택 (`design/DESIGN.md`) |
| 백엔드 | Supabase 기존 프로젝트 **Link Plan**을 공유. 로그인 계정을 함께 쓰고, 테이블은 접두사 `tcal_`로 분리 |

## 변경 이력

| 버전 | 날짜 | 주요 변경 |
|---|---|---|
| v0.1 | 2026-11-15 | 최초 작성. 자료 조사 결과를 바탕으로 입력 항목 확정(캠퍼스·전형 유형·지원 학년, 영어·면접·서류 평가 외 시험 종류, 공인 영어 성적, 주차·숙박·점심 항목 제외). Supabase(Link Plan) 공유, 인쇄 2종, D-day·겹침 경고·.ics 내보내기 포함 |
| v0.2 | 2026-11-15 | 달력 기간을 2026년 11월 ~ 2027년 2월 4개월로 확대(2월 최종 발표·등록·충원 포함). 인쇄를 세로 4장(한 달 한 장)·가로 2장(두 달 한 장)으로 바꾸고 인쇄할 달 선택 추가. 설날 연휴 공휴일 표시 추가. "달력 밖 일정" 목록 삭제 |
| v0.3 | 2026-11-15 | 앱 이름을 "편입 캘린더(TExam Calendar)"로 확정. 디자인 시안 A(원고지)·C(플래너)를 테마로 구현하고 설정에서 선택(F13 추가). 플래너 글꼴은 제목·큰 숫자 고운돋움, 작은 글씨 프리텐다드. 다크 모드 제외. 9장을 DESIGN.md 요약으로 교체 |

---

## 1. 개요

### 1.1 배경
편입 시험은 보통 12월부터 이듬해 1월까지 치릅니다. 수험생 한 명이 10곳 넘게 지원하는 경우가 흔합니다. 대학마다 원서 접수, 필기시험, 1단계 발표, 서류 제출, 면접 일정이 따로 있고, 같은 날 오전·오후로 두 대학 시험이 겹치기도 합니다. 일정과 수험 번호, 시험 장소, 교통 정보가 대학 홈페이지와 원서 접수 사이트(진학어플라이·유웨이)에 흩어져 있어 한눈에 관리하기 어렵습니다.

자료 조사로 확인한 일반적인 흐름은 다음과 같습니다(대학마다 다르므로 모집요강으로 확인).

| 시기 | 일정 | 참고 예시 |
|---|---|---|
| 11월 말 ~ 12월 초 | 대학별 모집요강 발표 | 연세대 2026학년도 요강 2025-11-28 게시 |
| 12월 초 ~ 중순 | 원서 접수(진학어플라이·유웨이 대행) | 건국대 2026학년도 12/2~12/4, 한양대 2025학년도 12/10~12/12 |
| 12월 하순 ~ 1월 | 필기시험(영어 등) | 건국대 2026학년도 12/24 |
| 1월 | 1단계 합격 발표, 서류 제출, 면접 | 건국대 1단계 발표 1/6 |
| 2월 | 최종 합격 발표, 등록, 1~3차 충원 | 한림대 2027학년도 연계편입 발표 2/4 예정 |

### 1.2 목표
- 지원한 모든 대학의 일정과 정보를 **11월~2월 4개월 달력 한 곳**에서 확인합니다. 원서 접수(11월 말~12월)부터 시험(12~1월), 최종 발표·등록·충원(2월)까지 이어서 봅니다.
- 스마트폰에서 날짜를 누르면 그날의 시험 정보(시각, 수험 번호, 장소, 교통, 준비물)를 **한 화면에서** 봅니다.
- 다음 시험까지 남은 날(D-day)과 **겹치는 일정**을 자동으로 알려 줍니다.
- 아이폰 캘린더로 일정을 내보내 알림을 받습니다(.ics).
- A4로 인쇄해 벽에 붙입니다(세로형: 한 달 한 장, 가로형: 두 달 한 장).
- 자녀와 부모가 각자의 기기에서 **같은 데이터**를 봅니다.

### 1.3 범위에서 제외하는 것 (Non-goals)
- 대학 모집요강 자동 수집(크롤링), 원서 접수 사이트 연동
- 가족 외 사용자 공유, 회원가입 기능(계정은 Link Plan 기존 계정 사용)
- 푸시 알림 서버(알림은 아이폰 캘린더 내보내기로 대신함)
- 편입 영어 학습 기능, 합격 예측
- 일반편입 외 전형(학사편입·특별편입 등) 전용 항목, 2학년 편입 관련 항목

---

## 2. 사용자와 사용 시나리오

### 2.1 사용자
| 사용자 | 역할 | 주 사용 기기 |
|---|---|---|
| 자녀(편입 준비생) | 대학·일정 입력, 서류·준비물 체크, 시험 당일 확인 | 아이폰 15 Pro, 데스크톱 |
| 부모 | 일정 확인, 함께 입력·수정, 인쇄 | 스마트폰, 데스크톱 |

두 사람 모두 같은 권한(조회·입력·수정·삭제)을 가집니다.

### 2.2 대표 시나리오
1. **모집요강 발표 후 입력(데스크톱)**: 자녀가 건국대 모집요강을 보고 대학을 등록합니다(학과, 시험 종류, 반영 비율, 전형료). 이어서 원서 접수 기간, 필기시험, 1단계 발표, 면접 일정을 추가합니다.
2. **날짜에서 바로 입력(스마트폰)**: 달력에서 12월 24일을 누르고 "이 날짜에 일정 추가"를 눌러 대학과 일정 종류를 고르면, 날짜가 미리 채워진 입력 폼이 열립니다.
3. **겹침 확인**: 1월 10일에 두 대학 시험을 입력하자 "같은 날 오전 시험과 오후 시험 간격이 90분입니다(기준 120분)"라는 주의 표시가 뜹니다.
4. **2월 발표·등록 관리**: 2월 달력에서 대학별 최종 발표일과 등록금 납부 마감일, 충원 발표 차수를 확인하고, 합격한 대학 중 등록할 곳을 정합니다.
5. **수험표 출력 후 정보 보완**: 원서 접수 후 수험 번호, 고사장, 입실 시각을 입력합니다. 교통 경로와 소요 시간을 넣으면 권장 출발 시각이 자동으로 계산됩니다.
6. **시험 전날 확인(스마트폰)**: 첫 화면 위쪽에 "D-1 건국대 영어 12/24(목) 입실 09:30"이 보입니다. 누르면 수험 번호, 장소 지도 링크, 대중교통, 준비물 체크리스트가 나옵니다.
7. **부모 확인**: 부모가 자신의 스마트폰에서 같은 달력을 보고, 데스크톱에서 A4로 인쇄해 냉장고와 책상 앞에 붙입니다.
8. **캘린더 알림**: 전체 일정을 .ics로 내보내 아이폰 캘린더에 추가하고, 전날 저녁과 2시간 전에 알림을 받습니다.

---

## 3. 프로젝트 폴더 구조와 개발 방식

Claude Code로 구현할 때는 `docs/`에 기획 문서를, `design/`에 디자인 가이드와 시안을 두고 시작합니다.

```text
transfer-calendar/
├── CLAUDE.md                 # Claude Code 작업 규칙(아래 3.2 참고)
├── docs/
│   ├── PRD.md                # 이 문서(기능 명세의 기준)
│   └── PRD.html              # 같은 내용의 HTML 보기용
├── design/
│   ├── DESIGN.md             # 디자인 가이드(색, 글꼴, 간격, 컴포넌트, 인쇄 규칙)
│   ├── design-mockup.html    # 화면 시안(모바일 달력, 상세 시트, 입력 폼, 인쇄 2종)
│   └── print-samples/        # 인쇄 시안 PDF(세로 4장, 가로 2장)
├── supabase/
│   └── migrations/
│       └── 20261115000000_tcal_init.sql   # 7장의 테이블·RLS 정의
├── src/                      # 앱 소스(8장 기술 스택)
├── tests/                    # 단위 테스트(D-day, 겹침, .ics, 출발 시각), E2E(아이폰 15 Pro 화면)
├── .env.example              # VITE_SUPABASE_URL, VITE_SUPABASE_ANON_KEY (값은 커밋하지 않음)
└── package.json
```

### 3.1 문서 간 역할
| 문서 | 정하는 것 | 충돌할 때 |
|---|---|---|
| `docs/PRD.md` | 무엇을 만드는가(기능, 데이터, 규칙) | 기능·데이터는 PRD가 우선 |
| `design/DESIGN.md` | 어떻게 보이는가(색, 글꼴, 배치, 인쇄 모양) | 시각 표현은 DESIGN이 우선 |
| `design/design-mockup.html` | 화면 시안 | DESIGN.md와 다르면 DESIGN.md가 우선 |

### 3.2 CLAUDE.md에 넣을 규칙(제안)
- 기능 명세는 `docs/PRD.md`, 시각 디자인은 `design/DESIGN.md`를 따른다.
- Supabase에는 **`tcal_` 접두사가 붙은 객체만** 만들고 고친다. Link Plan의 기존 테이블·함수·트리거·정책·Auth 설정(Site URL 등)은 읽기만 하고 바꾸지 않는다.
- 스키마 변경은 반드시 `supabase/migrations/`의 SQL 파일로 남긴다.
- 날짜·시간 계산은 모두 `Asia/Seoul` 기준으로 하고, 관련 함수에는 단위 테스트를 둔다.
- 화면 확인은 Playwright의 `iPhone 15 Pro` 기기 설정(393×852)과 데스크톱(1280×800)에서 한다.
- 인쇄 화면은 Playwright `page.pdf({ format: 'A4' })`로 PDF를 만들어 확인한다.

### 3.3 design 폴더
`design/DESIGN.md`(v0.2)와 `design/design-mockup.html`이 준비되어 있습니다. 두 테마(원고지·플래너)의 토큰, 글꼴, 컴포넌트, 인쇄 규칙은 DESIGN.md를 따릅니다(요약은 9장).

---

## 4. 입력 항목 정의

대학을 먼저 등록하고(4.1~4.6, 4.8), 그 대학에 일정을 여러 개 추가합니다(4.7). 필수 항목은 **대학 이름, 약칭, 학과** 세 가지뿐이고, 나머지는 정보가 나오는 대로 채웁니다.

### 4.1 A. 기본 정보
| 항목 | 입력 형식 | 필수 | 비고 |
|---|---|---|---|
| 지원 대학 | 텍스트 | ✔ | 예: 건국대학교 |
| 약칭 | 텍스트(2~4자) | ✔ | 달력 칸에 표시. 예: 건국, 성대 |
| 지원 학과(모집 단위) | 텍스트 | ✔ | 예: 경영학과 |
| 모집 인원 | 숫자 | | 모집요강 발표 후 |
| 경쟁률 | 숫자(소수 둘째 자리) | | 예: 25.28 → "25.28 : 1"로 표시 |
| 지원 우선순위 | 선택 | | 1지망 / 상향 / 적정 / 안정. 일정이 겹칠 때 판단 기준 |
| 표시 색상 | 색 선택(12색 팔레트) | | 등록 순서대로 자동 배정, 변경 가능 |

> 제외: 캠퍼스, 전형 유형(일반편입만 지원), 지원 학년(3학년만 지원)

### 4.2 B. 전형 방법
| 항목 | 입력 형식 | 비고 |
|---|---|---|
| 시험 종류 | 체크박스(복수 선택) | **영어 / 면접 / 서류 평가** |
| 반영 비율 | 텍스트 | 예: "1단계 영어 100% → 2단계 1단계 성적 60% + 면접 40%" |
| 단계 구분 | 선택 | 1단계만 / 1·2단계 |
| 문항 수·시험 시간 | 텍스트 | 예: 영어 40문항 60분 |

> 제외: 수학, 전공, 논술, 적성, 실기, 기타 시험 종류, 공인 영어 성적 반영 여부

### 4.3 C. 일정
일정은 대학마다 여러 개를 등록합니다. 저장 형식은 4.7을 따릅니다.

| 일정 종류 | 코드 | 날짜 형식 | 시간 | 달력 표시 |
|---|---|---|---|---|
| 원서 접수 | `apply` | 기간(시작~마감) | 마감 시각 | 시작일 "접수", 마감일 "마감" |
| 필기시험(영어) | `exam` | 하루 | 입실·시작·종료 | 강조(채움) |
| 고사장 발표 | `venue_notice` | 하루 | 시각 | 보조 |
| 1단계 합격 발표 | `first_result` | 하루 | 시각 | 보조 |
| 서류 제출 마감 | `doc_deadline` | 하루 또는 기간 | 마감 시각 | 마감일 "서류" |
| 면접 | `interview` | 하루 | 입실·시작·종료 | 강조(채움) |
| 최종 합격 발표 | `final_result` | 하루 | 시각 | 보조 |
| 등록금 납부 | `payment` | 기간 | 마감 시각 | 마감일 "등록" |
| 충원(추가합격) 발표 | `extra_result` | 하루 | 시각, 차수 | 보조 |
| 기타 | `other` | 하루 또는 기간 | 선택 | 보조 |

- 필기시험과 면접은 **입실 시각**과 **시작 시각**을 따로 받습니다. 종료 시각이 없으면 기본 시험 시간(설정, 기본 120분)으로 계산합니다.
- 고사장이 대학 기본 장소와 다르면(예: 면접 장소가 다른 건물) 일정마다 장소를 따로 입력할 수 있습니다.

### 4.4 D. 수험 정보
| 항목 | 입력 형식 | 비고 |
|---|---|---|
| 원서 접수처 | 체크박스(하나만 선택) | 진학어플라이 / 유웨이 / 대학 자체 |
| 수험 번호 | 텍스트 | 상세 화면에서 누르면 복사 |
| 접수 번호 | 텍스트 | 수험 번호와 다를 때 |
| 전형료 | 숫자(원) | 지원 현황에서 합계 표시 |
| 전형료 결제 완료 | 체크 | |
| 수험표 출력 완료 | 체크 | |
| 입학처 전화 | 전화번호 | 누르면 전화 걸기 |
| 입학처 홈페이지 | URL | |
| 모집요강 링크 | URL | |

### 4.5 E. 준비 서류
| 항목 | 입력 형식 | 비고 |
|---|---|---|
| 준비 서류 | 체크박스(복수 선택) | **학업 계획서 / 자기소개서 / 대학 성적 증명서 / 수료·졸업 예정 증명서 / 기타(직접 입력)** |
| 서류별 진행 상태 | 3단계 선택 | 준비 중 → 발급·작성 완료 → 제출 완료 |
| 제출 방법 | 선택 | 온라인 업로드 / 우편 / 방문 |
| 우편 마감 기준 | 선택 | 소인 기준 / 도착 기준 (우편일 때만) |
| 원본 대조 필요 | 체크 | |
| 서류 메모 | 텍스트 | 예: "성적 증명서 영문 1부" |

- 서류 제출 마감일은 C의 `doc_deadline` 일정으로 등록하고, 서류 목록과 연결해 보여 줍니다.
- 학사편입·특별편입·공인 영어 관련 서류는 이번 범위에서 뺍니다(일반편입만 지원, 공인 영어 반영 항목 제외에 따름).

### 4.6 F. 시험 장소와 교통
| 항목 | 입력 형식 | 비고 |
|---|---|---|
| 시험 장소 | 텍스트 | 건물, 호실까지. 예: "공학관 A동 301호" |
| 지도 링크 | URL | 네이버 지도·카카오맵 공유 링크. 비우면 장소 이름으로 지도 검색 링크를 만듦 |
| 대중교통 | 여러 줄 텍스트 | 노선, 환승, 출구 번호. 예: "2호선 건대입구역 2번 출구 → 도보 10분" |
| 예상 소요 시간 | 숫자(분) | 집에서 시험장까지 |
| 여유 시간 | 숫자(분) | 기본 30분 |
| 권장 출발 시각 | 자동 계산 | **입실 시각 − 예상 소요 시간 − 여유 시간** |

> 제외: 주차 가능 여부, 숙박 정보, 점심

### 4.7 일정 하나의 저장 항목
| 항목 | 형식 | 비고 |
|---|---|---|
| 대학 | 등록된 대학 선택 | 필수 |
| 일정 종류 | 4.3의 코드 | 필수 |
| 시작 날짜 | 날짜 | 필수. 날짜를 눌러 추가하면 미리 채움 |
| 종료 날짜 | 날짜 | 기간 일정일 때 |
| 입실 시각 / 시작 시각 / 종료 시각 | 시각 | 시험·면접 |
| 장소(일정별) | 텍스트, 지도 링크 | 비우면 대학 기본 장소 |
| 차수 | 숫자 | 충원 발표 |
| 메모 | 텍스트 | |

### 4.8 G. 시험 당일 준비물 · H. 결과와 메모
**G. 준비물 체크리스트(대학별)**: 대학을 등록하면 아래 기본 항목이 자동으로 들어가고, 더하거나 지울 수 있습니다.
- 수험표(2~3장 출력), 실물 신분증(주민등록증·여권·운전면허증. 모바일 신분증 인정 여부는 대학 공지 확인), 컴퓨터용 사인펜, 수정테이프(허용 여부 확인), 아날로그 손목시계, 교통카드, 소액 현금
- 반입 금지 물품 메모(텍스트)

**H. 결과와 메모**
| 항목 | 입력 형식 |
|---|---|
| 시험 후기·가채점 | 여러 줄 텍스트 |
| 결과 | 대기 / 1단계 합격 / 최종 합격 / 예비(번호 입력) / 불합격 |
| 자유 메모 | 여러 줄 텍스트 |

---

## 5. 기능 요구사항

우선순위: **P0** 첫 사용 버전에 반드시 포함, **P1** 시험 시작 전까지 포함, **P2** 여유가 되면.

### F1. 로그인과 가족 공유 (P0)
1. Link Plan과 같은 Supabase Auth 계정으로 로그인합니다. 이 앱에는 **회원가입 화면을 두지 않습니다**(가입은 Link Plan에서, 또는 Supabase 대시보드에서 계정 생성).
2. 처음 로그인한 사람이 "가족"을 만들면 8자리 초대 코드가 생깁니다. 다른 사람은 초대 코드를 입력해 같은 가족에 들어옵니다. 역할은 자녀/부모로 표시만 하고 권한은 같습니다.
3. 모든 데이터는 가족 단위로 저장되고, 같은 가족 구성원만 읽고 쓸 수 있습니다(RLS, 7.3).
4. 로그인 상태를 유지해 시험 당일 다시 로그인하지 않아도 되게 합니다.
5. 다른 구성원이 수정한 내용은 화면을 다시 열거나 당겨서 새로고침하면 반영됩니다. (P1) Supabase Realtime으로 즉시 반영합니다.

**완료 기준**: 자녀 계정으로 입력한 일정이 부모 계정에서 보이고, 가족이 아닌 계정으로는 조회되지 않습니다.

### F2. 대학 등록과 관리 (P0)
1. 4.1~4.6, 4.8 항목으로 대학을 등록·수정·삭제합니다. 삭제하면 그 대학의 일정·서류·준비물도 함께 지웁니다(확인 창 표시).
2. 입력 폼은 A~H 섹션으로 나누고, 처음에는 A(기본 정보)만 펼칩니다. 모바일은 한 열, 데스크톱은 두 열로 배치합니다.
3. 저장 전 필수 항목(대학, 약칭, 학과)을 확인합니다. 약칭이 다른 대학과 같으면 경고합니다.
4. 대학 목록(지원 현황) 화면: 우선순위·색·결과로 정렬하고, 전형료 합계와 결제 완료 여부, 수험표 출력 여부를 한눈에 봅니다.

**완료 기준**: 대학 12곳을 등록하고 목록에서 전형료 합계가 맞게 나옵니다.

### F3. 날짜를 눌러 일정 입력 (P0)
1. 달력에서 날짜를 누르면 그날의 상세 시트가 열리고, 맨 아래에 **"이 날짜에 일정 추가"** 버튼이 있습니다.
2. 버튼을 누르면 ① 대학 선택(등록된 대학 목록 + "새 대학 등록") ② 일정 종류 선택 ③ 시간·장소 입력 순서로 진행하며, 날짜는 미리 채워집니다.
3. 대학 상세 화면에서도 "일정 추가"로 같은 폼을 엽니다.
4. 기간 일정(원서 접수 등)은 시작·종료 날짜를 함께 고릅니다.
5. 저장할 때 F7의 겹침 검사를 하고, 겹치면 경고를 보여 주지만 저장은 할 수 있습니다.
6. 날짜·시각은 기기 기본 선택기(`input type="date"`, `type="time"`)를 씁니다.

**완료 기준**: 아이폰에서 날짜를 누르고 3번 이내의 화면 이동으로 시험 일정을 저장합니다.

### F4. 달력 보기 (P0)
1. **2026년 11월, 12월, 2027년 1월, 2월 네 달**을 보여 줍니다. 모바일은 한 달씩 보고 위쪽 탭(11월/12월/1월/2월)이나 좌우 밀기로 바꿉니다. 데스크톱은 두 달씩 나란히 보거나, 넓은 화면(1440px 이상)에서는 네 달을 2×2로 한 번에 봅니다.
2. 주 시작은 일요일입니다. 행 수가 달라도 칸 크기가 흔들리지 않게 합니다.

   | 달 | 1일 요일 | 마지막 날 | 주(행) 수 |
   |---|---|---|---|
   | 2026년 11월 | 일요일 | 30일 월요일 | 5주 |
   | 2026년 12월 | 화요일 | 31일 목요일 | 5주 |
   | 2027년 1월 | 금요일 | 31일 일요일 | 6주 |
   | 2027년 2월 | 월요일 | 28일 일요일 | 5주 |

3. 공휴일은 빨간 글씨로 표시합니다. 일요일도 빨간 글씨, 토요일은 파란 글씨입니다.
   - 12/25(금) 성탄절, 1/1(금) 신정
   - 2/6(토)~2/8(월) 설날 연휴, 2/9(화) 대체공휴일(연휴 중 일요일과 겹쳐서 생김. 정부 공고로 최종 확인)
   - 11월에는 공휴일이 없습니다.
   - 공휴일 목록은 코드 안 상수(`HOLIDAYS`)로 관리해, 바뀌면 한 곳만 고칩니다.
4. 날짜 칸에는 일정을 **대학 색 + 약칭**으로 표시합니다. 시험·면접은 채운 칩, 그 밖의 일정은 테두리 칩으로 구분합니다. 칸 하나에 칩은 최대 3개까지 보이고 나머지는 "+2"처럼 표시합니다.
5. 겹침이 있는 날짜에는 경고 표시(⚠)를 붙입니다(F7).
6. 오늘 날짜에 테두리를 표시합니다. 처음 열면 오늘이 속한 달을 보여 줍니다. 기간 전이면 11월, 기간이 끝난 뒤면 2월을 보여 줍니다.
7. 보기 필터: 전체 / 시험·면접만 / 마감만. 대학별 표시 끄기·켜기.
8. 일정 입력 범위는 달력 기간과 같은 **2026-11-01 ~ 2027-02-28**입니다. 범위를 벗어난 날짜는 저장하지 않고 안내합니다.
9. 달마다 일정 수 요약(예: "12월 · 시험 6 · 마감 9")을 탭 아래에 작게 보여 줍니다.

**완료 기준**: 393×852 화면에서 6주짜리 1월을 포함해 어느 달이든 한 달 전체가 스크롤 없이 보이고, 일정 3개가 있는 칸의 약칭이 잘리지 않습니다. 네 달 사이를 탭과 밀기로 오갈 수 있습니다.

### F5. 날짜·일정 상세 보기 (P0)
1. 날짜를 누르면 아래에서 올라오는 상세 시트(바텀 시트)에 그날의 일정을 시간순 카드로 보여 줍니다. 데스크톱은 오른쪽 패널에 보여 줍니다.
2. 시험·면접 카드에 들어갈 정보:
   - 대학 색 띠, 대학·학과, 일정 종류, **D-day**
   - 입실 시각·시작 시각·종료 시각
   - 수험 번호(누르면 복사), 원서 접수처
   - 시험 장소(누르면 지도 앱), 대중교통, **권장 출발 시각**
   - 준비물 체크리스트(바로 체크 가능), 반입 금지 메모
   - 입학처 전화(누르면 전화), 모집요강 링크
3. 그 밖의 일정 카드: 대학, 일정 종류, 시각·마감, 관련 서류 진행 상태, 메모.
4. 카드에서 "대학 상세", "수정", "삭제", "캘린더에 추가(.ics)"를 할 수 있습니다.
5. 대학 상세 화면은 A~H 정보를 섹션별로 보여 주고, 그 대학의 일정을 시간순으로 나열합니다.

**완료 기준**: 시험 당일 상세 시트 하나만 보고 출발 시각, 장소, 수험 번호, 준비물을 모두 확인할 수 있습니다.

### F6. D-day 표시 (P0)
1. 첫 화면 위쪽에 **다음 시험(필기·면접) 카드**를 보여 줍니다. 예: "D-3 · 건국 영어 · 12/24(목) 입실 09:30".
2. 카드를 누르면 다가오는 시험·면접 목록(D-day 순)을 펼칩니다.
3. 상세 카드와 대학 상세 화면에도 D-day를 표시합니다. 원서 접수·서류 마감은 "마감 D-2"처럼 따로 표시합니다.
4. 계산 규칙: 서울 시간(Asia/Seoul) 기준 **날짜 차이**로 계산합니다. 당일은 "D-DAY", 지난 일정은 "완료"로 표시하고 목록에서 뺍니다.
5. 같은 날 시험이 둘이면 이른 시각의 시험을 먼저 보여 줍니다.

**완료 기준**: 자정 직전·직후, 시험 당일, 지난 일정에서 D-day가 맞게 나옵니다(단위 테스트).

### F7. 일정 겹침 경고 (P0)
검사 대상은 시각이 있는 **필기시험과 면접**입니다.

| 등급 | 조건 | 표시 |
|---|---|---|
| 충돌(빨강) | 두 일정의 시간(입실~종료)이 겹침 | 달력 칸 ⚠ 빨강, 저장 시 경고 창 |
| 주의(주황) | 같은 날, 앞 일정 종료와 뒤 일정 입실 사이 간격이 기준(기본 120분)보다 짧음 | 달력 칸 ⚠ 주황, 저장 시 경고 |
| 참고(회색) | 같은 날 두 일정이 있지만 간격이 충분함 | 상세 시트에 "같은 날 일정 2개" 안내 |

1. 종료 시각이 없으면 시작 시각 + 기본 시험 시간(120분)으로 계산합니다. 입실 시각이 없으면 시작 시각을 씁니다.
2. 시각이 없는 같은 날 시험 두 개는 "주의"로 봅니다.
3. 경고 문구에는 두 대학과 간격을 씁니다. 예: "건국 영어 종료 12:00 → 성대 영어 입실 13:30, 간격 90분(기준 120분)".
4. 겹침 목록 화면(또는 첫 화면 알림 영역)에 모든 충돌·주의를 모아서 보여 주고, 각 항목에 두 대학의 우선순위를 함께 보여 줘 어느 쪽을 볼지 판단하게 돕습니다.
5. 기준 간격과 기본 시험 시간은 설정에서 바꿉니다.

**완료 기준**: 겹침 판정 함수에 대해 경계값(간격 정확히 120분, 종료 = 입실, 시각 없음) 단위 테스트가 통과합니다.

### F8. 아이폰 캘린더로 내보내기(.ics) (P0)
1. 내보내기 범위: **전체 일정 / 대학 하나 / 일정 하나 / 시험·면접만**.
2. 파일 형식(RFC 5545):
   - `BEGIN:VCALENDAR`, `VERSION:2.0`, `PRODID:-//TExam Calendar//KO`, `CALSCALE:GREGORIAN`, `METHOD:PUBLISH`, `X-WR-CALNAME:편입 시험 2027`
   - 시간대는 `VTIMEZONE`(Asia/Seoul, UTC+09:00, 일광 절약 시간 없음)과 `DTSTART;TZID=Asia/Seoul:...`로 씁니다.
   - 시각이 없는 일정은 종일 일정(`DTSTART;VALUE=DATE`, 종료일은 다음 날)으로, 기간 일정은 시작~종료 종일 일정으로 만듭니다.
   - `UID`는 일정 ID 기반으로 고정(`{event_id}@tcal`)해 다시 내보내도 같은 값을 씁니다.
   - `SUMMARY`: "[건국] 영어 필기" / `LOCATION`: 시험 장소 / `DESCRIPTION`: 학과, 수험 번호, 입실 시각, 권장 출발 시각, 대중교통, 준비물 / `URL`: 모집요강 링크
   - 알림(`VALARM`): 시험·면접은 **전날 20:00, 2시간 전**, 마감 일정은 **전날 09:00**(설정에서 변경)
   - 줄바꿈 CRLF, 75바이트 줄 접기, 쉼표·세미콜론·줄바꿈 이스케이프를 지킵니다.
3. 다운로드는 `text/calendar` 형식으로 하고, 파일 이름은 `편입시험_2027_전체.ics`처럼 붙입니다. 아이폰 Safari에서 열면 "캘린더에 추가" 화면이 나옵니다.
4. 홈 화면에 추가한 PWA 모드에서 다운로드가 막히면 공유 시트(`navigator.share`에 파일 전달)로 대신하고, 그래도 안 되면 "Safari에서 열기"를 안내합니다.
5. 같은 파일을 다시 가져오면 아이폰 캘린더에 중복으로 들어갈 수 있으므로, 내보내기 화면에 "변경된 일정만 내보내기(마지막 내보내기 이후 수정분)"를 두고 안내 문구를 넣습니다.
6. (P2) Supabase Edge Function으로 구독용 주소(webcal)를 만들어, 앱에서 바꾸면 아이폰 캘린더에도 자동 반영되게 합니다.

**완료 기준**: 내보낸 파일을 아이폰 15 Pro 실기기 Safari에서 열어 일정과 알림이 서울 시간으로 맞게 들어갑니다.

### F9. A4 인쇄 (P0)
두 가지 형식을 모두 만들고, 인쇄 미리보기 화면에서 고릅니다. 네 달 전체를 인쇄하면 아래와 같습니다.

| 형식 | 구성 | 칸에 들어가는 내용 | 아래쪽 범례 |
|---|---|---|---|
| **세로형(한 달 한 장)** | 4장: 11월, 12월, 1월, 2월 | 약칭, 일정 종류 기호, 시각, (선택) 수험 번호 | 그 달에 일정이 있는 대학의 표: 약칭·대학·학과·수험 번호·일정·장소 |
| **가로형(두 달 한 장)** | 2장: 11·12월, 1·2월 | 약칭, 일정 종류 기호, 시각 | 약칭·대학·수험 번호 한 줄 요약 |

- **인쇄할 달 선택**: 11월·12월·1월·2월을 체크박스로 골라 인쇄합니다(기본: 네 달 모두). 예를 들어 12월·1월만 고르면 세로 2장 또는 가로 1장이 됩니다. 가로형에서 한 달만 고르면 그 달만 한쪽에 인쇄합니다.
- 범례의 대학별 표는 세로형에서는 달마다, 가로형에서는 장마다 그 장에 나온 대학만 넣어 자리를 아낍니다.

1. 인쇄 전용 화면(`/print/portrait`, `/print/landscape`)을 따로 두고 각각 `@page { size: A4 portrait; margin: 10mm }`, `@page { size: A4 landscape; margin: 8mm }`를 씁니다. 크기는 mm 단위로 잡습니다.
2. 인쇄 옵션:
   - 포함할 일정: 시험·면접만 / 전체
   - 수험 번호 표시 켜기·끄기(벽에 붙일 때 가리고 싶으면 끔)
   - 흑백 모드: 색 대신 약칭과 기호(● 필기, ◆ 면접, ▲ 마감, ○ 발표)로 구분
3. 제목(예: "2027 편입 시험 일정 · 2026년 12월", 가로형은 "· 2027년 1~2월")과 인쇄 날짜를 넣어, 언제 인쇄한 것인지 알 수 있게 합니다. 바닥글에 "편입 캘린더(TExam Calendar)"를 넣습니다.
4. 인쇄물에도 선택한 테마의 글꼴과 머리글 강조색을 씁니다. 바탕은 두 테마 모두 흰색입니다.
5. 칸이 넘칠 때는 글자를 줄이지 않고 "+2"로 줄인 뒤 범례 표에서 확인하게 합니다. 칸·표가 페이지 경계에서 잘리지 않게 합니다(`break-inside: avoid`).
6. 대학 색이 인쇄되도록 `print-color-adjust: exact`를 쓰고, 화면용 메뉴·버튼은 숨깁니다.
7. Safari는 `@page size`를 무시할 수 있으므로 인쇄 화면에 "인쇄 창에서 방향을 세로/가로로 선택하세요"를 안내합니다. 데스크톱 크롬 인쇄를 기준으로 확인합니다.
8. "PDF로 저장"으로 파일을 만들어 휴대폰에서 인쇄소·편의점 출력도 할 수 있게 안내합니다.

**완료 기준**: 일정 40개(대학 12곳, 4개월) 기준으로 세로 4장, 가로 2장 각각 A4에 넘치지 않고 출력됩니다. 6주짜리 1월도 세로·가로 모두 한 장 안에 들어갑니다. 두 형식을 실제로 인쇄해 비교한 뒤 기본값을 정합니다.

### F10. 서류·준비물 체크리스트 (P1)
1. 대학별 준비 서류의 진행 상태를 바꾸고, 서류 제출 마감 D-day와 함께 보여 줍니다.
2. 전체 서류 화면: 모든 대학의 미제출 서류를 마감일 순으로 모아 보여 줍니다.
3. 준비물 체크리스트는 시험 전날 확인용으로, 시험이 끝나면 다음 시험을 위해 "체크 초기화"를 할 수 있습니다.

### F11. 설정 (P1)
테마(F13), 가족 이름·초대 코드, 구성원 목록, 겹침 기준 간격(기본 120분), 기본 시험 시간(기본 120분), 기본 여유 시간(기본 30분), .ics 알림 시각, 로그아웃.

### F12. 백업 (P2)
가족 데이터 전체를 JSON 파일로 내려받기(혹시 모를 Supabase 문제 대비).


### F13. 테마 선택 (P0)
1. 테마 2종: **원고지**(기본, 크림 바탕·파랑 강조·IBM Plex Sans KR)와 **플래너**(연보라 바탕·코랄 강조·둥근 모양, 제목·큰 숫자 고운돋움 + 작은 글씨 프리텐다드).
2. 두 테마는 화면 구조와 기능이 같고 색·글꼴·모서리 같은 디자인 토큰만 다릅니다(`design/DESIGN.md` 1장).
3. 더보기 › 설정 › 테마에서 미리보기 카드를 눌러 고르면 바로 적용합니다.
4. 선택은 기기마다 저장합니다(localStorage). 자녀와 부모가 서로 다른 테마를 써도 됩니다. 가족 공유 데이터에는 넣지 않습니다.
5. 첫 화면을 그리기 전에 저장된 테마를 적용해 깜빡임이 없게 하고, PWA 상태 표시줄 색도 테마에 맞춥니다.
6. 테마 목록은 설정 파일 하나로 관리해, 나중에 다크 테마 등을 더하기 쉽게 합니다.

**완료 기준**: 아이폰에서 테마를 바꾸면 달력·시트·폼·인쇄 화면이 모두 새 테마로 바뀌고, 앱을 다시 열어도 유지됩니다. 두 테마에서 기능 차이가 없습니다.

---

## 6. 화면 구성과 반응형 기준

### 6.1 화면 목록
| 화면 | 주요 요소 | 기능 |
|---|---|---|
| 로그인 | 이메일·비밀번호(Link Plan과 같은 로그인 방식) | F1 |
| 가족 만들기·참여 | 가족 만들기, 초대 코드 입력 | F1 |
| **달력(첫 화면)** | D-day 카드, 겹침 알림, 11월/12월/1월/2월 탭, 달력, 필터 | F4, F6, F7 |
| **날짜 상세 시트** | 그날 일정 카드, "이 날짜에 일정 추가" | F5, F3 |
| 일정 입력 폼 | 대학 선택 → 종류 → 시간·장소 | F3 |
| 대학 목록(지원 현황) | 대학 카드, 우선순위·결과, 전형료 합계 | F2 |
| 대학 상세 | A~H 섹션, 일정 목록, 서류, 준비물 | F2, F5, F10 |
| 대학 입력 폼 | A~H 섹션(접기·펼치기) | F2 |
| 겹침 목록 | 충돌·주의 목록 | F7 |
| 내보내기 | .ics 범위 선택, 다운로드·공유 | F8 |
| 인쇄 미리보기 | 세로형/가로형 탭, 인쇄할 달 선택, 인쇄 옵션 | F9 |
| 설정 | 테마 선택, F11 항목 | F11, F13 |

모바일은 아래쪽 탭 막대(달력 / 대학 / 서류 / 더보기)를 두고, 데스크톱은 왼쪽 메뉴 + 본문 + 오른쪽 상세 패널 구성입니다.

### 6.2 아이폰 15 Pro 최적화
| 항목 | 기준 |
|---|---|
| 화면 | 6.1인치, 2556×1179 픽셀, **CSS 기준 393×852**, 기기 화소 배율 3 |
| 안전 영역 | 위쪽 다이내믹 아일랜드·상태 표시줄 약 59pt, 아래쪽 홈 인디케이터 약 34pt. `viewport-fit=cover`와 `env(safe-area-inset-*)`로 여백 확보 |
| 높이 | Safari 주소창 높이가 바뀌므로 `100dvh` 사용 |
| 달력 칸 | 좌우 여백 12pt 기준 칸 너비 약 52pt. 약칭 2~4자 칩을 세로로 최대 3개 |
| 터치 영역 | 버튼·칩 최소 44×44pt |
| 글자 | 본문 16px 이상. **입력창 글자를 16px 이상**으로 해서 iOS의 입력 시 자동 확대를 막음 |
| 입력 | 기기 기본 날짜·시간 선택기, 전화번호는 `inputmode="tel"`, 숫자는 `inputmode="numeric"` |
| 홈 화면 추가 | PWA 매니페스트(`name`: "편입 캘린더(TExam Calendar)", `short_name`: "편입 캘린더"), `apple-touch-icon`, 상태 표시줄 색은 테마 바탕색. 홈 화면에서 앱처럼 전체 화면 실행 |
| 테마 | 밝은 테마 2종(원고지·플래너). 다크 모드는 지원하지 않음(기기 설정과 관계없이 선택한 테마 표시) |

### 6.3 데스크톱
- 1024px 이상: 달력 두 달을 나란히 보거나 한 달 + 오른쪽 상세 패널.
- 1440px 이상: 네 달을 2×2로 한 번에 보는 "전체 보기" 선택 가능(칸에는 약칭만 표시).
- 입력 폼은 두 열 배치. Tab 키로 다음 항목 이동, Enter로 저장.

---

## 7. 데이터 모델 (Supabase: Link Plan 공유)

### 7.1 공유 원칙
1. **Auth만 공유**합니다. 로그인은 Link Plan의 `auth.users` 계정을 그대로 씁니다.
2. 이 앱이 만드는 모든 DB 객체(테이블, 함수, 정책, 인덱스, 트리거)는 **`tcal_` 접두사**를 붙여 `public` 스키마에 만듭니다.
3. Link Plan의 기존 테이블·함수·트리거·Auth 설정을 **바꾸지 않습니다**. `auth.users`에 트리거를 달지 않습니다.
4. Auth 설정의 **Site URL은 그대로 두고**, Redirect URLs에 이 앱의 배포 주소만 추가합니다.
5. Link Plan에 `auth.users` 가입 시 실행되는 트리거(예: 프로필 생성)가 있을 수 있으므로, 이 앱에서는 회원가입을 하지 않습니다.
6. 클라이언트에는 anon(공개) 키만 넣습니다. service_role 키는 앱 코드와 저장소 어디에도 넣지 않습니다.

### 7.2 테이블
| 테이블 | 내용 | 주요 컬럼 |
|---|---|---|
| `tcal_families` | 가족(공유 단위) | id, name, invite_code, settings(jsonb), created_by |
| `tcal_family_members` | 가족 구성원 | family_id, user_id, role(student/parent), display_name |
| `tcal_universities` | 지원 대학(4.1~4.6, H) | 아래 SQL 참고 |
| `tcal_events` | 일정(4.3, 4.7) | university_id, event_type, start_date, end_date, check_in_time, start_time, end_time |
| `tcal_documents` | 준비 서류(4.5) | university_id, doc_type, status, submit_method |
| `tcal_checklist_items` | 준비물(4.8 G) | university_id, label, checked, sort_order |

날짜와 시각은 모두 서울 시간 기준이므로 `date`와 `time` 컬럼으로 따로 저장합니다(시간대 변환 오류 방지).

### 7.3 마이그레이션 초안(SQL)
```sql
-- supabase/migrations/20261115000000_tcal_init.sql
-- 이 파일은 tcal_ 객체만 만든다. Link Plan 기존 객체는 건드리지 않는다.

create table public.tcal_families (
  id          uuid primary key default gen_random_uuid(),
  name        text not null default '우리 가족',
  invite_code text not null unique default upper(substr(md5(gen_random_uuid()::text), 1, 8)),
  settings    jsonb not null default
              '{"overlap_gap_min":120,"default_exam_min":120,"default_buffer_min":30}'::jsonb,
  created_by  uuid not null default auth.uid() references auth.users(id),
  created_at  timestamptz not null default now()
);

create table public.tcal_family_members (
  family_id    uuid not null references public.tcal_families(id) on delete cascade,
  user_id      uuid not null references auth.users(id) on delete cascade,
  role         text not null check (role in ('student','parent')),
  display_name text,
  joined_at    timestamptz not null default now(),
  primary key (family_id, user_id)
);

create table public.tcal_universities (
  id               uuid primary key default gen_random_uuid(),
  family_id        uuid not null references public.tcal_families(id) on delete cascade,
  -- A. 기본 정보
  name             text not null,
  short_name       text not null check (char_length(short_name) between 1 and 4),
  department       text not null,
  quota            int,
  competition_rate numeric(6,2),
  priority         text check (priority in ('first','reach','target','safe')),
  color            text not null default '#2F6FED',
  -- B. 전형 방법
  exam_types       text[] not null default array['english']
                   check (exam_types <@ array['english','interview','document']),
  score_ratio      text,
  stage_type       text check (stage_type in ('one','two')),
  exam_format      text,
  -- D. 수험 정보
  apply_agency     text check (apply_agency in ('jinhak','uway','school')),
  exam_number      text,
  receipt_number   text,
  application_fee  int,
  fee_paid         boolean not null default false,
  ticket_printed   boolean not null default false,
  admission_phone  text,
  homepage_url     text,
  guide_url        text,
  -- F. 장소와 교통
  venue            text,
  map_url          text,
  transit          text,
  travel_min       int,
  buffer_min       int not null default 30,
  -- G. 준비물 메모, H. 결과
  prohibited_items text,
  result_status    text not null default 'pending'
                   check (result_status in ('pending','first_pass','final_pass','waitlist','fail')),
  waitlist_no      int,
  review_memo      text,
  memo             text,
  created_by       uuid default auth.uid() references auth.users(id),
  updated_by       uuid references auth.users(id),
  created_at       timestamptz not null default now(),
  updated_at       timestamptz not null default now()
);

create table public.tcal_events (
  id            uuid primary key default gen_random_uuid(),
  family_id     uuid not null references public.tcal_families(id) on delete cascade,
  university_id uuid not null references public.tcal_universities(id) on delete cascade,
  event_type    text not null check (event_type in
                ('apply','exam','venue_notice','first_result','doc_deadline',
                 'interview','final_result','payment','extra_result','other')),
  title         text,
  start_date    date not null check (start_date between '2026-11-01' and '2027-02-28'),
  end_date      date check (end_date is null or end_date >= start_date),
  check_in_time time,
  start_time    time,
  end_time      time,
  venue         text,
  map_url       text,
  round_no      int,
  memo          text,
  created_by    uuid default auth.uid() references auth.users(id),
  updated_by    uuid references auth.users(id),
  created_at    timestamptz not null default now(),
  updated_at    timestamptz not null default now()
);
create index tcal_events_family_date_idx on public.tcal_events (family_id, start_date);

create table public.tcal_documents (
  id                uuid primary key default gen_random_uuid(),
  family_id         uuid not null references public.tcal_families(id) on delete cascade,
  university_id     uuid not null references public.tcal_universities(id) on delete cascade,
  doc_type          text not null check (doc_type in
                    ('study_plan','self_intro','transcript','completion_cert','other')),
  custom_name       text,
  status            text not null default 'todo' check (status in ('todo','ready','submitted')),
  submit_method     text check (submit_method in ('upload','mail','visit')),
  mail_basis        text check (mail_basis in ('postmark','arrival')),
  original_required boolean not null default false,
  memo              text,
  updated_at        timestamptz not null default now()
);

create table public.tcal_checklist_items (
  id            uuid primary key default gen_random_uuid(),
  family_id     uuid not null references public.tcal_families(id) on delete cascade,
  university_id uuid not null references public.tcal_universities(id) on delete cascade,
  label         text not null,
  checked       boolean not null default false,
  sort_order    int not null default 0
);

-- 가족 구성원 여부 확인(정책에서 사용)
create or replace function public.tcal_is_member(fid uuid)
returns boolean
language sql stable security definer set search_path = public
as $$
  select exists (select 1 from public.tcal_family_members
                 where family_id = fid and user_id = auth.uid());
$$;

-- 초대 코드로 가족 참여
create or replace function public.tcal_join_family(code text, member_role text, name text)
returns uuid
language plpgsql security definer set search_path = public
as $$
declare fid uuid;
begin
  select id into fid from public.tcal_families where invite_code = upper(code);
  if fid is null then raise exception 'invalid invite code'; end if;
  insert into public.tcal_family_members (family_id, user_id, role, display_name)
  values (fid, auth.uid(), member_role, name)
  on conflict (family_id, user_id) do nothing;
  return fid;
end $$;

-- RLS
alter table public.tcal_families        enable row level security;
alter table public.tcal_family_members  enable row level security;
alter table public.tcal_universities    enable row level security;
alter table public.tcal_events          enable row level security;
alter table public.tcal_documents       enable row level security;
alter table public.tcal_checklist_items enable row level security;

create policy tcal_families_select on public.tcal_families
  for select using (public.tcal_is_member(id) or created_by = auth.uid());
create policy tcal_families_insert on public.tcal_families
  for insert with check (created_by = auth.uid());
create policy tcal_families_update on public.tcal_families
  for update using (public.tcal_is_member(id));

create policy tcal_members_select on public.tcal_family_members
  for select using (public.tcal_is_member(family_id));
create policy tcal_members_insert_creator on public.tcal_family_members
  for insert with check (user_id = auth.uid() and exists
    (select 1 from public.tcal_families f where f.id = family_id and f.created_by = auth.uid()));

-- 데이터 테이블 4개: 같은 가족만 모든 작업 허용
create policy tcal_universities_all on public.tcal_universities
  for all using (public.tcal_is_member(family_id)) with check (public.tcal_is_member(family_id));
create policy tcal_events_all on public.tcal_events
  for all using (public.tcal_is_member(family_id)) with check (public.tcal_is_member(family_id));
create policy tcal_documents_all on public.tcal_documents
  for all using (public.tcal_is_member(family_id)) with check (public.tcal_is_member(family_id));
create policy tcal_checklist_all on public.tcal_checklist_items
  for all using (public.tcal_is_member(family_id)) with check (public.tcal_is_member(family_id));
```

- `updated_at` 자동 갱신 트리거도 `tcal_set_updated_at` 이름으로 만들어 4개 데이터 테이블에만 답니다.
- 일정·서류·준비물의 `family_id`가 연결된 대학의 `family_id`와 같은지 확인하는 검사(트리거 또는 앱 로직)를 둡니다.
- 대학을 등록하면 앱에서 기본 준비물 7개를 `tcal_checklist_items`에 함께 넣습니다.

### 7.4 계산 규칙(앱에서 처리, 단위 테스트 대상)
| 값 | 규칙 |
|---|---|
| D-day | 서울 날짜 기준 `시험일 − 오늘`. 0이면 D-DAY, 음수면 완료 |
| 실제 종료 시각 | `end_time` 없으면 `start_time + 기본 시험 시간` |
| 실제 시작 시각(겹침용) | `check_in_time` 없으면 `start_time` |
| 권장 출발 시각 | `(check_in_time 또는 start_time) − travel_min − buffer_min` |
| 겹침 등급 | F7 표 |
| 일정 장소 | 일정의 `venue`가 없으면 대학의 `venue` |

---

## 8. 기술 스택

| 영역 | 선택 | 이유 |
|---|---|---|
| 프레임워크 | React + TypeScript + Vite | 화면이 여러 개이고 상태(가족, 대학, 일정)가 얽혀 있어 컴포넌트 구조가 편함. 정적 파일로 배포 가능 |
| 스타일 | Tailwind CSS + CSS 변수(디자인 토큰) | 반응형·테마 전환·인쇄(`print:` 접두사) 처리가 쉬움. 테마는 `data-theme` 속성으로 토큰만 교체 |
| 글꼴 | IBM Plex Sans KR·IBM Plex Mono(원고지), Pretendard·Gowun Dodum(플래너) | 자체 호스팅(`pretendard`, `@fontsource` 패키지), 선택한 테마의 글꼴만 불러옴 |
| 데이터 | `@supabase/supabase-js` v2 | Auth·DB·Realtime 한 번에 |
| 서버 상태 | TanStack Query | 캐시, 다시 불러오기, 저장 후 화면 갱신 |
| 날짜 | date-fns + `@date-fns/tz` (Asia/Seoul) | 가볍고 함수 단위로 테스트하기 쉬움 |
| 달력 | 직접 구현(CSS Grid 7열) | 네 달만 필요하고 인쇄 모양까지 맞춰야 하므로 라이브러리보다 직접 구현이 단순 |
| .ics | 직접 구현(`lib/ics.ts`) | 형식이 단순하고 시간대·알림·줄 접기를 정확히 통제. 단위 테스트로 검증 |
| PWA | vite-plugin-pwa | 홈 화면 추가, 오프라인에서 마지막 데이터 보기(P1) |
| 테스트 | Vitest(계산 함수), Playwright(iPhone 15 Pro·데스크톱 화면, A4 PDF) | 3.2의 확인 방식 |
| 배포 | Vercel(권장) 또는 GitHub Pages | 둘 다 무료·HTTPS. Vercel은 환경 변수와 SPA 경로 처리가 간단 |

- 라이브러리 버전은 `package.json`에 고정하고 `package-lock.json`을 커밋합니다.
- 환경 변수: `VITE_SUPABASE_URL`, `VITE_SUPABASE_ANON_KEY`(`.env.local`, 커밋 금지). `.env.example`만 커밋합니다.

---

## 9. 디자인 요약 (자세한 내용은 design/DESIGN.md)

| 항목 | 원고지(기본) | 플래너 |
|---|---|---|
| 콘셉트 | 크림색 종이 위 먹색 글씨와 파란 펜. 단정한 시험 일정표 | 연보라 다이어리에 붙인 둥근 스티커 |
| 바탕 / 강조 | #F7F5F0 / 파랑 #2B59C3 | #F3F0FA / 코랄 #C93F4A |
| 제목·큰 숫자 | IBM Plex Sans KR 700 / IBM Plex Mono | 고운돋움(Gowun Dodum) |
| 본문·작은 글씨 | IBM Plex Sans KR | 프리텐다드(Pretendard) |
| 모서리 | 4~16px | 10~26px, 칩은 알약형 |

**두 테마 공통**
- 대학 12색 팔레트(흰 글자 대비 4.5:1 이상). 색만으로 구분하지 않고 항상 약칭을 함께 표시
- 일정 기호: ● 필기시험, ◆ 면접, ▲ 마감(원서·서류·등록), ○ 발표, · 기타. 시험·면접은 채운 칩, 나머지는 테두리 칩
- 충돌은 빨강, 주의는 주황 + ⚠ 아이콘과 문구
- 상세 시트 카드 위쪽에 큰 숫자로 "입실 · 시작~종료 · 권장 출발"
- 인쇄: 흰 바탕, 얇은 회색 칸 선. 세로형 칸 높이 약 33mm(5주)·28mm(6주), 가로형은 6주 기준
- 일정 추가는 앱바 오른쪽 버튼(떠 있는 버튼은 달력 칸을 가려서 쓰지 않음)

---

## 10. 비기능 요구사항

| 구분 | 요구사항 |
|---|---|
| 보안 | 모든 `tcal_` 테이블에 RLS 적용. anon 키만 클라이언트에 사용. 다른 가족 데이터 조회 불가를 테스트로 확인 |
| 기존 앱 보호 | Link Plan의 테이블·함수·트리거·Auth 설정 변경 금지. 마이그레이션 파일에는 `tcal_` 객체만 포함(리뷰 시 확인) |
| 개인정보 | 수험 번호는 가족만 조회. 인쇄 시 수험 번호 숨기기 옵션 제공. 앱 주소에 개인정보를 넣지 않음 |
| 성능 | 첫 화면 2초 이내(LTE 기준), 달력 전환 즉시. 데이터는 가족 단위로 한 번에 불러옴(대학 20곳, 일정 100개 이하 가정) |
| 오프라인 | (P1) 마지막으로 불러온 데이터를 기기에 보관해 지하철 등 전파가 약한 곳에서도 시험 정보 조회 |
| 시간대 | 모든 날짜 계산은 Asia/Seoul. 기기 시간대가 달라도 같은 결과 |
| 호환성 | iOS Safari 17 이상(아이폰 15 Pro 기본), 최신 크롬·엣지. 인쇄는 데스크톱 크롬 기준 |
| 접근성 | 색 외에 약칭·기호·문구로 상태 구분. 글자 대비 4.5:1 이상. 터치 영역 44pt 이상 |
| 언어 | 한국어 UI, 날짜 형식 "12/24(목)" |

---

## 11. 개발 단계 (마일스톤)

원서 접수(12월 초) 전에 입력을 시작할 수 있도록 **M1~M3를 11월 말까지** 끝내는 것을 목표로 합니다.

| 단계 | 내용 | 결과물 |
|---|---|---|
| M0. 준비 | Link Plan의 로그인 방식·기존 테이블 이름 확인, 저장소·배포 설정 (디자인 가이드·시안은 준비 완료) | 저장소, 배포 주소 |
| M1. 데이터와 로그인 | 마이그레이션 적용, 로그인, 가족 만들기·초대 코드 참여 | F1 |
| M2. 대학·일정 입력 | 대학 폼(A~H), 일정 폼, 날짜 눌러 추가, 지원 현황 | F2, F3 |
| M3. 달력과 상세 | 11월~2월 달력, 상세 시트, 대학 상세, 아이폰 15 Pro 화면 맞춤 | F4, F5 → **첫 사용 버전** |
| M4. D-day·겹침 | D-day 카드·목록, 겹침 판정·표시·목록, 단위 테스트 | F6, F7 |
| M5. .ics 내보내기 | 범위 선택, 알림, 아이폰 실기기 확인 | F8 |
| M6. A4 인쇄 | 세로형(4장)·가로형(2장), 인쇄할 달 선택, 인쇄 옵션, PDF 확인 후 실제 인쇄 비교 | F9 |
| M7. 마무리 | 서류·준비물, 설정, PWA·오프라인, Realtime | F10, F11, P1 항목 |

- 테마(F13)는 M3에서 토큰 구조와 함께 만들고, 이후 모든 화면을 두 테마에서 확인합니다.

---

## 12. 위험 요소와 대응

| 위험 | 대응 |
|---|---|
| Link Plan 데이터·설정을 실수로 바꿈 | `tcal_` 접두사 규칙, 마이그레이션 리뷰, `auth.users` 트리거 금지, Site URL 변경 금지 |
| 이름 충돌 | 적용 전 `tcal_`로 시작하는 기존 객체가 없는지 확인 |
| Supabase 무료 플랜의 비활성 일시 중지(약 1주일 사용이 없으면 프로젝트가 멈춤) | Link Plan이 계속 쓰이는지 확인. 시험 기간에는 매일 쓰므로 위험은 낮지만, 멈추면 대시보드에서 재개하는 방법을 메모 |
| 시험 당일 네트워크 불안 | P1 오프라인 보관, 인쇄본 함께 사용, .ics로 아이폰 캘린더에도 저장 |
| iOS PWA에서 .ics 다운로드 실패 | 공유 시트 대안, Safari에서 열기 안내, 실기기 테스트 |
| .ics 재가져오기 시 중복 | 변경분만 내보내기, 안내 문구, (P2) 구독 주소 |
| Safari 인쇄 방향 무시 | 데스크톱 크롬 기준, 인쇄 창에서 방향 선택 안내, PDF 저장 |
| 대학 공지 변경(시험일·고사장 변경) | 대학 상세에 모집요강·공지 링크, "마지막 확인일" 메모 권장 |
| 입력 실수(오전·오후 혼동) | 24시간 표기, 저장 후 "12/24(목) 09:30 입실" 확인 문구 표시 |

---

## 13. 성공 지표 (가족 사용 기준)
- 지원한 모든 대학(10곳 이상)의 시험·면접 일정이 앱에 빠짐없이 들어가 있음
- 시험 당일 상세 시트 하나로 출발 시각·장소·수험 번호·준비물을 확인함
- 겹치는 시험을 원서 접수 전에 발견해 지원 여부를 판단함
- 인쇄본과 아이폰 캘린더, 앱의 일정이 서로 일치함
- 부모가 따로 묻지 않고도 일정을 확인함

---

## 14. 결정된 사항과 남은 확인 사항

### 14.1 결정된 사항 (v0.3)
| 항목 | 결정 |
|---|---|
| 앱 이름 | 편입 캘린더(TExam Calendar) |
| 테마 | 원고지(기본)·플래너 2종, 설정에서 선택. 다크 모드 제외 |
| 플래너 글꼴 | 제목·큰 숫자 고운돋움, 작은 글씨 프리텐다드 |
| 입력 단위 | 대학을 먼저 등록하고, 대학마다 일정을 여러 개 추가 |
| 데이터 저장 | Supabase Link Plan 프로젝트 공유, 접두사 `tcal_`, 로그인 계정 공유 |
| 공유 범위 | 자녀와 부모(같은 가족), 같은 권한 |
| 달력 기간 | 2026년 11월 ~ 2027년 2월(4개월). 입력 범위도 같음 |
| 인쇄 | 세로형(한 달 한 장, 4장)과 가로형(두 달 한 장, 2장) 모두 구현, 인쇄할 달 선택 가능. 실제 인쇄 후 기본값 결정 |
| 부가 기능 | D-day, 일정 겹침 경고, .ics 내보내기 모두 P0 |
| 제외 항목 | 캠퍼스, 전형 유형, 지원 학년, 영어·면접·서류 평가 외 시험 종류, 공인 영어 성적 반영, 주차, 숙박, 점심 |
| 서류 항목 정리 | 일반편입만 하므로 학사편입용 졸업·학위 증명서, 특별전형 증빙, 공인 영어 성적표를 서류 목록에서 뺌 |

### 14.2 개발 시작 시 확인할 사항
1. Link Plan의 로그인 방식(이메일·비밀번호, 매직 링크, 구글 등)과 자녀·부모 계정이 이미 있는지
2. Link Plan에 `auth.users` 가입 트리거나 `tcal_`로 시작하는 객체가 있는지
3. Supabase 요금제(무료/유료)와 Link Plan 사용 빈도(일시 중지 위험)
4. 배포 위치(Vercel 또는 GitHub Pages)와 앱 주소. Auth Redirect URLs에 추가할 주소
5. 접두사 `tcal_`를 그대로 쓸지(다른 이름을 원하면 마이그레이션 작성 전에 변경)
6. .ics 알림 기본 시각(전날 20:00, 2시간 전)이 괜찮은지
7. 시험 종류에서 "기타"를 완전히 뺄지(현재는 영어·면접·서류 평가 3개만 둠)
8. 다크 모드가 필요해지면 v0.1 시안 B(새벽 4시)를 세 번째 테마로 더할지
