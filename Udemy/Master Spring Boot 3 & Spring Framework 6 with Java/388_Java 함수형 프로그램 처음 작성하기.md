---
created: 2024-08-28 16:16
course-name: Master Spring Boot 3 & Spring Framework 6 with Java
instructor: Ranga Karanam
section: 섹션 25 - 부록 - Java 함수형 프로그래밍 소개
presentation: 
tags:
  - lecture-note
  - "#udemy"
---
## 강의 노트
#### 사전 작업
- FP01Structured 파일을 복사하여 FP01Functional 이름으로 생성한다.
- 메서드명을 *printAllNumberInListFunctional* 로 수정한다.
```java
package programming;

public class FP01Functional { // 📌
	public static void main(String[] args){
		
		printAllNumberInListFunctional(List.of(12, 9, 13, 4, 6, 2, 4, 12, 15));  // 📌 
	}
	
	private static void printAllNumberInListFunctional(List<Integer> numbers){ // 📌
		for(int number:numbers){ 
			System.out.println(number);
		}
	}
}
```

#### 함수형 프로그래밍
- 합수형 접근법에서 중요한 것은
	- ==무슨 작업을 수행하느냐== 이다.

- 무슨 작업을 수행하는지 생각하기 전에 
	- 먼저 **숫자 리스트를 숫자 스트림으로 변환**해야 한다.

- `[12, 9, 13, 4, 6, 2, 4, 12, 15]`
- 리스트를 어떻게 스트림으로 바꿀까?

- 숫자 스트림을 만들기 전에 **numbers.stream()** 을 입력한다.
	- 이 컬렉션을 스스로 사용해 순차 스트림을 변환한다고 한다.
		- 즉 ==리스트를 가져다가 숫자 스트림으로 변환==하게 된다.
			- 그래서 개별 숫자가 하나씩 차례대로 나오는 숫자 시퀸스가 된다.

- 숫자 스트림이 준비되고 나면 
	- 각 숫자에 해야할 작업을 지정할 수 있다.
		- 12에는 무슨 작업이 필요한지, 9에는 무슨 작업이 필요한지를 지정하는 것이다.
			- `System.out.println(number)`를 수행해야 한다.

- 이제 매개변수를 사용하여 출력하는 간단한 메서드를 만들 것이다.
	- 단지 숫자 하나를 출력하는 메서드이다.
```java
package programming;

public class FP01Functional { 
	public static void main(String[] args){
		printAllNumberInListFunctional(List.of(12, 9, 13, 4, 6, 2, 4, 12, 15));  
	}
	
	private static void print(int number){ // 📌 숫자를 출력하는 메서드 생성
		System.out.println(number);
	} 
	
	private static void printAllNumberInListFunctional(List<Integer> numbers){ 
		number.stream()
	}
}
```

 - 다음으로 이 numbers.stream에 수행할 작업은 
	 - 이 숫자 각각 12, 9, 13 ... 에 print 메서드를 호출하는 것이다.
		- forEach를 입력하고 여기에 정의하면 된다.

- 예를 들면 숫자 12에 수행할 동작을 정해야하는데 (`각 요소에 동작을 지정하려면`)
	- 그 방법이 **메서드 참조**를 이용하는 것이다.
	- 방법 : ==클래스 이름 뒤에 :: 를 넣고 메서드 이름을 입력하면 된다.==
		- 스태틱 메서드를 사용한다.
```java
package programming;

public class FP01Functional { 
	public static void main(String[] args){
		printAllNumberInListFunctional(List.of(12, 9, 13, 4, 6, 2, 4, 12, 15));  
	}
	
	private static void print(int number){ // 📌 숫자를 출력하는 메서드 생성
		System.out.println(number);
	} 
	
	private static void printAllNumberInListFunctional(List<Integer> numbers){ 
		// 📌 각 요소에 대해 수행할 작업을 지정한다. 
		number.stream() // 📌 number.stream() 
			.forEach(FP01Functional::print); // 📌 메서드 참조 구문
	}
}
```
- 이렇게 작성하면 숫자를 가져다가 스트림으로 변환하고 각 요소에 대해 숫자 출력을 실행하라는 의미가 된다.

- 실행하면
```
12
9
13
4
6
2
4
12
15
```
- 똑같은 결과가 출력된다.


---
## 추가 학습


---
## 다음 강의 노트 : [[]]
