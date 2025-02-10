---
created: 2024-08-22 16:49
course-name: Master Spring Boot 3 & Spring Framework 6 with Java
instructor: Ranga Karanam
section: 섹션 03 - Spring Framework를 사용하여 Java 객체를 생성하고 관리하기
presentation: 
tags:
  - lecture-note
  - "#udemy"
---
## 강의 노트
> 지난 강의에서 @Component와 @Bean을 사용했다.
> ==이 둘의 차이점은 무엇이고 언제 어느 것을 사용해야 하는가?==

#### @Component **vs** @Bean
###### WHERE?
- @Component
	- 모든 자바 클래스에서 사용할 수 있다.
		- `MarioGame 상단의 @Component, GameRunner 상단의 @Component 등`
- @Bean
	- 특정 메소드에 적용한다.
		- 일반적으로 Spring Configuration 클래스의 메소드에서 사용된다.

###### Ease os use (어느 것을 더 쉽게 사용할 수 있을까?)
- **@Component가 더 쉽다.**
	- 어노테이션만 추가하면 된다.

- *@Bean*은 더 복잡하다. 
	- Bean을 생성하려면 코드를 전부 작성해야 한다.
	- 의존성을 Autowiring 하기 위해서도 모든 코드를 작성해야 한다.

###### Autowiring 
- @Component
	- Autowiring을 위해 **생성자 주입**, **수정자 주입**, **필드 주입** 방법 중 하나를 사용할 수 있다.
- @Bean
	- 특정 메소드를 호출하여 Autowiring하거나 매개변수를 사용할 수 있다.
	- [[016_Spring Framework Java 구성 파일에서 자동 연결 구현#^344c2f|아래 예시]]에서 name, age, address 메소드를 호출하고, name, age, address3 파라미터를 추가했다.
```java
    @Bean
    public Person person2MethodCall(){
        return new Person(name(), age(), address());
    }

    @Bean
    public Person person3Parameters(String name, int age, Address address3){ 
        return new Person(name, age, address3); 
    }
```

> 모두 Autowiring은 할 수 있지만, 방법은 다르다.

###### Who creates Beans?
- @Component
	- Spring Framework
	- Spring Framework는 컴포넌트 스캔을 수행한다.
		- 존재하는 컴포넌트 클래스를 식별한다.
		- 의존성을 식별하며, Bean이 생성되고 의존성의 Autowiring이 완료됐는지 확인한다.

- Bean
	- Bean 생성 코드를 직접 작성해야 한다.

###### Recommended For (무엇이 권장되는가)
- 일반적으로 ==대부분 @Component가 권장==된다. 

- 애플리케이션을 생성하는 경우
	- 내부에서 컴포넌트 클래스에 대한 코드를 작성하고
		- **컴포넌트 클래스에 대한 Bean을 생성**한다면 
			- 이런 상황에서는 @Component 방식을 추천한다.
	- 소스 코드에서 @Component를 적용하면 Spring이 쉽게 Bean을 생성하고 해당 의존성을 관리하게 된다.

- @Bean을 선택한다면
	- Bean을 생성하기 전에 여러 사용자 정의 비즈니스 로직을 수행해야 되는 되는 경우를 살펴보면
		- ==비즈니스 로직을 작성한 후 Bean을 생성==할 수 있다.
			- 이런 상황에서는 @Bean을 사용하는 것이 좋다.

- @Bean이 자주 사용되는 다른 상황이 있는데
	- 제 3자 라이브러리 Bean을 인스턴스화 하는 것이다.
	- Spring Security를 사용할 수도 있다.
		- Spring Security 코드에 대한 액세스 권한이 없기 때문에 Spring Security 코드도 사용할 수 없고 
			- @Component를 추가할 수도 없다.
		- Spring Security에 대한 Bean을 생성하고자 한다면
			- 이 상황에서 가장 좋은 방법은 @Bean을 추가하는 것이다.
		- ==@Bean을 사용하고 메소드를 작성하여 Bean을 생성==한다.
			- 그러면 Security Bean을 설정할 수 있다.
			- 이렇게 하면 필요에 따라 Spring Security를 설정하는데 도움이 된다.

#### 정리하면
- @Component가 일반적으로 애플리케이션에 권장된다.
- 애플리케이션에 대한 컴포넌트를 작성하고 있고, 해당 Bean을 생성하고 있다면 가장 쉬운 방법은 @Component다

- 하지만, @Bean이 권장되는 상황도 있다.
	- Bean을 생성하기 전에 ==수행해야 할 비즈니스 로직이 많거나==, 
	- Spring Security 같은 ==제3자 라이브러리에 대한 Bean을 인스턴스화하고 있을 경우==


---
## 추가 학습


---
## 다음 강의 노트 : [[031_Java Spring 애플리케이션에 의존성이 있는 이유가 무엇인가]]
