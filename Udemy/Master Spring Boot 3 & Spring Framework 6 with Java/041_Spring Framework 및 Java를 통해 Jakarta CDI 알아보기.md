---
created: 2024-09-03 14:37
course-name: Master Spring Boot 3 & Spring Framework 6 with Java
instructor: Ranga Karanam
section: 섹션 04 - Spring Framework 고급 기능 살펴보기
presentation: 
tags:
  - lecture-note
  - "#udemy"
---
## 강의 노트
#### CDI는
> **Jakarta Contexts and Dependency Injection (CDI)** 에 대해 살펴볼 것이다.

- 이는 Jakarta EE에 속한 규격이고, Spring Framework에서 지원한다.
- CDI 규격은 2009년 12월에 Java EE 6 플랫폼에 도입됐다.

- CDI는 규격이고, *인터페이스* 이다.

- CDI에는 중요한 API Annotations 몇 가지가 정의돼있다.
	- Inject : `Autowired와 유사`
	- Named : `Component와 유사`
	- Qualifier : `Qualifier, Scope, Singleton은 이름이 같은 Spring 어노테이션과 유사`
	- Scope
	- Singleton 
	- 마찬가지로 Spring Framework로 지원된다.

#### CDI 사용 예시
- CDI를 사용하려면 xml에 의존성을 추가해야 한다.
###### 사전작업
- *pom.xml* 로 가서 의존성을 추가할 것이다.
	- spring-boot-starter 아래에 의존성을 추가할 것이다.
```xml
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter</artifactId>
		</dependency>
		
		<!--📌 이 위치에 작성 -->
		
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>
```

- groupId는 jakarta.inject로,
	- artifactId는 jakarta.inject-api라고 하며
		- version은 2.0.1인 의존성을 추가한다.
```xml
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter</artifactId>
		</dependency>

		<dependency> <!-- 📌  --> 
			<groupId>jakarta.inject</groupId>
			<artifactId>jakarta.inject-api</artifactId>
			<version>2.0.1</version>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>
```

- a0 패키지를 복사하여 *g1* 으로 저장한다.
- 클래스명을 *CdiContextLauncherApplication* 으로 변경한다.
- BusinessService 클래스를 생성한다.
	- private DataService dataService를 추가하고
		- 게터, 세터를 추가한다.
- DataService 클래스도 만들어준다.
```java
class BusinessService { // 📌
    private DataService dataService;

    public DataService getDataService() {
        return dataService;
    }

    public void setDataService(DataService dataService) {
        this.dataService = dataService;
    }
}

class DataService { // 📌

}

@Configuration
@ComponentScan
public class CdiContextLauncherApplication { // 📌

    public static void main(String[] args){

        try(var context = new AnnotationConfigApplicationContext 
        (CdiContextLauncherApplication.class)){
            Arrays.stream(context.getBeanDefinitionNames())
                    .forEach(System.out::println);
        }
    }
}
```
- 이제 세터 주입을 사용할 것이다.
	- 게터에 **@Autowired**를 추가한다.
- BusinessService와 DataService 클래스에 @Component도 추가한다.
- 게터에 `"Setter Injection"`을 출력하는 sysout을 추가한다.
```java
@Component // 📌
class BusinessService {
    private DataService dataService;

    @Autowired // 📌
    public DataService getDataService() {
        System.out.println("Setter Injection"); // 📌
        return dataService;
    }

    public void setDataService(DataService dataService) {
        this.dataService = dataService;
    }
}

@Component // 📌
class DataService {

}

@Configuration
@ComponentScan
public class CdiContextLauncherApplication {

    public static void main(String[] args){

        try(var context = new AnnotationConfigApplicationContext 
        (CdiContextLauncherApplication.class)){
            Arrays.stream(context.getBeanDefinitionNames())
                    .forEach(System.out::println);
        }
    }
}
```
- 실행하면
```
15:49:41.849 [main] INFO org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor -- Autowired annotation should only be used on methods with parameters: public com.in28minutes.learn_spring_framework.examples.g1.DataService com.in28minutes.learn_spring_framework.examples.g1.BusinessService.getDataService()
Setter Injection // 📌
org.springframework.context.annotation.internalConfigurationAnnotationProcessor
org.springframework.context.annotation.internalAutowiredAnnotationProcessor
org.springframework.context.annotation.internalCommonAnnotationProcessor
org.springframework.context.event.internalEventListenerProcessor
org.springframework.context.event.internalEventListenerFactory
cdiContextLauncherApplication
businessService // 📌
dataService // 📌
```
- businessService와 dataService가 출력되고 세터 주입을 이용하여 주입이 완료됐다.


- main 내부에 sysout 코드를 하나 더 추가한다.
```java
@Configuration
@ComponentScan
public class CdiContextLauncherApplication {

    public static void main(String[] args){

        try(var context = new AnnotationConfigApplicationContext (CdiContextLauncherApplication.class)){
            Arrays.stream(context.getBeanDefinitionNames())
                    .forEach(System.out::println);

            System.out.println(context.getBean(BusinessService.class) // 📌
                    .getDataService()); // 📌
        }
    }
}
```
- 실행하면
```
15:52:22.479 [main] INFO org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor -- Autowired annotation should only be used on methods with parameters: public com.in28minutes.learn_spring_framework.examples.g1.DataService com.in28minutes.learn_spring_framework.examples.g1.BusinessService.getDataService()
Setter Injection
org.springframework.context.annotation.internalConfigurationAnnotationProcessor
org.springframework.context.annotation.internalAutowiredAnnotationProcessor
org.springframework.context.annotation.internalCommonAnnotationProcessor
org.springframework.context.event.internalEventListenerProcessor
org.springframework.context.event.internalEventListenerFactory
cdiContextLauncherApplication
businessService
dataService
Setter Injection
null // 📌
```
- null이 출력된다.

- 왜 null이 출력되는가?
	- **Autowired를 잘못된 메소드에 넣었기 때문이다.**
		- 게터가 아니고 세터에 코드가 있어야한다.
```java

@Component
class BusinessService {
    private DataService dataService;

    public DataService getDataService() { // 📌
        return dataService;
    }

    @Autowired // 📌
    public void setDataService(DataService dataService) { 
        System.out.println("Setter Injection"); // 📌
        this.dataService = dataService;
    }
}
```
- 이제 세터 주입이 수행되어, 데이터 서비스에 주입할 수 있게 됐다.
###### CDI는 
- CDI는 대체 방식인데, 
	- *@Component* 대신 **@Named**를 사용한다.
	- *@Autowired* 대신 **@Inject**를 사용한다.
```java
@Named // 📌
class BusinessService {
    private DataService dataService;

    public DataService getDataService() {
        return dataService;
    }

    @Inject // 📌
    public void setDataService(DataService dataService) {
        System.out.println("Setter Injection");
        this.dataService = dataService;
    }
}

@Named // 📌
class DataService {

}

...

```
- 실행하면
```
Setter Injection // 📌 세터주입
org.springframework.context.annotation.internalConfigurationAnnotationProcessor
org.springframework.context.annotation.internalAutowiredAnnotationProcessor
org.springframework.context.annotation.internalCommonAnnotationProcessor
org.springframework.context.event.internalEventListenerProcessor
org.springframework.context.event.internalEventListenerFactory
cdiContextLauncherApplication
businessService
dataService
com.in28minutes.learn_spring_framework.examples.g1.DataService@4dbb42b7
```
- dataService가 자동 연결되고
	- 세터 주입도 수행되는 것을 확인할 수 있다.

###### 이번 강의에서
> CDI 어노테이션이 Spring 어노테이션을 대체하는 걸 살펴봤다.
	- 꼭 쓸 필요는 없지만 알아두면 좋다.

- Spring Framework에서는 CDI가 구현되므로
	- CDI 어노테이션도 지원된다.


---
## 추가 학습


---
## 다음 강의 노트 : [[043_Java Spring XML 설정 알아보기]]
