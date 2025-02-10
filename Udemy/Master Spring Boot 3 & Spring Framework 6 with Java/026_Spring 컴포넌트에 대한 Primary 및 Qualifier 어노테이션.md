---
created: 2024-08-14 17:24
course-name: Master Spring Boot 3 & Spring Framework 6 with Java
instructor: Ranga Karanam
section: 섹션 03 - Spring Framework를 사용하여 Java 객체를 생성하고 관리하기
presentation: 
tags:
  - lecture-note
  - "#udemy"
---
## 강의 노트
#### MarioGame도 @Component를 추가하면
- 오류가 발생한다 (*NoUniqueBeanDefinitionException*)

###### 어째서 고유한 Bean 정의가 없는 것인가?
- GameRunner는 GamingConsole의 구현이 필요하기 때문이다.
- Spring Framework가 GamingConsole 인터페이스의 구현을 찾고 있는데,
	- ==GamingConsole 인터페이스의 구현을 2개나 발견한 것이다.==
	- **MarioGame과 PacmanGame을 발견하고, 하나를 선택할 수 없는 상황**이다.
```java
@Component
public class GameRunner {

    private GamingConsole game;

    public GameRunner(GamingConsole game){
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

#### 어떻게 Spring Framework를 도와줄 수 있을까?
- 한 가지 방법으로 **@Primary** 를 사용할 수 있다.

###### MarioGame에 **우선권을 부여**한다면
```java
@Component
@Primary // 📌
public class MarioGame implements GamingConsole{

    public void up(){
        System.out.println("Jump");
    }

    public void down(){
        System.out.println("Go into a hole");
    }

    public void left(){
        System.out.println("Go back");
    }

    public void right(){
        System.out.println("accelerate");
    }
}
```
- 실행하면 MarioGame이 실행되는 것을 확인할 수 있다.
```
Jump
Runnning Game: com.in28minutes.learn_spring_framework.game.MarioGame@4fb3ee4e
Jump
Go into a hole
Go back
accelerate
```
- Primary는 여러 후보가 단일 값 의존성을 Autowiring할 자격이 있는 경우
	- Bean에 우선권을 부여한다.

#### 다른 대안 : @Quailifier 
###### SuperContraGame에서
- @Component를 추가한다
	- `실행하면 어떻게 될까? -> MarioGame 실행됨`

- MarioGame에 @Primary 어노테이션이 적용됐지만,
	- SuperContraGame에 우선권을 주고 싶다면? -> **@Qualifier** 사용

- **@Qualifier**를 추가하고 *SuperContraGameQualifier*를 추가한다
```java
@Component
@Qualifier("SuperContraGameQualifier") // 📌
public class SuperContraGame implements GamingConsole {

    public void up(){
        System.out.println("up");
    }
    public void down(){
        System.out.println("sit down");
    }
    public void left(){
        System.out.println("Go back");
    }
    public void right(){
        System.out.println("Shoot");
    }
}
```

- GameRunner에서 이 **@Qualifier를 사용하여 Auto-wiring**한다.
	- 생성자 주입이라는 것을 통해 Autowiring할 수 있다.
```java
@Component
public class GameRunner {

    private GamingConsole game;

	// GamingConsole 인스턴스는 생성자를 사용하여 이곳에서 Autowiring되고있다. 
	// 이 특정 매개변수에서 @Qualifier("SuperContraGameQualifier")를 추가하여 특정 Bean으로 지정한다.
    public GameRunner(@Qualifier("SuperContraGameQualifier") GamingConsole game){ // 📌
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

- 실행하면 SuperContraGame이 실행됨을 확인할 수 있다.
```
Jump
Runnning Game: com.in28minutes.learn_spring_framework.game.SuperContraGame@485966cc
up
sit down
Go back
Shoot
```
---
## 다음 강의 노트 : [[ 027_Primary와 Qualifier - 어떤 Spring 어노테이션을 사용할까]]
