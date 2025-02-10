---
created: 2024-08-06 00:29
course-name: Master Spring Boot 3 & Spring Framework 6 with Java
instructor: Ranga Karanam
section: 섹션 02 -Java Spring Framework 시작하기
presentation: 
tags:
  - lecture-note
  - "#udemy"
---
## 강의 노트
#### Bean의 기본 이름이 메서드의 이름이라는 것을 관찰할 수 있다. **Bean 이름을 사용자 지정** 할 수 있을까?
###### 기존 코드는
```java
@Configuration
public class HelloWorldConfiguration {
    @Bean
    public String name(){
        return "Ranga";
    }

    @Bean
    public int age(){
        return 15;
    }

    @Bean
    public Person person(){
        return new Person("Ravi", 20);
    }

    @Bean
    public Address address(){
        return new Address("Baker Street", "London");
    }
}
```

^14dbcf

###### address의 이름을 변경한다고 하면,
- **@Bean 에 attribute를 추가**한다.
```java
    @Bean(name = "address2") // 📌 하고싶은 이름으로 설정하면 된다.
    public Address address(){
        return new Address("Baker Street", "London");
    }
```

###### 속성을 추가하고 바로 실행하면 어떻게 되는가 -> 예외발생
- 기존 상태의 App02HelloWorldSpring 클래스를 실행한다면? ==예외가 발생한다==
	- **address 라는 이름의 Bean을 사용할 수 없기 때문이다**.
		- **address2 로 이름을 변경**했기 때문
```java
package com.in28minutes.learn_spring_framework;

import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class App02HelloWorldSpring {
    public static void main(String[] args){
        var context =
                new AnnotationConfigApplicationContext(HelloWorldConfiguration.class);

        System.out.println(context.getBean("name"));

        System.out.println(context.getBean("age"));

        System.out.println(context.getBean("person"));

        System.out.println(context.getBean("address")); // 📌
    }
}
```

- 따라서 아래와 같이 수정 후 실행한다.
```java
System.out.println(context.getBean("address2"));
```
- **address 객체는 이제 address2 라는 Bean 이름을 가진다.** 
- 실행시 ==Address[firstLine=Baker Street, city=London]== 를 출력한다

#### Spring Context에서 Bean을 검색하는 방법 살펴보기
###### 이름을 속성으로 전달하는 context.getBean을 사용하여
- 아래는 이름을 속성으로 전달한다.
```java
System.out.println(context.getBean("address2"));
```

- ==클래스도 매개변수로 사용할 수 있다.==
```java
System.out.println(context.getBean(Address.class));
```
- 출력은 동일하게 ==Address[firstLine=Baker Street, city=London]== 이다.

- Spring이 Bean을 관리하게 되면
	- **Bean의 이름을 사용하거나 Bean의 클래스를 사용**하여 다시 **검색**하고, 다시 가져와 **사용**할 수 있다.

> [!tip]
> Spring이 여러 Bean을 정의할 수 있으며, Spring에서 관리하는 객체를 검색할 수 있는 다양한 접근 방식을 제공한다는 사실에 주목해야 한다.

#### 객체 사이에 관계가 있으려면?
- *기존 Spring Bean과 관계가 있는 새로운 Spring Bean을 만든다*.
> [!note] 이 문제에 접근하는 두가지 방법
> 1. 메서드 직접 호출
> 2. 매개변수 추가가

#### 메서드 호출 방식은
###### 새로운 인물을 추가한다.
- 새로운 인물의 이름을 `person2MethodCall`로 지정하고
	- 적절할 메서드를 호출한다. (`name(), age()`)
```java
@Configuration
public class HelloWorldConfiguration {
    @Bean
    public String name(){
        return "Ranga";
    }

    @Bean
    public int age(){
        return 15;
    }

    @Bean
    public Person person(){
        return new Person("Ravi", 20);
    }

    @Bean // 📌
    public Person person2MethodCall(){
        return new Person(name(), age());
    }

    @Bean(name = "address2")
    public Address address(){
        return new Address("Baker Street", "London");
    }
}
```

^0da8cf

- 검색하기
```java
System.out.println(context.getBean("person2MethodCall"));
```
- 실행하면 ==Person[name=Ranga, age=15]== 가 출력된다
	- `name() 에서의 "Ranga"와 age()에서의 15`

###### (더 나아가서) 인물 객체에 주소 추가하기
- ==Person 레코드에 Address를 추가==한다.
```java
record Person (String name, int age, Address address){};
```

- 기존 *person(), person2MethodCall* 은 다음과 같으므로 오류가 난다.
```java
@Bean
public Person person(){
	return new Person("Ravi", 20); // 📌
}

@Bean
public Person person2MethodCall(){
	return new Person(name(), age()); // 📌
}
```

- 아래와 같이 해결했다.
	- person은 하드코드로 해결
	- person2MethodCall은 메소드 호출로 해결
```java
@Bean
public Person person(){
	return new Person("Ravi", 20, new Address("Main Street", "Utrecht")); // 📌
}

@Bean
public Person person2MethodCall(){
	return new Person(name(), age(), address()); // 📌
}
```

- 출력은 아래와 같다.
```
Ranga
15
Person[name=Ravi, age=20, address=Address[firstLine=Main Street, city=Utrecht]] // 📌
Address[firstLine=Baker Street, city=London]
Person[name=Ranga, age=15, address=Address[firstLine=Baker Street, city=London]] // 📌
```

###### 현재까지
- ==Bean을 만들 수 있고, 기존 Bean을 재활용할 수 있다는 점을 확인했다.==
- 지금까지 본 접근 방식은 Bean 메서드를 직접 호출하는 것이다.
	- 다른 방식도 있다.

#### 다른 방식
###### 새로운 Bean *person3Parameters 를 생성* 한다.
```java
@Bean
public Person person3Parameters(String name, int age, Address address2){ // name, age, address2 📌
	return new Person(name, age, address2);
}
```
- 호출되는 변수명은 Bean 이름인 address2 
- 메서드를 호출하는 대신 **매개변수를 추가**한다.
- person3Parameters 라는 Bean을 만드는데 name, age, address2 라는 값이 사용된다.
	- 한개의 Spring Bean을 만드는데 다른 기존 Spring Bean을 사용한 것이다.

###### [[016_Spring Framework Java 구성 파일에서 자동 연결 구현#^0da8cf|Spring이 관리하는 Bean]]을 가져오는 다른 방법도 있다.
- 아래 코드를 추가하고 실행하면
```java
System.out.println(context.getBean("person3Parameters"));
```

- 아래 결과가 출력된다.
```
Ranga
15
Person[name=Ravi, age=20, address=Address[firstLine=Main Street, city=Utrecht]]
Address[firstLine=Baker Street, city=London]
Person[name=Ranga, age=15, address=Address[firstLine=Baker Street, city=London]]
Person[name=Ranga, age=15, address=Address[firstLine=Baker Street, city=London]] // 📌
```

- 원한다면 값에 변화를 줄 수 있다.
	- address2 대신 address3를 사용하도록 변경했다.
```java
@Configuration
public class HelloWorldConfiguration {
    @Bean
    public String name(){
        return "Ranga";
    }
    @Bean
    public int age(){
        return 15;
    }
    @Bean
    public Person person(){
        return new Person("Ravi", 20, new Address("Main Street", "Utrecht"));
    }
    @Bean
    public Person person2MethodCall(){
        return new Person(name(), age(), address());
    }

    @Bean
    public Person person3Parameters(String name, int age, Address address3){ // 📌
        return new Person(name, age, address3); // 📌
    }

    @Bean(name = "address2")
    public Address address(){
        return new Address("Baker Street", "London");
    }

    @Bean(name = "address3") // 📌
    public Address address(){
        return new Address("Montinagar", "Hyderabad");
    }
}
```

^344c2f

- 아래 코드 그대로 실행하면 오류가 발생한다.
	- 여러개의 Bean이 발견됐다는 예외를 출력한다.
```java
public class App02HelloWorldSpring {
    public static void main(String[] args){
        // 1: Launch a Spring Context
        var context =
                new AnnotationConfigApplicationContext(HelloWorldConfiguration.class);

        System.out.println(context.getBean("name"));

        System.out.println(context.getBean("age"));

        System.out.println(context.getBean("person"));

        System.out.println(context.getBean("address2"));
        
        System.out.println(context.getBean(Address.class)); // 📌 오류 원인
        
        System.out.println(context.getBean("person2MethodCall"));

        System.out.println(context.getBean("person3Parameters"));
    }
}
```
- 오류의 원인은 ==context.getBean(Address.class)== 이다. 
	- 주석으로 처리하여 실행한다.
- 실행하면 아래 결과를 출력한다.
```
Ranga
15
Person[name=Ravi, age=20, address=Address[firstLine=Main Street, city=Utrecht]]
Address[firstLine=Baker Street, city=London]
Person[name=Ranga, age=15, address=Address[firstLine=Baker Street, city=London]]
Person[name=Ranga, age=15, address=Address[firstLine=Montinagar, city=Hyderabad]] // 📌 address3가 실행됨을 알 수 있다.
```

#### 이 단계에서는 
- **Bean 설정**에 대해 알아야 할 중요한 점을 배웠다.
- 첫 번째로 **Bean에 사용자 지정 이름**을 설정할 수 있다는 점을 배웠다.
- 두 번째로 다양한 방법으로 **Spring Context에서 Bean을 검색**할 수 있다는 것을 배웠다.
	- Bean 이름을 사용하거나
	- 클래스이름의 형식을 사용하는 것
- 세 번째로 **기존 Bean을 사용하여 새로운 Bean**을 만들 수 있다는 것을 배웠다. 
- `System.out.println(context.getBean(Address.class));`이 코드를 주석처리 하지 않으면 문제가 생김을 확인했다.  ^444bf7
	- `여러개의 Bean이 발견됐다는 예외를 출력한다.`
---
## 다음 강의 노트 : [[017_Spring Framework에 대한 질문 - 학습할 내용]]
