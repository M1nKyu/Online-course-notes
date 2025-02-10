---
created: 2024-08-22 17:32
course-name: Master Spring Boot 3 & Spring Framework 6 with Java
instructor: Ranga Karanam
section: 섹션 03 - Spring Framework를 사용하여 Java 객체를 생성하고 관리하기
presentation: 
tags:
  - lecture-note
  - "#udemy"
---
## 강의 노트
#### 왜 애플리케이션은 많은 의존성을 가지는가?
- 이전까지 만들어본 것들은 매우 적은 클래스를 가지지만,
	- ==Real World Applications 는 매우 복잡==하다.
###### 애플리케이션에는 Multiple Layers가 있다.
- **Web** <- **Business** <- **Data** 
	- 각 레이어는 ==아래에 있는 레이어에 의존==한다.
		- Web Layer의 클래스는 Business Layer 클래스에게 말을 걸 수 있다. 
			- 따라서 Business Layer 클래스는 Web Layer 클래스의 의존성이다.
		- Business Layer의 클래스가 Data Layer 클래스에게 말을 걸 수 있다.
			- 따라서 Data Layer 클래스는 Business Layer 클래스의 의존성이다.

- 모든 애플리케이션에는 이러한 의존성이 수천개가 있는데,
	- Spring Framework를 사용하면
		- 객체가 아닌, 의존성과 Wiring에 집중할 수 있다.
			- 애플리케이션의 비즈니스 로직에 집중할 수 있다.

- Spring Framework는 ==객체의 생명주기를 관리==한다.
	- 우리는 이 Framework를 사용하여 
		- *@Component* 어노테이션을 사용하여 컴포넌트를 표시하고
		- *@Autowired* 어노테이션을 사용하여 의존성을 표시하기만 하면 된다.

- Spring Framework는 객체를 생성, 의존성을 Autowiring 하고, 전체 시스템의 준비를 수행한다.

#### 예제 - BusinessCalculationService
- 우리는 MongoDb와 MySQL에 데이터를 갖고 있다.
	- 하나의 DB에서 다른 DB로 유연하게 이동할 수 있어야 한다.
		- ==따라서, DataService 인터페이스를 생성해야 한다.==
```java
public interface DataService // 📌 DataService는 retrieveData라는 메소드를 포함하고 있다.
	int[] retrieveData();

public class MongoDbDataService implements DataService // 📌 구현 1
	public int[] retrieveData()
		return new int[] { 11, 22, 33, 44, 55 };

public class MySQLDataService implements DataService // 📌 구현 2
	public int[] retrieveData()
		return new int[] { 1, 2, 3, 4, 5 };

public class BusinessCalculationService
	public int findMax() // 📌 findMax 메소드는 반환되는 데이터의 최댓값을 찾아준다.
		return Arrays.stream(dataService.retrieveData()) // 📌최댓값을 구하기 위해 함수형 프로그래밍을 사용한다
						.max().orElse(0); 
```
- ==BusinessCalculationService가 DataService에게 말을 걸어야 하며==
	- 이에 대한 비즈니스 로직을 수행해야 한다.

- findMax 메소드는 반환되는 데이터의 최댓값을 찾아준다.
	- 예를 들어 MongoDbDataService 메소드의 반환되는 데이터가 11, 22, 33, 44, 55라고 하면
		- findMax 메소드는 11, 22, 33, 44, 55 중 55를 최댓값으로 구할 것이다.


###### 연습해보기
- 생성자 주입을 사용하여 의존성 주입하기
- MongoDbDataService에 primary를 사용하여 우선권 부여하기
	- `주어진 2개의 컴포넌트 중 MongoDbDataService가 우선권을 가질 것이다.`
- Spring Context를 만들기
	- 어노테이션을 사용하는 것을 선호한다.
	- 이 Spring Context를 시작한 후
		- BusinessCalculationService를 Spring Context에서 검색하고, findMax 메소드 실행하기

---
## 추가 학습


---
## 다음 강의 노트 : [[032_예제 Real World Java Spring Framework Example 솔루션]]
