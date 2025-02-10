---
created: 2024-08-09 17:22
course-name: Master Spring Boot 3 & Spring Framework 6 with Java
instructor: Ranga Karanam
section: 섹션 02 -Java Spring Framework 시작하기
presentation: 
tags:
  - lecture-note
  - "#udemy"
---
## 강의 노트
> 지난 단계에서 Spring Framework와 다양한 유형의 Bean 설정 방법을 많이 보았다.
> 	이제 그 지식을 게임 애플리케이션에 반영한다.

###### 사전 리팩토링
- `com/in28minutes/learn_spring_framework/helloworld` 패키지를 만든다.
	- `App02HelloWorld`와 `HelloWorldConfiguration`을 패키지로 이동시킨다.
	- 정상 실행되는지 확인한다.

- 이 후에 `App02HelloWorldSpring.java` 에서 아래 부분에 경고가 발생하는 것을 확인할 수 있다.
```java
// AnnotationConfigApplicationContext는
// Java 기반 구성 클래스를 사용하여 애플리케이션 컨텍스트(Application Context)를 구성하고 초기화하는 데 사용되는 클래스
var context = new AnnotationConfigApplicationContext(HelloWorldConfiguration.class)
```
- 따라서 `App02HelloWorldSpring.java`를 아래처럼 수정한다. (**Try-with-resources**)
	- 코드 내에서 예외가 발생하면 상황별 단서가 자동으로 호출된다.
```java
public class App02HelloWorldSpring {
    public static void main(String[] args){
	    // 📌 try (var context ...) { 나머지 }
        try(var context = new AnnotationConfigApplicationContext(HelloWorldConfiguration.class)){
	        
            System.out.println(context.getBean("name"));
            System.out.println(context.getBean("age"));
            System.out.println(context.getBean("person"));
            System.out.println(context.getBean("address2"));
            System.out.println(context.getBean("person2MethodCall"));
            System.out.println(context.getBean("person3Parameters"));
			
            System.out.println(context.getBean(Person.class));
            System.out.println(context.getBean(Address.class));
			
            System.out.println(context.getBean("person5Qualifier"));
        }
    }
}
```

#### App03GamingSpringBeans.java 생성
- `App01GamingBasicJava.java` 파일을 복사하여 `App03GamingSpringBeans.java`를 생성한다. 
- 이 ==파일 전체를 Spring Configuration의 일부로 작성하고자 한다==
	- ==두 단계==를 거쳐야 한다.
		- 첫번째로 이 파일로 configuration 파일을 만들고
		- 다음으로 그 configuraion을 사용해 Spring Context를 만드는 것이다.

###### 단계 1. 설정파일 만들기
- 새 클래스를 생성하고 이름을 `GamingConfiguration`으로 설정한다.
	- `만들려 하는 것은 Bean이다.`

- `@Configuration`를 추가하고 Bean을 추가한다.
```java
@Configuration
public class GamingConfiguration {
    @Bean
    public GamingConsole game () {
        var game = new PacmanGame();
        return game;
    }
    
	// 기존 App01GamingBasicJava.java에 있던 내용 (나중에 해결할 것임) 
	// var game = new PacmanGame();  
	// var gameRunner = new GameRunner(game);  
	// gameRunner.run();
}
```

> GamingConfiguration에 game이 정의됐고, 앞서 만든 App03GamingSpringBeans로 이동하여 Spring Context를 만들 것이다.


###### 단계 2. Spring Context 만들기
- App03GamingSpringBeans로 이동하여
	- **AnnotationConfigApplicationContext**를 입력하고
		- 여기에서 GamingConfiguration을 사용한다.
	- ==이 부분은 context이다==.
```java
var context = new AnnotationConfigApplicationContext(GamingConfiguration.class);
```

- context.getBean을 입력하고, Bean의 type을 *GamingConsole.class* 로 지정한다.
	- 그리고 **up()** 메서드를 호출한다.
```java
public class App03GamingSpringBeans {
    public static void main(String[] args){
		
        var context = new AnnotationConfigApplicationContext(GamingConfiguration.class);
		
        context.getBean(GamingConsole.class).up(); // 📌
    }
}
```

- 이 파일을 실행하면
```java
UP
```

> 지금까지 간단한 Spring Context를 만들었다.
> 설정파일 GamingConfiguration을 만들었고, 
> 	여기서 GamingConsole game()을 정의했고, 
> 	활용하고 있는 게임은 PacmanGame이며 
> 	AnnotationConfigApplicationContext를 사용하는 Spring Context로 실행하고 있다.


###### 오류가 표시되어 리소스 누출 컨텍스트가 닫히지 않았다고 나오면
- ==리소스 블록에 코드를 넣는다.==
```java
try(var context = new AnnotationConfigApplicationContext (GamingConfiguration.class)){
	context.getBean(GamingConsole.class).up();
}
```

> 우리가 만들려 한 건 GamingConsole만이 아니다.


#### GameRunner도 가져와야 한다
- GameRunner 인스턴스도 만들어야 한다.
###### GamingConfiguration에서
- GameRunner 클래스도 만든다.
- 내용은 기존의 코드를 사용한다.
```java
    @Bean
    public GameRunner gameRunner(){
        var gameRunner = new GameRunner(game);
        return game;
    }
```
- game 에서 오류가 날 것이다. 
	- ==game을 어떻게 전달할까?==
		- 한가지 옵션은 여기서 game **메서드를 직접 호출** 하는 것이다.
		- 다른 옵션은 game을 **매개 변수로 전달**하는 것이다.
```java
    @Bean
    public GameRunner gameRunner(GamingConsole game){
        var gameRunner = new GameRunner(game);
        return gameRunner;
    }
```

###### App03GamingSpringBeans 에서
- 사용하지 않는 주석 모두 삭제 후
	- 인풋을 정리한다.
- GameRunnerBean을 가져오고 실행하는 코드도 추가한다.
```java
public class App03GamingSpringBeans {
    public static void main(String[] args){

        try(var context = new AnnotationConfigApplicationContext (GamingConfiguration.class)){

            context.getBean(GamingConsole.class).up();

            context.getBean(GameRunner.class).run(); // 📌
        }
    }
}
```

- 실행하면
```
UP
Runnning Game: com.in28minutes.learn_spring_framework.game.PacmanGame@4d5b6aac
UP
DOWN
LEFT
RIGHT
```

#### 이번 강의에서는
- game과 GameRunner를 Spring Bean으로 실행했다.
	- game과 GameRunner는 Spring Bean이며 
		- Spring Context에서 Bean을 가져와 실행했다.
---
## 다음 강의 노트 : [[022_Java Spring Framework에 대한 더 많은 질문 - 학습할 내용]]
