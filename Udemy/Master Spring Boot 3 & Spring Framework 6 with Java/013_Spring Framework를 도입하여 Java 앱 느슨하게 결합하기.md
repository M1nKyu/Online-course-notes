---
created: 2024-07-14 18:54
course-name: Master Spring Boot 3 & Spring Framework 6 with Java
instructor: Ranga Karanam
section: 섹션 02 -Java Spring Framework 시작하기
presentation: "[[course-presentation-master-spring-and-spring-boot.pdf#page=13&selection=38,0,38,6|course-presentation-master-spring-and-spring-boot, page 13]]"
tags:
  - lecture-note
  - "#udemy"
---
## 핵심 개념
- 의존성

## 상세 노트
- [[012_Java 인터페이스를 도입하여 느슨하게 결합된 앱 만들기#연습|앞 강의]]에서 추가한 Pacman을 실행하는 `AppGamingBasicJava` 에서 
	1. `line 23`: 객체 생성
	2. `line 24`: 객체 생성 + 종속성(의존성) 연결 (`Wiring of Dependencies`) 
	- **GamingConsole은 GameRunner 클래스의 의존성이다**
		- =="의존한다": 한 클래스가 다른 클래스를 사용한다==
```java 
// AppGamingBasicJava.java
public class AppGamingBasicJava {
    public static void main(String[] args){
    
        var game = new PacmanGame(); // 1. 객체 생성
        var gameRunner = new GameRunner(game); // 2. 객체 생성 + 의존성 연결
        gameRunner.run();
    }
}
```

- 우리가 하려는 건 게임에 의존성을 주입하는 것
	- 클래스는 생성되고 **GameRunner 클래스에 주입되거나 결합** 

- 엔터프라이즈 애플리케이션의 경우 수 천개의 클래스가 있고,
	- 수천개의 의존성이 생성되며 수천 개의 의존성이 필요한 곳에 주입

- 이 객체를 수동으로 직접 생성, 관리, 실행하는 대신
	- [[course-presentation-master-spring-and-spring-boot.pdf#page=13&selection=38,0,38,6|Spring 프레임워크가 하도록 하는 게 어떤가?]]


---
## 다음 강의 노트: [[014_첫 번째 Java Spring Bean 및 Java Spring 설정 시작]]