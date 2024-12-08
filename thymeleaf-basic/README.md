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
타임리프에서 변수를 사용할 때는 **변수 표현식**을 사용

- **변수 표현식 형식**: `${...}`
- 스프링 EL(Spring Expression Language)을 통해 다양한 데이터 구조의 속성에 접근 가능

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

## 타임리프 유틸리티 객체들
- `#message`: 메시지, 국제화 처리
- `#uris`: URI 이스케이프 지원
- `#dates`: `java.util.Date` 서식 지원
- `#calendars`: `java.util.Calendar` 서식 지원
- `#temporals`: 자바8 날짜 서식 지원
- `#numbers`: 숫자 서식 지원
- `#strings`: 문자 관련 편의 기능
- `#objects`: 객체 관련 기능 제공
- `#bools`: boolean 관련 기능 제공
- `#arrays`: 배열 관련 기능 제공
- `#lists`, `#sets`, `#maps`: 컬렉션 관련 기능 제공
- `#ids`: 아이디 처리 관련 기능 제공 (자세한 설명은 후술)


## 타임리프 URL 표현식
### 1. 단순 URL
- `@{/hello}` → `/hello`

### 2. 쿼리 파라미터
- `@{/hello(param1=${param1}, param2=${param2})}`
- 결과: `/hello?param1=data1&param2=data2`

### 3. 경로 변수
- `@{/hello/{param1}/{param2}(param1=${param1}, param2=${param2})}`
- 결과: `/hello/data1/data2`

### 4. 경로 변수 + 쿼리 파라미터
- `@{/hello/{param1}(param1=${param1}, param2=${param2})}`
- 결과: `/hello/data1?param2=data2`

### 5. 경로 표현
- 상대경로: `hello`
- 절대경로: `/hello`


## 리터럴 (Literals)
리터럴은 소스 코드에서 고정된 값을 나타내는 표기법
### 타임리프의 리터럴 종류
- 문자: `'hello'`
- 숫자: `10`
- 불린: `true`, `false`
- null: `null`

### 문자 리터럴 사용 규칙
1. 기본적으로 작은 따옴표(`'`)로 감싸야 함
   예: `<span th:text="'hello'">`

2. 공백 없이 이어지는 경우 따옴표 생략 가능
   예: `<span th:text="hello">`

3. 공백이 포함된 경우 반드시 따옴표 사용
   예: `<span th:text="'hello world!'">`

### 리터럴 대체 (Literal substitutions)
변수와 텍스트를 편리하게 조합할 수 있는 문법:
`<span th:text="|hello ${data}|">`


## 타임리프 비교 연산 및 조건식
### 비교 연산
- HTML 엔티티 사용 주의:
    - `>` (gt)
    - `<` (lt)
    - `>=` (ge)
    - `<=` (le)
    - `!` (not)
    - `==` (eq)
    - `!=` (neq, ne)

### 조건식
- 자바의 조건식과 유사

### Elvis 연산자
- 조건식의 편의 버전

### No-Operation
- `_` 사용 시 타임리프가 실행되지 않은 것처럼 동작
- HTML 내용을 그대로 활용 가능
- 예: 데이터가 없을 때 "데이터가 없습니다." 문구 그대로 출력


## 속성 값 설정
### 타임리프 태그 속성
- `th:*` 속성은 기존 속성을 대체하거나, 없으면 새로 생성.
- 예: `<input type="text" name="mock" th:name="userA" />` → `<input type="text" name="userA" />`

### 속성 추가
- `th:attrappend`: 속성 값 뒤에 추가
- `th:attrprepend`: 속성 값 앞에 추가
- `th:classappend`: class 속성에 추가

### `checked` 처리
- HTML에서 `checked` 속성은 값과 관계없이 체크됨.
- `th:checked`는 값이 `false`면 속성 제거.
- 예: `<input type="checkbox" name="active" th:checked="false" />` → `<input type="checkbox" name="active" />`


## 반복
### 반복 처리
- `th:each`를 사용해 컬렉션 데이터를 반복 실행.
- 예: `<tr th:each="user : ${users}">`

### 반복 상태 값
- 반복 상태 값 제공:
  - `index`: 0부터 시작
  - `count`: 1부터 시작
  - `size`: 전체 크기
  - `even`, `odd`: 짝수/홀수 여부
  - `first`, `last`: 처음/마지막 여부
  - `current`: 현재 객체


## 조건부 평가
### if, unless
- 조건이 맞지 않으면 태그 자체가 렌더링되지 않음.
- 예: `<span th:text="'미성년자'" th:if="${user.age lt 20}"></span>`

### switch
- `*`는 조건이 없을 때의 기본값 처리.


## 주석
### 주석 종류
1. 표준 HTML 주석: 렌더링 후 유지.  
   예: `<!-- 이 주석은 유지됨 -->`
2. 타임리프 파서 주석: 렌더링 시 제거.  
   예: `<!--/* 이 주석은 제거됨 */-->`
3. 타임리프 프로토타입 주석: 렌더링 후 표시.  
   예: `<!--/*[ 이 내용은 렌더링 후 표시됨 ]*/-->`

## 블록
- `<th:block>`: HTML 태그 없이 로직을 처리할 때 사용. 렌더링 시 제거.
- 예:
  ```html
  <th:block th:if="${user != null}">
    <p th:text="${user.name}"></p>
  </th:block>
