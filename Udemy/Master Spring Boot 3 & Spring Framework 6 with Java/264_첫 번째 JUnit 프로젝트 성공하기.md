---
created: 2025-02-10 20:24
course-name: Master Spring Boot 3 & Spring Framework 6 with Java
instructor: Ranga Karanam
section: 섹션 14 - JUnit으로 단위 테스트하기
presentation: 
tags:
  - lecture-note
  - "#udemy"
last_modified: 2025-02-10T20:59:00
---
### 단위 테스트 대상인 `MyMath` 클래스 생성
- `/main/java/junit_5_in_steps` 패키지에 `MyMath` 클래스 생성
```java
public class MyMath {
    // {1, 2, 3} => 1+2+3 = 6
    public int calculateSum(int[] numbers){

        int sum = 0;

        for(int number: numbers){
            sum += number;
        }

        return sum;
    }
}
```
---
### 단위 테스트 작성 (`MyMathTest` 클래스 생성)
> 중요한 것은 단위 테스트를 같은 폴더에 작성하지 않는 것이다.
> 모든 소스 코드는 `src` 폴더에 있으며, 테스트나 단위 테스트는 `test` 폴더에 있어야 한다.
- `/test/java/junit_5_in_steps` 패키지에 `MyMathTest` 클래스 생성
```java
// 자동 완성 기능을 찾지못하여 직접 작성.

package junit_5_in_steps;  
  
import org.junit.jupiter.api.Test;  
import static org.junit.jupiter.api.Assertions.*;  
  
class MyMathTest {  
	@Test  
	void test() {  
		fail("Not yet implemented");  
	}  
}
```
- `fail`은 `assertion`이라고 불린다.
	- 테스트에 통과하지 못하고 `Not yet implemented`를 띄운다.
	- 이대로 실행하면 실패하고 `org.opentest4j.AssertionFailedError: Not yet implemented`를 확인할 수 있다.
	- 실패하지 않으려면 `fail("Not yet implemented");`를 지우면 된다.

> JUnit에 관한 첫번째 교훈: 실패가 없으면 통과한다.
#### 테스트 조건(Assert) 작성하기
> 테스트에서 할 일은 여러 조건을 작성하는 것이다. (`Test Condition` or `Assert`)
> Assert는 특정 동작에 대한 assert 검사를 진행하고, 
	- 이 assert 중 하나라도 실패하면 단위 테스트에 통과하지 못한 것이다.

- 먼저 MyMath 인스턴스를 생성해야 한다.
```java
class MyMathTest {
    @Test
    void test() {
        int numbers[] = {1, 2, 3};
        MyMath math = new MyMath();
        int result = math.calculateSum(numbers);
        System.out.println(result); // 실행하면 6이 출력되며, 테스트에 통과한다.
    }
}
```

> 이제 여기서 할 일은 예상치와 비교하여 결과를 확인하는 것이다.
	- 이 메서드에서 반환되는 결과를 예상치인 6과 비교하여 확인해야 한다.

- `assert`의 메서드가 필요하다. 
	- JUnit 프레임워크의 일부로 제공되는 `assertEquals` 메서드를 사용한다.
	- `assertEquals`에 정수 두개를 쉼표로 나열하면 된다.
```java
class MyMathTest {

    @Test
    void test() {
 
        int numbers[] = {1, 2, 3};
        MyMath math = new MyMath();
        int result = math.calculateSum(numbers);
        System.out.println(result);

        int expectedResult = 6; // 예상치 6
        assertEquals(expectedResult, result); // 📌 expectedResult가 6일 때: Tests passed 문구를 확인할 수 있다.
    
	    
    }
}
```
- `int expectedResult = 5`로 수정하여 실행한다면 `Test failed` 문구와 함께, 아래와 같은 결과가 출력된다.
```java
6 

org.opentest4j.AssertionFailedError: 
Expected :5
Actual   :6
```
---
## 다음 강의 노트 : [[265_첫 코드에서 첫 단위테스트 수행하기]]
