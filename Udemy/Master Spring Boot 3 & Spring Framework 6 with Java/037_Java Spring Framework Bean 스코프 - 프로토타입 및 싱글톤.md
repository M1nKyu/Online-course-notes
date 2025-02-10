---
created: 2024-08-28 17:29
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
- a0 패키지를 복사하여 *e1* 이름으로 붙여넣는다.
- 패키지 내부의 클래스명을 *BeanScopestLauncherApplication* 로 변경한다.

- 2개의 Bean을 만들고 (*NormalClass*, *PrototypeClass*)
	- PrototypeClass에 **@Scope, @Component** 어노테이션을 추가한다
	- NormalClass에 **@Component** 어노테이션을 추가한다.
```java
@Component // 📌
class NormalClass { }

@Scope // 📌
@Component
class PrototypeClass { }

@Configuration
@ComponentScan
public class BeanScopestLauncherApplication {

    public static void main(String[] args){

        try(var context = new AnnotationConfigApplicationContext (BeanScopestLauncherApplication.class)){
            Arrays.stream(context.getBeanDefinitionNames())
                    .forEach(System.out::println);
        }
    }
}
```

#### PrototypeClass 생성하기
- **@Scope(value= ConfigurableBeanFactory.SCOPE_PROTOTYPE)** Scope 어노테이션의 속성값을 지정한다.

- value= ConfigurableBeanFactory 뒤에 점을 입력하면 두 가지 스코프가 표시된다.
	- *SCOPE_PROTOTYPE* : 프로토타입 스코프
	- *SCOPE_SINGLETON* : 싱글톤 스코프

- *SCOPE_PROTOTYPE*를 정의한 코드를 살펴보면
	- `String SCOPE_PROTOTYPE = "prototype";` 
		- `prototype`이라는 값이 있다.

###### 일반 클래스와 프로토타입 클래스는 어떻게 다른가
- 위 코드에서 getBeanDefinitionNames() 를 사용하는데, 이 코드를 삭제한다.
	- 그리고 컨텍스트를 출력할 것이다.

- `getBean()`을 사용하고, 
	- PrototypeClass.class를 출력하도록 한다. (3번 반복하도록 한다.)
		- `PrototypeClass의 인스턴스를 가져오려는 것이다.`
```java
@Component
class NormalClass {}

@Scope(value=ConfigurableBeanFactory.SCOPE_PROTOTYPE)
@Component
class PrototypeClass {}

@Configuration
@ComponentScan
public class BeanScopestLauncherApplication {

    public static void main(String[] args){

        try(var context = new AnnotationConfigApplicationContext 
        (BeanScopestLauncherApplication.class)){
	        System.out.println(context.getBean(NormalClass.class)); // 📌
			System.out.println(context.getBean(NormalClass.class)); // 📌

            System.out.println(context.getBean(PrototypeClass.class)); // 📌
            System.out.println(context.getBean(PrototypeClass.class)); // 📌
            System.out.println(context.getBean(PrototypeClass.class)); // 📌
        }
    }
}
```

- 실행하면
```
com.in28minutes.learn_spring_framework.examples.e1.NormalClass@16a0ee18
com.in28minutes.learn_spring_framework.examples.e1.NormalClass@16a0ee18
com.in28minutes.learn_spring_framework.examples.e1.PrototypeClass@3d6f0054
com.in28minutes.learn_spring_framework.examples.e1.PrototypeClass@505fc5a4
com.in28minutes.learn_spring_framework.examples.e1.PrototypeClass@5fbdfdcf
```
- NormalClass의 context.getBean 호출은 특정 객체가 출력되는 것을 확인할 수 있다. (@16a0ee18)
	- 두 가지 모두 해시코드가 동일하다.
	- 동일한 Bean이 다시 검색되는 것이다.

- PrototypeClass의 경우 context.getBean을 호출할 때마다 새로운 해시코드 값이 나타난다.
	- 특정한 Bean의 새 인스턴스를 얻게 되는 것이다.
	- 매번 이 Spring Context에서 새로운 Bean을 가져온다.

> [!summary] 요약하면
> - NormalClass의 반환되는 인스턴스는 항상 같다
> - PrototypeClass의 반환되는 인스턴스는 항상 다르다.

- **기본적으로** Spring Framework에서 생성되는 모든 Bean은 **싱글톤**이다.

- Bean을 참조할 때마다 매번 다른 인스턴스를 생성하고 싶은 경우엔
	- ==프로토타입을 사용==하면 된다.

#### 다시 정리하면
- Spring Bean Scopes 에는 중요한 두 가지 스코프 (**싱글톤**, **프로토타입**) 이 있다.

- 싱글톤은 Spring IoC Container 당 객체 인스턴스가 오직 하나이다.
- 프로토타입은 Spring IoC Container 당 객체 인스턴스가 여러개일 수 있다.

###### ※ 그 외의 웹애플리케이션에서만 특정하게 적용되는 스코프
- Web-aware Spring Application에서 사용되는 스코프로
	- **Request** 스코프
		- HTTP 요청당 객체 인스턴스가 하나 생성된다.
	- **Session** 스코프
		- HTTP 세션당 객체 인스턴스가 하나 생성된다.
		- `동일한 사용자에 해당하는 여러 번의 요청이 같은 세션에 속해있을 수 있다.` 
	- **Application** 스코프
		- 웹 애플리케이션 전체에 객체 인스턴스가 하나이다.
	- **Websocket** 스코프
		- 웹소켓 인스턴스당 객체 인스턴스가 하나이다.

###### Java Singleton (GOF) vs Spring Singleton
> Java 싱글톤과 Spring 싱글톤을 비교할 때 꼭 알아야 하는 내용이다.

- 싱글톤은 디자인 패턴이다.
	- GoF(Gang of Four) Design Pattern 이라는 책이 있다.
		- 유명한 4명이 쓴 책이다.
		- 이 책에서 설명하는 Java 싱글톤의 정의는 Spring 싱글톤의 정의와 다르다.

- 어떻게 다른가?
	- Spring 싱글톤은 Spring IoC Container 당 객체 인스턴스가 하나이다.
	- Java 싱글톤은 JVM 하나 당 객체 인스터스가 하나이다.

- JVM에 Spring IoC Container를 하나만 실행한다면
	- Spring 싱글톤과 Java 싱글톤은 같은 의미일 수 있다.

- 하지만, JVM에 Spring IoC Container를 여러개 실행하면
	- Spring 싱글톤은 Java 싱글톤과 다르다.
	- `보통 JVM에 Spring IoC Container 여러개를 실행하진 않는다.`

- ==그러므로 99.99%의 경우 Spring 싱글톤과 Java 싱글톤은 같다.==





---
## 추가 학습


---
## 다음 강의 노트 : [[038_프로토타입과 싱글톤 비교하기]]
