# Frontend News Curator - 기획 문서

## 🎯 서비스 목표
- **중요도 높은 정보**를 **정확하게** 선별하여 전달
- 정보 과부하 방지: 적지만 정말 필요한 정보만
- 중요한 소식 놓치기 방지: styled-components deprecation 같은 케이스

## 📊 수집 전략

### 기본 원칙
- **소스 신뢰도**에 따른 계층적 수집
- **공식 소스 우선**, 커뮤니티 검증 거친 정보 보조
- 매일 적은 수의 소스를 체크하되, 신뢰할 만한 곳들로만

### 1단계: 공식 소스 (최우선)
**GitHub 릴리즈 모니터링**
```
주요 라이브러리/프레임워크:
- facebook/react
- vuejs/core  
- vercel/next.js
- vitejs/vite
- angular/angular
- sveltejs/svelte
- tailwindlabs/tailwindcss
- microsoft/TypeScript
```

**수집 방법**: GitHub API 활용
- Daily check for new releases
- Major/Minor 릴리즈만 필터링
- Breaking changes, deprecation 키워드 감지

### 2단계: 커뮤니티 검증 소스 (보조)
**Reddit**
- r/javascript (상위 10개 포스트)
- r/reactjs (상위 10개 포스트)
- 높은 upvote 수 = 커뮤니티 검증 완료

**Hacker News**
- Front page 기술 관련 포스트
- 높은 score + comments 수

### 3단계: 주요 개발자 개인 채널 (향후 추가)
**Twitter/X 계정들**
- @dan_abramov (React 팀)
- @youyuxi (Vue 창시자) 
- @vercel (Next.js 공식)
- @maxstoiber (styled-components 창시자)

## 🔍 중요도 판단 기준

### 자동 필터링 기준
1. **공식 소스인가?** 
   - GitHub official release > 개인 블로그
2. **Breaking change 포함인가?** 
   - Major version, deprecation, maintenance mode
3. **커뮤니티 반응 크기** 
   - GitHub stars, Reddit upvotes, HN comments
4. **영향 범위** 
   - npm 다운로드 수로 사용자 규모 판단

### 우선순위 스코어링
```
긴급 (즉시 알림): 
- Major version 업데이트
- Deprecation/maintenance mode 발표
- Critical security fix

중요 (일간 리포트):
- Minor version with new features
- 새로운 인기 라이브러리 등장
- 성능 관련 중요 개선

참고 (주간 요약):
- Patch updates
- 커뮤니티 토론 이슈
- 학습 자료 추천
```

## 🤖 분석 및 평가 전략

### 하이브리드 아키텍처 (비용 최적화)

**클라우드 자동화 (GitHub Actions)**
```
📊 수집 단계
├── GitHub API로 릴리즈 수집
├── Reddit/HN 스크래핑  
├── 원시 데이터 JSON으로 저장 (data/YYYY-MM-DD.json)
└── 로컬에 알림 (새 데이터 있을 때)
```

**로컬 분석 (MCP + Claude)**
```
🤖 분석 & 큐레이션 단계
├── MCP로 수집된 데이터 읽기
├── Claude로 중요도 분석
├── 한국어 요약 생성
└── 수동 큐레이션 (사람이 최종 판단)
```

### AI 분석 프롬프트 전략
**Claude API 활용**
- 긴 릴리즈 노트 요약
- 실무 영향도 분석
- 한국어 설명 생성
- 구조화된 JSON 응답

**분석 기준**
1. 실무 영향도 (1-10)
2. 시급성 (1-10)  
3. 트렌드 중요성 (1-10)
4. 액션 필요 여부

## 📧 전달 방식

### React.email 활용 (실무 연습 겸)
**이메일 템플릿 종류**
- `urgent-alert.tsx`: 긴급 알림 (breaking changes 등)
- `daily-digest.tsx`: 일일 요약 리포트
- `weekly-summary.tsx`: 주간 트렌드 분석

**발송 플로우**
1. MCP에서 분석 완료 → 중요도 판단
2. React.email로 렌더링 → HTML 생성  
3. Nodemailer로 발송 → Gmail SMTP

**템플릿 구조**
```
email-templates/
├── templates/
│   ├── daily-digest.tsx
│   ├── urgent-alert.tsx
│   └── weekly-summary.tsx
├── components/
│   ├── header.tsx
│   ├── article-card.tsx
│   └── footer.tsx
└── send/
    └── mailer.ts
```

## 🚀 구현 단계

### Phase 1: 기본 수집 시스템
- GitHub API 연동
- 데이터 수집 & 저장
- GitHub Actions 자동화

### Phase 2: 분석 & 큐레이션  
- MCP 연동 설정
- Claude API 분석 로직
- 로컬 큐레이션 워크플로우

### Phase 3: 이메일 발송
- React.email 셋업
- 템플릿 개발
- 발송 시스템 구축

### Phase 4: 확장 & 개선
- 추가 소스 연동 (Reddit, HN)
- 개인화 기능
- 피드백 루프 구축

---

## 🎯 개발 방향성

**학습 중심 개발**
- 각 기술 스택을 배워가며 구현
- 실무에 적용 가능한 패턴 학습
- 단계별 리뷰 & 개선

**실무 연습 요소들**
- React.email: 이메일 템플릿 관리
- MCP: Claude와의 효율적인 협업
- GitHub Actions: CI/CD 자동화
- API 설계: 확장 가능한 구조