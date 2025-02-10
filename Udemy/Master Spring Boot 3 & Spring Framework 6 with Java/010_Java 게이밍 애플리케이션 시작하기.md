---
created: 2024-07-12 17:12
course-name: Master Spring Boot 3 & Spring Framework 6 with Java
instructor: Ranga Karanam
section: 섹션 02 -Java Spring Framework 시작하기
presentation: 
tags:
  - lecture-note
  - "#udemy"
---
## 핵심 개념
- 

## 상세 노트
- src/main/java/com.in28minutes.... 에 `AppGamingBasicJava.java` 생성 

- AppGamingBasicJava
	- MarioGame 클래스와 GameRunner 클래스는 생성되지 않았으므로 빨간줄 표시가 날 것임
	- `Alt + Enter`
 눌러서 클래스 생성하기 -> 디렉토리 끝에 `.game` 추가한 패키지에 클래스 생성
```java
package com.in28minutes.learn_spring_framework;

import com.in28minutes.learn_spring_framework.game.GameRunner;
import com.in28minutes.learn_spring_framework.game.MarioGame;

public class AppGamingBasicJava {
	public static void main(String[] args){
		var marioGame = new MarioGame();
		var gameRunner = new GameRunner(marioGame);
		gameRunner.run();
	}
}
```
- 클래스를 생성했지만 marioGame에서 오류 
	- GameRunner 클래스에 MarioGame을 인자로 하는 ==생성자 선언해야 함==
- ../game/GameRunner
```java
public class GameRunner {
    MarioGame game;
    
	// 생성자 생성 
    public GameRunner(MarioGame game){
        this.game = game;
    }
    
	// run 메소드 생성
    public void run(){
        System.out.println("Runnning Game: " + game);
    }
}
```
> System.out.println() 사용했지만 좋은 사례는 아님.
> 	보통 로깅 프로그램을 사용하는게 좋음 

- MarioGame에 기능을 추가
- ../game/MarioGame
```java
package com.in28minutes.learn_spring_framework.game;

public class MarioGame {
    public void up(){ System.out.println("Jump"); }
    public void down(){ System.out.println("Go into a hole"); }
    public void left(){ System.out.println("Go back"); }
    public void right(){ System.out.println("accelerate"); }
}
```

- ../game/GameRunner
```java
package com.in28minutes.learn_spring_framework.game;

public class GameRunner {
    MarioGame game;

    public GameRunner(MarioGame game){
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

^a4cb87

- 실행 결과
```
Runnning Game: com.in28minutes.learn_spring_framework.game.MarioGame@4783da3f
Jump
Go into a hole
Go back
accelerate
```



> [!important] .
> GameRunner 클래스가 특정 클래스에 강하게 결합되어 있다는 것을 확인

---
## 🆕 ==[[011_느슨한 결합과 강한 결합]]==

