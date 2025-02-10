---
created: 2024-09-06 18:04
course-name: Master Spring Boot 3 & Spring Framework 6 with Java
instructor: Ranga Karanam
section: 섹션 04 - Spring Framework 고급 기능 살펴보기
presentation: 
tags:
  - lecture-note
  - "#udemy"
---
## 강의 노트
> 지금까지는 @Component를 사용했다.
> 	Spring Bean을 만들때마다 @Component를 추가했다.
> 	하지만, 다른 어노테이션도 사용할 수 있다.

#### @Component는 
- @Component는 제네릭 어노테이션이며 모든 클래스에 적용 가능하다.
- 특정 클래스에 Spring Bean을 생성하려는 경우 사용할 수 있다.

- 모든 Spring Stereotype Annotation의 Base이다.

- @Component는 다양하게 나뉘어 진다.
	- **@Service**
		- 어노테이션한 클래스에 *비즈니스 로직*이 있음을 나타낸다
		- 클래스에 비즈니스로직이 있다면 @Component대신 @Service를 사용할 수 있다.
	- **@Controller**
		- 웹 컨트롤러와 같은 컨트롤러 클래스인 경우에 자주 사용된다.
		- 웹 애플리케이션과 REST API에서 컨트롤러를 정의하는데 사용된다.
	- **@Repository**
		- Bean이 데이터베이스와 통신하는 경우
			- 데이터를 저장, 검색, 조작하는 경우 클래스에 사용한다.

###### 이전에 만들었던 *c1.RealWorldSpringContextLauncherApplication*를 보면
- 여기에 비즈니스 계산 서비스를 만들었다.

- 이 클래스를 만들때 @Component를 사용했는데
	- 이제 ==@Component 대신 @Service 어노테이션을 사용==할 수 있다.

- RealWorldSpringContextLauncherApplication 클래스
```java
// @Component
@Service //📌
@ComponentScan
public class RealWorldSpringContextLauncherApplication {

    public static void main(String[] args){
		...
    }
}
```

- MongoDbDataService 클래스
	- @Component 대신 **@Repository** 사용이 권장된다.
```java
// @Component
@Primary
@Repository
public class MongoDbDataService implements DataService{
    @Override
    public int[] retrieveData() {
        return new int[] { 11, 22, 33, 44, 55 };
    }
}
```
- ==일반적으로 Repository 클래스에서 데이터베이스와의 상호작용을 수행한다.==
	- MongoDbDataService의 목표는 MongoDb 데이터베이스와 통신하는 것이 목표이다.

- MySQLDataService 클래스도 마찬가지로 @Repository를 사용할 수 있다.
```java
//@Component
@Repository // 📌
public class MySQLDataService implements DataService{
    @Override
    public int[] retrieveData() {
        return new int[] { 1, 2, 3, 4, 5 };
    }
}
```

- RealWorldSpringContextLauncherApplication를 실행하면
	- 정상 작동한다.

> [!important] Stereotype Annotation이라고 불리는
> - @Component
> - @Service
> - @Controller
> - @Repository

#### 무엇을 사용해야 할까?
> 강사 추천: **최대한 구체적인 어노테이션**의 사용

- ==비즈니스 로직==을 포함한 클래스 -> *@Service*
- ==웹 컨트롤러== 클래스 -> *@Controller*
- ==DB와 통신==하는 클래스  -> *@Repository*

- 모두 해당되지 않을 때는 **제네릭 어노테이션**인 **@Component**를 사용한다.

###### 왜 최대한 구체적인 어노테이션?
- 프레임워크에 ==자신이 의도한 바를 더 자세히 나타낼 수== 있다.
	- 특정 클래스의 역할에 대한 정보를 더 주는 것이다.

- 나중에 *AOP(관점 지향 프로그래밍)* 을 사용하여 어노테이션을 감지하고  
	- 그 위에 부가적인 동작을 추가할 수 있다.
		- 예를 들어, *@Repository 어노테이션*이 있다면 Spring Framework에서 *자동으로 JDBC 예외 변환 기능에 연결*한다



---
## 추가 학습


---
## 다음 강의 노트 : [[046_중요한 Spring Framework 어노테이션]]
