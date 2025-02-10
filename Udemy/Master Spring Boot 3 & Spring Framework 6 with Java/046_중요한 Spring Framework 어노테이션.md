---
created: 2024-09-06 18:36
course-name: Master Spring Boot 3 & Spring Framework 6 with Java
instructor: Ranga Karanam
section: 섹션 04 - Spring Framework 고급 기능 살펴보기
presentation: 
tags:
  - lecture-note
  - "#udemy"
---
## 강의 노트
> 지난 여러 섹션에서 배웠던 중요 Spring Annotations에 대해 리뷰

|                           Annotation                           | Description                                                                                                                                                                                                                                                |
| :------------------------------------------------------------: | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|                         @Configuration                         | 클래스가 ==@Bean 메서드==를 하나 이상 선언함을 나타낸다.<br><br>Spring Container에서 처리하여 Bean 정의를 생성한다.<br><br>@Configuration을 추가하면 Java 설정 파일을 만든다는 것을 의미한다.<br><br>Java 설정 파일에서는 메서드를 몇개든 정의할 수 있고, 이러한 메서드에 @Bean 을 추가하면 메서드로 반환되는 모든 값에 Spring이 Bean을 자동으로 생성한다.            |
|                         @ComponentScan                         | Spring Framework는 ==모든 컴포넌트가 정의된 위치를 알아야== 한다. @ComponentScan으로 가능하다.<br><br>어노테이션을 사용하여 ==컴포넌트를 스캔할 패키지를 지정==할 수 있다.<br><br>패키지를 지정하지 않으면 어노테이션을 선언한 클래스의 패키지에서 스캔한다.<br><br>현재패키지 뿐만 아니라 하위 패키지에서도 컴포넌트를 스캔할 수 있다.                                       |
|                             @Bean                              | Spring Container로부터 관리되는 Bean을 생성하는 메서드를 나타낸다.                                                                                                                                                                                                             |
|                           @Component                           | 어노테이션한 클래스가 컴포넌트임을 나타낸다.<br><br>@Component 클래스가 ComponentScan에 속한다면 Spring Bean이 생성된다.                                                                                                                                                                     |
|                            @Service                            | 어노테이션한 클래스에 비즈니스로직이 있음을 나타내는 @Component의 한 종류이다.                                                                                                                                                                                                           |
|                          @Controller                           | 어노테이션한 클래스가 컨트롤러임을 나타내는 @Component의 한 종류이다.                                                                                                                                                                                                                |
|                          @Repository                           | 어노테이션한 클래스가 DB에서 데이터를 검색, 조작하는 데 사용된다는 의미로 @Component의 한 종류이다.                                                                                                                                                                                             |
|                            @Primary                            | 여러 Bean이 단일 값 의존성에 자동 연결될 후보일 때 Bean에 우선순위를 부여해야 함을 나타낸다.<br><br>매우 일반적이다, 우선순위를 지정할 뿐이다.                                                                                                                                                                  |
|                           @Qualifier                           | Autowiring 시 후보 Bean의 한정자로 필드나 매개변수에서 사용된다.<br><br>구체적이다, 모든 컴포넌트에 한정자를 추가할 수 있고 Autowiring 시 한정자를 사용할 수 있다.                                                                                                                                               |
|                             @Lazy                              | Spring Bean은 기본적으로 즉시 초기화이다.<br>Context가 실행되는 대로 초기화된다.<br><br>Bean을 지연 초기화하려면 @Lazy를 사용한다.                                                                                                                                                                |
| @Scope(value =<br>ConfigurableBeanFactory.<br>SCOPE_PROTOTYPE) | 프로토타입은 Bean을 참조할 때마다 인스턴스가 새로 만들어진다는 뜻이다.<br><br>하지만 기본적인 스코프는 싱글톤이다. (`SCOPE_SINGLETON`)<br>싱글톤 스코프일 경우 IoC컨테이너에 특정 Bean의 인스턴스 하나씩만 주어진다.<br>싱글톤 스코프일 경우 Bean을 100번 참조해도 같은 인스턴스가 사용된다.                                                                   |
|                         @ProConstruct                          | 의존성 주입이 수행된 이후 초기화를 위해 실행될 메서드를 나타낸다.<br><br>모든 의존성을 Bean에 주입한 후 초기화하려는 경우, <br>모든 의존성이 준비되는 대로 DB에서 몇가지 값을 가져오려는 경우 <br>@ProConstruct를 사용하면 된다.                                                                                                           |
|                          @PreDestroy                           | Container에서 인스턴스를 삭제하는 과정을 거치고 있음을 알려주는 콜백 알림을 수신하는 메서드를 나타낸다.<br><br>보통 특정 Bean에서 보유하는 리소스를 해제하는 데 사용된다.<br><br>Container나 Spring IoC 컨텍스트에서 Bean이 삭제되기 전에 @PreDestroy 어노테이션이 붙은 메서드를 호출한다.<br><br>리소스를 해제or정리해야 하면 @PreDestroy 어노테이션이 붙은 메서드에 구현하는 게 좋다. |
|                             @Named                             | @Named는 CDI 어노테이션이며 @Component와 유사하다.<br><br>`Spring에서 CDI (Contexts & Dependency Injection) 어노테이션을 구현하는 규격이다., `                                                                                                                                          |
|                            @Inject                             | CDI 어노테이션이며, Autowired와 아주 비슷하다.                                                                                                                                                                                                                           |



---
## 추가 학습


---
## 다음 강의 노트 : [[047_간단한 복습 - 중요한 Spring Framework 개념]]
