---
created: 2024-08-26 16:57
course-name: Master Spring Boot 3 & Spring Framework 6 with Java
instructor: Ranga Karanam
section: 섹션 03 - Spring Framework를 사용하여 Java 객체를 생성하고 관리하기
presentation: 
tags:
  - lecture-note
  - "#udemy"
---
## 강의 노트
#### 사전 작업
- a0 패키지를 복붙하고
	- 패키지명을 c1으로 지정한다.
- 패키지 내부의 `SimpleSpringContextLauncherApplication` 클래스명을
	- `RealWorldSpringContextLauncherApplication`으로 변경한다.
```java
// 📌 RealWorldSpringContextLauncherApplication

@Configuration
@ComponentScan
public class SimpleSpringContextLauncherApplication {

    public static void main(String[] args){

        try(var context = new AnnotationConfigApplicationContext (SimpleSpringContextLauncherApplication.class)){
            Arrays.stream(context.getBeanDefinitionNames())
                    .forEach(System.out::println);
        }
    }
}
```

###### DataService 인터페이스 생성
- c1 패키지에 DataService 인터페이스를 생성한다.
- DataService에는 retrieveData 메소드가 포함되어 있다.
```java
public interface DataService {
    int[] retrieveData();
}
```

###### 구현 클래스 생성
- MongoDbDataService 클래스
```java
public class MongoDbDataService implements DataService{
    @Override
    public int[] retrieveData() {
        return new int[] { 11, 22, 33, 44, 55 };
    }
}
```

- MySQLDataService 클래스
	- 위 클래스를 복사하여 생성하면 된다.
```java
public class MySQLDataService implements DataService{
    @Override
    public int[] retrieveData() {
        return new int[] { 1, 2, 3, 4, 5 };
    }
}
```

###### BusinessCalculationService 생성
- 새 클래스 생성한다. (BusinessCalculationService)
- 이 클래스에는 비즈니스 로직을 수행하는 findMax 메소드가 필요하다.
```java
public class BusinessCalculationService {
    public int findMax(){
        return Arrays.stream(dataService.retrieveData()) // 📌dataService 빨간줄 발생
                .max().orElse(0);
    }
}
```

- `dataService`에서 오류가 발생한 것을 확인할 수 있다.
	- 코드 한줄을 추가한다.
```java
public class BusinessCalculationService { 
	private DataService dataService; // 📌
	
    public int findMax(){
        return Arrays.stream(dataService.retrieveData())
                .max().orElse(0);
    }
}
```

#### [[031_Java Spring 애플리케이션에 의존성이 있는 이유가 무엇인가#연습해보기|연습해보기]] 솔루션 
###### 생성자 주입을 사용한 의존성 주입
- DataService는 BusinessCalculationService의 의존성인데,
	- 우리는 생성자 주입을 사용해야 한다.

- BusinessCalculationService 클래스에 생성자를 생성한다.
- 그 다음, 이것을 컴포넌트로 만들기 위해 *@Component*를 추가한다.
```java
@Component // 📌
public class BusinessCalculationService {
    private DataService dataService;

    public BusinessCalculationService(DataService dataService) { // 📌 생성자
        super();
        this.dataService = dataService;
    }

    public int findMax(){
        return Arrays.stream(dataService.retrieveData())
                .max().orElse(0);
    }
}
```

- MySQLDataService와 MongoDbDataService에도 *@Component*를 적용한다.
	- **이제 모두 Spring Framework가 관리하게 되었다!**


###### MongoDB에 우선권 주기
- ==위 상태 그대로== 실행하게 된다면, ==예외가 발생==할 것이다.
	- 일치하는 후보가 여러개이기 때문이다.
		- 이러한 이유로 우선권을 주어야 한다.
```java
@Primary // 📌
@Component
public class MongoDbDataService implements DataService{
    @Override
    public int[] retrieveData() {
        return new int[] { 11, 22, 33, 44, 55 };
    }
}
```

###### Spring Context를 생성하기
> 이제 Spring Context를 생성하고, Bean을 검색한 다음 findMax를 실행할 수 있다.

- RealWorldSpringContextLauncherApplication으로 이동한다.
	- Context를 시작하고 여기에 정의된 모든 Bean을 출력해야 하는데,
		- 그전에 실행하여 결과부터 보자면
```
org.springframework.context.annotation.internalConfigurationAnnotationProcessor
org.springframework.context.annotation.internalAutowiredAnnotationProcessor
org.springframework.context.annotation.internalCommonAnnotationProcessor
org.springframework.context.event.internalEventListenerProcessor
org.springframework.context.event.internalEventListenerFactory
realWorldSpringContextLauncherApplication
businessCalculationService
mongoDbDataService
mySQLDataService
```
- 모든 Bean을 출력하고 있다. 

- 우리는 ==BusinessCalculationService를 얻고 이에 대해 메소드를 실행해야 한다==.
	- 어떻게 할까?
		- **context.getBean()** 를 추가하고, **이 Bean에 정의된 findMax메소드도 실행**해야 한다.
			- 이것을 확인하기 위해 *sysout도 한다*.
```java
@Configuration
@ComponentScan
public class RealWorldSpringContextLauncherApplication {

    public static void main(String[] args){

        try(var context = new AnnotationConfigApplicationContext (RealWorldSpringContextLauncherApplication.class)){
            Arrays.stream(context.getBeanDefinitionNames())
                    .forEach(System.out::println);

            System.out.println( // 📌
                    context.getBean(BusinessCalculationService.class).findMax()); // 📌
        }
    }
}
```

- 실행해보면
```
org.springframework.context.annotation.internalConfigurationAnnotationProcessor
org.springframework.context.annotation.internalAutowiredAnnotationProcessor
org.springframework.context.annotation.internalCommonAnnotationProcessor
org.springframework.context.event.internalEventListenerProcessor
org.springframework.context.event.internalEventListenerFactory
realWorldSpringContextLauncherApplication
businessCalculationService
mongoDbDataService
mySQLDataService
55 // 📌
```
- 55가 추가로 출력됐다.

- 일반적 RealWorldApplication은 다음과 같은 방식으로 주어진다.
	- ==Data Service를 호출하는 Business Service==를 갖게되는 것이다.\
```
BusinessCalculationService
	|
DataService
	└ MongoDbDataService
	└ MySQLDataService
```

#### 마무리
- 보시다시피 비즈니스 로직에만 집중하고 있다.
- 데이터 서비스 및 비즈니스 서비스 ==인스턴스를 생성하는 방법, 의존성을 와이어링 하는 방법에는 그다지 주목할 필요가 없다==.
- 이제 이 모든 것들은 Spring Framework가 담당하고 있으며, 
	- **Spring Framework 덕분에 우리가 비즈니스 로직에 집중**할 수 있는 것이다.

---
## 추가 학습


---
## 다음 강의 노트 : [[033_Java와 함께 Spring Framework 알아보기-섹션2 복습]]
