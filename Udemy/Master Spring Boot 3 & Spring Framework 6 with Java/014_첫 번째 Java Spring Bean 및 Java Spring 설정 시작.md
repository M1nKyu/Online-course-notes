---
created: 2024-07-15 00:03
course-name: Master Spring Boot 3 & Spring Framework 6 with Java
instructor: Ranga Karanam
section: 섹션 02 -Java Spring Framework 시작하기
presentation: 
tags:
  - lecture-note
  - "#udemy"
---
## 상세 노트
- JVM 내부에 Spring 컨텍스트를 생성하고 
	- name이 생성된 Bean 만들 것임 (name을 관리하는)

- 사전 작업
	- `AppGamingBasicJava` 이름 변경 -> `App01GamingBasicJava`
	- Spring 프로젝트 생성 시 생성된 애플리케이션 클래스 (`LearnSpringFrameworkApplication`) 삭제
	- 새 파일 생성:`App02HelloWorldSpring.java`  

- 할 작업
	1. Launch a Spring Context  
	2. Configure the things that we want Spring to manage - `@Configuration`
```java
public class App01HelloWorldSpring {
    public static void main(String[] args){
        // 1: Launch a Spring Context

        // 2: Configure the things that we want Spring to manage - @Configuration
    }
}
```

#### 설정 클래스(`Configuration Class`) 
- ==Spring이 관리해야 하는 것==을 설정하는 데 사용할 수 있는 접근 방식 중 하나
- `@Configuration` 에서 설정 클래스를 만들고 `name` 등 모든 것을 정의할 수 있음 
- Spring 컨텍스트를 시작할 수도 있음 (Launch a Spring Context)

###### Spring context란?
- 내용추가필요 

#### Configuration Class 생성하기
- `HelloWorldConfiguration`클래스 생성 (`com.in28minutes.learn_spring_framework.HelloWorldConfiguration`)
- **@Configuration 을 추가하여 설정 클래스임을 나타냄**
- **==설정 클래스 안에서 메서드를 정의==하여 Spring Bean을 정의할 수 있음**
```java
// HelloWorldConfiguration
import org.springframework.context.annotation.Configuration;  
  
@Configuration  
public class HelloWorldConfiguration {} // 여기 안에서 Spring Bean을 정의할 수 있음 
```

#### 1. 컨텍스트 준비하기 
- ==@Configuration 클래스 파일을 사용하여 Spring 컨텍스트를 시작하도록 함== 
	- `AnnotationConfigApplicationContext` 클래스 사용
	- `HelloWorldConfiguration.class` 설정파일을 실행하도록 함
```java
// App01HelloWorldSpring.java

public class App01HelloWorldSpring {
    public static void main(String[] args){
        // 1: Launch a Spring Context
        var context = new AnnotationConfigApplicationContext(HelloWorldConfiguration.class);
    }
}
```

- 현재까지 상태
	- JVM이 있고, Spring 컨텍스트를 실행했으며
		- Spring에 Bean을 관리하라는 명령을 내리려 함 (`name 객체를 관리하는`)
--- 
## 다음 강의 노트: [[015_Spring Java 설정 파일에서 더 많은 Java Spring Bean 만들기]]
