---
created: 2024-09-03 13:01
course-name: Master Spring Boot 3 & Spring Framework 6 with Java
instructor: Ranga Karanam
section: 섹션 04 - Spring Framework 고급 기능 살펴보기
presentation: 
tags:
  - lecture-note
  - "#udemy"
---
## 강의 노트
#### 사전 작업
- 패키지 a0 을 복사하여 f1으로 저장한다.
- 클래스명을 PrePostAnnotationsContextLauncherApplication 으로 변경한다.

#### .
- 간단한 클래스를 생성할 것이다.
```java
// 📌
@Component
class SomeClass{

}

@Configuration
@ComponentScan
public class PrePostAnnotationsContextLauncherApplication {

    public static void main(String[] args){

        try(var context = new AnnotationConfigApplicationContext (PrePostAnnotationsContextLauncherApplication.class)){
            Arrays.stream(context.getBeanDefinitionNames())
                    .forEach(System.out::println);
        }
    }
}
```

> 위 특정 클래스의 인스턴스에 모든 의존성을 연결하는대로 초기화를 수행하려 한다고 해볼 것이다.

- SomeDependency 클래스도 생성하고, 이 클래스는 SomeClass에서 사용할 것이다.
```java
// 📌
@Component
class SomeClass{
    private SomeDependency someDependency; 

    public SomeClass(SomeDependency someDependency){ // 생성자
        super();
        this.someDependency = someDependency;
    }
}
...
```
- 실행하면
```
org.springframework.context.annotation.internalConfigurationAnnotationProcessor
org.springframework.context.annotation.internalAutowiredAnnotationProcessor
org.springframework.context.annotation.internalCommonAnnotationProcessor
org.springframework.context.event.internalEventListenerProcessor
org.springframework.context.event.internalEventListenerFactory
prePostAnnotationsContextLauncherApplication
someClass // 📌
someDependency  // 📌
```


- 예를들어 위의 생성자가 실행된 후 의존성이 준비됐을 때, 
	- sysout을 수행하여 `All dependencies are ready!` 라고 출력하도록 수정하면
```java
@Component
class SomeClass{
    private SomeDependency someDependency;

    public SomeClass(SomeDependency someDependency){
        super();
        this.someDependency = someDependency;
        System.out.println("All dependencies are ready!"); // 📌
    }
}
```
- 실행하면
```
All dependencies are ready! // 📌
org.springframework.context.annotation.internalConfigurationAnnotationProcessor
org.springframework.context.annotation.internalAutowiredAnnotationProcessor
org.springframework.context.annotation.internalCommonAnnotationProcessor
org.springframework.context.event.internalEventListenerProcessor
org.springframework.context.event.internalEventListenerFactory
prePostAnnotationsContextLauncherApplication
someClass
someDependency
```
- 문구 출력 후 Bean이 모두 나열된다.

###### 이 특정 의존성에서 Bean이 준비되는 대로, 의존성을 연결하는 대로 초기화 하려면
- SomeClass 내부에 initialize() 메서드를 생성한다.
- 초기화 과정의 일부로 의존성에서 특정 메서드를 호출하도록 할 것이다.

```java
@Component
class SomeClass{
    private SomeDependency someDependency;

    public SomeClass(SomeDependency someDependency){
        super();
        this.someDependency = someDependency;
        System.out.println("All dependencies are ready!");
    }
	
    public void initialize(){ // 📌
        someDependency.getReady();
    }
}

@Component
class SomeDependency{
    public void getReady(){ // 📌
        System.out.println("Some logic using SomeDependency");
    }
}

@Configuration
@ComponentScan
public class PrePostAnnotationsContextLauncherApplication {

    public static void main(String[] args){

        try(var context = new AnnotationConfigApplicationContext (PrePostAnnotationsContextLauncherApplication.class)){
            Arrays.stream(context.getBeanDefinitionNames())
                    .forEach(System.out::println);
        }
    }
}
```
- someDepency를 데이터베이스로 사용하고 싶으며
	- 데이터베이스에서 데이터를 가져와 초기화하려고 한다.

- 의존성을 연견하는대로 초기화를 실행하려면?
	- **@PostConstruct**를 사용한다.

```java
@Component
class SomeClass{
    private SomeDependency someDependency;

    public SomeClass(SomeDependency someDependency){
        super();
        this.someDependency = someDependency;
        System.out.println("All dependencies are ready!");
    }

    @PostConstruct // 📌
    public void initialize(){
        someDependency.getReady();
    }
}
```
- 실행하면
```
All dependencies are ready! // 📌
Some logic using SomeDependency // 📌
org.springframework.context.annotation.internalConfigurationAnnotationProcessor
org.springframework.context.annotation.internalAutowiredAnnotationProcessor
org.springframework.context.annotation.internalCommonAnnotationProcessor
org.springframework.context.event.internalEventListenerProcessor
org.springframework.context.event.internalEventListenerFactory
prePostAnnotationsContextLauncherApplication
someClass
someDependency
```
- 가장 먼저 `All ...`이 출력되고, `Some ...`이 출력된다.
	- 의존성이 준비되고, 의존성이 준비된 후에는 초기화된다.
	- 이때 Spring은 ==자동으로 의존성을 연결==하고, 
		- 의존성을 자동 ==연결하는대로 PostConstruct를 어노테이션한 메서드를 호출==한다.

#### PreDestroy
> 애플리케이션 종료 전에, Context에서 Bean이 삭제되기 전에 뭔가 해야 할 수도 있을 것이다.
> 	- **@PreDestroy**로 할 수 있다.

- 보통 PreDestroy에서는 무엇을 하는가?
	- cleanup
	- DB 등에 연결돼있다면 cleanup으로 종료할 수 있다.
```java
@Component
class SomeClass{
    private SomeDependency someDependency;

    public SomeClass(SomeDependency someDependency){
        super();
        this.someDependency = someDependency;
        System.out.println("All dependencies are ready!");
    }

    @PostConstruct
    public void initialize(){
        someDependency.getReady();
    }

    @PreDestroy // 📌
    public void cleanup(){
        System.out.println("Cleanup");
    }
}
```
- 실행하면
```
All dependencies are ready!
Some logic using SomeDependency
org.springframework.context.annotation.internalConfigurationAnnotationProcessor
org.springframework.context.annotation.internalAutowiredAnnotationProcessor
org.springframework.context.annotation.internalCommonAnnotationProcessor
org.springframework.context.event.internalEventListenerProcessor
org.springframework.context.event.internalEventListenerFactory
prePostAnnotationsContextLauncherApplication
someClass
someDependency
Cleanup // 📌
```
- Context에서 Bean이 삭제되기 바로 전에 Cleanup이 나타난다.


#### 정리하면
- ==의존성을 연결하는 대로 초기화 논리를 실행==하려는 경우,
	- 예를 들어 DB 등에서 데이터를 가져오려는 경우에 **@PostConstruct**를 사용할 수 있다.

- 컨테이너에서 ==Bean이 삭제되기 전에==,
	- Application Context에서 삭제되기 전에
		- ==cleanup을 수행하려는 경우==
			- **@PreDestroy**를 사용할 수 있다.


---
## 추가 학습


---
## 다음 강의 노트 : [[040_Jakarta EE의 발전-J2EE 및 Java EE와 비교]]
