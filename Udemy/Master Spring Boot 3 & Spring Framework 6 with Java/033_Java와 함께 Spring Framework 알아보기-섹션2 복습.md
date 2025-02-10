---
created: 2024-08-27 16:28
course-name: Master Spring Boot 3 & Spring Framework 6 with Java
instructor: Ranga Karanam
section: 섹션 03 - Spring Framework를 사용하여 Java 객체를 생성하고 관리하기
presentation: 
tags:
  - lecture-note
  - "#udemy"
---
## 강의 노트
#### 지난 2개 섹션을 통해
- Spring Framework에 대해 설명했다.
	1. Coupling (결합도)
	2. Java Interfaces
		- 인터페이스를 사용한 느슨한 결합의 도입
	3. Spring Container
		- Spring Container를 시작하는 방법
		- Application Context를 사용하여 Spring Container를 시작했으며
			- 여러 Spring Bean을 만들었다.
	4. Java Bean vs Spring Bean
	5. Dependency Injection
		- 의존성, Autowiring, 의존성 주입의 이해
	6. Dependency Injection Types
		- 의존성 주입의 여러 유형
	7. Annotation
	8. @Bean vs @Component
	9. @Primary vs @Qualifier
	10. Hands-on

- 여기서 학습한 개념은 Spring 기반의 훌륭한 애플리케이션을 구축하는데 유용하기도 하지만
	- 문제를 디버깅하는데에도 유용하다.

#### 다음 섹션은
- 다음 섹션을 통해 한층 심화된 내용을 다룰 것이다.
###### 1. Lazy Initialization (지연 초기화)
- 지금까지 우리가 만든 Bean은 Spring Container의 시작에서 초기화되었다.

###### 2. Bean Scopes
- Spring Framework에 있는 다양한 Bean 범위를 다룰 것이다.
- **프로토타입**과 **싱글톤** 두가지 중요한 개념에 집중할 것이다. 
###### 3. PostConstruct & PreDestroy 
- Bean의 의존성이 준비된 후 
	- 어떤 ==작업을 수행하거나, Spring Context에서 Bean이 제거되기 전==에
		- 작업을 해야 한다면 
			- 이 경우에 ==PostContstruct와 PreDestroy 메소드==가 매우 유용할 것이다.

###### 4. Jakarta EE

###### 5. Context & Dependency Injection (CDI)
- Jakarta EE의 의존성 주입과 관련된 중요한 사양 중 하나이다.

###### 6. XML Configuration
- 지금까지 강좌에서는 Java Configuration을 사용하고 있었다.
- 우리가 정의한 모든 설정은 Java 소스 파일에 정의되어 있다.
- 20년 전 대다수의 프로젝트에 XML 설정을 사용했었다.

- 자바와 XML 설정의 차이점을 비교해보고
	- 언제 XML, 언제 Java 설정을 사용해야할 지 알아볼 것이다.

###### 7. Alternatives - @Conponent
- @Component와 관련된 여러 대안에 대해 알아볼 것이다.
	- 이를 Spring Stereo Type Annotation (스테레오타입 어노테이션) 이라고 한다.
- 관련하여 @Component, @Service, @Repository 등을 알아볼 것이다.

###### 8. Spring Big Picture
- Spring Framework에 대한 전반적인 모습을 그릴 것이다.

###### 9. Spring Modules & Projects

###### 10. Why is Spring Popular?
- 오늘날 Spring Framework가 자바 세계 최고의 Framework인 이유를 알아볼 것이다.



---
## 추가 학습


---
## 다음 강의 노트 : [[033.5_섹션 퀴즈]]
