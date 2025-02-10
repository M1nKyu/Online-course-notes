---
created: 2024-08-27 16:57
course-name: Master Spring Boot 3 & Spring Framework 6 with Java
instructor: Ranga Karanam
section: 섹션 04 - Spring Framework 고급 기능 살펴보기
presentation: 
tags:
  - lecture-note
  - "#udemy"
---
## 강의노트
#### 사전작업
- a0 패키지 복사하여 d1 이름으로 저장한다.
- 클래스명은 LizyInitializationLauncherApplication으로 변경한다.

- ClassA과 ClassB 클래스를 생성한다.
```java
@Component // 📌
class ClassA {

}

@Component // 📌
class ClassB {

}

@Configuration
@ComponentScan
public class LizyInitializationLauncherApplication {
    public static void main(String[] args){
		...
    }
}
```

#### 일반적인 경우
> ClassB에 초기화 로직이 있도록 만들 것이다.
- ClassB에 생성자를 생성한다.
```java
@Component
class ClassB {
    private ClassA classA;

    public ClassB(ClassA classA){ // 📌 생성자
	    // 📌 이 곳에 복잡한 로직 등을 둘 수 있다.
        this.classA = classA;
    }
}
```
- Bean을 생성하고 있는 ClassA가 있고
	- ClassA Bean을 사용하여 초기화하는 ClassB가 있다.

- 생성자에 `System.out.println("Some Initialization logic");` 코드도 추가하여 애플리케이션을 실행하면
```
Some Initialization logic
org.springframework.context.annotation.internalConfigurationAnnotationProcessor
org.springframework.context.annotation.internalAutowiredAnnotationProcessor
org.springframework.context.annotation.internalCommonAnnotationProcessor
org.springframework.context.event.internalEventListenerProcessor
org.springframework.context.event.internalEventListenerFactory
lizyInitializationLauncherApplication
classA
classB
```
- 여기에서는 ClassB Bean을 사용하고 있지 않는다.
	- ==Spring Context를 실행하면 초기화가 자동==으로 일어난다.

- Some Initialization logic이 자동으로 호출되는 것을 확인할 수 있다.
	- Bean을 로드하지 않고 ==Bean 메서드를 호출하지 않더라도 자동으로 Bean이 초기화==된다.

- **어떻게 하면 그렇게 되지 않을까?**
	- **@Lazy** 어노테이션을 사용한다.
#### @Lazy
- ClassB에 *@Lazy*를 추가한다.
- 결과를 직관적으로 확인하기 위해 `Arrays.stream(context.getBeanDefinitionNames())........` 코드를 생략하여 실행하면
	- 아무것도 출력되지 않는다.

- 초기화 논리가 이제 실행되지 않는 것을 확인할 수 있고,
	- Bean 초기화가 일어나지 않는다.
	- **ClassB Bean은 시작할 때 초기화되지 않는다.**

###### 그럼 ClassB Bean은 언제 초기화되는가?
- ClassB Bean을 ==사용할 때 초기화==된다.

- 문구를 출력하는 코드를 추가해본다.
```java
@Configuration
@ComponentScan
public class LizyInitializationLauncherApplication {

    public static void main(String[] args){

        try(var context = new AnnotationConfigApplicationContext (LizyInitializationLauncherApplication.class)){
//            Arrays.stream(context.getBeanDefinitionNames())
//                    .forEach(System.out::println);
            System.out.println("Initialization of context is completed"); // 📌
        }
    }
}
```
- 다음으로 Context 초기화가 완료된 후에 누군가가 이 Bean을 사용하도록 가정한다.
```java
@Component
class ClassA { }

@Component
@Lazy
class ClassB {
    private ClassA classA;

    public ClassB(ClassA classA){
        this.classA = classA;
        System.out.println("Some Initialization logic");
    }

    public void doSomething(){ // 📌
        System.out.println("Do Something");
    }

}

@Configuration
@ComponentScan
public class LizyInitializationLauncherApplication {

    public static void main(String[] args){

        try(var context = new AnnotationConfigApplicationContext (LizyInitializationLauncherApplication.class)){
	        Arrays.stream(context.getBeanDefinitionNames()).forEach(System.out::println);
	        
            System.out.println("Initialization of context is completed"); // 📌
            context.getBean(ClassB.class).doSomething(); // 📌
        }
    }
}
```

- 실행하면 잘 동작한다.
```
Initialization of context is completed
Some Initialization logic
Do Something
```
- Initialization of the context is completed 이후에 Bean이 로드된다.
- **사용되기 전에(사용하려고 할때)** 로드되는 것이다.
- ClassB를 참조하거나 사용할 때만 이 Bean이 로드된다.

- 이것이 @Lazy의 기능이다.

- ==Component를 사용하는 모든 클래스나 Bean에 어노테이션을 적용한 모든 메서드에서 Lazy를 사용할 수 있다.==

#### Lazy Initialization의 자세한 정보
- Spring Bean의 Default Initialization은 즉시(Eager) 초기화이다.
	- *@Lazy*를 지정하지 않는 한, 각 Spring Bean은 시작 즉시 초기화된다.

- 추천
	- 항상 *즉시 초기화*를 추천한다.
		- Spring 구성에오류가 있을 경우, 즉시 초기화를 사용하면 
			- 애플리케이션이 시작할 때 오류를 확인할 수 있다

- 그렇지만, @Lazy 를 사용하여 Bean이 지연 초기화되도록 구성할 수는 있다.
	- 권장되지는 않으며, 자주 사용되지도 않는다.

- @Lazy 어노테이션은
	- @Component와 @Bean이 사용되는 거의 모든 곳에서 사용할 수 있다.
	- Lazy-resolution proxy(Lazy 해결 프록시)가 실제 의존성 대신 주입된다.
	- @Configuration 클래스에서도 사용할 수 있다.
		- `클래스 내의 모든 @Bean 메서드가 지연 초기화 된다.`


> [!important] Lazy는
> 복잡한 초기화 논리가 많고 시작 시 지연시키고 싶지 않은 상황이면 지연 초기화를 고려해 볼 수 있다.
> 하지만, 지연초기화를 사용하는 경우 **애플리케이션이 시작될 때 Spring 구성 오류가 발견되지 않는다.**





---
## 추가 학습


---
## 다음 강의 노트 : [[036_지연 초기화와 즉시초기화의 비교]]
