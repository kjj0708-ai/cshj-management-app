# 실행자들 실행실적 관리 앱

## 프로젝트 개요

모임 멤버들의 출석·미션 실적을 관리하고 랭킹을 시각화하는 웹앱.

## 기술 스택

- **순수 HTML + CSS + Vanilla JS (ES6+)** — 프레임워크 없음
- **단일 파일**: `index.html` 하나로 완결
- **외부 라이브러리**: Chart.js CDN 1개만 허용
- **데이터 저장**: `localStorage` 전용 (서버·DB 없음)
- **빌드 없음**: 브라우저에서 직접 열면 즉시 동작

## 파일 구조

```
index.html        # 앱 전체 (HTML + CSS + JS 포함)
CLAUDE.md         # 이 파일
.claude/
  launch.json     # 개발 서버 설정
  settings.local.json
```

## localStorage 키 구조

| 키 | 타입 | 설명 |
|----|------|------|
| `members` | Array | 회원 목록 |
| `sessions` | Array | 회차(모임) 목록 |
| `attendance` | Object | 출석 기록 (`memberId__sessionId` → `"O"/"X"`) |
| `missions` | Array | 미션 목록 |
| `missionScores` | Object | 미션 점수 (`memberId__missionId` → 숫자) |
| `settings` | Object | 레벨 기준, 출석 점수 설정 |

## 점수 계산 공식

```
총점 = 출석 점수 + 미션 점수
출석 점수 = O 횟수 × settings.attendScore (기본 10점)
미션 점수 = 해당 회원의 missionScores 합산
레벨 = settings.levels 를 minScore 내림차순 순회, 총점 >= minScore 첫 번째 적용
```

## 화면 구성 (5개 탭)

- **랭킹**: 총점 내림차순, 레벨 배지, 순위 변동 표시
- **회원**: 회원 CRUD, 상세 페이지(출석 이력·미션 점수 막대)
- **출석**: 회차 CRUD, O/X 토글 편집, 참석자/불참자 분리
- **미션**: 카테고리 필터, 회원×미션 교차표, 점수 입력
- **설정**: 레벨 기준 편집, 출석 점수 설정, JSON 내보내기/가져오기, 전체 초기화

## 개발 규칙

- 프레임워크·빌드 도구 추가 금지
- `crypto.randomUUID()`로 ID 생성
- 미션 점수 입력 시 최대점수 초과 불가 검증 필수
- 전체 초기화는 2단계 확인 다이얼로그 필수
- 최대 너비 480px, 모바일 우선 레이아웃

## CSS 변수

```css
--bg-page: #F5F5F0
--bg-card: #FFFFFF
--bg-muted: #F1EFE8
--border: rgba(0,0,0,0.10)
--text-main: #1A1A18
--text-sub: #6B6B67
--text-hint: #9A9A96
--accent: #378ADD
--success: #639922
--danger: #E24B4A
```

## 아바타 색상 (index % 5)

| index | bg | text |
|-------|----|------|
| 0 | #EEEDFE | #3C3489 |
| 1 | #E1F5EE | #085041 |
| 2 | #FAECE7 | #712B13 |
| 3 | #E6F1FB | #0C447C |
| 4 | #FAEEDA | #633806 |

## 레벨 배지 색상

| 레벨 | bg | text |
|------|----|------|
| 레전드 | #FAEEDA | #633806 |
| 다이아 | #E6F1FB | #0C447C |
| 골드 | #FAEEDA | #854F0B |
| 실버 | #F1EFE8 | #444441 |
| 브론즈 | #FAECE7 | #712B13 |

## MCP 설정

- **Playwright MCP**: 자동화 테스트용 (`npx @playwright/mcp@latest`)
