---
created: 2024-08-07 00:46
course-name: Master Spring Boot 3 & Spring Framework 6 with Java
instructor: Ranga Karanam
section: 섹션 02 -Java Spring Framework 시작하기
presentation: 
tags:
  - lecture-note
  - "#udemy"
---
## 강의 노트
> Java Bean, POJO, Spring Bean의 차이가 무엇일까

#### POJO는 일반적인 오래된 Java 객체라는 의미이다.
- **모든 Java 객체는 POJO** 이다.
- 지금까지 강의에서 만든 Bean은 POJO이다. (일반적인 오래된 Java 객체이다.)

#### 클래스의 인스턴스를 Java Bean이라고 부를 때는 세가지 중요한 제한이 따른다.
1. **인수를 가진 생성자가 없다**
2. **게터와 세터가 존재**해야 한다.
3. **Serializable 인터페이스를 구현**한다.
```java
// EJB 
class JavaBean implements Serializable { // 📌 3. Serializable 인테페이스 구현
	// 📌 1. 인수를 가진 생성자 없ㅇ음
	public JavaBean() {} // 기본생성자이므로 굳이 명시할 필요는 없다.
	
	private String text;
	private in number;
	
	// 📌 2. 게터, 세터 존재
	public String getText() { return text;}
	public void setText(String text) {this.text = text;}
	
	public String getNumber() {return number;}
	public void setString(String number) {this.number = number;}
}
```

> Java Bean의 개념은 더 이상 중요하지 않다.
> 	엔터프라이즈 Java Bean을 사용하는 사람이 많지 않음

> 염두에 두어야할 개념은 POJO
> 	즉, 만들고 있는 거의 모든 클래스와 Spring Bean이다.

> Spring Framework에서 관리하는 모든 것은 Spring Bean이 될 수 있다.
---
#### 📌 비교해보자면
###### Java Bean은 
- **세 가지 제약을 준수하는 클래스일 뿐**이다.
###### POJO (Plain Old Java Object)는
- 아무 제약 없는 일반적인 오래된 Java 객체이다
- ==모든 Java 객체는 POJO==이다.
###### Spring Bean은
- ==Spring이 관리하는== 모든 Java 객체이다.
- Spring 사용자는 컨테이너, Bean Factory 또는 *Application Context*를 사용하여 객체를 관리한다.
- ==IOC Container가 관리하는 모든 객체==는 Spring Bean이다.

---
## 다음 강의 노트 : [[020_Spring Framework Bean 자동 연결 살펴보기 - 기본 및 한정자]]
