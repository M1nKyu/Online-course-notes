---
created: 2024-08-07 00:02
course-name: Master Spring Boot 3 & Spring Framework 6 with Java
instructor: Ranga Karanam
section: 섹션 02 -Java Spring Framework 시작하기
presentation: 
tags:
  - lecture-note
  - "#udemy"
---
## 강의 노트
#### Spring Container 란?
- 이를 ==Spring Context== 라고도 한다.
- Spring Container는 Spring Bean과 그들의 lifecycle까지 관리한다.

- 우리는 Java 클래스들 (POJOs)와 설정파일(Config)도 만들었다.
	- 그리고 이를 Container(IOC Container) 에 인풋으로 전달했다.
- ![[Pasted image 20240807003005.png|300x300]]

###### 인풋으로 전달한 것을 코드를 보면
- **Java 클래스 두 개**를 만들었고, (POJOs)
```java
record Person (String name, int age, Address address){};
record Address (String firstLine, String city){};
```

- **Bean에 대한 모든 정의가 있는 설정 파일** HelloWorldConfiguration을 만들었었다. (Config)
```java
@Configuration
public class HelloWorldConfiguration { ... }
```

- 이는 ==Spring Container를 만들기 위한 인풋==이다.
###### Spring Container의 아웃풋은 **Ready System** 이라고 한다.
- 아래와 같은 형식의 시스템이다. (*런타임 시스템*)
- ![[Pasted image 20240807002732.png|400x250]]
- JVM 내부는 설정한 모든 Bean을 관리하는 Spring Context가 있다.

- **Java class와 config를 만들면 IOC 컨테이너가 런타임 시스템을 만든다.**
	- ==이 시스템이 Spring Context를 만들고 모든 Bean을 관리==하는 것이다.

###### Spring Container는 다양한 용어로 사용된다.
- Spring Context 라고도 하고,
	- IOC Context 라고도 한다.

> IOC는 제어의 역전을 의미한다. (나중에 논의)

> [!abstract] 결론적으로 아래는 어떻게 부르든 동일한 것을 의미한다.
> - Spring Container
> - Spring Context
> - IOC Container

#### Spring Container나 Spring IOC Container에 대해 얘기할 때 논의 대상이 되는 IOC Container는 두가지
###### 첫 번째는 Bean Factory
- 이는 Basic한 Spring Container이다.
> 강사도 Bean Factory를 직접 사용한 적은 단 한번도 없다고 한다.
- 유일한 사용 사례는 메모리에 심한 제약이 있는 IOT Application 뿐이다.

###### 두 번째는 ==Application Context== 
- *엔터프라이즈* 전용 기능이 있는 고급 Spring 컨테이너이다.
- 웹 애플리케이션을 구축하거나 *국제화 기능*이 필요한 경우 또는
	- *Spring AOP* 또는 *Spring 측면 지향 프로그램*과 잘 통합되도록 하려는 
		- 시나리오에서 Application Context를 사용한다.

- 일반적으로 대부분 엔터프라이즈 애플리케이션에는 이런 모든 기능이 필요하며
	- 따라서, ==Application Context를 가장 자주 사용하는 Container로 보아야 한다==.

- 코드에서도 Application Context를 사용하고 있다.
	- AnnotationConfigApplicationContext를 만들고 있다.
```java
public class App02HelloWorldSpring {
    public static void main(String[] args){

        var context =
                new AnnotationConfigApplicationContext(HelloWorldConfiguration.class); // 📌

        System.out.println(context.getBean("name"));
	    ...
    }
}
```

- Application Context는 웹 애플리케이션, 웹 서비스, REST API, 마이크로서비스에 사용하는 것이 좋다.

---
## 추가 학습
#### Java class와 Config를 만들면 IOC Container가 Runtime System을 만든다는 것의 의미
###### Java 클래스와 Config 생성
- 개발자가 일반 Java 클래스와 설정 클래스(보통 @Configuration 어노테이션이 붙은)를 작성합니다.
	- 이 클래스들은 애플리케이션의 구조와 동작을 정의합니다.

###### IoC 컨테이너의 역할
- IoC 컨테이너(예: Spring의 ApplicationContext)는 이러한 클래스와 설정을 읽고 해석합니다.
- 컨테이너는 클래스 간의 의존성을 분석하고 관리합니다.

###### Runtime System 생성
- IoC 컨테이너는 이 정보를 바탕으로 **실행 시간에 객체들을 생성하고 연결**합니다.
	- 이렇게 생성된 객체 네트워크가 "Runtime System"을 구성합니다.

> [!important]
> 핵심은 개발자가 **객체 생성과 의존성 관리를 직접 하지 않고, 프레임워크에 위임한다는 것**이다.
> 이로 인해 코드의 결합도가 낮아지고 유지보수성이 향상된다.

#### Runtime System과 Ready System
###### Runtime System (런타임 시스템):
   - 정의: 프로그램이 **실행되는 동안 작동하는 환경** 또는 시스템을 말합니다.
   - 특징:
     - 프로그램 실행 중에 **메모리 할당, 객체 생성, 가비지 컬렉션** 등을 관리합니다.
     - Java의 경우, ==JVM(Java Virtual Machine)이 런타임 시스템의 핵심 부분==입니다.
   - 역할: 애플리케이션의 실제 동작을 지원하고 관리합니다.

###### Ready System (레디 시스템):
   - 정의: **실행 준비가 완료된 시스템**을 의미합니다.
   - 특징:
     - 모든 필요한 구성요소가 초기화되고 연결된 상태입니다.
     - 실제 실행이나 사용자 요청을 처리할 준비가 된 상태를 말합니다.
   - 역할: 애플리케이션이 즉시 작동할 수 있도록 준비된 상태를 나타냅니다.

###### 주요 차이점:
- **Runtime System**은 프로그램 **실행 중 지속적으로 작동**하는 환경입니다.
- **Ready System**은 **실행 직전**의 준비된 상태를 나타냅니다.

###### Spring Framework의 맥락에서:
- IoC 컨테이너가 ==모든 빈(Bean)을 생성하고 의존성을 주입한 후의 상태가 "Ready System"==이라고 볼 수 있습니다.
- 애플리케이션이 실제로 요청을 처리하기 시작하면 "Runtime System" 단계로 진입합니다.

- 이 두 개념은 프로그램의 생명주기에서 서로 다른 단계를 나타내며, 애플리케이션의 준비 상태와 실행 상태를 이해하는 데 중요합니다.

#### 엔터프라이즈(Enterprise)
- 복잡성, 규모, 안정성, 보안 등이 주요 고려사항이며, 이에 맞는 솔루션과 접근 방식이 필요하다.
- 소프트웨어 개발에서 "엔터프라이즈급"이라고 하면, 
	- ==대규모 비즈니스 환경==에서 사용할 수 있을 만큼 ==견고하고 확장 가능==한 시스템을 의미한다.

#### 국제화 기능의 의미
- 국제화 기능은 웹 애플리케이션을 ==여러 언어와 지역에 맞게 적응시키는 기능==을 의미합니다. Application Context와 관련하여 국제화 기능은 다음과 같은 요소들을 포함합니다:

1. 메시지 소스 관리:
    - Application Context는 MessageSource 인터페이스를 통해 다국어 메시지를 관리합니다.
    - 각 언어별로 별도의 프로퍼티 파일(예: messages_en.properties, messages_ko.properties)을 사용하여 메시지를 저장합니다.
2. 로케일 해석:
    - LocaleResolver를 사용하여 현재 사용자의 로케일(언어 및 지역 설정)을 결정합니다.
    - 세션, 쿠키, HTTP 헤더 등을 기반으로 로케일을 판단할 수 있습니다.
3. 동적 메시지 해석:
    - 코드 내에서 하드코딩된 문자열 대신 메시지 키를 사용합니다.
    - Application Context가 런타임에 적절한 언어의 메시지로 해석합니다.
4. 포맷팅:
    - 날짜, 시간, 숫자, 통화 등을 현재 로케일에 맞게 포맷팅합니다.
5. 리소스 번들 로딩:
    - 다양한 언어별 리소스(텍스트, 이미지 등)를 로드하고 관리합니다.

#### Spring AOP (관점(측면) 지향 프로그램)
- AOP는 관점 지향 프로그래밍의 약자로, 횡단 관심사(cross-cutting concerns)를 모듈화하는 프로그래밍 패러다임입니다.
###### 주요 개념:
- Aspect: 여러 클래스에 걸쳐 적용되는 기능
- Join Point: 코드 실행 중 Aspect를 적용할 수 있는 지점
- Advice: 특정 Join Point에서 실행되는 코드
- Pointcut: Join Point의 집합을 정의하는 표현식
###### Spring에서 AOP를 사용하는 주요 이점:
- 코드의 중복 제거
- 비즈니스 로직과 부가 기능의 분리
- 모듈성 향상
- 유지보수성 개선

- 예를 들어, 로깅이나 트랜잭션 관리 같은 공통 기능을 여러 메소드나 클래스에 쉽게 적용할 수 있습니다.
---
## 다음 강의 노트 : [[019_Java Bean, POJO, Spring Bean 살펴보기]]
