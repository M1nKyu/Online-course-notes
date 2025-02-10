---
created: 2024-08-27 17:25
course-name: Master Spring Boot 3 & Spring Framework 6 with Java
instructor: Ranga Karanam
section: 섹션 25 - 부록 - Java 함수형 프로그래밍 소개
presentation: 
tags:
  - lecture-note
  - "#udemy"
---
## 강의 노트
- ==함수형 프로그래밍은 패러다임의 전환이다.==
#### 패러다임 전환은
- 함수형 프로그래밍을 마스터하려면 **문제 해결에 관한 사고 방식을 바꿔야 한다**는 의미이다.

- 이 강의는 그런 사고 방식에 도움을 줄 것이다.

#### 강의를 통해
- 함수형 프로그래밍을 단계적으로 설명할 것이며
	- 첫 번째 섹션에서 다룰 주요 목표는 두 가지이다.
		1. 함수형 프로그래밍의 차이를 이해하는 것
		2. 함수형 프로그래밍의 몇 가지 개념을 이해하는 것

- 람다식에 대해 알아보고 메서드 참조라는 것도 알아볼 것이다.

- 전통적인 방식으로 예제를 작성한 후
	- 코드를 함수형 프로그래밍으로 작성하는 방법을 살펴 볼 것이다. 

#### 사전 작업
- 자바 버전은 9 이상이면 좋다.
- 패키지를 생성한다 (*functional-programming-with-java*)

#### 예제 1: 숫자 집합 출력하기
> 숫자 리스트를 만들고, 이 리스트에 있는 각 요소를 한 라인에 하나씩 출력한다.
###### 클래스 생성
- src.programming 패키지에
	- FP01Structured 클래스 생성

###### 전통적 접근
```java
package programming;

public class FP01Structured {
	public static void main(String[] args){
		// 전통적인 구조적 방법
		// 숫자 리스트를 생성하기 위헤 List.of()를 입력하고 숫자 시퀸스를 입력한다 (Java.util.List 임포트 필요)
		printAllNumberInListStructured(List.of(12, 9, 13, 4, 6, 2, 4, 12, 15)); 
	}
	
	// 메소드를 생성한다.
	private static void printAllNumberInListStructured(List<Integer> numbers){
		// 어떻게 출력할 수 있을까?
		// 첫 번째로 생각해야 할 것은 루프 방법의 결정이다. 루프 방법을 결정하고 개별적인 숫자를 각각 출력할 것이다.
		// 전통적 for루프와 향상된 for루프 중에서 선택해야 한다.
		for(int number:numbers){ // 향상된 for 루프 선택
			System.out.println(number);
		}
	}
}
```
- 위와 같이 전통적인 접근법에서는 문제를 해결할 때마다  방법에 집중한다.
	- 가장 처음에 한 일이 숫자 리스트에 어떻게 루프를 적용할 지를 선택하는 것이다.

- 함수형 접근법에서는 같은 문제를 어떻게 해결할까? (다음 강의)



---
## 추가 학습


---
## 다음 강의 노트 : [[388_Java 함수형 프로그램 처음 작성하기]]
