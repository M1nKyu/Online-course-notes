---
created: 2024-09-06 18:38
course-name: Master Spring Boot 3 & Spring Framework 6 with Java
instructor: Ranga Karanam
section: 섹션 04 - Spring Framework 고급 기능 살펴보기
presentation: 
tags:
  - lecture-note
  - "#udemy"
---
## 강의 노트
> 지금까지 Spring Core를 배웠다.
- IoC Container, 의존성 주입, Auto Wiring 등 이전에 학습한 모든 기능
	- 이 모든 것은 기본적인 구성요소이다.
		- 훌륭한 웹 애플리케이션을 만들거나
		- REST API를 생성하거나
		- 인증 및 권한 부여를 구현하거나
		- DB에 통신하거나
		- 다른 시스템과 통합하거나
		- 단위 테스트를 잘 수행하려 할때 필요한 요소이다.

> 지금까지 배운 모든 개념은 앞으로 어떤 애플리케이션을 만들더라도 아주 유용할 것이다.

- 이제 Spring을 더 큰 시각으로 살펴본다.
	- Spring Framework
	- Spring Modules
	- Spring Projects

#### Spring Framework 와 Modules
###### Spring Framework에는 Spring 모듈이 여러개 포함돼있다.
- Spring Framework의 핵심 기능은
	- IoC 컨테이너, 의존성 주입, Auto-Wiring 등 인데,
		- *Core* 모듈에서 이러한 기능을 제공한다.
- Web
	- 웹 애플리케이션이나 REST API를 빌드할 때 *Spring MVC*를 사용할 수 있다.
- Web Reactive
	- 리액티브 애플리케이션을 빌드할 때 
		- *Spring WebFlux* 등의 모듈을 사용할 수 있다.
- Data Access
	- Spring *JDBC*를 사용할 수 있다
- Integration
	- 다른 애플리케이션과 통합할 때
		- Spring *JMS*를 사용한다.
- Testing
	- 단위 테스트(Unit Test)를 작성할 때는 *Spring 테스트 모듈*을 사용한다.

###### 왜 Spring Framework는 모듈로 나뉘어져 있는가?
- Spring Framework가 유명한 주된 이유는 *유연성*
	- 각 애플리케이션마다 요구사항은 다르고,
		- 각 애플리케이션은 다양한 모듈을 사용한다.
- 애플리케이션에서 Spring Framework의 모든 기능을 사용할 필요가 없고
	- 원하는대로 선택하면 된다.
#### Spring Projects
- 애플리케이션 아키텍처는 계속 발전하고 있다.
	- Web -> REST API -> Microservices -> Cloud -> ...

- 스프링은 계속 발전하고 있다.
	- Spring은 ==Spring Project를 통해 발전==한다.
		- *첫번째 프로젝트*: Spring Framework
		- *Spring Security*: 웹 애플리케이션이나 REST API에 보안을 추가
		- *Spring Data*: 동일한 방식으로 여러 DB와 통합할 떄 Spring Data를 사용
			- `이전에는 관계형 DB를 많이 사용했는데 요즘은 NoSQL DB를 많이 사용한다`
			- Spring Data는 NoSQL 및 RDB 없이 통합할 수 있는 한가지 방법이다.
		- *Spring Integration*: 다른 애플리케이션과 통합하는 데 유용한 Spring Project
			- 대부분 애플리케이션은 다른 애플리케이션과 통신하기 때문에
				- 애플리케이션에서 통합 관련 문제를 많이 겪는다.
		- *Spring Boot*: 마이크로서비스를 빌드하는 최고의 프레임워크
			- 마이크로서비스 아키텍처는 갈수록 인기를 더 끌었는데,
				- 이를 아주 빠르게 빌드하기 위해 도입된 새로운 프로젝트
			- 시간이 지나면서 클라우드로 발전하게 됨
				- `Spring Native Application을 빌드할 때는 Spring Cloud를 사용`
###### 계층 구조
> 지금까지 이 계층 구조를 살펴봤다.
- ![[Pasted image 20240912184847.png|400x300]]
- Spring Projects 아래에 첫 번째로 Spring Framework가 있고
	- Spring Framework에는 Core, Test, MVC, JDBC라는 여러 모듈이 있다.
- Spring Boot, Spring Cloud 등 다양한 Spring Project들도 있다.

###### 왜 Spring 생태계가 인기가 많은가
>Spring 관련 프레임워크는 다양한 엔터프라이즈 애플리케이션을 빌드할 때 가장 중요한 프레임워크였다.

- ==Spring은 Bean 생성, Bean과 의존성 연결을 한다.==
	- 느슨하게 결합하여 유지보수가 가능한 애플리케이션을 아주 쉽게 만들 수 있다.
	- 단위 테스트 작성 역시 아주 수월하다.
- ==보일러플레이트 코드를 줄여준다.==
	- Spring을 이용하면 비즈니스로직에 집중할 수 있다.
		- 메서드마다 Exception Handling을 작성할 필요가 없다.
			- 확인된 모든 예외는 런타임 또는 확인되지 않은 예외로 전환된다.
- ==아키텍처의 유연성==
	- Spring이 여러개의 모듈로 나뉘었고,
		- 별개의 프로젝트도 다양하다고 이야기했다.
	- 사용할 모듈과 프로젝트를 고르고 선택할 수 있다.
- ==시간에 따라서 발전한다.==
	- Spring Framework는 첫번째 프로젝트였지만
		- 시간이 지나며 아키텍처가 진화하고 
			- 애플리케이션에 새로운 요구가 생기며 다양한 프로젝트가 도입됐다.
	- Spring 생태계도 함께 발전했다.
		- Spring Security > Spring Data > Spring Boot > Spring Cloud 등 새로운 프로젝트가 많이 도입됐다.




---
## 추가 학습


---
## 다음 강의 노트 : [[048.5_섹션 퀴즈]]
