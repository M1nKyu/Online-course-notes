---
created: 2024-09-03 16:03
course-name: Master Spring Boot 3 & Spring Framework 6 with Java
instructor: Ranga Karanam
section: 섹션 04 - Spring Framework 고급 기능 살펴보기
presentation: 
tags:
  - lecture-note
  - "#udemy"
---
## 강의 노트
#### 사전 작업
######  이번 강의에서 필요한 스니펫
- /learn-spring-framework-02/src/main/resources/contextConfiguration.xml New
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context" xsi:schemaLocation="
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd"> <!-- bean definitions here -->
	
 	
</beans>
```

###### 패키지 생성
- a0 패키지를 복사하여  *h1* 이름으로 생성한다.
- 내부의 클래스명을 *XmlConfigurationContextLauncherApplication* 으로 변경한다.
```java
@Configuration // 📌 Java 설정을 사용함을 나타내는 어노테이션
@ComponentScan
public class XmlConfigurationContextLauncherApplication {

    public static void main(String[] args){

        try(var context = new AnnotationConfigApplicationContext (XmlConfigurationContextLauncherApplication.class)){
            Arrays.stream(context.getBeanDefinitionNames())
                    .forEach(System.out::println);
        }
    }
}
```

#### XML 설정 파일
###### XML 설정 파일 만들기
- XML 설정파일을 사용하기 위해 
	- 설정 파일을 만든다.

- /src/main/resources로 가서 XML 파일을 만든다. (*contextConfiguration.xml*)
	- ==여기에 Spring 설정을 전부 둘 수 있다.==
	- Bean도 둘 수 있다.

- 구글에 `spring xml configuration example` 를 검색하면 한 [페이지](https://docs.spring.io/spring-framework/docs/4.2.x/spring-framework-reference/html/xsd-configuration.html)가 나오는데 
	- [context schema](https://docs.spring.io/spring-framework/docs/4.2.x/spring-framework-reference/html/xsd-configuration.html#xsd-config-body-schemas-context:~:text=40.2.8%C2%A0the%20context%20schema)를 검색하면 간단한 XML 예시가 있다.
		- 이를 복사하여 (위의 스니펫) XML 파일에 붙여넣는다.

###### XML 컨텍스트 실행해보기
- 현재 *AnnotationConfigApplicationContext* 를 사용하고 있는데,
	- 이는 Java 설정을 실행하는데 사용된다.

- 하지만, XML과 관련된 것을 실행하려 하고 있기 때문에
	- **ClassPathXmlApplicationContext** 라는 클래스를 사용한다.
		- 이 것으로 ==설정 파일 위치를 전달할 수== 있다.
			- 클래스 경로에 src/main/resources가 바로 있으니 경로를 복사하여 그대로 붙여 넣는다.
	- 그리고 @Configuration과 @ComponentScan은 더이상 정의할 필요가 없다.
```java
// 📌 두 어노테이션(@Configuration, @ComponentScan) 삭제
public class XmlConfigurationContextLauncherApplication {

    public static void main(String[] args){
        try (var context = 
	        new ClassPathXmlApplicationContext("contextConfiguration.xml")){ // 📌
	            Arrays.stream(context.getBeanDefinitionNames())
	                    .forEach(System.out::println);
        }
    }
}
```

> Java로 정의할 수 있는 것이라면,
> 	XML로도 정의할 수 있다.

- 실행하면 아무것도 출려되지 않는다.
	- 정의된 Bean이 아직 없기 때문이다.


#### XML Context에서 Bean의 정의는
> contextConfiguration.xml 에서 정의한다.
###### name이라는 id의 Bean을 만든다고 하면
- `<beans></beans>` 내부에 
	- `<bean></bean>` 을 정의한다.
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans ...>
        <bean id="name"></bean> <!-- 📌 id가 name인 Bean-->
</beans>
```

###### 클래스도 지정할 수 있다.
- java.lang.String으로 지정한다면
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans ...>
        <bean id="name" class="java.lang.String"></bean> <!-- 📌 id가 name인 Bean-->
</beans>
```

###### 여기서 생성자 주입을 사용하고 싶으면
- 생성자 인수를 설정해야 한다. (**constructor-arg**)
- Bean 정의는 0개 이상의 생성자 인수를 지정할 수 있다.
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans ...>
        <bean id="name" class="java.lang.String">
	        <constructor-arg value="Ranga"></constructor-arg> <!--📌-->
        </bean> 
</beans>
```

###### 실행하면
```
name
```
- name 이라는 Bean이 출력하는 것을 볼 수 있다.

- name Bean의 값을 얻으려면 sysout 하면 된다.
```java
public class XmlConfigurationContextLauncherApplication {

    public static void main(String[] args){
        try (var context = new ClassPathXmlApplicationContext("contextConfiguration.xml")){
            Arrays.stream(context.getBeanDefinitionNames())
                    .forEach(System.out::println);

            System.out.println(context.getBean("name")); // 📌 Bean의 값을 출력하는 코드
        }
    }
}
```
- 실행하면
```
name
Ranga // 📌
```

###### XML 설정에 원하는 만큼 Bean을 많이 정의할 수 있다.
- 예를 들면 지난 강의에서 age를 xml에 정의한다면
```xml
<beans ...>
	<bean id="name" class="java.lang.String">  
	<constructor-arg value="Ranga"></constructor-arg>  
	</bean>  
	  
	<bean id="age" class="java.lang.Integer">  <!-- 📌 -->
	<constructor-arg value="15"></constructor-arg>  
	</bean>
</beans>
```
- 이 Bean도 *System.out.println(context.getBean("age"));* 로 Bean의 값을 출력할 수 있다.

###### 컴포넌트 스캔 (`<context:component-scan>`)
- XML 설정에서 정의할 수 있는 흥미로운 것 중 하나이다.
- 컴포넌트를 스캔할 패키지를 설정할 수 있다.

- game 패키지에서 컴포넌트를 스캔하려 한다고 하면
```xml
<beans ...>
        <bean id="name" class="java.lang.String">
            <constructor-arg value="Ranga"></constructor-arg>
        </bean>

        <bean id="age" class="java.lang.Integer">
            <constructor-arg value="15"></constructor-arg>
        </bean>
		
		<!--📌-->
        <context:component-scan base-package="com.in28minutes.learn_spring_framework.game">
        </context:component-scan>
        
</beans>
```

- 닫는 요소를 삭제하고 아래처럼 변경해도 된다.
	- `<context:component-scan base-package="com.in28minutes.learn_spring_framework.game" />`
	- constructor-arg도 마찬가지로 가능하다.
		- `<constructor-arg value="Ranga" />`

- 실행하면
```
name
age
gameRunner
gamingAppLauncherApplication
marioGame
pacmanGame
superContraGame
org.springframework.context.annotation.internalConfigurationAnnotationProcessor
org.springframework.context.annotation.internalAutowiredAnnotationProcessor
org.springframework.context.annotation.internalCommonAnnotationProcessor
org.springframework.context.event.internalEventListenerProcessor
org.springframework.context.event.internalEventListenerFactory
Ranga
15
```
- 더욱 많은 컴포넌트가 실행된 것을 볼 수 있다.
	- 모두 Bean으로 실행되고 있다.

- Component Scan 시
	- 특정한 패키지의 컴포넌트를 스캔하기 때문에 ==정의된 모든 것을 직접 가져온다==.

###### 컴포넌트 스캔을 대체할 방법은
- Bean을 각각 따로 정의하는 것이다.

- `<bean>` 을 쓰고 id와 만들려는 Bean이 같도록 한다.

###### PacmanGame을 사용하고 싶다면
- 관련되는 Bean을 만든다.
- 먼저 패키지를 복사해야 한다.
	- `com.in28minutes.learn_spring_framework.game.PacmanGame` 
```xml
<beans ...>
        <bean id="name" class="java.lang.String">
            <constructor-arg value="Ranga"></constructor-arg>
        </bean>

        <bean id="age" class="java.lang.Integer">
            <constructor-arg value="15"></constructor-arg>
        </bean>
		
		<!-- 📌 -->
        <bean id="game" class="com.in28minutes.learn_spring_framework.game.PacmanGame"/>
        
</beans>
```

- 실행하면
```
name
age
game // 📌
Ranga
15
```

> contextConfiguration에서 Bean을 하나 더 정의할 것이다.
> 	GameRunner를 정의할 것이다.

- Bean을 만들고, 생성자 인수를 전달하도록 한다.
	- 생성자 주입을 수행할 것이다.
	- **xml에서 생성자를 주입하는 방식**은 아래와 같다.
```xml
    <bean id="gameRunner" class="com.in28minutes.learn_spring_framework.game.GameRunner">
        <constructor-arg value="game" /> <!--📌-->
    </bean>
```
- ==GameRunner로 가서 game에 주입==하도록 하는 것이다.

- 실행하면 오류가 발생한다.
	- contextConfiguration의 위 코드에 해당하는 부분에서 생성자 인수는 **value**가 아니라 **ref**이다. 
```xml
<!-- 수정 전 -->
<constructor-arg value="game" /> <!-- 📌 -->

<!-- 수정 후 -->
<constructor-arg ref="game" />
```
- 다시 실행하면
```
name
age
game
gameRunner //📌
Ranga
15
```

- 원한다면 game도 실행하도록 할 수 있다.
	- 아래 코드를 추가하고 다시 실행한다.
```java
context.getBean(GameRunner.class).run(); 
```

> [!check]
> - **하지만 이제 XML 설정은 자주 사용되지 않는다.**
> - 그래도 이해하고 인지하는 것은 매우 중요하다
> 	- 여러 오래된 프로젝트에서는 아직도 XML Configuration을 사용하기 때문이다.

---
## 추가 학습


---
## 다음 강의 노트 : [[044_Java 어노테이션과 XML 설정 알아보기]]
