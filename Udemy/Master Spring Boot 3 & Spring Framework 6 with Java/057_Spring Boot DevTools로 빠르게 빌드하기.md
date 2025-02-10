---
created: 2024-10-30 16:27
course-name: Master Spring Boot 3 & Spring Framework 6 with Java
instructor: Ranga Karanam
section: 섹션 05 - Spring Boot 시작하기
presentation: 
tags:
  - lecture-note
  - "#udemy"
---
## 강의 노트
> 애플리케이션을 빠르게 빌드하는 데 도움을 주는 Spring Boot의 기능
> 	1. Spring Initializr
> 	2. Spring Boot Starter Project
> 	3. Auto Configuration
> 	4. **Spring Boot DevTools**

#### Spring Boot DevTools
- 개발자의 생산성은 매우 중요하다.
- 왜 코드를 변경할 때마다 수동으로 서버를 재시작해야 하는가?
	- 자동으로 서버를 시작하고 코드 변경사항을 적용하는 것은 어떤가?

###### DevTools 사용하기
- pom.xml을 연다
- dependencies에 다음을 추가한다.
```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-devtools</artifactId>
</dependency>
```
- 서버를 시작하고, `CoursesController`를 수정한다
	- 변경사항을 수정하자마자 Console에서는 다시 시작이 실행된다.
```java
@RequestMapping("/courses")
public List<Courses> retrieveAllCourses() {
	return Arrays.asList(
			new Courses(1, "Learn AWS", "in28minutes"),
			new Courses(2, "DevOps", "in28minutes"),
			new Courses(3, "Learn Azure", "in28minutes") // 📌
	);
}
```

> Spring Boot DevTools의 의존성을 추가하면
> 	java 파일이나 property파일을 변경하는 경우
> 		바로 확인할 수 있다. 다시 시작하지 않아도 된다.


> 주의점: pom.xml을 수정한 경우 반드시 수동으로 재시작해야 한다.


---
## 추가 학습


---
## 다음 강의 노트 : [[058_Spring Boot 프로덕션 환경 배포 준비 -1- Profile]]
