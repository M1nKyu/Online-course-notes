---
created: 2024-08-18 23:28
course-name: Master Spring Boot 3 & Spring Framework 6 with Java
instructor: Ranga Karanam
section: 섹션 03 - Spring Framework를 사용하여 Java 객체를 생성하고 관리하기
presentation: 
tags:
  - lecture-note
  - "#udemy"
---
## 강의 노트
#### 이번 강의에서
- @Primary와 @Qualifier 를 비교하며 자세히 살펴볼 것이다
- ==이 중 어떤 것을 사용해야 할까?==

###### 예시: 복잡한 알고리즘을 작성한다고 가정할 때
- 여러 Sort 알고리즘을 사용하려고 한다.
- 그래서 *SortingAlgorithm 인터페이스*를 생성하였다.
- 구현한 알고리즘은 *QuickSort, BubbleSort, RadixSort* 
```java
@Component @Primary
class QuickSort implement SortingAlgorith {}

@Component
class BubbleSort implement SortingAlgorithm {}

@Component @Qualifier("RadixSortQualifier")
class RadixSort implement SortingAlgorithm {}

@Component
class ComplexAlgorithm
	@Autowired
	private SortingAlgorithm algorithm;

@Component
class AnotherComplexAlgorithm
	@Autowired @Qualifier("RadixSortQualifier")
	private SortingAlgorithm iWantToUseRadixSortOnly;
```
- ComplexAlgorithm이 컴포넌트라는 것이 확인되고 SortingAlgorithm을 Autowire 해야 한다.
	- AnotherComplexAlgorithm도 마찬가지로 SortingAlgorithm 써야한다.

###### 그렇다면, @Primary와 @Qualifier를 사용해야 하는 경우는 각각 언제인가
- @Primary는 
	- 여러 후보가 자격이 있을 때, 한 Bean에 우선권을 주는 것이다.
- @Qualifier는
	- 특정 Bean이 Auto-wired 되어야 한다.

###### 위 코드에서
- RadixSort에 @Qualifier가 RadixSortQualifier 이름으로 지정되어 있다.
- AnotherComplexAlgorithm도 마찬가지이다.

- AnotherComplexAlgorithm은 RadixSort만 사용하려고 한다.
- ComplexAlgorithm은 SortingAlgorithm을 사용하는데, 가장 높은 우선순위를 달라고 작성했다.

- @Primary와 @Qualifier 중 선택할 때는 
	- 항상 **특정 의존성을 사용하는 클래스의 관점에서 생각**해야 한다.
		- 따라서, ComplexAlgorithm과 AnotherComplexAlogorithm의 관점에서 생각해야 한다.

###### ComplexAlgorithm은 @Autowired만 사용하고 있고, @Qualifier는 사용하지 않는다
- ==@Autowired만 사용한다면 가장 적합한 SortingAlgorithm을 요청하는 것이다.==
- 어떤 SortingAlgorithm이든 상관없다는 것이다.

###### 하지만 @Qualifier를 사용한다면 
- ==기본적으로 @Autowiried와 @Qualifier를 같이 사용한다.==
- 특정 SortingAlgorithm만 사용한다고 명시하였다.
- **@Qualifier가 @Primary보다 더 높은 우선순위를 가진다.**

- AnotherComplexAlgorithm은 RadixSort로 Auto-wiring된다.

#### 요약하면
- @Primary와 @Qualifier 중 결정할 때
	- 의존성을 사용하는 클래스의 관점에서 생각해야 한다.
- 어떤 SortingAlgorithm을 사용해도 괜찮다면,
	- @Autowired를 사용한다.
- 특정 SortingAlgorithm을 사용해야 하면
	- @Qualifier를 사용한다.
- `@Qualifier("RadixSortQualifier")`와 같이 어노테이션이 없을 때,
	- **Bean의 이름을 한정자로 사용**하여 Auto-wiring 할 수 있다.
		- 예를 들어, `@Qualifier("radixSort");`
---
## 다음 강의 노트 : [[028_Spring Framework 알아보기-의존성 주입의 다양한 유형]]
