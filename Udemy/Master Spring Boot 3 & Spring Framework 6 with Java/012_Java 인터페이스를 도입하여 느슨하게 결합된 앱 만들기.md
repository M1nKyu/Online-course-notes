---
created: 2024-07-14 16:50
course-name: Master Spring Boot 3 & Spring Framework 6 with Java
instructor: Ranga Karanam
section: 섹션 02 -Java Spring Framework 시작하기
presentation: 
tags:
  - lecture-note
  - "#udemy"
---
## 핵심 개념
- 인터페이스에 느슨한 결합 도입 

## 상세 노트
- `GameRunner` 클래스가 `GamingConsole` 인터페이스와 상호작용 하고 모든 클래스가 `GamingCosole` 인터페이스를 도입하도록 하는 것

- `GamingConsole` 인터페이스 생성하기 (`com.in28minutes.learn_spring_framework.game.GamingConsole`)
```java
public interface GamingConsole { // 📌 Interface 생성
    void up();
    void down();
    void left();
    void right();
}
```

- `MarioGame`, `SuperContraGame`, `PacmanGame` 클래스가 `GamingConsole` 인터페이스를 구현하도록 한다.
```java
// SuperContraGame.java
public class SuperContraGame implements GamingConsole{...}
```
```java
// MarioGame.java
public class MarioGame implements GamingConsole {...}
```
```java
// PacmanGame.java
public class PacmanGame implements GamingConsole {...}
```

> [!important] GamingConsole 인터페이스 생성으로인한 장점          
>`AppGamingBasicJava` 파일은 게임을 추가한다고 해도 코드를 변경하지 않아도 되는 장점이 생긴다.

- `GameRunner`는 현재 다음과 같음 
	- `GamingConsole`을 활용하도록 변경해야 함
```java
// 기존
public class GameRunner {
    MarioGame game;

    public GameRunner(MarioGame game){
        this.game = game;
    }

    public void run(){...}
}
//===================================================================
// 변경 후
public class GameRunner {

	// 클래스를 직접 사용하지 않고 인터페이스를 사용하도록 변경 
    private GamingConsole game;

    public GameRunner(GamingConsole game){
        this.game = game;
    }

    public void run(){...}
}
```

- `AppGamingBasicJava`
	- 현재 MarioGame을 실행하는 상태
	- `SuperContraGame`으로 전환하려면 주석을 다시 설정하면 됨 
```java
public class AppGamingBasicJava {
    public static void main(String[] args){
//        var game = new SuperContraGame();
        var game = new MarioGame();
        var gameRunner = new GameRunner(game); // 📌 매개값으로 GamingColse 인터페이스의 구현체인 MarioGame의 객체를 전달했다.
        gameRunner.run();
    }
}
```
- **게임을 변경하려 할 때, GameRunner 클래스에는 아무 변경을 할 필요가 없어진다**
	- ==**GamingConsole 인터페이스를 도입함으로써 GameRunner 클래스가 특정 게임에서 분리된다 !**==


###### 연습
- 팩맨 게임을 만들어 실행하기 
```java
// PacmanGame.java
package com.in28minutes.learn_spring_framework.game;

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

// AppGamingBasicJava.java
public class AppGamingBasicJava {
    public static void main(String[] args){
//        var game = new SuperContraGame();
//        var game = new MarioGame();
        var game = new PacmanGame();
        var gameRunner = new GameRunner(game);
        gameRunner.run();
    }
}
```
---
## 🆕 ==[[013_Spring Framework를 도입하여 Java 앱 느슨하게 결합하기]]==

