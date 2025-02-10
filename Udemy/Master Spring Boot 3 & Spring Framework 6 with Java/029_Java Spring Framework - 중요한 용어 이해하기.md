---
created: 2024-08-20 17:26
course-name: Master Spring Boot 3 & Spring Framework 6 with Java
instructor: Ranga Karanam
section: 섹션 03 - Spring Framework를 사용하여 Java 객체를 생성하고 관리하기
presentation: 
tags:
  - lecture-note
  - "#udemy"
---
## 강의 노트
#### 이번 강의에서
- Spring Framework에서 일반적으로 사용되는 몇 가지 중요한 ==용어를 복습==한다.
---
#### @Component
- @Component를 클래스에 추가하는 경우
	- 클래스의 인스턴스는 Spring Framework가 관리하게 된다.

- @Component를 추가할 때마다 특정 클래스가 Component Scan에 있다면
	- 해당 클래스의 인스턴스, 즉 **Spring Bean이 생성**되고
		- **Spring Framework에 의해 관리**된다.
---
#### Dependency (의존성)
- GameRunner에 GamingConsole의 구현(impl)이 필요하다는 것을 확인했다.
	- Mario, SuperContra, PacMan 중 하나가 필요했다.

- GamingConsole 구현을 살펴보면
	- 예를 들어, MarioGame은 GameRunner의 의존성이다.
---
#### Component Scan
- Spring은 컴포넌트의 위치를 파악해야 한다.

- 어떻게 Spring Framework에 **검색 위치를 알려줄** 수 있을까?
	- 바로 **Component Scan을 정의해야** 한다.

- ==Component Scan 정의는 어떻게== 하는가?
	- **@ComponentScan** 어노테이션을 추가해야 한다.

- @ComponentScan에서는 패키지 이름을 명시적으로 지정할 수 있다.
	- `예시 : @ComponentScan("com.in28minutes")`
		- 이렇게 하면 ==지정한 패키지와 그 하위 패키지를 검색==하여 컴포넌트를 찾아낸다.

- 패키지 이름 없이 @ComponentScan을 추가한다면
	- *현재 패키지*와 *모든 하위 패키지*가 Spring Framework에 의해 스캔된다.
---
#### Dependency Injection (의존성 주입)
- 애플리케이션을 실행하면 **가장 먼저 Component Scan을 수행**한다.
	- 모든 컴포넌트를 찾으려고 할 것이다.
	- ==컴포넌트의 의존성이 무엇인지 식별하고, 모두 Auto-wiring한다==

- 이러한 **Bean과 의존성을 식별하고 모두 wiring 하는 전체 프로세스**를 Dependency Injection이라고 한다. 
---
#### IOC (제어반전, Inversion of Control)
- **의존성 주입과 관련**된 다른 용어 한가지

- 첫 프로젝트(learn-spring-framework-01)에서 
```java
var game = new PacmanGame(); 

var gameRunner = new GameRunner(game);
```
- 여기에서 객체 생성을 위한 코드를 작성하고 있다.
	- 객체 생성과 의존성에 대한 코드를 작성한 것이다.
	- 이는 우리가 직접 제어하고 있다.
	- ==프로그래머가 명시적으로 코드를 작성하여 의존성을 통해 객체를 생성한 것이다.==

- 하지만, GamingAppLauncherApplication에서는 누가 제어하고 있는가?
	- 프로그래머는 ComponentScan을 정의하고 
		- 몇 개의 **@Component를 MarioGame, GameRunner 클래스에 정의**했다. 
	- ==객체 생성, 와이어링하는 실제 작업은 Spring Framework가 수행한다.==

- **제어권이 프로그래머(명시적으로 작성된 코드)에서 Spring Framework로 넘어간 것**이다.
---
#### Spring Beans
- Spring Framework가 관리하는 **모든 객체를 Spring Bean**이라고 한다.
---
#### IOC container
- Bean의 생명주기와 의존성을 관리하는 Spring Framework의 컴포넌트이다.
	- `ApplicationContex`와 `BeanFactory` 라는 2가지 유형을 다뤘다.
		- ApplicationContext는 많은 복잡한 기능을 제공, BeanFactory는 더 간단한 기능을 제공한다.
		- BeanFactory는 잘 사용하지 않는다.
---
#### Autowiring (자동 와이어링)
- Spring Bean에 대한 의존성의 **Wiring 프로세스**를 말한다.
- Spring이 특정 **Bean을 만나면 Bean이 필요한 의존성이 무엇인지 식별**한다.

- 예시로, Spring이 GameRunner 클래스를 만나면 
	- 이 클래스에 **생성자가 있으므로 이것이 의존성**이고
		- **game 객체가 필요하다는 것을 알게 된다**.
```java
@Component
public class GameRunner {
	
    private GamingConsole game;
	
    public GameRunner(@Qualifier("SuperContraGameQualifier") GamingConsole game){
        this.game = game;
    }
	
    public void run(){
        System.out.println("Runnning Game: " + game);
        game.up();
        game.down();
        game.left();
        game.right();
    }
}
```

- 다른 예시로, Spring이 YourBusiniessClass를 만나면
	- 여기에 생성자가 있고, **2개의 의존성이 필요**하다는 것을 파악한다.
		- 이것을 찾아서 생성자 주입을 통해 Auto wiring을 한다.
```java
@Component
class YourBusinessClass {

    Dependency1 dependency1;
    Dependency2 dependency2;

    @Autowired
    public YourBusinessClass(Dependency1 dependency1, Dependency2 dependency2) {
        System.out.println("Constructor Injection - YourBusinessClass");
        this.dependency1 = dependency1;
        this.dependency2 = dependency2;
    }
		...
	
}
```

> 모든 용어가 서로 밀접하게 연관되어 있다는 것을 확인할 수 있다.
---
## 다음 강의 노트 : [[030_Java Spring Framework - @Component 와 @Bean 비교]]
