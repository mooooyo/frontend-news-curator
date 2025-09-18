# Frontend News Curator - 전체 기획 정리

## 🎯 프로젝트 목표
**"프론트엔드 개발 생태계의 진짜 중요한 변화만 선별해서 개인에게 정확하게 전달"**

### 핵심 가치
- **중요도 우선**: 정보 과부하 방지, 정말 필요한 정보만
- **정확성 중심**: 검증된 소스 우선, 잘못된 정보 최소화
- **실무 지향**: 당장 내 프로젝트에 영향주는 정보 우선
- **개인화**: 내가 사용하는 기술 스택 중심으로

### 해결하려는 문제
- styled-components maintenance mode 같은 중요한 소식 놓치기
- 너무 많은 뉴스레터/블로그로 인한 정보 피로
- 트렌드 파악의 어려움 (뭐가 정말 중요한 변화인지)

## 📊 정보 수집 전략

### 수집 대상 (우선순위별)

#### 1순위: 공식 소스 (GitHub API)
**주요 라이브러리/프레임워크 릴리즈**
```
- facebook/react
- vuejs/core  
- vercel/next.js
- vitejs/vite
- angular/angular
- sveltejs/svelte
- tailwindlabs/tailwindcss
- microsoft/TypeScript
```

#### 2순위: npm 생태계 트렌드
**분석 기준 3가지**
1. **얼마나 많이 쓰는가?**
   - 주간/월간 다운로드 수
   - GitHub stars, dependents 수
   - Stack Overflow 언급 빈도

2. **유지보수가 되고 있는가?**
   - 마지막 릴리즈 날짜
   - 이슈 응답률/해결률
   - 활성 컨트리뷰터 수

3. **이용자수의 큰 변화가 있나?**
   - 월간 다운로드 증감률 (±20% 이상)
   - 경쟁 라이브러리 대비 점유율 변화

#### 3순위: 커뮤니티 검증 소스
**Reddit**
- r/javascript, r/reactjs (상위 포스트)
- 높은 upvote = 커뮤니티 검증 완료

**Hacker News**
- Front page 기술 관련 포스트
- 높은 score + comments 수

#### 4순위: 주요 개발자 개인 채널 (향후)
- Twitter/X 주요 메인테이너들
- 개인 블로그, 컨퍼런스 발표

### 수집 방법
- **GitHub API**: 릴리즈, 이슈, discussions
- **npm API**: 다운로드 통계, 패키지 메타데이터
- **웹 스크래핑**: Reddit, Hacker News
- **키워드 트렌딩**: 급증하는 검색어 감지

## 🤖 분석 및 평가 전략

### 하이브리드 아키텍처 (비용 최적화)

#### 클라우드 자동화 (GitHub Actions)
```
📊 수집 단계 (24시간 주기)
├── GitHub API로 릴리즈 수집
├── npm API로 패키지 트렌드 수집
├── Reddit/HN 스크래핑  
├── 원시 데이터 JSON으로 저장 (data/YYYY-MM-DD.json)
└── 새 데이터 있을 시 로컬에 알림
```

#### 로컬 분석 (MCP + Claude)
```
🤖 분석 & 큐레이션 단계 (수동 트리거)
├── MCP로 수집된 데이터 읽기
├── Claude API로 중요도 분석
├── 한국어 요약 및 영향도 평가
└── 사람이 최종 큐레이션 (품질 보장)
```

### 중요도 판단 기준

#### 자동 필터링 (1차)
- **공식성**: GitHub official > 커뮤니티 > 개인 블로그
- **Breaking changes**: major version, deprecation, security fix
- **영향 범위**: npm 다운로드 수, GitHub stars
- **커뮤니티 반응**: upvotes, comments, mentions

#### AI 분석 (2차 - Claude API)
**평가 기준**
1. **실무 영향도** (1-10): 내 프로젝트에 영향을 줄까?
2. **시급성** (1-10): 당장 알아야 할 정보인가?  
3. **트렌드 중요성** (1-10): 앞으로 중요해질 정보인가?
4. **액션 필요 여부**: 구체적으로 뭘 해야 하는가?

#### 우선순위 분류
```
🔴 긴급 (즉시 알림): 
- Major version 업데이트
- Deprecation/maintenance mode 발표
- Critical security fix
- npm 다운로드 급변 (30%+ 변화)

🟡 중요 (일간 리포트):
- Minor version with new features
- 새로운 인기 라이브러리 등장 (급성장)
- 성능 관련 중요 개선

🟢 참고 (주간 요약):
- Patch updates
- 커뮤니티 토론 이슈
- 학습 자료 추천
```

## 📧 전달 방식

### React.email 활용 (실무 연습 겸)

#### 이메일 템플릿 종류
```
email-templates/
├── templates/
│   ├── urgent-alert.tsx      # 🔴 긴급 알림
│   ├── daily-digest.tsx      # 🟡 일일 요약 
│   └── weekly-summary.tsx    # 🟢 주간 트렌드
├── components/
│   ├── header.tsx
│   ├── article-card.tsx
│   ├── npm-trend-card.tsx
│   └── footer.tsx
└── send/
    └── mailer.ts
```

#### 발송 플로우
1. **MCP에서 분석 완료** → Claude가 중요도 판단
2. **React.email로 렌더링** → 예쁜 HTML 생성  
3. **Nodemailer로 발송** → Gmail SMTP 활용

#### 콘텐츠 구성
- **헤드라인**: 가장 중요한 1-2개 소식
- **릴리즈 요약**: 주요 라이브러리 업데이트
- **npm 트렌드**: 급변하는 패키지들
- **액션 아이템**: 구체적으로 해야 할 일들

## 🚀 구현 로드맵

### Phase 1: 기본 수집 시스템 (2-3주)
- GitHub API 연동 및 데이터 수집
- 기본적인 JSON 저장 구조
- GitHub Actions 자동화 설정

### Phase 2: npm 트렌드 분석 (2주)
- npm API 연동
- 다운로드 수, 의존성 추적
- 변화율 계산 및 이상 징후 탐지

### Phase 3: 분석 & 큐레이션 (2-3주)
- MCP 연동 설정
- Claude API 분석 로직 개발
- 로컬 큐레이션 워크플로우 구축

### Phase 4: 이메일 발송 시스템 (2주)
- React.email 셋업 및 템플릿 개발
- 발송 로직 구현
- 테스트 및 디버깅

### Phase 5: 확장 & 개선 (지속적)
- Reddit/HN 스크래핑 추가
- 개인화 기능 (관심 기술스택별)
- 피드백 루프 및 품질 개선

## 🎯 성공 지표

### 정량적 지표
- **적시성**: 중요한 소식을 며칠 내에 캐치했는가?
- **정확성**: 잘못된 정보나 과장된 소식은 없었는가?
- **실용성**: 실제로 프로젝트에 도움이 되는 정보 비율

### 정성적 지표
- **"아, 이거 놓칠 뻔했네"** 순간들
- **실무에서 바로 적용**한 정보들
- **시간 절약**: 여러 소스 체크하는 시간 단축

## 💡 차별화 포인트

### vs Bytes.dev 같은 기존 서비스
- **개인화**: 내 기술 스택 중심
- **즉시성**: AI 자동 분석으로 더 빠른 감지
- **실무 중심**: "읽을거리" < "해야할 일"
- **한국어**: 복잡한 기술 내용 한국어 요약

### 핵심 가치 제안
"매일 30분씩 여러 사이트 체크하는 대신, 5분만에 정말 중요한 정보만 확인"

---

## 🛠 개발 원칙

**학습 중심 개발**
- 각 기술 스택을 배워가며 구현
- 실무에 적용 가능한 패턴 학습
- 단계별 리뷰 & 개선

**점진적 개선**
- 완벽하게 만들려 하지 말고 빠르게 시작
- 사용하면서 문제점 발견 및 개선
- 피드백 기반 지속적 발전