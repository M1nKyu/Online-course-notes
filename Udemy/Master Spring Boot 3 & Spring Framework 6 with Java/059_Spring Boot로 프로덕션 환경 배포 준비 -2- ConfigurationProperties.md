---
created: 2024-10-30 17:42
course-name: Master Spring Boot 3 & Spring Framework 6 with Java
instructor: Ranga Karanam
section: 섹션 05 - Spring Boot 시작하기
presentation: 
tags:
  - lecture-note
  - "#udemy"
---
## 강의 노트
> 프로필을 사용하면 다양한 환경에 맞는 여러 설정을 보유할 수 있었다.
> 이번 강의 : 애플리케이션의 복잡한 설정을 지정하는 방법

- 현재 통신하는 통화 서비스의 여러 값을 설정한다고 가정
```properties
logging.level.org.springframework=debug  
spring.profiles.active=dev

# 📌📌📌📌
currency-service.url= 
currency-service.username=
currency-service.key=
```
- Spring Boot를 사용하여 이 애플리케이션 설정을 관리할 수 있는 방법?
- 여기서 프로퍼티 값을 정의하고 애플리케이션에서 활용할 수 있는 방법?

#### 방법: **configuration 프로퍼티** 사용
	- `CurrencyServiceConfiguration` 클래스 생성
		- 이 클래스에서 위의 값들을 정의할 것이다.
```java
@ConfigurationProperties(prefix = "currency-service") // currency-service는 접두사로 설정했음.
public class CurrencyServiceConfiguration {
    private String url;
    private String username;
    private String key;
}
```

- getter, setter도 추가한다
- @Component도 추가하여 Spring이 관리하도록 한다.
```java
@ConfigurationProperties(prefix = "currency-service")
@Component
public class CurrencyServiceConfiguration {
    private String url;
    private String username;
    private String key;

    public String getUrl() {
        return url;
    }

    public void setUrl(String url) {
        this.url = url;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getKey() {
        return key;
    }

    public void setKey(String key) {
        this.key = key;
    }
}
```

- 이제, 값들은 application.properties에서 설정하면 된다.
```properties
logging.level.org.springframework=debug
spring.profiles.active=dev

currency-service.url=http://default.in28minutes.com
currency-service.username=defaultusername
currency-service.key=defaultkey
```
- CurrencyServiceConfiguration의 프로퍼티에 매핑된다.

###### 확인하기
- 빠르게 확인할 수 있는 방법: 간단한 컨트롤러 만들기
- CourseController 복제 (`CurrencyConfigurationController.java`)
```java
@RestController
public class CurrencyConfigurationController {

    @RequestMapping("/currency-configuration")
    public CurrencyConfigurationController retrieveAllCourses() {
        return ;
    }
}
```

- 만들었던 `CurrencyServiceConfiguratio`n의 인스턴스를 가져오려고 한다.
	- 클래스의 `@Component`가 Spring이 이것의 인스턴스를 만들도록 한다.

- `@Autowired`를 입력하고 `private CurrencyConfigurationController configuration;` 입력한다.
	- Bean을 자동 연결
	- Spring이 Bean을 만들면 채워 넣고 여기서 컨트롤러에서 자동 설정한다.

- 간단한 서비스 응답으로 이를 반환하기
	- 실행후 `/currency-configuration` 접속
	- 설정한 값이 표시된다.

- dev 프로필에서 다른 프로퍼티를 사용하려면?
	- `application-dev.properties`로 이동하여 설정
```properties
logging.level.org.springframework=trace

currency-service.url=http://dev.in28minutes.com
currency-service.username=devusername
currency-service.key=devkey
```


> 복잡한 애플리케이션 설정을 만들어야 한다면, 
> 	`@ConfigurationProperties`를 사용하면 된다.
> 		Spring 컴포넌트를 만들고 `@ConfigurationProperties`로 어노테이션을 달면 된다


> Spring Boot의 ConfigurationProperties와 Profile의 조합은 강력하다.
> 	이를 통해 애플리케이션에 필요한 모든 설정을 외부화할 수 있다.


---
## 추가 학습


---
## 다음 강의 노트 : [[]]
