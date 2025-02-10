---
created: 2024-09-06 18:37
course-name: Master Spring Boot 3 & Spring Framework 6 with Java
instructor: Ranga Karanam
section: 섹션 04 - Spring Framework 고급 기능 살펴보기
presentation: 
tags:
  - lecture-note
  - "#udemy"
---
## 강의 노트
> 지난 섹션에서 배웠던 중요 Spring 개념 복습

|            Concpet             | Description                                                                                                                                                                 |
| :----------------------------: | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 의존성 주입<br>Dependency Injection | Spring Framework에서는 Bean을 확인해야 하고, 의존성을 확인해야 하며 의존성을 Bean에 연결해야 한다.<br>이 과정을 의존성 주입이라고 부른다. (제어의 역전이라고도 부른다.)                                                               |
|             생성자 주입             | 의존성을 주입하기위해 생성자를 호출한다면 생성자 주입이다.                                                                                                                                            |
|             세터 주입              | 세터 메서드를 사용하여 Bean에 의존성을 주입한다면 세터 주입이다.                                                                                                                                      |
|             필드 주입              | 세터 또는 생성자가 없을 때,<br>Spring Framework는 Reflection을 사용하여 의존성을 주입한다.                                                                                                           |
|         IOC Container          | IoC 컨테이너, 또는 제어의 역전 컨테이너는<br>Spring Bean과 Bean의 수명을 책임지는 Spring IoC Context 이다.<br><br>IoC 컨테이너는 Bean 생성, 전체 수명, 종료를 맡는다.<br><br>두가지 유형 : Bean Factory, Application Context |
|          Bean Factory          | <br>                                                                                                                                                                        |
|      Application Context       | 엔터프라이즈급 기능이 있는 고급 Spring IoC 컨테이너이다.<br><br>앱 애플리케이션에서 사용하기 쉽다.<br><br>국제화 기능을 제공하며,<br>Spring AOP 외에도 잘통합된다.                                                               |
|          Spring Bean           | Spring에서 관리하는 모든 객체                                                                                                                                                         |
|          Auto-wiring           | Spring Bean이 있는 곳에는 의존성이 있다.<br>Spring은 올바른 의존성을 찾아서 Bean을 연결해야 한다.<br>이 과정을 Auto-wiring이라고 한다.                                                                             |


---
## 추가 학습


---
## 다음 강의 노트 : [[048_Spring 전체 구조 알아보기 - Framework, 모듈, 프로젝트]]
