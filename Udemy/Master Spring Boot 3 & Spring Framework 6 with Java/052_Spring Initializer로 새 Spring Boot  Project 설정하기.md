---
created: 2024-09-23 16:30
course-name: Master Spring Boot 3 & Spring Framework 6 with Java
instructor: Ranga Karanam
section: 섹션 05 - Spring Boot 시작하기
presentation: 
tags:
  - lecture-note
  - "#udemy"
---
## 강의 노트
#### 프로젝트 생성
>[Spring initializer 접속](https://start.spring.io/)
>	여기서 간단한 몇 가지 선택으로 Spring Boot Project를 시작할 수 있다.

```
- 빌드도구: maven

- 언어: Java

- 스프링부트: 3이상 Milestone 버전(M3)
	- Snapshot은 개발 중이므로 사용하지 않는 것이 좋다.
- 메타데이터
	- Group, Artifact : 클래스 이름 및 패키지 이름과 유사하다.
		- Group id: com.in28minutes.springboot
		- Artifact id: learn-spring-boot
	- 나머지는 디폴트 값 지정

- 자바버전: 17 이상

- 의존성 추가
	- Spring Web
		- REST API를 위해 필요하다.
		- Spring MVC로 웹앱과 REST API 빌드에 사용한다.
		- Apache Tomcat을 웹 컨테이너로 사용한다.

- Generate 클릭
```

#### 작동하기
- Project를 Open한다.
- `src/main/java` 의 
	- `com.in28minutes.springboot.learn_spring_boot` 패키지의
		- *LearnSpringBootApplication.java* 파일의 main 메소드를 실행해본다.


---
## 추가 학습


---
## 다음 강의 노트 : [[053_Spring Boot를 사용하여 Hello World API 빌드]]
