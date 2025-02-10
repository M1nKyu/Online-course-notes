---
created: 2024-10-30 17:05
course-name: Master Spring Boot 3 & Spring Framework 6 with Java
instructor: Ranga Karanam
section: 섹션 05 - Spring Boot 시작하기
presentation: 
tags:
  - lecture-note
  - "#udemy"
---
## 강의 노트
> 프로덕션 환경에서 사용가능한 애플리케이션을 쉽게 만들 수 있도록하는 Spring Boot의 기능

#### 1. Managing App. Configuration Using Profiles
- 프로필을 사용하여 애플리케이션 설정을 관리하는 기능

- 애플리케이션에는 다양한 환경이 있다.
	- `Dev, QA, Stage, Prod, ...`
- 동일한 애플리케이션의 다양한 환경에는 다양한 설정이 필요하다.
	- 다른 데이터베이스와 통신
	- 다른 웹 서비스를 호출

- 어떻게?
	- 프로필: 환경별 설정을 제공한다.
###### 내 환경에 맞는 다양한 항목 설정하기
- 각 환경에 맞는 별도의 프로필을 만들면 된다.

- dev환경과 prod환경 두개만 있다고 가정하면
```
dev
prod
```

- application.properties에는 `logging.level.org.springframework=debug`가 정의되어 있다.
	- 하지만 내 dev 환경에서는 디버그 수준이 아닌 trace 수준에서 정보를 출력하려 한다.
		- `trace는 더 많은 로그 출력`
	- prod 프로필에서는 info 수준에서 구성하려고 한다.
```properties
dev
~~~
logging.level.org.springframework=trace

prod
~~~
logging.level.org.springframework=info
```

- 어떻게? 
	- 프로필을 사용하면 된다.
	- `application.properties`파일을 복제한다(`application-dev.properties`와 `application-prod.properties`)
```properties
# application-dev.properties 에 복사
logging.level.org.springframework=trace

# application-prod.properties 에 복사
logging.level.org.springframework=trace
```

- 현재 `application.properties`는 어떤 프로필도 사용하고 있지 않다.
	- 프로필을 사용하고 있지 않으면 애플리케이션은 `application.properties`에서 설정된 디폴트 properties을 사용한다

###### 프로덕션 환경이라면
- prod profile 설정 (`application.properties`에 추가)
```properties
# application.properties

logging.level.org.springframework=debug
spring.profiles.active=prod  #📌
```
- 특정 프로필을 설정하려면 `application.properties`에 있는 디폴트 설정값과 `application-prod`의 값을 함께 병행해야 한다.

- 프로필 값은 `application-prod.properties`의 우선순위가 더 높다
	- 따라서 우선순위가 높은 info 로그만 당장 출력


###### 개발 환경이라면
- 개발환경에 배포한다고 가정하면 활성 프로필을 dev로 설정하면 된다.
```properties
# application.properties
logging.level.org.springframework=debug  
spring.profiles.active=dev
```
- 실행시 trace 정보가 많이 출력된다.
	- trace, debug, info 출력

###### 지원되는 여러 로깅 수준
- `trace` : 로그의 모든 정보 (아래 모든)
- `debug`: 훨씬 더 많은 정보 (아래 모든)
- `info`: info 수준의 로깅된 모든 정보 (아래 모든)
- `warning` : 아래보다 한 단계 위 많은 정보 (아래 모든)
- `error` :오류와 예외만

- `off` : 전체 로깅을 끈다.




---
## 추가 학습


---
## 다음 강의 노트 : [[059_Spring Boot로 프로덕션 환경 배포 준비 -2- ConfigurationProperties]]
