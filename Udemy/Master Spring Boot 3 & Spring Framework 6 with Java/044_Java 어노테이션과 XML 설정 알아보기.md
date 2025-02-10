---
created: 2024-09-06 17:54
course-name: Master Spring Boot 3 & Spring Framework 6 with Java
instructor: Ranga Karanam
section: 섹션 04 - Spring Framework 고급 기능 살펴보기
presentation: 
tags:
  - lecture-note
  - "#udemy"
---
## 강의 노트
> 어노테이션과 XML 설정을 비교할 것이다.

|          구분          |                          Annotations                           |                               XML Configuration                               |
| :------------------: | :------------------------------------------------------------: | :---------------------------------------------------------------------------: |
|     Ease of use      | Very Easy<br>특정 클래스, 특정 메소드나 변수 자체 소스에 아주 가깝게 어노테이션을 정의할 수 있다. |    Cumbersome(매우 번거롭다)<br>Bean 인스턴스를 만들려면 패키지 이름을 포함해 클래스 전체 이름을 나타내야 한다.     |
|  Short and concise   |                              Yes                               |                                      No                                       |
|     Clean POJOs      |       No.<br>POJOs are polluted with Spring Annotations        | Yes<br>No change in Java code.<br>`XML을 사용한다면 @Component 같은 어노테이션을 삭제할 수 있다.` |
|   Easy to Maintain   |                    Yes<br>소스에 매우 가깝게 표시하므로                     |                                    No<br>                                     |
|    Usage Frequncy    |                   Almost all recent projects                   |                                    Rarely                                     |
|    Recommendation    |            Either of the is fine BUT be consistent             |                                Do NOT mix both                                |
| Debugging difficulty |        Hard<br>`Spring Framework에 대한 이해도가 높아야 해결하기 쉬움`         |                                    Midium                                     |


---
## 추가 학습


---
## 다음 강의 노트 : [[045_Spring Framework 스테레오타입 어노테이션]]
