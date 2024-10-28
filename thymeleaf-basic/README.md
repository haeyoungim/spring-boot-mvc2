# Thymeleaf
Thymeleaf는 Java 기반의 서버 사이드 템플릿 엔진으로, 동적 웹 페이지 생성에 사용됨

## 특징

### 1. 서버 사이드 HTML 렌더링 (SSR)
- 백엔드에서 HTML을 동적으로 렌더링하는 템플릿 엔진
- 서버에서 데이터를 처리하고 HTML에 삽입하여 클라이언트에 전달

### 2. 내추럴 템플릿
- 순수 HTML 구조를 유지하면서 템플릿 기능 제공
- 웹 브라우저에서 직접 열어 내용 확인 가능
- JSP와 달리 소스코드가 섞이지 않아 가독성이 우수

### 3. 스프링 통합 지원
- 스프링 프레임워크와 자연스럽게 통합
- 스프링의 다양한 기능을 쉽게 활용 가능

## 이스케이프 (Escape)

HTML에서 특수 문자를 안전하게 표시하기 위해 사용됨

- **대상 문자**: `<`, `>`, `"`, `'`, `&`
- HTML 엔티티로 변환 (예: `<` → `&lt;`)
- `th:text`와 `[[...]]`는 기본적으로 이스케이프 적용
- XSS(Cross-Site Scripting) 공격 방지에 도움

## 언이스케이프 (Unescape)

HTML 태그를 그대로 렌더링하고 싶을 때 사용함

- `th:utext`와 `[(...)]`로 언이스케이프 적용
- 보안상 위험할 수 있어 필요한 경우에만 제한적으로 사용

> **보안 팁**: 이스케이프는 기본적으로 적용하고, 꼭 필요한 경우에만 언이스케이프를 사용하는 것이 안전함


## 변수 - SpringEL

타임리프에서 변수를 사용할 때는 **변수 표현식**을 사용합니다.

- **변수 표현식 형식**: `${...}`
- 스프링 EL(Spring Expression Language)을 통해 다양한 데이터 구조의 속성에 접근할 수 있습니다.

### 다양한 표현식 사용

- **Object**
    - `${user.username}`: `user` 객체의 `username` 프로퍼티 접근
    - `${user['username']}`: 위와 동일
    - `${user.getUsername()}`: 메서드 직접 호출

- **List**
    - `${users[0].username}`: 리스트의 첫 번째 객체의 `username` 프로퍼티 접근
    - `${users[0]['username']}`: 위와 동일
    - `${users[0].getUsername()}`: 메서드 직접 호출

- **Map**
    - `${userMap['userA'].username}`: 맵에서 `userA` 객체의 `username` 프로퍼티 접근
    - `${userMap['userA']['username']}`: 위와 동일
    - `${userMap['userA'].getUsername()}`: 메서드 직접 호출

### 지역 변수 선언

- **th:with**: 특정 태그 내에서만 사용할 수 있는 지역 변수를 선언할 때 사용

# Thymeleaf 기본 객체

Thymeleaf는 여러 기본 객체를 제공하여 다양한 기능에 접근할 수 있음

## 제공되지 않는 객체 (Spring Boot 3.0부터)

- `${#request}` - HTTP 요청 객체
- `${#response}` - HTTP 응답 객체
- `${#session}` - HTTP 세션 객체
- `${#servletContext}` - 서블릿 컨텍스트 객체

## 제공되는 객체

### 로케일 접근

- `${#locale}` - 현재 요청의 로케일 정보를 제공합니다.

### HTTP 요청 파라미터 접근

- **`param`**: HTTP 요청 파라미터에 접근할 수 있습니다.
  - 예시: `${param.paramData}`

### HTTP 세션 접근

- **`session`**: HTTP 세션 데이터에 접근할 수 있습니다.
  - 예시: `${session.sessionData}`

### 스프링 빈 접근

- **`@`**: 스프링 컨텍스트에서 빈(bean)에 접근할 수 있습니다.
  - 예시: `${@helloBean.hello('Spring!')}`