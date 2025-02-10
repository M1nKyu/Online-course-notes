---
created: 2024-08-06 23:02
course-name: Master Spring Boot 3 & Spring Framework 6 with Java
instructor: Ranga Karanam
section: 섹션 02 -Java Spring Framework 시작하기
presentation: 
tags:
  - lecture-note
  - "#udemy"
---
## 강의 노트
#### 생각해볼 수 있는 질문들
###### Spring Container, Spring Context, IOC Container, Application Context는 무엇인가? -> [[018_Spring IOC 컨테이너 살펴보기 - 애플리케이션 컨텍스트 및 Bean Factory|바로가기]]

###### Java Bean과 Spring Bean은 무슨 차이? -> [[019_Java Bean, POJO, Spring Bean 살펴보기|바로가기]]

###### Spring 프레임워크가 관리하는 Bean을 나열하려면 어떻게?

###### 여러개의 Bean을 사용할 수 있으면 어떻게 되는가?
- [[016_Spring Framework Java 구성 파일에서 자동 연결 구현#^444bf7|앞 강의 ]]에서 address2와 address3가 일치하는 Bean으로, 오류가 발생하는데 그 이유는 무엇인가?
```java
System.out.println(context.getBean("address2"));

System.out.println(context.getBean(Address.class)); // 📌
```
- 여기서 검색하고 있는 것은 Bean의 type이기 때문이다.
	- ==address2와 address3의 Bean의 type은 같다.==
```java
    @Bean(name = "address2")
    public Address address(){
        return new Address("Baker Street", "London");
    }

    @Bean(name = "address3") 
    public Address address(){
        return new Address("Montinagar", "Hyderabad");
    }
```
- 여러개의 Bean을 사용할 수 있으면 어떻게 되고,
	- Spring은 어떤 것을 사용해야 할까?
		- Spring이 어떤 것을 우선순위로 정하도록 해야 할까?

###### 스프링은 객체를 관리하고 자동 연결(Auto-wiring)을 수행한다.
- 하지만 코드를 작성해 객체를 만드는 것은 우리다.
	- HelloWorldConfiguration에서 `return new Person(...)` 또는 `return new Address(...)`와 같이.
- 우리가 객체를 만들기위해 코드를 작성한다.
- ==하지만, Spring이 객체를 만들기 시작하면 안될까?==
- ==Spring이 객체를 만들도록 하려면 어떻게 해야할까?==

>Spring은 시작하기 가장 어려운 프레임워크에 속한다.
>	이해할 개념이 매우 많다는 점에서.
---
## 다음 강의 노트 : [[018_Spring IOC 컨테이너 살펴보기 - 애플리케이션 컨텍스트 및 Bean Factory]]
