---
created: 2024-09-03 14:20
course-name: Master Spring Boot 3 & Spring Framework 6 with Java
instructor: Ranga Karanam
section: 섹션 04 - Spring Framework 고급 기능 살펴보기
presentation: 
tags:
  - lecture-note
  - "#udemy"
---
## 강의 노트
- 이전에는 J2EE 였고
	- 중간에는 Java EE 였으며
		- 현재는 Jakarta EE라고 불린다.
#### 간략한 역사
- 초기 Java 버전에서 엔터프라이즈 기능 대부분은 JDK(자바 개발 키트)에 Java 언어로 직접 구축되어 있었다.
- 시간이 지나면서 분리됐다.
	- J2EE(Java 2 Platform, Enterprise Edition)에 따라 분리됐다.
		- `서블릿, JSP, EJB 등 관련된 모든 표준이 새로운 J2EE에 따라 만들어 졌다.`
	- 이후 J2EE는 Java EE가 되었다.
		- `2EE를 Java EE (Java 플랫폼, 엔터프라이즈 에디션)으로 바꾸어 브랜드를 개선했다.`
	- 시간이 흐른 뒤, Jakarta EE가 되었다.
		- `Java EE 소유자인 Oracle에서 Java EE의 권리를 Eclipse Foundation에 넘겼고`
			- `Eclipse Foundation에서 여론 조사를 통해 Java EE를 Jakarta EE로 변경했다.`

###### 오늘날 Jakarta EE에는 어떤 것이 속해있는가
1. Jakarta Server Pages (JSP)
	- 이전에는 Java Server Pages 였다.
2. Jakarta Standard Tag Library (JSTL)
	- 이전에는 Java Standart Tag Library 였다.
3. Jakarta Enterprise Beans (EJB)
4. Jakarta RESTful Web Service (JAX-RS)
5. Jakarta Bean Validation
6. Jakart Contexts and Dependency Injection (CDI) : `의존성 주입 방법과 관련된 규격`
7. Jakarta Persistence (JPA)

> 이제 Spring 6와 Spring Boot 3에서 Jakarta를 지원해서 좋다.

- 이전에는 *javax.* 패키지를 사용했지만
	- 이제는 *jakarta.* 패키지를 사용한다.

- J2EE, Java EE, Jarkarta EE은 규격 그룹이다
	- JSP, JSTL, EJB, CDI, JPA가 그 예이다.

---
## 추가 학습


---
## 다음 강의 노트 : [[041_Spring Framework 및 Java를 통해 Jakarta CDI 알아보기]]
