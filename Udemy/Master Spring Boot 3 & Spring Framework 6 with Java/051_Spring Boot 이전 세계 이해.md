---
created: 2024-09-12 19:22
course-name: Master Spring Boot 3 & Spring Framework 6 with Java
instructor: Ranga Karanam
section: 섹션 05 - Spring Boot 시작하기
presentation: 
tags:
  - lecture-note
  - "#udemy"
---
## 강의 노트
> Spring Boot 이전 세계에 대한 **대략적인** 개요
#### World Before Spring Boot 
- Spring Project를 설정하는 작업이 쉽지 않았다..
- 많은 것을 설정한 후에야 production-ready 한 애플리케이션을 얻을 수 있었다.

- 의존성 관리를 해야했고, 
	- 설정이 많이 필요했으며
		- 비기능 요구사항을 수동으로 구현해야 했다.
###### 1. 의존성 관리 (pom.xml)
- REST API를 만드는 경우에
	- Spring Framework, Spring MVC Framework, JSON binding framework, 로깅 ... 이 필요하다.
		- 따라서 모든 의존성과 그에 포함된 버전을 사용하여 pom.xml을 만들어야 했다.
		- pom.xml에서 프레임워크와 그 버전을 관리해야 한다.
	- 단위 테스트를 하려면 Spring Test Framework, Mockito, JUnit을 가져와야 하고
		- 이에 대한 모든 버전을 관리해야 한다.

###### 2. web.xml
- 웹 애플리케이션의 많은 것을 설정하는데 필요하다.
	- 예를 들어,
		- Spring MVC를 사용할 경우 DispatcherServlet을 설정하는데 필요하다.

###### 3. Spring Configuration
- *Component Scan*을 정의해야 하고
- 웹 애플리케이션을 빌드한다면, *View Resolver*를 정의해야 한다.
- 데이터베이스 관련 애플리케이션을 빌드한다면, 데이터 소스를 정의해야 한다.

- 따라서, 여러 설정을 적절히 지정해야 애플리케이션을 사용할 수 있다.

###### 4. NFRs (Nonfunctional Requirements, 비기능 요구사항)
- Logging: `애플리케이션에는 로깅은 필요하다.`
- Error Handling: `우수한 오류 처리 기능이 필요하다.`
- Monitoring: `프로덕션 단계의 애플리케이션을 모니터링 할 수 있어야 한다, 측정항목을 살펴볼 수 있어야 한다.`

> Spring Boot 이전 세계에서는 이러한 작동하지 NFRs를 수동으로 구현해야 했다.
> 	- Logging, Error Handling, Monitoring


#### 정리하면, 이전 세계에서는
1. Dependency Management (**pom.xml**)
2. Define Wep App Configuration (**web.xml**)
3. Manage Spring Beans (**context.xml**)
4. Implement Non Functional Requirements (**NFRs**)

> ==이 모든 작업은 새 프로젝트를 만들 때마다 반복해야 했다.==

## 추가 학습


---
## 다음 강의 노트 : [[052_Spring Initializer로 새 Spring Boot  Project 설정하기]]
