# modern_API
스프링 6와 스프링 부트 3로 배우는 모던 API 개발 도서 

## 1부 RESTful 웹 서비스

---
<details>
<summary>1부.RESTful 웹 서비스 기본사항</summary>

## REST(REpresentation State Transfer)
  
- 클라이언트-서버개념을 기반으로 한다.
- 스테이트리스(stateless) => 특정 상태를 유지하지 않는다.
- HTTP 캐시 제어(cache control) => REST API를 캐시할 수 있다.
- 주요 컴포넌트
  - 리소스와 URI
    - World Wide Web(WWW)상의 모든 문서는 HTTP 관점에서 리소스로 여겨진다.
    - URI : 웹에서 리소스의 위치, 이름, 또는 이 둘을 모두 사용해 리소스를 식별하는 문자열을 의미한다.
      - URL : 리소스의 네트워크 위치를 식별하는 식별자 역할
        - 리소스에 도달하는 방법도 알려준다.
        - 프로토콜 이름, 권한 컴포넌트의 일부인 호스트 이름과 포트
        - 경로
        - 선택 사항인 쿼리 및 프로그먼트 컴포넌트를 포함한다.
      - URN : 
        - 스킴으로 시작하는 URI 타입을 의미하다.
      - 구문 : scheme:[//authority]path[?query][#fragment]
        - scheme : HTTP, HTTPS, MAILTO, FILE, FTP등이 대표적인 예시다. 반드시 IANA에 등록되어 있어야 한다.
        - authority :
          - 선택적 필드
          - 사용자 정보
          - 호스트
          - 포트
        - path : 슬래시 문자로 구분된 일련의 세그먼트가 포함된다.
        - query : 선택적 컴포넌트다. 물음표가 온다. 각 매개변수는 &로 구분되고 매개변수 값은 등호 연산자를 사용해서 할당한다.
        - fragment : 선택적 필드다. 부속 리소스를 가리키는 프래그먼트 식별자를 가진다.
  - HTTP 메소드와 상태 코드
    - POST
      - 이름과 값 쌍을 제출할 때 URL의 크기의 제한이 없다.
    - GET
      - 리소스 읽기 오퍼레이션과 연결된다.
    - PUT
      - 일반적으로 리소스 갱신 오퍼레이션과 연결된다.
      - 깃허브 API v3에서는 PUT을 사용해서 기존 리소스를 교체하는 방법을 채택함.
    - DELETE
      - 리소스 삭제 오퍼레이션과 연결된다.
    - PATCH
      - 리소스 부분 갱신 오퍼레이션과 연결된다.
  - HATEOAS(Hypermedia as the Engine of Application State)
    - HATEOAS라는 하이퍼미디어를 통해 동적으로 정보를 제공한다.
    - 클라이언트가 서버의 응답을 통해 애플리케이션 상태를 변경하고 탐색할 수 있도록 하이퍼미디어를 포함한다.
    - 고정된 엔드포인트를 하드코딩하지 않고, 서버가 제공하는 링크를 따라가면서 동적으로 API를 탐색할 수 있다.
- REST API 설계 베스트 프랙티스
  - 엔드포인트 경로에서 리소스의 이름을 지정할 때 동사형이 아닌 명사형 단어를 사용한다.
  - 엔드포인트 경로에서 컬렉션 리소스의 이름을 지정할 때 복수형을 사용
  - 하이퍼미디어 사용
  - API 버전 관리
    - 헤더 사용
    - 엔드포인트 경로 사용
  - 중첩된 리소스
  - API보안
    - 암호화된 통신을 위해 항상 HTTPS를 사용한다.
    - 항상 OWASP의 주료 API 보안 위협 및 취약점을 살핀다.
    - 보안 REST API에는 인증이 있어야 한다.
      - REST API는 스테이트리스 특징을 가지고 있어서 쿠키나 세션말고 JWT, OAuth 2.0기반 토큰을 사용해야 한다.
  - 문서 유지 관리
  - 권장되는 상태 코드 준수
  - 캐싱 보장
    - REST API응답에 추가 헤더를 제공하는 방법
      - ETag : 리소스 표현의 해시 또는 체크섬 값을 포함하는 특별한 헤더 값
      - Last-Modified : RFC-1123형식의 타임 스탬프값을 사용하고 ETag보다 정확도가 떨어진다.
  - 단위시간당 요청량 제한 유지 관리
    - X-Ratelimit-Limit : 현재 기간에 허용된 요청 수
    - X-Ratelimit-Remaining : 현재 기간에 남은 요청 수
    - X-Ratelimit-Reset : 현재 기간의 남은 시간
    - X-Ratelimit-Used : 현재 기간에 사용된 요청 수
  - 어포던스 : 행동유동성
</details>

<details>
<summary>2장 스프링의 개념과 REST API</summary>

## 스프링의 패턴과 패러다임 이해하기
### IOC란v
- 객체의 생성 및 흐름 제어를 개발자가 직접 하는것이 아닌, 프레임워크나 컨테이너가 대신 제어하는 설계 원칙이다.
### DI란
- 객체간의 의존성을 코드 내부에서 직접 생성하는게 아니라 외부에서 주입하는 디자인 패턴 및 기술
- IOC가 큰 개념이고 DI가 IOC를 실천하기 위한 방법으로 생각하면 좋다.
### AOP란
- OOP와 함꼐 작동하는 프로그래밍 패러다임이다.
- SRP(Single Responsibility Principle)단일 책임 원칙으로 불리기도 한다.
- 횡단 관심사를 추상화하고 캡슐화한다.
- 코드의 여러 부분에 걸쳐 관점 동작을 추가한다.
- 코드를 쉽게 유지하고 확장할 수 있도록 횡단 관심사에 대한 코드를 모듈화한다.
### IOC 컨테이너
- BeanFactory
- ApplicationContext
  - 통합 라이프 사이클 관리
  - BeanPostProcessor, BeanFactoryPostProcessor 자동 등록한다.
  - MessageSource에 쉽게 액세스할 수 있는 국제화
  - 내장된 ApplicationEvent를 사용한 이벤트 발행
  - 웹 애플리케이션을 위한 애플리케이션 레이어 특화 컨텍스트인 WebApplicationContext제공
  - 설정 메타데이터를 이용해서 지시를 수행한다.
  - 스프링 컨테이너에 설정 메타데이터, 비즈니스 객체를 넣으면 사용할 준비가 된 완전하게 설정된 시스템을 생성한다.
- 위 두개의 인터페이스가 중요한 기능을 한다.
## Bean과 그 범위
- @Bean을 붙이면 초기화 및 파괴 수명 주기 메소드도 전달할 수 있다.
  - bean의 이름은 첫 글자가 소문자인 클래스 이름이다.
  - name 애트리뷰트를 사용해서 bean의 이름과 별칭을 정의할 수 있다.
  - @Controller, @Service, @Repository, @Configuration, @Component에는 @Bean이 포함되어 있다.
### ComponentScan 애노테이션
- bean의 자동 스캔을 허용한다.
- 베이스 패키지 내의 모든 클래스를 살펴보고 bean을 찾는다.
- @Component로 메타-에노테이트된 다른 애노테이션을 스캔한다.
- 베이스 패키지 : Spring Framework에서 컴포넌트들을 자동으로 검색하는 기준이 되는 패키지를 의미한다.
  - @SpringBootApplication을 이용해서 이 애노테이션이 있는 패키지가 기본 베이스 패키지가 될 수 있다.
  - @ComponentScan을 이용해서 다른 패키지를 스캔하는게 가능하다.
### Bean의 범위
- 기본 범위는 싱글톤이다. 즉 IOC컨테이너당 하나의 인스턴스만 생성되고 동일한 인스턴스가 주입된다.
- 요청이 올 때마다 새 인스턴스를 생성하려면 그 bean을 프로토타입 범위로 정의할 수 있다.
- 범위
  - 싱글톤
  - 프로토타입
  - 요청 : 유효한 HTTP 요청 라이프 사이클 동안에 HTTP 요청마다 하나의 bean 인스턴스가 생성된다.
  - 세션 : HTTP 세션, 다시 말해 유효한 Http세션 라이프 사이클 동안에 하나의 인스턴스가 생성된다.
  - 응용 프로그램 : 응용 프로그램 범위, 다시 말해 유효한 서블릿 컨텍스트 라이프 사이클 동안에 하나의 인스턴스가 생성된다. 
  - 웹소켓 : 각 WebSocket 세션에 대해 단일 인스턴스가 생성된다.
## 자바를 사용하여 bean 설정
### @Import 애노테이션
- 컨텍스트를 수동으로 인스턴스화하려면 @Import를 사용해서 설정을 모듈화해야 한다.
- 명시적으로 설정 클래스만 불러올 때 사용한다.
### @DependsOn 애노테이션
- 특정 Bean이 다른 Bean보다 먼저 생성되어야 하는 경우 명시적으로 지정이 가능하다.
- 원래 spring은 자동으로 생성한다.
### 주입을 하는 방법
- 생성자로 의존성 정의
- 설정자 메소드로 의존성 정의
- 클래스 프로퍼티를 사용한 의존성 정의
## 애노테이션을 사용하여 bean의 메타데이터 설정
### @Autowired
- bean을 주입하기 위해 리플렉션을 사용한다. -> 다른 주입 접근 방법보다 비용이 많이 든다.
  - 리플렉션을 사용할 때 JVM은 메서드나 필드의 타입과 위치를 런타임에 동적으로 해석하고 호출한다.
  - JVM의 JIT 컴파일러가 리플렉션을 사용하는 코드를 효율적으로 최적화 하기 어렵다.
  - 컴파일 시점에서 타입 체크가 이루어지지 않고 런타임에 이뤄져서 예외 발생 가능성이 높다.
  - 리플렉션 API를 사용할 때 객체 메타데이터가 메모리에 로드되고 Bean이 많아질수록 메타데이터가 메모리에 축적된다.
- bean을 주입할 생성자 또는 설정자 메소드가 없는 경우에만 작동한다.
- 런타임에 동적으로 객체를 주입할 수 있다.
### 타입별 일치
### 한정자별 일치
- @qualifier()메서드를 이용해서 하나의 클래스에 대해 두개의 구현체가 있는 경우 bean에는 클래스가 한개만 등록되어서 두개의 구현체가 오면 @NoUniqueBeanDefinitionException이 반환되는 오류를 막을 수 있다.
### 이름으로 일치
- @Inject
- @Resource 
- 두가지는 잘 사용하지 않는다.
### @Primary의 목적?
- @Primary가 있는 bean 애노테이션은 auto-wired필드에 주입된다.
- 두개의 클래스에 대한 구현체가 두가지 존재할때 우선순위를 둘 수 있게 해준다.
### @Value는 언제 쓰는가
- .yml, .properties 외부 프로퍼티 파일의 사용을 지원해준다.
- @Properties는 추가적인 프로퍼티 파일을 로드할 때 사용됩니다.
  - YAML파일은 읽을 수 없다.
```java
@Configuration
@PropertySource("classpath:custom.properties") //다음과 같은 형태로 사용이 가능하다.
public class AppConfig {
    
    @Value("${custom.value}")
    private String customValue;

    @Value("${app.version}")
    private String appVersion;

}

```

## AOP용 코드 작성
- AOP는 횡단 코드를 모듈화해 개발자의 코드 작성 및 수정을 돕는다.
- @Aspect 애노테이션은 클래스를 Aspect로 표시하는데 사용된다.
  - @Around는 Advice를 정의하는 메소드 애노테이션이다.
    - 성능측정, 트랜잭션 관리, 등에 사용된다.
    - 대상은 메서드이다.
  ```java
    @Around("execution(* com.example.service.*.*(..))")
    public Object logExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
    long start = System.currentTimeMillis();

      Object proceed = joinPoint.proceed();

      long duration = System.currentTimeMillis() - start;
      System.out.println(joinPoint.getSignature() + " took " + duration + "ms");
      return proceed;
  }
    ```
  - com.example.service 패키지의 모든 클래스 내에 있는 모든 메서드가 실행될 때 이 @Around로 선언된 Advice가 함께 실행됩니다.
    - (Joinpoint)특정시간을 사용하면 대상객체 와 프록시를 갭처할 수 있다.  
      - Joinpoint는 스프링이 제공하는 AOP Advice가 적용될 수 있는 지점
    - 프록시 - 스프링 AOP 모듈에 의해 생성된다.
      - CGLIB나 JDK Proxgy lib를 이용해서 대상 객체에 어드바이스를 적용한 후 생성되는 객체다.
      - 대상 객체를 직접 수행하지 않고 서브클래스를 만들어 Advice에 삽입한다.
    - Advice는 대상 객체에 적용된다.
      - 스프링 AOP는 대상 객체의 서브 클래스를 생성하고 메소드를 오버라이드 하고 어드바이를 삽입한다.
## 스프링 부트를 사용하는 이유
  - 스프링 부트에 컨테이너가 없는 이유 
    - 오늘날의 클라우드 환경 또는 PaaS는 안정성, 관리 또는 확장성과 같은 컨테이너 기반 웹 아키텍처가 제공하는 대부분의 기능을 제공한다.
    - 따라서 자체적으로 초경량 컨테이너를 만드는데 중점을 둔다.
## 서블릿 디스패처의 중요성 이해
  - 모델 : 모델은 애플리케이션 데이터를 포함하는 자바 객체다. 또한 응용 프로그램의 상태를 나타낸다.
  - 뷰 : HTML/JSP/템플릿 파일로 구성된 프레젠테이션 레이어다. 데이터를 렌더링 하고 HTML출력을 생성한다.
  - 컨트롤러 : 사용자 요청을 처리하고 모델을 믿르하다.
  - DispatcherServlet : MVC의 일부분으로 프런트 컨트롤러 역할을 한다.
  - 스프링 MVC의 사용자 요청 흐름
    - 사용자는 HTTP 요청을 보내고, DispatcherServlet이 요청을 수신한다.
    - DispatcherServlet는 배턴을 HandlerMapping에 전달한다. 
    - HandlerMapping은 요청된 URL에 대해 올바른 컨트롤러를 찾는 작업을 수행하여 DispatcherServlet에 다시 전달한다.
    - DispatcherServlet은 HandlerAdaptor를 사용해서 Controller처리
    - HandlerAdaptor는 Controller에서 메소드 호출
    - Controller은 관련 로직을 수행 및 응답 구성
    - 스프링은 자바에서 JSON/XML 반환을 위해 요청 및 응답 객체의 마샬링/언마샬링을 사용한다. => 한 객체의 메모리 표현방식을 저장 또는 전송에 적합한 다른 데이터 형식으로 변환하거나 반대의 과정
    - 
</details>

<details>
<summary>3장. API명세 및 구현</summary>

## OAS(OpenAPI Specification)로 API 설계
- 설계 우선 접근 방식을 사용해야 한다 -> 관리, 잦은 수정, 관련 부서와의 문제
- string, number, integer, boolean, object, array를 지원한다.
## OAS를 스프링 코드로 변환
- src/main/resources/api/config.json
</details>