---
created: 2024-08-19 16:20
course-name: Master Spring Boot 3 & Spring Framework 6 with Java
instructor: Ranga Karanam
section: 섹션 03 - Spring Framework를 사용하여 Java 객체를 생성하고 관리하기
presentation: 
tags:
  - lecture-note
  - "#udemy"
---
## 강의 노트
#### 이번 강의는
- Spring에 있는 **여러 유형의 의존성 주입**에 대해 알아볼 것이다.

> [!summary] 의존성 주입에는 3가지 유형이 있다.
> 1. 생성자 기반 (Constructor-based)
> 2. 수정자 기반 (Setter-based)
> 3. 필드 기반 (Field)

- 앞에서 주입을 하는데 사용한 방법이 **생성자 기반**의 의존성 주입이다.
- ==Setter 메소드==를 사용하여 주입을 하는 경우가 **수정자 기반**의 주입이다.
- ==Setter나 생성자가 없다면== 필드 주입을 사용할 수 있다. **필드 기반**은 ==리플렉션을 이용==하여 의존성을 주입한다.

#### 사전작업
- GamingAppLauncherApplication을 game 패키지를 옮긴다.
- GamingAppLauncherApplication를 복사하여 이름을 `DepInjectionLauncherApplication`으로 하여 복사한다.
- 위치는 `package com.in28minutes.learn_spring_framework.examples.a1;` 로 한다.
- main의 기존 코드를 삭제한다.
- 현재 패키지에서 ComponentScan을 수행하려면 @ComponentScan을 변경해야 한다.
	- @ComponentScan을 사용하는 경우 자동으로 컴포넌트 스캔을 수행한다는 점이 장점이다.
	- 현재 패키지에서 컴포넌트 스캔을 자동으로 수행하기 때문에 패키지를 명시적으로 지정할 필요가 없다.
```java
package com.in28minutes.learn_spring_framework.examples.a1; // 📌 패키지 위치 

import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Configuration
@ComponentScan // ("com.in28minutes.learn_spring_framework.examples.a1") //  📌 ComponentScan 패키지를 명시적으로 지정하지 않아도 된다. 
public class DepInjectionLauncherApplication {

    public static void main(String[] args){

        try(var context = new AnnotationConfigApplicationContext (DepInjectionLauncherApplication.class)){

        }
    }
}
```
- 이렇게 Spring Context를 정리하여 시작할 준비를 마쳤다.
	- 실행하여 Spring Context를 시작해본다.

###### 어떤 Bean이 사용됐는지 확인하려면
```java
@Configuration
@ComponentScan
public class DepInjectionLauncherApplication {
    public static void main(String[] args){
        try(var context = new AnnotationConfigApplicationContext (DepInjectionLauncherApplication.class)){
            
            Arrays.stream(context.getBeanDefinitionNames()) // 📌
                    .forEach(System.out::println); // 📌 
                     
        }
    }
}
``` 

- 실행하면 다음이 출력된다.
```
org.springframework.context.annotation.internalConfigurationAnnotationProcessor
org.springframework.context.annotation.internalAutowiredAnnotationProcessor
org.springframework.context.annotation.internalCommonAnnotationProcessor
org.springframework.context.event.internalEventListenerProcessor
org.springframework.context.event.internalEventListenerFactory
depInjectionLauncherApplication
```

###### 빈 런처 생성해놓기
> 이제 텅 빈 Spring Context 런처가 준비됐다.
> 복사하여 빈 런처로 사용할 수 있다.
- depInjectionLauncherApplication의 사본을 생성하여 패키지를 `com.in28minutes.learn_spring_framework.examples.a0;` 으로 지정한다.
	- 이름은 *SimpleSpringContextLauncherApplication*로 지정한다.

#### 간단한 예시
- YourBusinessClass 라는 클래스가 주어질 것이다.
	- 이 클래스에는 2개의 의존성이 있다. (*Dependency1, Dependency2*)

> 생성자 주입, 수정자 주입, 필드 주입을 사용하여 **Dependency1과 Dependency2를 YourBusinessClass에 주입**하는 방법을 알아본다.
> 이번 강의에서는 클래스를 DepInjectionLauncherApplication에 직접 생성할 것이다.
> 	YourBusiniessClass와 Dependency1, Dependency2가 *모두 자체 파일 안에 위치*시킨다.
> `(보통 별도의 파일을 만들어 클래스를 생성)`

```java
class YourBusinessClass {

}

class Dependency1 {

}

class Dependency2 {

}

@Configuration
@ComponentScan
public class DepInjectionLauncherApplication {
	// ... (생략)
}
```
- 이렇게 모든 클래스가 주어졌으므로, 
	- 이에 대한 Bean을 생성할 차례이다.
		- ==Bean은 어떻게 생성 ?== -> **@Component**

- 각 클래스에 관한 Bean을 생성하기 위해 @Component를 적용한다.
```java
@Component // 📌
class YourBusinessClass {

}

@Component // 📌
class Dependency1 {

}

@Component // 📌
class Dependency2 {

}

@Configuration
@ComponentScan
public class DepInjectionLauncherApplication {

    public static void main(String[] args){

        try(var context = new AnnotationConfigApplicationContext (DepInjectionLauncherApplication.class)){
            Arrays.stream(context.getBeanDefinitionNames())
                    .forEach(System.out::println);
        }
    }
}
```

- 코드를 실행하면 
```
org.springframework.context.annotation.internalConfigurationAnnotationProcessor
org.springframework.context.annotation.internalAutowiredAnnotationProcessor
org.springframework.context.annotation.internalCommonAnnotationProcessor
org.springframework.context.event.internalEventListenerProcessor
org.springframework.context.event.internalEventListenerFactory
depInjectionLauncherApplication
dependency1
dependency2
yourBusinessClass
```

- Bean이 이 특정 클래스 집합에 대해 생성되었다.
	- 그 이유가 무엇일까?
		- Spring Context를 실행하고 있기 때문이다.

###### Spring Context를 시작하기 위해 무엇을 사용하고 있는가?
- 같은 클래스를 사용하고 있다.
- 이것이 Configuration 클래스인데, 우리는 @ComponentScan도 갖고 있다.
	- ComponentScan은 자동으로 현재 패키지를 스캔한다.

###### 사용자 정의 패키지를 여기에 지정하면 어떨까?
- `@ComponentScan("지정할 패키지")`
	- 지정한 특정 패키지가 스캔될 것이다.
		- 제거하면 스캔되는 대상은 바로 현재 이 패키지가 되는 것이다.

- 위 코드처럼 
	- 생성한 모든 클래스가 현재 패키지에 생성되고
	- @Component를 적용하자마자 Spring은 자동으로 Bean을 생성한다.

> ==이제 해야할 일은 의존성 주입==이다.
> 	이 의존성을 YourBusinessClass에 주입해야 한다.

###### YourBusinessClass에 의존성 주입하기
- 어떻게?
	- **Dependency1 dependency2;** 코드를 추가한다.
	- **Dependency2 dependency2;** 코드도 추가한다.
	- **public String toString(){}** 을 추가하여 의존성 주입이 잘 작동하는지 확인하도록 한다. 
```java
@Component
class YourBusinessClass {

    Dependency1 dependency1; // 📌
    Dependency2 dependency2; // 📌
	 
	 // 📌
    public String toString(){
        return "Using " + dependency1 + " and " + dependency2;
    }
}
```

- 실행하면 
```
org.springframework.context.annotation.internalConfigurationAnnotationProcessor
org.springframework.context.annotation.internalAutowiredAnnotationProcessor
org.springframework.context.annotation.internalCommonAnnotationProcessor
org.springframework.context.event.internalEventListenerProcessor
org.springframework.context.event.internalEventListenerFactory
depInjectionLauncherApplication
dependency1
dependency2
yourBusinessClass
```
- 이전과 그대로이다.
	- ==아직 Bean을 얻지 못했기 때문이다.==
		- `context.getBean(타입)` 을 사용해야 한다.

```java

@Configuration
@ComponentScan
public class DepInjectionLauncherApplication {

    public static void main(String[] args){

        try(var context = new AnnotationConfigApplicationContext (DepInjectionLauncherApplication.class)){
            Arrays.stream(context.getBeanDefinitionNames())
                    .forEach(System.out::println);

            System.out.println(context.getBean(YourBusinessClass.class)); // 📌 이 코드를 추가하여 Bean을 얻는다.
        }
    }
}
```
- 다시 실행하면 다음 한줄이 추가된다.
```
Using null and null
```

- ==왜 null이 출력되는가?==
	- Spring Framework가 이 **의존성들을 Auto-wiring하지 않은 것**이다.
- Auto-wiring을 하려면 다음과 같이 Spring Framework에 ==요청==해야 한다.
```java
@Component
class YourBusinessClass {
    @Autowired // 📌 Field에서 Auto-wiring을 하자마자 Spring이 자동으로 필드 주입을 한다.
    Dependency1 dependency1;

    @Autowired // 📌
    Dependency2 dependency2;

    public String toString(){
        return "Using " + dependency1 + " and " + dependency2;
    }
}
```
- 실행하면 다음이 추가된다.
```
Using com.in28minutes.learn_spring_framework.examples.a1.Dependency1@1a18644 
	and
		com.in28minutes.learn_spring_framework.examples.a1.Dependency2@5acf93bb
```
- ==a1.Dependency1과 a2.Dependency2가 있는데, 이것들이 Auto-wiring에 사용된 것이다.==

> 지금 여기서 사용하고 있는 것은 ==Filed 주입==이다.
> 	따라서 **생성자나 수정자가 없다**. (No Setter or Constructor)
> 	의존성은 **리플렉션을 사용하여 주입**된 것이다.


#### 나머지 2개의 주입 유형을 알아보면
###### setter 주입
- 두 의존성 (Dependency1, Dependency2) 에 대해서 Setter를 생성할 것이다.
> 우클릭 -> Generate -> Setter 생성

```java
@Component
class YourBusinessClass {

    @Autowired
    Dependency1 dependency1;

    @Autowired
    Dependency2 dependency2;

    // 📌 Setter
    public void setDependency1(Dependency1 dependency1) {
        this.dependency1 = dependency1;
    }
    
    public void setDependency2(Dependency2 dependency2) {
        this.dependency2 = dependency2;
    }

    public String toString(){
        return "Using " + dependency1 + " and " + dependency2;
    }
}
```
- 2개의 메소드 집합이 생성됐다. 

- 다음으로, **@Autowired를 Setter로 변경**한다.
```java
@Component
class YourBusinessClass {

    // Dependencies
    Dependency1 dependency1;
    Dependency2 dependency2;

    @Autowired // 📌
    public void setDependency1(Dependency1 dependency1) {
        this.dependency1 = dependency1;
    }

    @Autowired // 📌
    public void setDependency2(Dependency2 dependency2) {
        this.dependency2 = dependency2;
    }

    public String toString(){
        return "Using " + dependency1 + " and " + dependency2;
    }
}
```

- 현재 @Autowired는 필드가 아닌 Setter에 있다.
	- 어떤 결과가 나타날까?
```
org.springframework.context.annotation.internalConfigurationAnnotationProcessor
org.springframework.context.annotation.internalAutowiredAnnotationProcessor
org.springframework.context.annotation.internalCommonAnnotationProcessor
org.springframework.context.event.internalEventListenerProcessor
org.springframework.context.event.internalEventListenerFactory
depInjectionLauncherApplication
dependency1
dependency2
yourBusinessClass
Using com.in28minutes.learn_spring_framework.examples.a1.Dependency1@2c1b194a and com.in28minutes.learn_spring_framework.examples.a1.Dependency2@4dbb42b7
```
- 의존성이 Dependency1과 Dependency2를 사용하여 설정됐지만,
	- Setter 주입이 됐는지는 확인하지 못한다.

- 확인을 위해 ==각 setter에 sysout을 추가==한다.
```java
@Autowired
public void setDependency1(Dependency1 dependency1) {
	System.out.println("Setter Injection - setDependency1 ");
	this.dependency1 = dependency1;
}

@Autowired
public void setDependency2(Dependency2 dependency2) {
	System.out.println("Setter Injection - setDependency2 ");
	this.dependency2 = dependency2;
}
```

- 실행하면
	- setter 주입을 사용하고 있다는 것을 확인할 수 있다.
```
Setter Injection - setDependency1 // 📌
Setter Injection - setDependency2  // 📌
org.springframework.context.annotation.internalConfigurationAnnotationProcessor
org.springframework.context.annotation.internalAutowiredAnnotationProcessor
org.springframework.context.annotation.internalCommonAnnotationProcessor
org.springframework.context.event.internalEventListenerProcessor
org.springframework.context.event.internalEventListenerFactory
depInjectionLauncherApplication
dependency1
dependency2
yourBusinessClass
Using com.in28minutes.learn_spring_framework.examples.a1.Dependency1@2c1b194a and com.in28minutes.learn_spring_framework.examples.a1.Dependency2@4dbb42b7
```

###### 생성자 주입
> 앞에서 만든 Setter를 주석처리한다.
> 우클릭 -> Generate -> 두 의존성을 사용하여 생성자 생성 

- 생성한 생성자에 **@Autowired**를 추가하여 ==의존성이 Auto-wiring되도록== 한다.
- ==확인하기 위한 sysout도 추가==한다.
```java
@Component
class YourBusinessClass {

    Dependency1 dependency1;
    Dependency2 dependency2;

	// 📌 Constructor
	@Autowired // 📌
    public YourBusinessClass(Dependency1 dependency1, Dependency2 dependency2) {
	    System.out.println("Constructor Injection - YourBusinessClass"); // 📌 확인을 위한 출력
	    
        this.dependency1 = dependency1;
        this.dependency2 = dependency2;
    }

    public String toString(){
        return "Using " + dependency1 + " and " + dependency2;
    }
}
```

- 실행하면
```
Constructor Injection - YourBusinessClass // 📌
org.springframework.context.annotation.internalConfigurationAnnotationProcessor
org.springframework.context.annotation.internalAutowiredAnnotationProcessor
org.springframework.context.annotation.internalCommonAnnotationProcessor
org.springframework.context.event.internalEventListenerProcessor
org.springframework.context.event.internalEventListenerFactory
depInjectionLauncherApplication
dependency1
dependency2
yourBusinessClass
Using com.in28minutes.learn_spring_framework.examples.a1.Dependency1@f5acb9d and com.in28minutes.learn_spring_framework.examples.a1.Dependency2@4fb3ee4e
```

- ==생성자 주입의 장점은 Auto-wiring이 의무가 아니라는 것이다.==
	- @Autowired를 삭제하고 실행하면
```
Constructor Injection - YourBusinessClass
org.springframework.context.annotation.internalConfigurationAnnotationProcessor
org.springframework.context.annotation.internalAutowiredAnnotationProcessor
org.springframework.context.annotation.internalCommonAnnotationProcessor
org.springframework.context.event.internalEventListenerProcessor
org.springframework.context.event.internalEventListenerFactory
depInjectionLauncherApplication
dependency1
dependency2
yourBusinessClass
Using com.in28minutes.learn_spring_framework.examples.a1.Dependency1@6cd28fa7 and com.in28minutes.learn_spring_framework.examples.a1.Dependency2@614ca7df
```
- 그대로 의존성이 적절하게 Auto-wiring된 것을 확인할 수 있다.

- @Autowired를 추가하지 않아도 여기 있는 ==모든 의존성으로 생성자를 생성==한다면
	- Spring이 ==자동으로 생성자를 사용하여 객체를 만들 것==이다.

#### 정리하면
- 의존성 주입은 다양한 방법으로 가능하다.
	- 필드주입, 수정자 주입, 생성자 주입 

> [!hint] 추천 유형 (어떤 유형이 적절할까?)
> - Spring 팀은 **생성자 기반 주입**을 추천한다.
> 	- 모든 초기화가 하나의 메소드에서 발생하기 때문이다.
- 또한 Spring 팀은 생성자 주입을 쉽게 사용하도록 만들었다.
	- ==생성자 주입을 사용하기 위해 @Autowired를 입력할 필요도 없다.==
---
## 다음 강의 노트 : [[029_Java Spring Framework - 중요한 용어 이해하기]]
