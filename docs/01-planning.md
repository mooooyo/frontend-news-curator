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

## 🚀 Phase 1 구현 계획

### 시작점: GitHub 릴리즈 모니터링
**이유**:
- 구현 단순함 (GitHub API 잘 정리됨)
- 정확성 100% (공식 소스)
- 중요도 높음 (메이저 라이브러리들)

### 기술 스택
- Node.js + TypeScript
- GitHub API
- 간단한 이메일 발송 (Nodemailer)

---

## 📝 다음 단계에서 정해야 할 것들

1. **평가 및 분석 방법**
   - AI 활용 방안
   - 콘텐츠 요약/재작성 전략

2. **전달 방식**
   - 이메일 vs 웹 대시보드
   - 전달 주기 (일간/주간)
   - 콘텐츠 포맷

3. **기술 구현 세부사항**
   - 아키텍처 설계
   - 데이터 저장 방식
   - 자동화 (GitHub Actions)

4. **성공 지표 및 개선 방안**
   - 어떻게 품질을 측정할 것인가
   - 피드백 루프 설계
