# 경북청년인재스쿨 운영 플랫폼 — 프로젝트 현황

> 마지막 업데이트: 2026-06-15 (UI 대규모 개편 완료)  
> 담당자: careerable7@gmail.com  
> Claude Code가 이어서 작업할 수 있도록 작성된 상세 현황 문서입니다.

---

## 1. 프로젝트 개요

| 항목 | 내용 |
|------|------|
| 프로젝트명 | 경북청년인재스쿨 운영 플랫폼 |
| 목적 | 경북 청년 취업지원 프로그램 전 과정 운영 관리 |
| 운영 목표 | 2026년 7월 정식 운영 |
| 현재 단계 | UI 대규모 개편 완료 — 로그인 플로우, 8개 메뉴, 참여자 상세 패널 구현 |
| 기술 스택 | 순수 HTML/CSS/JS (단일 파일) → 추후 Supabase 백엔드 연동 예정 |

### 플랫폼 범위

'경북청년인재스쿨 운영 플랫폼'으로 범위를 한정한다.  
취업지원 플랫폼 전반이 아닌, 해당 프로그램 운영에 특화된 관리 시스템.

### 핵심 관리 기능 (6개)

1. **참여자 관리** — 기수별 등록, 5단계 진행, 상태 추적
2. **출석 관리** — 교육 회차별 출석 체크 및 출석률 집계
3. **상담일지** — 참여자별 상담 기록, 위험도(normal/warning/high_risk) 분류
4. **제출관리** — 과제 제출 워크플로우, 관리자 검토·피드백
5. **취업현황** — 전형 단계 추적, 경북 취업률 KPI 집계
6. **관리자 대시보드** — 전체 현황 한눈에 파악

---

## 2. 현재 완료된 작업

### ✅ 완료

- [x] MVP 기능 범위 확정 (6개 핵심 기능)
- [x] 데이터베이스 설계 완성 (9개 테이블, RLS 정책 초안 포함)
- [x] 프론트엔드 MVP 시연본 제작 (단일 HTML 파일)
  - [x] 랜딩페이지 (첫 화면, 기능 소개, 플랫폼 진입 버튼)
  - [x] 관리자 대시보드 (실제 데이터 연동 구조, 참여자 10명 목데이터)
  - [x] 참여자 전체 목록 (검색/필터/정렬 동작)
  - [x] 참여자 개인 대시보드 (5단계 진행도, 주간미션, 포인트)
  - [x] 교육관리 골격 (개발중 배지, 실제 UI 레이아웃)
  - [x] 제출관리 골격 (개발중 배지)
  - [x] 취업현황 골격 (개발중 배지, 전형 파이프라인 레이아웃)
  - [x] 공지/자료실 골격 (개발중 배지)
  - [x] 가입승인 골격 (개발중 배지)
- [x] GitHub 저장소 생성 및 코드 push
- [x] Vercel 배포 완료 (공개 URL 생성)

### ❌ 미완료 (다음 작업)

- [ ] Supabase 프로젝트 생성 및 DB 구축
- [ ] 로그인/인증 (Supabase Auth)
- [ ] 가입승인 실제 기능 구현
- [ ] 참여자 관리 실제 기능 (CRUD)
- [ ] 출석 관리 실제 기능
- [ ] 제출관리 실제 기능
- [ ] 취업현황 실제 기능
- [ ] 공지/자료실 실제 기능
- [ ] 상담일지 기능 (골격도 미제작)
- [ ] 프론트엔드 SPA 또는 멀티파일 구조 전환

---

## 3. 현재 파일 구조

```
c:\Users\caree\projects\gyeongbuk-school\    ← Git 저장소 루트
├── index.html                                ← MVP 전체 (단일 파일, 약 750줄)
└── PROJECT_STATUS.md                         ← 이 파일
```

### 원본 소스 파일 (Git 외부)

```
c:\Users\caree\OneDrive\바탕 화면\취업플랫폼 프로젝트(바이브코딩)\
└── gyeongbuk_dashboard.html    ← 초기 프로토타입 (index.html의 원본)
```

### index.html 내부 구조

```
<section id="landing">          ← 랜딩페이지 (첫 화면)
<div id="app">
  <nav>                         ← 퍼플 상단 네비게이션 (6개 메뉴)
  <div class="sub-tab-bar">     ← 대시보드 서브탭 (관리자개요/참여자목록/개인뷰)
  
  <!-- 대시보드 페이지 (실제 연결) -->
  <div id="page-admin">         ← 관리자 개요
  <div id="page-table">         ← 참여자 전체 목록
  <div id="page-participant">   ← 참여자 개인뷰
  
  <!-- 골격 페이지 (개발중) -->
  <div id="page-education">     ← 교육관리
  <div id="page-submission">    ← 제출관리
  <div id="page-employment">    ← 취업현황
  <div id="page-notice">        ← 공지/자료
  <div id="page-approval">      ← 가입승인
  
  <div class="modal-backdrop">  ← 참여자 추가 모달
</div>
<div id="toast">
<script>                        ← 네비게이션, 목데이터, 테이블 렌더링
```

### JS 핵심 함수

| 함수 | 역할 |
|------|------|
| `enterApp()` | 랜딩 → 앱 진입 |
| `exitApp()` | 앱 → 랜딩 복귀 (로고 클릭) |
| `switchPage(id)` | 메인 메뉴 전환 (dashboard/education/submission/employment/notice/approval) |
| `switchDashboard(sub)` | 대시보드 서브탭 전환 (admin/table/participant) |
| `renderTable2()` | 참여자 목록 테이블 렌더링 |
| `applyFilter2()` | 참여자 목록 필터/검색 |
| `addParticipant()` | 참여자 추가 모달 처리 |

### 디자인 토큰

| 항목 | 값 |
|------|-----|
| Primary | `#5B4FCF` (퍼플) |
| Accent | `#FFD166` (옐로) |
| Success | `#27500A` / `#EAF3DE` |
| Warning | `#BA7517` / `#FAEEDA` |
| Danger | `#791F1F` / `#FCEBEB` |
| Background | `#F5F4F9` |
| 폰트 | `-apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif` |
| 아이콘 | Tabler Icons (CDN: `@tabler/icons-webfont@latest`) |

---

## 4. 데이터베이스 설계 요약

### 기술 스택

- **플랫폼**: Supabase (PostgreSQL)
- **인증**: Supabase Auth (이메일/비밀번호)
- **파일 저장**: Supabase Storage

### 테이블 목록 (9개, MVP 확정)

| 순서 | 테이블 | 역할 |
|------|--------|------|
| 1 | `cohorts` | 기수 관리 (2024년 1기, 2기 ...) |
| 2 | `profiles` | 로그인 계정 (auth.users 연동) |
| 3 | `participants` | 참여자 상세 정보 |
| 4 | `education_sessions` | 교육 회차 |
| 5 | `attendance` | 출석 기록 |
| 6 | `assignments` | 과제 정의 |
| 7 | `submissions` | 제출 기록 |
| 8 | `counseling_logs` | 상담일지 |
| 9 | `employment_records` | 취업현황 |

### 핵심 컬럼 메모

**participants**
- `hope_job` (희망직무), `hope_region` (희망근무지역), `manager_memo` (관리자 메모) 포함
- `current_stage` (int 1~5), `status` (pending/active/completed/dropout)
- `status = 'pending'` → 가입승인 대기 상태

**counseling_logs**
- `risk_level` (normal/warning/high_risk) — 이탈위험 참여자 분류용
- `is_private` (bool) — 참여자 열람 차단 여부

**employment_records**
- `application_stage` (resume/interview_1/interview_2/final_interview/hired)
- `is_gyeongbuk` (bool) — **핵심 KPI**: 경북 지역 취업 여부
- `current_status` (employed/resigned/unknown) — hired 이후 재직 여부

### RLS 정책 원칙

| 역할 | 권한 |
|------|------|
| admin / staff | 전체 테이블 CRUD |
| participant | 본인 row만 READ (counseling_logs는 is_private=false만) |

### 헬퍼 함수 (Supabase SQL 작성 시 사용)

```sql
CREATE OR REPLACE FUNCTION is_admin_or_staff()
RETURNS boolean LANGUAGE sql SECURITY DEFINER STABLE AS $$
  SELECT EXISTS (
    SELECT 1 FROM profiles WHERE id = auth.uid() AND role IN ('admin', 'staff')
  );
$$;
```

### 첫 관리자 계정 설정 주의사항

Supabase RLS 활성화 후 첫 관리자는 **Dashboard의 Table Editor**에서  
`profiles.role`을 직접 `admin`으로 변경해야 함.  
앱을 통해 자신의 role을 변경하는 것은 RLS 정책상 불가.

### Supabase Storage 버킷

| 버킷명 | 용도 | 공개 여부 |
|--------|------|----------|
| `submissions` | 제출 파일 저장 | 비공개 |
| `materials` | 공지/자료실 파일 | 공개 |

---

## 5. GitHub 상태

| 항목 | 내용 |
|------|------|
| 저장소 URL | https://github.com/careerable7-coder/gyeongbuk-youth-school |
| 공개 여부 | Public |
| 기본 브랜치 | `master` |
| 최근 커밋 | `feat: UI 대규모 개편 - 로그인 플로우, 8개 메뉴, 참여자 상세, 상담일지 추가` |
| 커밋 수 | 3개 |

### 로컬 저장소 경로

```
c:\Users\caree\projects\gyeongbuk-school\
```

### Push 방법 (새 터미널에서)

```powershell
# PATH 새로고침 필요 시
$env:PATH = [System.Environment]::GetEnvironmentVariable("PATH","Machine") + ";" + [System.Environment]::GetEnvironmentVariable("PATH","User")

Set-Location "c:\Users\caree\projects\gyeongbuk-school"
& "C:\Program Files\Git\cmd\git.exe" add .
& "C:\Program Files\Git\cmd\git.exe" commit -m "커밋 메시지"
& "C:\Program Files\Git\cmd\git.exe" push origin master
```

> **주의**: Git이 새 터미널에서 인식 안 될 경우 전체 경로  
> `C:\Program Files\Git\cmd\git.exe` 로 실행하거나 VS Code 재시작.

---

## 6. Vercel 상태

| 항목 | 내용 |
|------|------|
| 공개 URL | https://gyeongbuk-youth-school.vercel.app |
| 프로젝트명 | gyeongbuk-youth-school |
| Vercel 계정 | careerable7-2833 |
| 배포 방식 | Vercel CLI (`vercel --prod --yes`) |
| 자동 배포 | GitHub 연동 미완성 (수동 배포 필요) |
| 배포 상태 | READY (Production) |

### 재배포 방법

```powershell
$env:PATH = [System.Environment]::GetEnvironmentVariable("PATH","Machine") + ";" + [System.Environment]::GetEnvironmentVariable("PATH","User")
Set-Location "c:\Users\caree\projects\gyeongbuk-school"
vercel --prod --yes
```

### GitHub 자동 배포 연동 방법 (미완료)

Vercel 배포 시 아래 오류가 발생했음:
```
Error: Failed to link careerable7-coder/gyeongbuk-youth-school.
You need to add a Login Connection to your GitHub account first.
```

해결 방법:
1. https://vercel.com/careerable-s-projects/gyeongbuk-youth-school 접속
2. Settings → Git → Connect Git Repository
3. GitHub 계정 연결 후 `careerable7-coder/gyeongbuk-youth-school` 선택
4. 이후부터는 `git push`만 하면 자동 배포됨

---

## 7. 다음 작업 목록 (TODO)

### 2026-06-15 완료된 UI 개편 (10개 항목)

- [x] **1. 랜딩페이지** — 상단 네비(로고+로그인 버튼), 히어로(KPI 4개+CTA), 기능 소개 그리드, 푸터
- [x] **2. 메뉴 8개로 확장** — 대시보드, 참여자관리, 교육관리, 제출관리, **상담일지(신규)**, 취업현황, 공지/자료, 가입승인
- [x] **3. 랜딩 상단 로그인 버튼** — 오른쪽 상단 "로그인" 버튼 → 유형 선택 화면으로 이동
- [x] **4. 로그인 유형 선택 화면** — 관리자/참여자 카드 UI, 각 역할 설명 포함
- [x] **5. 회원가입 구조** — 유형 선택 → 관리자/참여자 별도 양식 (UI 프로토타입)
- [x] **6. 로그인 플로우** — 랜딩 → 유형선택 → 로그인폼 → 대시보드 (관리자/참여자 분리)
- [x] **7. 기존 페이지 유지** — 관리자 대시보드, 참여자관리 목록, 교육관리, 제출관리, 취업현황, 공지/자료, 가입승인
- [x] **8. 참여자 상세 슬라이드 패널** — 이름/기수/희망직무/희망근무지역/취업준비단계/담당상담사/출석률/제출률/상담횟수/최근상담일/취업상태/메모
- [x] **9. Supabase Auth 역할 분리 대비 구조** — admin/participant 앱 영역 분리, 로그아웃 플로우 준비
- [x] **10. PROJECT_STATUS.md 업데이트** ← 지금 이 항목

### 즉시 가능한 작업 (코드만 수정)

- [ ] **Vercel GitHub 자동배포 연동**  
  위 6번 항목 참고. 1회성 수동 작업.

### Supabase 연동 (핵심 백엔드 작업)

- [ ] **Supabase 프로젝트 생성**
  1. https://supabase.com 접속 → 새 프로젝트 생성
  2. 프로젝트 이름: `gyeongbuk-youth-school`
  3. 비밀번호 저장 필수

- [ ] **DB 테이블 생성**  
  이전 대화에서 확정된 SQL 전체를 Supabase SQL Editor에서 순서대로 실행:
  1. `is_admin_or_staff()` 헬퍼 함수
  2. `cohorts` 테이블
  3. `profiles` 테이블 + `handle_new_user()` 트리거
  4. `participants` 테이블
  5. `education_sessions` 테이블
  6. `attendance` 테이블
  7. `assignments` 테이블
  8. `submissions` 테이블
  9. `counseling_logs` 테이블
  10. `employment_records` 테이블
  11. 인덱스 생성
  12. RLS 활성화 + 정책 등록

- [ ] **Storage 버킷 생성**  
  `submissions` (비공개), `materials` (공개)

- [ ] **Supabase 환경변수 준비**  
  ```
  SUPABASE_URL=https://[프로젝트ID].supabase.co
  SUPABASE_ANON_KEY=[anon public key]
  ```

### 프론트엔드 → Supabase 연동 (기능 구현 순서)

아래 순서로 구현하는 것을 권장함 (의존성 고려):

1. **로그인 화면** — Supabase Auth 이메일 로그인  
   - 현재 랜딩 → 앱 진입 버튼을 로그인 폼으로 교체
   - 로그인 성공 시 role 확인 → admin이면 대시보드, participant이면 개인 대시보드

2. **가입승인** — 가장 먼저 구현해야 참여자를 시스템에 등록할 수 있음  
   - `participants.status = 'pending'` 목록 조회
   - 승인 버튼 → `status = 'active'` 업데이트

3. **제출관리** — 관리자 일상 업무의 핵심  
   - `submissions` 테이블 CRUD
   - 파일 업로드 → Supabase Storage `submissions/` 버킷
   - 관리자 검토 → `status`, `feedback` 업데이트

4. **참여자 대시보드** — 목데이터 → 실제 데이터 연동  
   - 로그인한 참여자의 `participant_id` 기반 데이터 조회
   - 출석률, 제출률 실시간 집계

5. **교육관리** — 출석 체크 기능  
   - `education_sessions` CRUD
   - 출석 체크 화면 → `attendance` 테이블 insert

6. **취업현황** — `employment_records` CRUD  
   - `is_gyeongbuk` 필드 기반 경북 취업률 집계 쿼리

7. **공지/자료실** — `notices`, `materials` 테이블 (현재 설계 미포함, 추가 필요)

### 구조 개선 (기능 구현 진행에 따라)

- [ ] 단일 HTML 파일 → 멀티파일 구조 전환 검토  
  현재 750줄 단일 파일. 기능이 추가될수록 유지보수 어려움.  
  옵션 A: Vanilla JS + 파일 분리 (html/css/js)  
  옵션 B: 경량 프레임워크 도입 (Alpine.js, Vue 3 등)

---

## 8. 환경 설정 메모

### 로컬 개발 환경

| 도구 | 설치 경로 | 상태 |
|------|----------|------|
| Git 2.54 | `C:\Program Files\Git\cmd\git.exe` | 설치됨 |
| GitHub CLI 2.94 | `C:\Program Files\GitHub CLI\gh.exe` | 설치됨 |
| Node.js 24.16 | 시스템 PATH | 설치됨 |
| npm 11.13 | 시스템 PATH | 설치됨 |
| Vercel CLI 54.14 | npm global | 설치됨 |

> 새 PowerShell 세션에서 git/gh 인식 안 될 경우:
> ```powershell
> $env:PATH = [System.Environment]::GetEnvironmentVariable("PATH","Machine") + ";" + [System.Environment]::GetEnvironmentVariable("PATH","User")
> ```

### 계정 정보

| 서비스 | 계정 |
|--------|------|
| GitHub | careerable7-coder |
| Vercel | careerable7-2833 |
| 이메일 | careerable7@gmail.com |
| Supabase | 미가입 (다음 단계) |

---

## 9. 참고사항

### 프로그램 5단계 구조

경북청년인재스쿨 프로그램은 5단계로 구성되며, 모든 화면의 단계 분류 기준이 된다.

| 단계 | 명칭 | 내용 |
|------|------|------|
| 1 | 자기이해 | 강점·성격 발견, 직무 탐색 |
| 2 | 역량개발 | 직무 역량 강화, 자격증 취득 |
| 3 | 취업준비 | 이력서·자기소개서 작성 |
| 4 | 취업실행 | 입사지원, 면접 준비 |
| 5 | 사후관리 | 취업 후 적응 지원 |

### 핵심 KPI

- **경북 취업률** = `employment_records.is_gyeongbuk = true` 비율  
  → 지자체 사업 실적 보고의 가장 중요한 지표
- **평균 제출률** = 참여자별 `submissions` 제출 수 / `assignments` 총 수
- **평균 출석률** = 참여자별 `attendance.status = 'present'` 수 / `education_sessions` 총 수

### 목데이터 참여자 목록 (현재 index.html에 하드코딩)

| 이름 | 나이 | 단계 | 취업현황 | 지원기업 |
|------|------|------|----------|----------|
| 김이음 | 26 | 4단계 | 취업준비중 | (주)아이엠테크 |
| 박하늘 | 28 | 5단계 | 면접준비중 | (주)프라이드솔루션 |
| 이서연 | 25 | 취업완료 | 취업성공 | 경북PRIDE기업 |
| 최민준 | 29 | 2단계 | 취업준비중 | 미정 |
| 정유진 | 27 | 3단계 | 취업준비중 | (주)테크노파크 |
| 강다온 | 24 | 취업완료 | 취업성공 | 대경이앤씨 |
| 윤시우 | 30 | 1단계 | 이탈위험 | 미정 |
| 임채원 | 26 | 4단계 | 면접준비중 | (주)클라우드원 |
| 한소율 | 27 | 취업완료 | 취업성공 | 경북창조경제혁신센터 |
| 오준혁 | 28 | 3단계 | 취업준비중 | 미정 |
