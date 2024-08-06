## JUnit vs AssertJ
### 가독성
- JUnit의 `assertEquals(a, b)`메서드는 인자가 두 개라 어떤 값이 실제 값이고, 예상 값인지 알기 어렵다.
- AssertJ의 `assertThat` 메서드를 사용하면 가독성이 좋은 테스트 코드를 작성할 수 있다.
```java
// JUnit5
assertEquals(a, b);

// AssertJ
assertThat(a).isEqualTo(b);
```

### 다양한 검증 메서드
- `usingRecursiveComparison()`
	- 객체의 동일성이 아니라 동등성을 비교할 때 사용하는 메서드
	- 객체의 주소가 아니라 필드를 직접 비교
	- 객체를 참조하는 필드가 있어도, 해당 객체도 재귀적으로 필드를 비교
	- `ignoringFields("filedName")` 로 특정 필드를 제외할 수도 있다.
```java
assertThat(maxDish.get())  
        .usingRecursiveAssertion()
        .ignoringFields("type")
        .isEqualTo(new Dish("pork", false, 800, Dish.Type.MEAT));
```



## 테스트 메서드 실행 순서
- 테스트 메서드는 위에서 아래로 순서대로 실행되지 않는다.
- 실제 _단위 테스트는_ 일반적으로 실행 순서에 의존해서는 안 되지만 특정 테스트 메서드 실행 순서를 적용해야 하는 경우가 있다.
- `@TestMethodOrder` 애노테이션에  `MethodOrderer`의 구현체를 넣어 어떤 순서로 실행할지 정할 수 있다.
- `MethodOrderer` 구현체
	- `MethodOrderer.MethodName.class`
	    - 메서드 이름 순으로 결정
	- `MethodOrderer.DisplayName.class`
		- `@DisplayName` 순서대로 정렬
	- `MethodOrderer.OrderAnnotation.class`
	    - `@Order`에 정의한 순서대로 결정
	- `MethodOrderer.Random`
		- 랜덤으로 결정

```java
@TestMethodOrder(OrderAnnotation.class)  
class OrderedTestsDemo {  
  
    @Test  
    @Order(1)  
    void nullValues() {  
        // perform assertions against null values  
    }  
  
    @Test  
    @Order(2)  
    void emptyValues() {  
        // perform assertions against empty values  
    }  
  
    @Test  
    @Order(3)  
    void validValues() {  
        // perform assertions against valid values  
    }  
  
}
```