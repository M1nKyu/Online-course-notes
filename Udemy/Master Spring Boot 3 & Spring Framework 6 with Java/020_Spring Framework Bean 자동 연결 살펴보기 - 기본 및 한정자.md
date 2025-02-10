---
created: 2024-08-08 16:55
course-name: Master Spring Boot 3 & Spring Framework 6 with Java
instructor: Ranga Karanam
section: 섹션 02 -Java Spring Framework 시작하기
presentation: 
tags:
  - lecture-note
  - "#udemy"
---
## 강의 노트
> [!question] 질문
>- Spring이 관리하는 Bean 프레임워크를 모두 나열하려면 어떻게?
> - 일치하는 여러개의 Bean을 사용할 수 있으면 어떻게 되는가?

#### Spring이 관리하는 Bean 프레임워크를 모두 나열하려면
- Spring Bean을 ==나열하려면 Context를 요청==해야 한다.
```java
context.getBeanDefinitionNames(); // 레지스트리에 정의된 Bean 이름 모두 반환
context.getBeanDefinitionCount(); // 레지스트리에 정의된 Bean 개수 반환
```
- getBeanDefinitionNames()에 마우스를 올려보면 반환값이 *String*임을 확인 가능하다.

- ==*context.getBeanDefinitionNames()* 를 반복하여 아래의 모든 것을 출력하고자 한다==.
```java
public class App02HelloWorldSpring {
    public static void main(String[] args){
        var context =
                new AnnotationConfigApplicationContext(HelloWorldConfiguration.class);

        System.out.println(context.getBean("name"));
        System.out.println(context.getBean("age"));
        System.out.println(context.getBean("person"));
        System.out.println(context.getBean("address2"));
        System.out.println(context.getBean("person2MethodCall"));
        System.out.println(context.getBean("person3Parameters"));

        context.getBeanDefinitionNames(); // 📌
    }
```

###### 📌 함수형 프로그래밍을 사용하여 손쉽게 반복하도록 하면 (부록을 통해 함수형프로그래밍 이해 필요)
> 거의 모든 엔터프라이즈는 Java 8 이상을 사용하며 **함수형 프로그래밍을 이해하는 것은 매우 중요**하다.

- Arrays 배열로 스트림을 만든다.
- *.forEach* 를 사용하여 이 스트림의 모든 요소에 대해 `System.out.println`을 실행하려 한다.
- 결과 스트림의 각 요소에 대해 `System.out.println`을 하려면
	- *System.out::println*을 입력한다. (여기서 사용하는 것을 메서드 참조라고 한다.)
```java
Arrays.stream(context.getBeanDefinitionNames())
		.forEach(System.out::println); 
```

- 실행하면 관리되는 모든 Bean 이름이 출력됨을 확인할 수 있다.
```
Ranga
15
Person[name=Ravi, age=20, address=Address[firstLine=Main Street, city=Utrecht]]
Address[firstLine=Baker Street, city=London]
Person[name=Ranga, age=15, address=Address[firstLine=Baker Street, city=London]]
Person[name=Ranga, age=15, address=Address[firstLine=Montinagar, city=Hyderabad]]
org.springframework.context.annotation.internalConfigurationAnnotationProcessor
org.springframework.context.annotation.internalAutowiredAnnotationProcessor
org.springframework.context.annotation.internalCommonAnnotationProcessor
org.springframework.context.event.internalEventListenerProcessor
org.springframework.context.event.internalEventListenerFactory
helloWorldConfiguration 📌  
name 📌
age 📌
person 📌
person2MethodCall 📌
person3Parameters 📌
address2 📌
address3 📌
```
- 설정파일 helloWorldConfiguration 도 Bean이 관리한다.
	- 그 아래의 Bean 모두 설정 파일에서 만든 기타 Bean이다.

#### 여러 개의 일치하는 Bean을 사용할 수 있으면 어떻게 되는가
###### System.out.println(context.getBean(Address.class)) 주석을 해제한다면
- 아래 코드를 실행하면 오류가 발생한다.
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
        System.out.println(context.getBean("person2MethodCall"));
        System.out.println(context.getBean("person3Parameters"));

        System.out.println(context.getBean(Address.class)); // 📌

    }
}
```

- 실행 결과의 일부는
	- 2개의 예외가 발견됐다.
	- address2, address3
```
No qualifying bean of type 'com.in28minutes.learn_spring_framework.Address' available: expected single matching bean but found 2: address2,address3
```

###### System.out.println(context.getBean(Person.class)) 을 추가한다면
```java
		...
		
        System.out.println(context.getBean("name"));
        System.out.println(context.getBean("age"));
        System.out.println(context.getBean("person"));
        System.out.println(context.getBean("address2"));
        System.out.println(context.getBean("person2MethodCall"));
        System.out.println(context.getBean("person3Parameters"));

        System.out.println(context.getBean(Person.class)); //📌
        System.out.println(context.getBean(Address.class)); 
```

^072888

- 실행 결과 일부는
	- 동일한 예외지만 ==3개가 발견==됐다.
	- person, person2MethodCall, person3Parameters
```
No qualifying bean of type 'com.in28minutes.learn_spring_framework.Person' available: expected single matching bean but found 3: person,person2MethodCall,person3Parameters
```

#### 스프링 프레임워크가 이 중 **하나에 우선순위를 부여**하도록 하려면?
###### HelloWorldConfiguration에서
- person4Parameters 생성
	- String name, int age 다음 address3 대신 address 입력
```java
    @Bean // 📌
    public Person person4Parameters(String name, int age, Address address){ // name, age, address2
        return new Person(name, age, address);
    }

    @Bean(name = "address2")
    public Address address(){
        return new Address("Baker Street", "London");
    }

    @Bean(name = "address3")
    public Address address3(){
        return new Address("Montinagar", "Hyderabad");
    }
}
```
- 현재 이름이 address2, address3 인 Bean 밖에 없음.
	- 이러면 어떻게 될까?

- 실행하면 오류 발생
	- 유형이 Address인 자격을 충족하는 Bean을 이용할 수 없다고 출력됨
```
No qualifying bean of type 'com.in28minutes.learn_spring_framework.Address' available: expected single matching bean but found 2: address2,address3
```

###### 일치하는 Bean이 여러 개일 경우 **후보** 라고 한다.
- 일치하는 후보가 여러 개인 시나리오에서는 Spring에서 예외를 출력한다.
	- 이 문제를 수정하는 방법은 **이 중 하나를 Primary(기본)로 만드는 것**이다.

- **@Primary**를 사용하여 Primary로 만들 수 있다. 📌

```java
    @Bean(name = "address2")
    @Primary // 📌
    public Address address(){
        return new Address("Baker Street", "London");
    }

    @Bean(name = "address3")
    public Address address3(){
        return new Address("Montinagar", "Hyderabad");
    }
```

- [[020_Spring Framework Bean 자동 연결 살펴보기 - 기본 및 한정자#^072888|위 코드를 다시 실행하면]] Address 문제를 해결했다는 것을 확인할 수 있다. 
	- Person 항목의 문제만 해결하면 된다.

- person은 4개이므로 이 중 하나를 Primary로 만들어 줘야 한다.
- 여러개의 후보 Bean 중에서 person4Parameters를 Primary로 만들면
```java
@Bean
@Primary // 📌
public Person person4Parameters(String name, int age, Address address){ // name, age, address2
	return new Person(name, age, address);
}
```
- ==실행하면 정상적으로 실행된다.==

###### Primary에 더해 문제를 해결하는 옵션 **@Qualifier**
- `@Qualifier`를 추가하고 한정자 이름을 지정하면
```java
    @Bean(name = "address3")
    @Qualifier("address3qualifier")
    public Address address3(){
        return new Address("Montinagar", "Hyderabad");
    }
```
- 이러면 org Spring 프레임워크 Bean Factory 주석 한정자에 인풋한다.

- 이제 ==한정자를 객체 외부에 사용할 수== 있다.
- 위 address를 자동으로 연결하는 경우 **기본 주소를 사용하지 않고 다른 것을 사용하고자 할 때 한정자를 활용**할 수 있다.

- 예를 들어, person5Qualifier를 생성하고, 아래의 Bean을 사용하고자 한다면 
```java
@Bean
public Person person5Qualifier(String name, int age, Address address){ // 📌
	return new Person(name, age, address);
}

@Bean(name = "address3")
@Qualifier("address3qualifier")
public Address address3(){
	return new Address("Montinagar", "Hyderabad");
}
```

- 아래와 같이 수정한다.
```java
@Bean
public Person person5Qualifier(String name, int age, 
							   @Qualifier("address3qualifier") Address address){ // 📌
	return new Person(name, age, address);
}
```

- 아래 코드를 추가하여 실행하면
```java
System.out.println(context.getBean("person5Qualifier"));
```

- 실행 결과에 다음이 출력된다.
```
Person[name=Ranga, age=15, address=Address[firstLine=Montinagar, city=Hyderabad]]
```
---
#### 정리하면
- 여러 개의 일치하는 Bean이 있을 경우
	- 첫 번째 옵션은 그 중 하나를 Primary로 지정하는 것이고,
	- 다른 옵션은 한정자를 만드는 것이다.
---
## 다음 강의 노트 : [[021_Spring Framework를 사용하여 게이밍 앱의 Bean 관리]]
