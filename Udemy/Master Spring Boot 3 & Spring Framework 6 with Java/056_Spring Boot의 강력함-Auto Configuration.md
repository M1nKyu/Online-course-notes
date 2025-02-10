---
created: 2024-10-30 15:53
course-name: Master Spring Boot 3 & Spring Framework 6 with Java
instructor: Ranga Karanam
section: 섹션 05 - Spring Boot 시작하기
presentation: 
tags:
  - lecture-note
  - "#udemy"
---
## 강의 노트
> Spring Boot를 사용하여 웹앱을 빌드할 때 많은 configuration이 필요하다.
	- Component Scan, DispatcherServlet, Data Sources, JSON Conversion, ...
> 어떻게 간소화?
> 	- 애플리케이션 용 자동화 설정인 **Auto Configuration**을 사용한다.
#### Auto Configuration
- Auto Configuration을 결정하는 데 사용되는 두가지
	1. 클래스 경로에 있는 프레임워크에 따라 생성된다.
	2. 기존 설정
		- Spring Boot는 디폴트 자동 설정을 제공한다.
			- 이를 오버라이드할 수 있다.

- 로직은 어디에서 정의?
	- 특정 jar에서 정의 (`spring-boot-autoconfigure.jar`)

###### Auto Configuration을 좀 더 자세히 알아보려면
- `application.properties`에서 로깅 수준을 설정한다.
- 서버를 시작해보면 로그가 훨씬 많이 표시된다.
	- 디폴트는 INFO 수준으로 아주 적은 정보만 출력
```properties
logging.level.org.springframework=debug // org.springframework를 디버그 수준에서 로깅하려는 것
```
- 로그를 자세히 살펴보면 `CONDITIONS EVALUATION REPORT`가 보일 것이다.
	- `Positive matches`는 자동 설정된 항목
	- `Negative matches`는 자동 설정되지 않은 항목이다.
- `Positive matches` 항목 중에서
	- `ErrorMvcAutoConfiguration` 클래스는 자동으로 오류페이지를 설정한다.

###### 좋은 예: Spring Boot Starter Web
- Spring Boot Starter Web을 사용하면 이 모든 것들이 자동으로 설정된다.
	- Dispatcher Servlet 
	- Embedded Servlet container 
	- Default Error Pages
	- Bean <-> JSON 
		- Spring Boot Starter Web이 클래스 경로에 있을때 Bean에서 JSON으로의 메시지변환을 자동 제공
		- 메소드에서 Bean 배열 리스트를 반환할 때 자동으로 JSON 변환된 이유이다.


> 
---
## 추가 학습


---
## 다음 강의 노트 : [[057_Spring Boot DevTools로 빠르게 빌드하기]]
