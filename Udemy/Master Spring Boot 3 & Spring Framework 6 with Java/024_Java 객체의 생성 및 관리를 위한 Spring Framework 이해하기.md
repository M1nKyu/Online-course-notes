---
created: 2024-08-12 17:48
course-name: Master Spring Boot 3 & Spring Framework 6 with Java
instructor: Ranga Karanam
section: 섹션 03 - Spring Framework를 사용하여 Java 객체를 생성하고 관리하기
presentation: 
tags:
  - lecture-note
  - "#udemy"
---
## 강의 노트
- 우리는 객체를 생성하기 위해 코드를 작성한다.
	- 그런데 **Spring으로 객체를 어떻게 생성**할 수 있을까?

#### 사전 설정 - 새 프로젝트 만들기
- 기존의 `learn-spring-framework` 프로젝트 이름을 `learn-spring-framework-01` 로 변경한다. 
- `learn-spring-framework-01` 을 복제하고 `learn-spring-framework-02`로 이름을 지정한다.
- `helloworld` 패키지를 삭제한다.
- `App01GamingBasicJava` 를 삭제한다.
- `App03GamingBasicJava`이 정상적으로 실행하는지 확인한다. 

#### Bean을 수동으로 만들 필요가 없다면?
- 현재 GamingConfiguration 클래스에서 Bean이 *수동으로 생성돼있다*.

###### 방법
- 우선 ==Configuration 파일과 App 파일을 결합==해야 한다.
- App03GamingSpringBeans 코드에 GamingConfiguration  코드를 붙여넣고, GamingConfiguration 파일을 삭제한다.
	- public은 삭제한다. 

- 붙여넣은 뒤의 App03GamingSpringBeans.java는 아래와 같다.
```java
// 📌 붙여넣은 내용 
@Configuration
class GamingConfiguration {

    @Bean
    public GamingConsole game () {
        var game = new PacmanGame();
        return game;
    }

    @Bean
    public GameRunner gameRunner(GamingConsole game){
        var gameRunner = new GameRunner(game);
        return gameRunner;
    }
}

public class App03GamingSpringBeans {
    public static void main(String[] args){

        try(var context = new AnnotationConfigApplicationContext (GamingConfiguration.class)){

            context.getBean(GamingConsole.class).up();

            context.getBean(GameRunner.class).run();
        }
    }
}
```

- App03GamingSpringBeans 을 실행해보면 정상적으로 출력함을 확인할 수 있다. 
	- 현재 Pacman 게임을 실행중이다.
```
UP
Runnning Game: com.in28minutes.learn_spring_framework.game.PacmanGame@4d5b6aac
UP
DOWN
LEFT
RIGHT
```


###### 다른 방법으로, 
- ==Configuration 클래스에서 Bean을 제거하고, 런처 클래스(App03GamingSpringBeans)에 입력==해본다.
	- GamingConfiguration 클래스는 삭제한다.

- ==App03GamingSpringBeans를 Configuration 파일==로 만들것이다. -> **@Configuration** 

- 한 군데 오류가 발생할 것이다. 
	- AnnotationConfigApplicationContext를 GamingConfiguration으로 실행하고 있기 때문이다.
	- *Configuration 어디에? -> App03GamingConfigurationBeans 클래스*
		- `new AnnotationConfigApplicationContext(App03GamingSpringBeans.class)`로 변경한다.
```java
@Configuration // 📌
public class App03GamingSpringBeans {
	
	// 📌
    @Bean
    public GamingConsole game () {
        var game = new PacmanGame();
        return game;
    }

    @Bean
    public GameRunner gameRunner(GamingConsole game){
        var gameRunner = new GameRunner(game);
        return gameRunner;
    }

    public static void main(String[] args){

		//try(var context = new AnnotationConfigApplicationContext (GamingConfiguration.class)){ // 📌
        try(var context = new AnnotationConfigApplicationContext (App03GamingSpringBeans.class)){

            context.getBean(GamingConsole.class).up();

            context.getBean(GameRunner.class).run();
        }
    }
}
```

- 실행하면 이전과 같은 결과로 정상적으로 실행된다.
```
UP
Runnning Game: com.in28minutes.learn_spring_framework.game.PacmanGame@4d5b6aac
UP
DOWN
LEFT
RIGHT
```

#### 흥미로운 점은 이보다 **훨씬 더 간단하게 작업할 수 있다는 것이다.**
- 지금까지 *@Bean* 을 사용하여 수동으로 Bean을 생성했다.

- ==이 대신 Spring에 Bean을 생성해달라고 요청할 수도 있다.==
###### Pacman 게임 생성을 Spring에 요청하기
- `var game = new PacmanGame();` 
	- 이 코드를 직접 작성하는 대신 Spring이 PacmanGame을 생성하는 방법은
		- ==**어노테이션**을 추가하면 된다.==

> [!important]
> 특정 클래스의 인스턴스 생성을 Spring에 요청하려면 **어노테이션을 추가**해야 한다.

- PacmanGame 클래스에서 **@Component**를 추가한다.
	- 이것이 ==Spring이 관리할 컴포넌트이다==.
```java
@Component // 📌
public class PacmanGame implements GamingConsole{
    @Override
    public void up() {System.out.println("UP");}

    @Override
    public void down() {System.out.println("DOWN");}

    @Override
    public void left() {System.out.println("LEFT");}

    @Override
    public void right() {System.out.println("RIGHT");}
}
```
- Spring이 우리에게 PacmanGame 인스턴스를 생성해주는 것이다.

- App03GamingConfigurationBeans로 돌아가서
	- `game()` Bean을 삭제하고 실행하여 자동으로 Bean을 생성하는지 확인해본다.
```java
@Configuration
public class App03GamingSpringBeans {
	
    @Bean // 📌 삭제할 Bean 
    public GamingConsole game () {
        var game = new PacmanGame();
        return game;
    }

    @Bean
    public GameRunner gameRunner(GamingConsole game){ // 📌 위 Bean을 삭제하면 game을 찾지 못함. 
        var gameRunner = new GameRunner(game); 
        return gameRunner;
    }

    public static void main(String[] args){

        try(var context = new AnnotationConfigApplicationContext (App03GamingSpringBeans.class)){

            context.getBean(GamingConsole.class).up();

            context.getBean(GameRunner.class).run();
        }
    }
}
```

###### 삭제 후 실행하면 오류가 발생한다. 
- NoSuchBeanDefinitionException을 확인할 수 있다.
```
Caused by: org.springframework.beans.factory.NoSuchBeanDefinitionException: 
No qualifying bean of type 'com.in28minutes.learn_spring_framework.game.GamingConsole' available: expected at least 1 bean which qualifies as autowire candidate. Dependency annotations: {}
```

- Spring은 GamingConsole의 구현, 즉 game을 찾으려고 한다. 
	- 하지만 Spring은 이 컴포넌트를 찾지 못한다.

- PacmanGame에 @Component를 추가했는데, *어째서 Spring이 PacmanGame을 찾지 못하는가?*
	- ==Spring에게 PacmanGame을 찾아야하는 곳을 알려줘야 하기 때문이다.==

- PacmanGame 클래스는 `package com.in28minutes.learn_spring_framework.game` 패키지에 정의되어 있다. 
	- 이 ==특정 패키지에서 PacmanGame을 검색해야 한다고 Spring Framework에 알려줘야 한다==

###### Spring에게 검색할 패키지를 알려주려면
- **@ComponentScan** 을 사용한다. 
	- `@ComponentScan("package com.in28minutes.learn_spring_framework.game")` 를 추가한다.
	- 이 안에 어떤 컴포넌트가 있든지 Spring은 Bean을 생성해야 한다.
```java
@Configuration
@ComponentScan("com.in28minutes.learn_spring_framework.game") // 📌
public class App03GamingSpringBeans {

    @Bean
    public GameRunner gameRunner(GamingConsole game){
        var gameRunner = new GameRunner(game);
        return gameRunner;
    }

    public static void main(String[] args){
        try(var context = new AnnotationConfigApplicationContext (App03GamingSpringBeans.class)){

            context.getBean(GamingConsole.class).up();

            context.getBean(GameRunner.class).run();
        }
    }
} 
```

- 실행하면 정상적으로 실행하는 것을 확인할 수 있다.
```
UP
Runnning Game: com.in28minutes.learn_spring_framework.game.PacmanGame@53ce1329
UP
DOWN
LEFT
RIGHT
```

> Spring이 PacmanGame 인스턴스를 생성하고, 이 인스턴스가 GamingConsole로 Auto Wiring 됨을 주목하자.
> 	`public GameRunner gameRunner(GamingConsole game){...}`에서 game에 파라미터로 전달된 것이다.

###### gameRunner Bean 생성도 Spring이 하도록 변경
- GameRunner 클래스에도 @Component를 추가한다.
```java
@Component // 📌
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

- Bean을 삭제한다.
```java
@Configuration
@ComponentScan("com.in28minutes.learn_spring_framework.game") // 📌
public class App03GamingSpringBeans {

	// 📌삭제
    // @Bean
    // public GameRunner gameRunner(GamingConsole game){
    //    var gameRunner = new GameRunner(game);
    //    return gameRunner;
    // }

    public static void main(String[] args){
        try(var context = new AnnotationConfigApplicationContext (App03GamingSpringBeans.class)){

            context.getBean(GamingConsole.class).up();

            context.getBean(GameRunner.class).run();
        }
    }
} 
```

> ==코드의 양을 크게 줄였다.==

> [!important]
> Spring은 
> 	- 객체를 관리하고 Auto Wiring을 수행할 뿐만 아니라
> 	- 객체를 생성해준다.  (@Component를 바탕으로, @ComponentScan을 사용하여)

###### 애플리케이션이 잘 작동하지 않는다면
- @ComponentScan을 제대로 설정했는지 확인해야 한다.

 >추천하는 방법은 ComponentScan에 설정한 패키지로 이동하는 것이다.
 >	- 예를 들어, `com.in28minutes.learn_spring_framework.game` 패키지에서 
 >		- 파일들을 *@Component*가 있는지 확인한다.

#### 클래스명 변경 -> GamingAppLauncherApplication 으로
- 변경 후 정상실행되는지 확인해본다.

> 만약 MarioGame, SuperContraGame에도 @Component가 있다면 어떻게 될까?
---
## 추가 학습
- MarioGame, SuperContraGame에도 @Component를 추가하고 바로 실행하니 오류가 발생했다.

---
## 다음 강의 노트 : [[026_Spring 컴포넌트에 대한 Primary 및 Qualifier 어노테이션]]
