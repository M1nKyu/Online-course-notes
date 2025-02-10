---
created: 2024-08-05 22:41
course-name: Master Spring Boot 3 & Spring Framework 6 with Java
instructor: Ranga Karanam
section: 섹션 02 -Java Spring Framework 시작하기
presentation: 
tags:
  - lecture-note
  - "#udemy"
---
## 강의 노트
#### HelloWorldConfiguration 클래스에 age Bean 추가하기
```java
@Configuration
public class HelloWorldConfiguration {

    @Bean
    public String name(){
        return "Ranga";
    }

    @Bean // 📌
    public int age(){
        return 15;
    }
}
```

#### age Bean을 Spring에서 관리하도록 하고, 컨텍스트에서 값을 검색하도록 하려면
###### App02HelloWorldSpring 클래스에서
```java
public class App02HelloWorldSpring {
    public static void main(String[] args){
        // 1: Launch a Spring Context
        var context =
                new AnnotationConfigApplicationContext(HelloWorldConfiguration.class);

        System.out.println(context.getBean("name"));

        System.out.println(context.getBean("age")); // 📌 15가 출력
    }
}
```

#### Spring이 사용자 지정 클래스 객체를 관리하도록 할 수 있을까? 
###### HelloWorldConfiguration.java에서 레코드 생성
```java
record Person (String name, int age){}; // 📌
record Address (String firstLine, String city){};

@Configuration
public class HelloWorldConfiguration { ... }
```

> [!note] record 란?
> JDK 16에서 추가된 기능
> 일반적으로 자바 클래스를 생성할 때 수많은 getter, setter, 메소드, 생성자를 만들게 된다.
> 이렇게 Java Bean을 만드는 번거로움을 없애기 위해 도입된 기능이다.
> **setter, getter, 생성자 등을 만들 필요가 없어진다!** (자동으로 생성된다)
> ```java
> record Person (String name, int age){}; // 두 개의 인수가 있는 Person 생성자가 자동으로 생성된다
> ```
###### 확인해보기-> HelloWorldConfiguration 클래스에서 Bean 추가
```java
@Configuration
public class HelloWorldConfiguration {
	
	...
	
	// 📌 Bean 생성시 생성자가 자동으로 생성된다
    @Bean  // @Bean을 추가하여 Spring이 관리하도록 한다.
    public Person person(){ // person이라는 Spring Bean을 만든다.
        return new Person("Ravi", 20); 
        // person.name; person.age 를 사용할 수도 있다. (getter, setter 메소드도 자동으로 생성된다.) 📌
    }
}
```

###### App02HelloWorldSpring 클래스로 돌아와서
```java
public class App02HelloWorldSpring {
    public static void main(String[] args){
	    ...
		System.out.println(context.getBean("person")); // 📌

    }
}
```
- ==Person[name=Ravi, age=20]==가 출력된다

#### Address 생성: firstLine과 city를 갖는다고 할 때,
###### App02HelloWorldSpring.java은
```java
record Person (String name, int age){};
record Address (String firstLine, String city){}; // 📌

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
    public Address address(){
        return new Address("Baker Street", "London");
    }
}
```

###### HelloWorldConfiguration 에서
```java
public class App02HelloWorldSpring {
    public static void main(String[] args){
        // 1: Launch a Spring Context
        var context =
                new AnnotationConfigApplicationContext(HelloWorldConfiguration.class);

        // 2: Configure the things that we want Spring to manage - @Configuration
        // HelloWorldConfiguration - @Configuration
        // name - @Bean

        // 3: Retrieving Bean managed by Spring
        System.out.println(context.getBean("name"));

        System.out.println(context.getBean("age"));

        System.out.println(context.getBean("person"));

        System.out.println(context.getBean("address")); // 📌
    }
}
```

#### 이 단계에서는
- Spring이 관리하는 Bean을 추가했다
- 몇개의 사용자 지정 클래스(person, address)를 만들었고
	- Spring이 관리하는 클래스 객체가 생겼다.
---
## 다음 강의 노트 : [[016_Spring Framework Java 구성 파일에서 자동 연결 구현]]
