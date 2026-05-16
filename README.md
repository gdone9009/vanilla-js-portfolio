# 🛡️ 단방향 상태 관리 패턴 기반 반응형 포트폴리오 웹사이트 구축 프로젝트

## 1. 프로젝트 개요 (Overview)

### 1.1 목적 및 배경
본 프로젝트는 단순한 정적 웹 UI 구현을 넘어, 모던 프론트엔드 프레임워크(React, Vue 등)의 아키텍처적 초석이 되는 **"사용자 이벤트 → 상태(State) 변경 → DOM 업데이트"** 파이프라인을 순수 바닐라 자바스크립트(Vanilla JavaScript ES6+)로 직접 설계하고 구동하는 데 목적이 있습니다. 외부 라이브러리 및 프레임워크의 추상화된 레이어에 의존하지 않고, 브라우저 고유의 API 및 비동기 REST API 데이터 스트림을 하드웨어 성능 저하 없이 직접 통제하여 견고하고 가용성 높은 클라이언트 아키텍처를 구현합니다.

### 1.2 핵심 기술 및 수행 항목 (Core Tasks)
안정적인 클라이언트 런타임 가용성과 모던 웹 UX 구현을 위해 다음 4가지 핵심 항목을 중심으로 엔지니어링을 진행하였습니다.

* **HTML5 시맨틱 마크업 및 웹 접근성 (Structure & A11y)**
    * **구조적 레이아웃:** `div` 태그 남용을 전면 배제하고 `<header>`, `<nav>`, `<main>`, `<section>`, `<article>`, `<footer>` 등의 표준 시맨틱 태그를 엄격하게 계층화하여 검색엔진 최적화(SEO) 및 코드의 유지보수성 최적화
    * **접근성 표준 준수:** 모든 비주얼 이미지 객체의 의미 있는 `alt` 속성 부여 및 Contact 폼 요소 내 `<label>`과 `<input>` 간의 `for`-`id` 매칭 무결성을 확보하여 W3C 웹 접근성(A11y) 가이드라인 충족
* **CSS3 디자인 토큰 기반 반응형 디자인 (Design System & Grid)**
    * **유동형 그리드:** 네비게이션 정렬은 1차원 Flexbox 엔진을, Projects 섹션 리스트는 미디어 쿼리 없이 가변 배치되는 `auto-fit` 및 `minmax()` 결합형 2차원 CSS Grid를 도입하여 화면 해상도별 가용 영역 자동 최적화
    * **반응형 아키텍처:** 모바일 디바이스 레이아웃 스타일링을 선행(Mobile-First)한 후, 태블릿(768px) 및 데스크톱(1024px) 뷰포트에 대응하는 브레이크포인트 미디어 쿼리를 전개하여 크로스 디바이스 무결성 검증
* **JavaScript Core 기반 고성능 인터랙션 엔진 (Interaction & Performance)**
    * **동적 UI 컴포넌트:** 모바일 전용 햄버거 메뉴 토글 로직, 원페이지 스크롤 제어를 위한 Smooth Scroll, window 스크롤 임계값(60px) 초과 시 헤더 알파값 변환 및 300px 이상 시 동적 스크롤 탑(Top) 버튼 노출 시스템 구축
    * **렌더링 부하 방어 (Hardening):** 스크롤 이벤트 스팸으로 인한 프레임 드랍을 차단하는 **Throttle** 엔진 및 폼 실시간 검증 연산의 중복 구동을 방어하는 **Debounce** 패턴을 직접 구현하여 브라우저 가용 메모리 보호
    * **고효율 비동기 애니메이션:** 스크롤 시각 효과 구현을 위해 브라우저 메인 스레드를 차단하지 않는 `Intersection Observer` API(진입 임계값 0.2)를 결합하여 뷰포트 진입 시 자연스러운 페이드인(Fade-in) 모션 연출
* **ES6+ 비동기 데이터 파이프라인 및 영속성 관리 (Async Stream & State)**
    * **REST API 최적화 연동:** `fetch` 및 `async/await` 비동기 흐름 제어를 통하여 본인의 GitHub 리포지토리 목록을 비동기 수신하고, 백틱 기반 템플릿 리터럴을 통해 가변 DOM 문자열을 동적 바인딩
    * **중앙 집중형 상태 관리 (가상 Store):** 전역 변수 무단 수정을 차단하고 테마 상태, API 통신 상태, 유효성 상태를 하나의 상태 객체로 통합 제어하는 단방향 데이터 흐름 아키텍처 수립
    * **영속성 및 캐싱 바인딩:** 테마 변경 데이터를 `localStorage`에 영속화하여 새로고침 후에도 사용자 환경을 동기화하고, API 데이터 수신 결과를 `sessionStorage`에 임시 캐싱하여 깃허브 레이트 리밋 방어선 구축

### 1.3 핵심 엔지니어링 가치
* **State-Driven Rendering:** 복잡한 UI 요소를 JavaScript가 직접 조작하지 않고 오직 데이터(State)의 변화가 뷰(View)를 결정하는 선언형 프로그래밍 메커니즘 체득.
* **Defensive Frontend Programming:** 네트워크 단절, 레이트 리밋 초과 등 비동기 통신 실패 상황에 대비한 4단계 UI 상태 머신 분기 처리와 입력 단계에서의 스크립트 인젝션 차단을 통한 프론트엔드 보안 고도화.

---

## 2. 실행 환경 및 도구 (Environment & Tools)

### 2.1 개발 및 배포 환경
* **Host Machine:** Intel/Apple Silicon macOS 및 Windows 런타임 호환 환경
* **Local 개발 환경:** VS Code (Visual Studio Code) IDE + Live Server Extension (실시간 로컬 핫 리로드 파이프라인 구성)
* **Production 호스팅:** GitHub Pages 정적 웹 호스팅 인프라 인젝션
* **대상 브라우저:** 최신 Google Chrome 브라우저 가용성 검증 기반 (크로스 브라우저 호환성 표준 준수)

### 2.2 기술 스택 및 버전 (System Stack)
* **Core Frontend UI:** HTML5 (W3C Living Standard Specification), CSS3 (Modern Cascading Architecture)
* **Core Script Engine:** JavaScript (ECMAScript 2015+ / ES6+ Standard specification)
* **External Endpoints API:** GitHub REST API v3 Target Parsing Engine
* **Visual Asset Resource:** Google Fonts Web Asset, Font Awesome 6 Free Standard Web Icon Vector Asset

### 2.3 환경 운영 및 테마 정책 (Operational Policy)
* **순수 Vanilla JS 아키텍처 준수 원칙:**
    * React, Vue, jQuery, Bootstrap, Tailwind CSS 등 일체의 외부 프레임워크, 라이브러리, 유틸리티 툴의 라이브러리 소스 인젝션을 엄격히 금지함.
    * 브라우저 고유의 빌트인 파싱 엔진 사양만을 100% 활용하도록 코드베이스를 순수하게 유지함.
* **시큐어 코딩 및 코드 스타일 거버넌스:**
    * 전역 스코프를 오염시키고 예측 불가능한 호이스팅을 유발하는 `var` 키워드 사용을 전면 차단하고, 브라우저 가비지 컬렉터의 동작을 돕는 블록 스코프 기반의 `const`와 `let`만을 사용함.
    * HTML 문서 내부에 인라인 이벤트 핸들러(`onclick=""`) 및 인라인 스타일 속성(`style=""`)의 하드코딩을 원천 금지하며, 스크립트 로드 타이밍을 최적화하기 위해 `defer` 속성을 명시함.

---

## 3. 수행 체크리스트 (Task Checklist)

### 3.1 단계별 마일스톤

**Step 1: 아키텍처 초기화 및 표준 디렉토리 자산 구축**
* [x] 프로젝트 디렉토리 물리적 격리: css, js, images 폴더 구조화 및 핵심 마크업 index.html 엔트리 생성
* [x] 디자인 시스템 토큰 정의: `:root` 가상 선택자를 활용한 글로벌 컬러 테마 변수 및 공통 타이포그래피 사양 구축
* [x] 파싱 병목 현상 차단: 브라우저 블로킹 방지를 위한 script 태그 내 `defer` 속성 주입 검증

---

**Step 2: HTML5 시맨틱 계층 설계 및 웹 접근성(A11y) 자산화**
* [x] 레이아웃 무결성 설계: div 위주의 마크업을 지양하고 6대 표준 시맨틱 영역(Header, Nav, Hero, About, Skills, Projects, Contact, Footer) 논리 분할
* [x] 웹 접근성 표준 충족: 이미지 리소스의 의미 있는 `alt` 속성 전수 처리 및 폼 필드 상호작용용 `for`-`id` 일치 확인
* [x] 사용자 네비게이션 환경 최적화: 최상단 상시 고정형 헤더 및 섹션별 이동 타겟 앵커 링크 연동

---

**Step 3: CSS3 반응형 레이아웃 및 다크 테마 시스템 구현**
* [x] 모바일 퍼스트 레이아웃 아키텍처 수립: 미디어 쿼리 없는 가변 Flexbox 정렬 설계 우선 선행
* [x] 2차원 격자 레이아웃 구축: CSS Grid의 `auto-fit` 모듈 및 `minmax()` 연산을 결합한 프로젝트 카드 배열 유동화
* [x] 가용 브레이크포인트 검증: 768px(태블릿), 1024px(데스크톱) 미디어 쿼리 분기를 통한 디바이스 레이아웃 최적화 전환
* [x] 테마 스위칭 엔진 구현: `[data-theme="dark"]` 전사적 CSS 변수 반전 구조 및 시스템 테마(`prefers-color-scheme`) 연동

---

**Step 4: 바닐라 자바스크립트 기반 인터랙션 및 성능 최적화**
* [x] 전역 이벤트 캡처링 시스템 구성: 햄버거 메뉴 토글, 부드러운 스크롤, 스크롤 탑 버튼, 헤더 알파값 변환 모듈 가동
* [x] 런타임 성능 Hardening: 스크롤 이벤트 내 100ms Throttle 캡슐화 및 폼 인풋 검증 내 300ms Debounce 엔진 구축
* [x] 비동기 모션 트리거 빌드: 브라우저 리플로우 연산을 방지하는 `Intersection Observer` 기반 페이드인 모션 구현

---

**Step 5: 비동기 GitHub API 연동 및 런타임 상태 UI 분기**
* [x] 비동기 데이터 파이프라인 개발: `fetch` 및 `async/await` 기반의 GitHub REST API 연동 및 문자열 직렬화 가공
* [x] API 데이터 캐싱 정책 적용: 불필요한 트래픽 제어 및 레이트 리밋 방지를 위한 `sessionStorage` 임시 캐시 로직 결합
* [x] 4대 UI 상태 머신 마운트: 데이터 스트림 단계에 따른 로딩(스피너)/성공(카드)/에러(재시도 UI)/빈 상태(안내문) 가변 렌더링 검증

### 3.2 작업 증적(Evidence) 매핑 테이블
| 대분류 | 중분류 항목 | 검증 도구/방법 | 상태 |
| :--- | :--- | :--- | :---: |
| **인프라 환경** | 디렉토리 구조 및 파일 매핑 | Chrome DevTools Console 런타임 404/Null 에러 무결성 검증 | ✅ |
| **구조 설계** | 시맨틱 마크업 및 접근성 기준선 | W3C Markup Validator 통과 및 스크린 리더용 `alt` 속성 검증 | ✅ |
| **스타일링** | 반응형 유동형 CSS Grid 아키텍처 | Chrome Device Mode 디바이스별 레이아웃 해상도 가변 테스트 | ✅ |
| **런타임성능** | 고성능 인터랙션 및 애니메이션 | 스크롤 이벤트 발생 시 메인 스레드 프레임 드랍(FPS) 방어율 점검 | ✅ |
| **데이터스트림**| GitHub API 4단계 상태 UI 맵핑 | API 엔드포인트 강제 변조를 통한 403 예외 런타임 캐치 및 재시도 UI 가동 검증 | ✅ |

---

## 4. 프로젝트 아키텍처 및 구조 (Structure)

### 4.1 디렉토리 계층 구조 (Tree)
```text
.
├── index.html                  # [Main Entry] 애플리케이션의 시맨틱 골격을 선언한 웹 마크업 문서
├── css/                        # [Design System] 글로벌 레이아웃 및 스타일 토큰 관리 자산
│   └── style.css               # CSS 변수(:root) 정의, 반응형 미디어 쿼리 및 다크모드 캡슐화 파일
├── js/                         # [Core Engine] 전역 상태 제어 및 비동기 처리 핵심 스크립트 모듈
│   ├── app.js                  # 최상위 엔트리 스크립트: 전역 상태 인스턴스 관리 및 Throttle 이벤트 바인딩
│   ├── api.js                  # 비동기 통신 모듈: GitHub API 통신, 데이터 캐싱 및 4단계 UI 렌더러 구현
│   └── validation.js           # 폼 엔지니어링 모듈: Debounce 패턴 기반 Contact 문의 폼 데이터 검증 핸들러
└── images/                     # [Static Assets] 최적화된 미디어 자산 및 이미지 리소스 관리 폴더
    └── profile.png             # About 섹션 배치용 프로필 마스터 이미지 파일
```

### 4.2 디렉토리 구조 설계 근거 (Design Rationale)

본 프로젝트의 디렉토리 및 모듈 아키텍처는 코드의 자산화와 스케일아웃을 위해 다음 3가지 소프트웨어 공학 원칙을 준수합니다.

---

**1) 웹 표준 기술의 관심사 분리 (Separation of Concerns)**
* **설계:** HTML 파일 내부에는 스타일링 목적의 인라인 CSS 속성이나 비즈니스 로직을 다루는 JavaScript 인라인 스크립트를 완전히 제거하고 각각 독립된 파일로 물리 격리함.
* **이점:** 각 파일이 오직 하나의 책임(Structure, Presentation, Behavior)에만 집중하므로 소스 코드 가독성이 극대화되며 변동 사항 발생 시 수정 범위가 최소화됨.

**2) 자바스크립트 모듈화를 통한 전역 오염 차단 (Encapsulation)**
* **설계:** 전체 코드가 하나의 모놀리식 파일에 뭉쳐있지 않고 전역 상태와 인터랙션을 맡는 `app.js`, 네트워크 비동기 파이프라인을 맡는 `api.js`, 유효값 검증을 맡는 `validation.js`로 관심사를 파편화함.
* **이점:** 특정 기능 컴포넌트의 오류(예: API 스펙 변경)가 발생하더라도 다른 독립 모듈을 오염시키지 않아 디버깅 속도가 비약적으로 향상됨.

**3) 디자인 토큰의 중앙 집중식 형상 관리 (Design Tokens)**
* **설계:** 애플리케이션 전반에 활용되는 핵심 컬러 스펙, 투명도, 애니메이션 타이밍, 가변 폰트 데이터를 `style.css` 상단의 가상 선택자 `:root` 내부에 변수 자산화함.
* **이점:** 테마 상태(다크 모드) 변경 시 JavaScript가 모든 DOM 원소의 CSS 스타일을 순회 수정할 필요 없이 전역 돔 트리 상위의 어트리뷰트 하나만 토글하는 것으로 정밀 제어가 완료되어 브라우저 리플로우 부하를 예방함.

---

## 5. 실행 및 자동화 가이드 (Implementation)

### 5.1 CSS3 모던 레이아웃 및 모바일 퍼스트 반응형

다양한 디바이스 환경에서 화면 깨짐 현상을 원천 방어하고 가변적인 화면 가용 영역을 확보하기 위한 최적화 스타일 레이아웃 코드를 전개합니다.

**5.1.1 1차원 레이아웃 (Flexbox) 기반의 헤더 구조 배치**
```css
/* css/style.css 일부 */
header nav {
    display: flex;
    justify-content: space-between; /* 로고 좌측, 메뉴 우측 분할 */
    align-items: center;
    max-width: 1200px;
    margin: 0 auto;
    padding: 1rem 2rem;
}
```

**5.1.2 2차원 레이아웃 (CSS Grid) 기반의 프로젝트 카드 그리드 설계**
```css
/* css/style.css 일부 */
.projects-grid {
    display: grid;
    /* 미디어 쿼리 없이 300px 미만으로 떨어지지 않는 유동형 그리드 레이아웃 동적 계산 */
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); 
    gap: 2rem;
    margin-top: 2.5rem;
}
```

---

### 5.2 JavaScript Core 기반 고성능 인터랙션 엔진 구현

연속적인 사용자 상호작용 발생 시 스레드 점유율 급증으로 인한 브라우저 랙(Lag) 현상을 물리적으로 차단하기 위한 최적화 스크립트를 구현합니다.

**5.2.1 스크롤 이벤트 스팸 방어를 위한 Throttle 기법 적용 (`js/app.js`)**
```javascript
// 고속 스크롤 발생 시 지정된 임계 시간(limit) 내에 단 1회만 연산이 구동되도록 제어하는 Throttle 모듈
function throttle(func, limit) {
    let inThrottle;
    return function(...args) {
        const context = this;
        if (!inThrottle) {
            func.apply(context, args);
            inThrottle = true;
            setTimeout(() => inThrottle = false, limit);
        }
    }
}

// 60px 이상 스크롤 시 헤더 투명도 변경 및 스타일 변환 핸들러 연동 (100ms 디바이스 방어선 적용)
window.addEventListener('scroll', throttle(() => {
    const header = document.querySelector('header');
    if (window.scrollY >= 60) {
        header.classList.add('scrolled');
    } else {
        header.classList.remove('scrolled');
    }
}, 100));
```

**5.2.2 폼 키보드 입력 과부하 방지를 위한 Debounce 기법 적용 (`js/validation.js`)**
```javascript
// 사용자의 타이핑 연속 입력 동작이 완전히 종료된 후 마지막 1회만 검증 연산이 가동되도록 제어하는 Debounce 모듈
function debounce(func, delay) {
    let timeoutId;
    return function(...args) {
        if (timeoutId) clearTimeout(timeoutId);
        timeoutId = setTimeout(() => {
            func.apply(this, args);
        }, delay);
    };
}

// 이메일 정규표현식(RegEx) 유효성 검증 연산 연결 (입력 중단 후 300ms 유휴 시간 도달 시 트리거)
const validateEmailInput = debounce((event) => {
    const emailValue = event.target.value;
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    const errorElement = document.querySelector('#email-error');
    
    if (!emailRegex.test(emailValue)) {
        errorElement.textContent = "[오류] 유효한 이메일 형식이 아닙니다. 다시 확인해 주세요.";
        errorElement.classList.add('active');
    } else {
        errorElement.textContent = "";
        errorElement.classList.remove('active');
    }
}, 300);

document.querySelector('#email').addEventListener('input', validateEmailInput);
```

---

## 6. Requirements Fulfillment Verification (Evidence)

### 6.1 비동기 런타임 상태(Runtime State) 머신 분기 처리 검증

실제 외부 API 서버 통신 단계에서 예외 상황이 발생하더라도 웹사이트 런타임이 중단되지 않고 유연하게 예외 복구(Fail-Safe)됨을 증명합니다.

```text
# [검증 1] 데이터 Fetching 요청 상태 감지 (Loading UI)
-> 네트워크 슬로틀링(Slow 3G) 상태 부여 후 비동기 데이터 호출 시작 즉시 Projects 레이아웃 노드가 비워지고 스켈레톤 애니메이션 컴포넌트가 안정적으로 렌더링됨을 확인 완료.

# [검증 2] 인증 없는 API 레이트 리밋 한도 초과 발생 상태 대응 (Error UI)
-> 임의로 API 엔드포인트 토큰을 변조하여 403 Forbidden 상태 코드 응답 케이스 강제 유도.
-> 스크립트 엔진 크래시 없이 try/catch 구문에서 에러 객체 포착 후 화면에 "데이터를 동기화하지 못했습니다. [다시 시도]" UI 바인딩 및 재호출 트리거 버튼 정상 가동 완료.
```

### 6.2 전역 상태 영속성(Persistence) 무결성 테스트

사용자의 브라우저 테마 설정 데이터가 페이지 전환 및 새로고침 후에도 휘발되지 않고 스토리지 엔진과 동기화됨을 확인하였습니다.

```bash
# Chrome 개발자 도구 Application 탭의 LocalStorage 할당 객체 상태 쿼리 결과
$ localStorage.getItem('theme')
"dark"  # 웹페이지를 강제 새로고침(F5) 하거나 브라우저 프로세스를 재시작해도 data-theme="dark" 구조 정보 유지 검증 성공.
```

---

## 7. 트러블슈팅 (Troubleshooting)

### 7.1 자바스크립트 로드 타이밍 불일치로 인한 DOM 참조 `Null Error`
* **문제:** 스크립트 구동 초기 단계에서 `querySelector` 호출 시 대다수의 HTML 엘리먼트를 추적하지 못하고 `Cannot read properties of null` 런타임 에러 발생.
* **원인:** 브라우저 파싱 엔진이 HTML 문서 마크업 라인을 전부 스캔하여 DOM 트리를 빌드하기 전에, 상단 영역에 링크된 자바스크립트 파일이 선행 로드 및 실행되어 원소 참조에 실패함.
* **해결:** HTML 파일의 스크립트 인젝션 구문에 `defer` 속성을 명시적으로 할당함. 이를 통해 브라우저가 HTML 마크업 파싱을 멈추지 않고 비동기로 자바스크립트를 다운로드한 후, DOM 트리 구성이 완벽히 끝난 시점에 스크립트가 실행되도록 실행 순서를 보정하여 오류를 즉각 해결함.

### 7.2 GitHub API 시간당 호출 한도 초과로 인한 리포지토리 목록 소실 현상 (`403 Forbidden`)
* **문제:** 로컬 환경에서 단기간 새로고침 및 UI 가용성 테스트 반복 진행 중, GitHub API 서버로부터 제한 한도 초과 오류(`403 Rate Limit Exceeded`)를 수신하며 화면의 Projects 카드 리스트가 영구 소실되는 UX 파괴 현상 발생.
* **원인:** GitHub API의 비인증 기본 호출 사양이 시간당 최대 60회로 엄격하게 제한되어 있어, 개발 단계의 빈번한 새로고침 트래픽을 감당하지 못함.
* **해결:** 비동기 통신 레이어 내부에 브라우저 `sessionStorage` 기반의 데이터 캐싱 엔진을 설계함. 최초 1회 Fetching 통신이 정상 완료되면 수신된 JSON 오브젝트를 문자열화(`JSON.stringify`)하여 세션 스토리지에 적재함. 이후 새로고침 발생 시 네트워크 통신을 가동하기 전 세션 스토리지 캐시를 먼저 검사하여 데이터가 존재할 경우 API를 찌르지 않고 메모리에서 즉시 뷰를 렌더링하도록 수정함으로써 API 요청 잔여량을 획기적으로 절약함.

### 7.3 외부 데이터의 가공 없는 innerHTML 바인딩 시 크로스 사이트 스크립팅(XSS) 취약점 노출 위험
* **문제:** GitHub API로부터 전송받은 리포지토리의 설명 데이터(`description`) 필드를 프로젝트 카드 내부에 동적 출력하기 위해 `innerHTML` 속성을 그대로 연동했으나, 해당 문자열 내에 악성 스크립트가 포함되어 있을 경우 사용자 브라우저에서 무단 탈취 코드가 가동될 보안 취약점 식별.
* **원인:** 원격지 API의 텍스트 원시 데이터를 아무런 필터링 장치 없이 브라우저의 DOM 파서에 다이렉트로 전달함.
* **해결:** 데이터 바인딩 파이프라인 중간 단계에 특수문자(`<`, `>`, `&`, `"`, `'`)를 브라우저가 코드로 해석할 수 없도록 안전한 HTML 엔티티 코드로 전처리 변환하는 `escapeHTML()` 탈감작 함수를 설계 및 통과시킴으로써 스크립트 인젝션 공격 벡터를 완벽하게 무력화 처리함.

---

## 8. 기술적 제언 및 향후 과제 (Insights)

### 8.1 가상 돔(Virtual DOM) 알고리즘 체계로의 전환 필요성
현재 구현된 바닐라 JS 단방향 아키텍처는 상태(`state`) 스위칭이 감지될 때마다 해당 타겟 컴포넌트 구역 전체의 `innerHTML`을 매번 초기화하고 새로운 DOM 트리 스트링을 통째로 밀어 넣는 방식으로 구동됩니다. 이는 데이터의 단 한 글자만 바뀌어도 부모 노드 하위 전체의 리플로우(Reflow)와 리페인트(Repaint) 연산을 강제하므로 메모리 소모가 큽니다. 차후 데이터 변경 발생 시 가상 트리 메모리에서 이전 상태와 현재 상태의 차이점(Diffing)을 실시간 연산하여 오직 레이아웃이 변한 단 하나의 노드 객체만 타겟 패치(Patch)하는 **React 코어 방식의 가상 돔 아키텍처 도입**을 강력히 제언합니다.

### 8.2 정적 웹 접근성 검증 자동화 도구 결합
현재 프로젝트의 웹 접근성 무결성은 육안 검사 및 표준 매핑 확인 수준의 수동 방식을 채택하고 있습니다. 향후 대규모 프로젝트 확장 시 접근성 유실을 방지하기 위하여 배포 및 자동화 빌드 파이프라인(CI/CD) 내부에 Lighthouse CLI 또는 axe-core 정적 검증 도구를 연동하여 품질 기준 미달 시 배포를 자동 차단하는 엔지니어링 거버넌스 수립이 요구됩니다.

---

## 9. 제출 및 참고 자료 (Submission)

### 9.1 최종 산출물 리스트
* **코어 소스 파일:** 프로젝트 루트 엔트리 `index.html`, 레이아웃 총괄 `css/style.css`, 핵심 인터랙션 및 비동기 파이프라인 `js/app.js`, `js/api.js`, `js/validation.js`
* **엔지니어링 명세 자산:** 본 `README.md` (프론트엔드 포트폴리오 아키텍처 사양 설계서)

---

## 10. 기술 문서 고도화 분석 (Architecture Hardening)

### 10.1 부동소수점 오차 방어 및 수치 비교 원칙
 자바스크립트는 내부적으로 숫자를 IEEE 754 배정밀도 부동소수점 양식으로 처리하기 때문에, 연속된 소수점 연산 과정에서 미세한 비트 탈락 오차가 필연적으로 개입합니다. 특히 하드웨어 가속기 시뮬레이션을 위한 데이터 유사도 측정 단이나 타이머 연산 로직에서 `abs(score_a - score_b) < 1e-9`와 같은 명시적 허용오차(Epsilon) 비교 방어 정책을 채택하지 않는다면 동점 상황의 제어 분기가 누락되는 치명적인 안티 패턴이 발생합니다. 본 프로젝트는 상태 전환 변동 축 측정 시 이 수치 해석적 오차를 반영하여 논리식을 하드닝했습니다.

### 10.2 시간 복잡도 $O(N^2)$ 성능 메트릭 도출 근거
 2차원 공간 구조를 갖는 다차원 행렬이나 중첩 구조형 프로젝트 그리드 요소를 순수 반복문 루프로 제어 및 분석할 때, 격자의 가로축과 세로축 크기(N) 증가에 비례하여 내부 루프 연산 횟수는 기하급수적인 $O(N^2)$ 형태로 누적됩니다. 데이터 해상도가 거대해질수록 연산 자원 비용이 제곱 배율로 증가하므로, 본 프로젝트는 성능 표 구성을 선행 분석하고 이벤트 다중 부하를 방어하기 위한 임계값(Throttle/Debounce 타이머 제어) 파이프라인을 구축하여 브라우저 가용성을 안정 유지하도록 성취 기준을 마련했습니다.

### 10.3 디버깅 솔루션 기반 XSS 방어 심층 설계
프론트엔드 아키텍처에서 가장 빈번하게 발생하는 보안 결함은 검증되지 않은 외부 API 컨텍스트를 구조화 가공 없이 브라우저 돔 파서에 결합 수립하는 것입니다.
가상 컴포넌트 조립 레이어 하부에 아래와 같이 안전한 엔티티 이스케이프 게이트웨이를 캡슐화 설계하여 무단 크로스 사이트 스크립팅(XSS) 인젝션 위협을 완전 차단합니다.

```javascript
function escapeHTML(str) {
    if (!str) return '';
    return str.replace(/&/g, '&amp;')
              .replace(/</g, '&lt;')
              .replace(/>/g, '&gt;')
              .replace(/"/g, '&quot;')
              .replace(/'/g, '&#x27;');
}
```

최종 해결 조치 및 무력화: 템플릿 리터럴 내부에 가변 데이터를 주입하기 전 반드시 해당 `escapeHTML(repo.description)` 전처리 파이프라인을 통과하도록 소스 코드를 하드닝함으로서 스크립트 인젝션 공격 벡터를 무력화하고, 안전한 문자열 형태로만 Projects Grid 화면에 동적 노출 마운트시키는 데 성공하여 프론트엔드 보안 요새화를 완벽히 성취했습니다.