---
created: 2024-10-30 15:34
course-name: Master Spring Boot 3 & Spring Framework 6 with Java
instructor: Ranga Karanam
section: 섹션 05 - Spring Boot 시작하기
presentation: 
tags:
  - lecture-note
  - "#udemy"
---
## 강의 노트
- 일반적으로 애플리케이션 빌드에는 프레임워크가 많이 필요하다.
	- Build Rest API: `Spring, Spring MVC, Tomcat, JSON conversion ...`
	- Write Unit Tests: `Spring Test, JUnit, Mockito ...`

 - 어떻게 이러한 프레임워크를 그룹화하여 쉽게 빌드할 수 있게 하는가?
 	- **Starter Project가 제공**
 		- `다양한 기능을 위한 편리한 의존성 디스크립터 제공`
- pom.xml
	- `spring-boot-starter-web` : REST API 웹 앱을 빌드할 수 있다.
	- `spring-boot-starter-test` : 단위 테스트를 작성할 수 있다.
```xml
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>
```
- `ctrl+클릭`하면 여러 의존성이 정의된 것을 확인할 수 있다.
```xml
<!--spring-boot-starter-web에서 정의된 의존성 --->
  <dependencies>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter</artifactId>
      <version>3.4.0-M3</version>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-json</artifactId>
      <version>3.4.0-M3</version>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-tomcat</artifactId>
      <version>3.4.0-M3</version>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-web</artifactId>
      <version>6.2.0-RC1</version>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>6.2.0-RC1</version>
      <scope>compile</scope>
    </dependency>
  </dependencies>
```
- 웹 앱플리케이션을 빌드하려고 한다면, Starter를 사용하면 필요한 모든 것이 정의되어 있다.

- 프로젝트에 필요한 기능에 따라 다양한 Starter Project를 사용할 수 있다.
	- 웹앱 or REST API : Spring Boot Starter Web
		- `spring-webmvc, spring-web, spring-boot-starter-tomcat, spring-boot-starter-json`
	- 유닛 테스트: Spring Boot Starter Test
	- 데이터베이스와 통신:
		- JPA 사용: Spring Boot Starter Data JPA
		- JDBC 사용: Spring Boot Starter JDBC
	- 웹앱, REST API 보호: Spring Boot Starter Security

###### 중요한 두가지는
- Spring Boot Starter Project가 편리한 의존성 디스크립터이다.
- Spring Boot가 다양한 Starter Project를 제공한다.

> 클래스 경로에 올바른 의존성이 있으면 다 된 것일까? 
> 	(X) : 의존성을 Config 해야한다.

---
## 추가 학습


---
## 다음 강의 노트 : [[056_Spring Boot의 강력함-Auto Configuration]]
