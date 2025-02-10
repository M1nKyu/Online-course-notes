---
created: 2024-09-23 17:00
course-name: Master Spring Boot 3 & Spring Framework 6 with Java
instructor: Ranga Karanam
section: 섹션 05 - Spring Boot 시작하기
presentation: 
tags:
  - lecture-note
  - "#udemy"
---
## 강의 노트
> 간단한 REST API 만드는 방법

- `com.in28minutes.springboot.learn_spring_boot` 패키지에 *CourseControlle 클래스* 생성

- 아래 URL/courses 를 사용하여 간단한 REST API를 만들 것이다.
```
http://localhost:8080/courses
[
	{
		"id": 1,
		"name": "Learn AWS",
		"author": "in28minutes"
	}
]
```

- CourseController에서 하려는 작업은
	- REST API를 만드는 것이다.
		- 따라서 주석 **@RestController**를 추가한다.
```java
@RestController
public class CourseController {
	// 📌 여기에 메소드를 만들고 만들려는 링크를 노출할 수 있다.
	// /courses  
	// Course: id, name, author
}
```

```java
@RestController
public class CourseController {
    // /courses
    // Course: id, name, author

    public List<Courses> retrieveAllCourses() { // 📌
        return Arrays.asList( 
                new Courses(1, "Learn AWS", "in28minutes"),
                new Courses(2, "DevOps", "in28minutes")
        );
    }
}
```
- 위에서 컴파일 오류가 여러 개 표시된다.
	- Course 클래스가 없기 때문이다.

- *Shift + Enter*로 Create Class를 한다.
	- 이 클래스에 Constructor, Getters, toString을 생성할 것이다.
		- 우클릭 -> Generate 
			- -> toString 생성
			- -> Getter 생성
			- -> Constructor 생성
```java
public class Courses {
    private long id;
    private String name;
    private String author;

    public Courses(long id, String name, String author) { // 📌
        super();
        this.id = id;
        this.name = name;
        this.author = author;
    }

    public long getId() { // 📌
        return id;
    }

    public String getName() { // 📌
        return name;
    }

    public String getAuthor() { // 📌
        return author;
    }

    @Override
    public String toString() { // 📌
        return "Courses{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", author='" + author + '\'' +
                '}';
    }
}
```

- 다음 할 작업은 URL을 위 특정 메소드(`retrieveAllCourses()`)에 매핑하는 것이다.
	- **@RequestMapping** 을 사용하면 된다.
```java
@RestController
public class CourseController {

    @RequestMapping("/courses") // 📌
    public List<Courses> retrieveAllCourses() {
        return Arrays.asList(
                new Courses(1, "Learn AWS", "in28minutes"),
                new Courses(2, "DevOps", "in28minutes")
        );
    }
}
```

> 이제 간단한 CourseController가 있고, 여기서 /courses를 노출한다.
> `http://localhost:8080/courses` URL 을 사용할 수 있어야 하고
> 	이 URL로 이동하면 코스 목록이 반환되어야 한다.

- 실행 후 브라우저에서 http://localhost:8080/courses 를 입력한다.
	- JSON 응답이 반환되는 것을 확인할 수 있다.
```
[
  {
    "id": 1,
    "name": "Learn AWS",
    "author": "in28minutes"
  },
  {
    "id": 2,
    "name": "DevOps",
    "author": "in28minutes"
  }
]
```

> Spring Boot를 사용해 REST API를 만드는 것은 매우 쉽다.
> 다른 것에는 집중할 필요가 없고,
> 비즈니스 로직에만 집중하면 된다.
> Spring beans나 xml을 하나도 설정하지 않아도 된다.
> 
> 어떻게 가능한가? -> 다음 강의

---
## 추가 학습


---
## 다음 강의 노트 : [[054_Spring Boot의 목표 이해하기]]
