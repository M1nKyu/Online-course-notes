---
created: 2024-08-12 17:31
course-name: Master Spring Boot 3 & Spring Framework 6 with Java
instructor: Ranga Karanam
section: 섹션 02 -Java Spring Framework 시작하기
presentation: 
tags:
  - lecture-note
  - "#udemy"
---
## 강의 노트
#### 질문5: Spring은 객체를 관리하고 자동 연결을 수행한다.
- 하지만 코드를 작성해 객체를 만드는 것은 사람이다.
- ==Spring이 객체를 만들도록 하려면 어떻게 해야할까?==

- 이전 강의에서는 아래와 같이 객체를 만들기 위한 코드를 작성했다.
	- Spring이 관리해 주지만, 객체를 만드는 것은 사람이다.
```java
@Configuration
public class GamingConfiguration {

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
```

#### 질문6: Spring Framework가 이것들을 정말 쉽게 해줄까? 
- 다음 단계에서 답변할 것이다. 
---
## 다음 강의 노트 : [[023_Java Spring Framework 살펴보기 - 섹션1 - 검토]]
