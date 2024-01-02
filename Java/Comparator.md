## Comparator
`Comparator`은 `java.util`에 정의되어 있는 비교를 위한 함수형 인터페이스다.
compare라는 추상 메서드를 가지고있어서 값의 정렬이나 대소 비교를 할 때 람다 표현식을 활용해 사용할 수 있다.
compare 외에도 revered와 같은 디폴트 메서드를 제공한다.

## comparing
Comparator의 comparing 메서드를 활용하면 정렬을 보다 쉽게 구현할 수 있다.
```java
inventory.sort((Apple a1, Apple a2) -> 
			   a1.getWeight().compareTo(a2.getWeight()));
```
위 코드는 아래 코드와 동일한 동작을 한다.
```java
inventory.sort(comparing(Apple::getWeight));
```

아래는 comparing 메서드의 내부 구현 코드이다.
메서드 파라미터로 Function<>을 전달받아 compareTo로 값을 비교하고 Comparator 객체를 반환한다.
```java
public static <T, U extends Comparable<? super U>> Comparator<T> comparing(  
        Function<? super T, ? extends U> keyExtractor)  
{  
    Objects.requireNonNull(keyExtractor);  
    return (Comparator<T> & Serializable)  
        (c1, c2) -> keyExtractor.apply(c1).compareTo(keyExtractor.apply(c2));  
}
```

## 역정렬
Comparator은 reversed 라는 디폴트 메서드를 제공한다. 따라서 역정렬을 따로 구현할 필요없다.
reversed 메서드는 역정렬한 Comparator을 반환한다.
```java
inventory.sort(comparing(Apple::getWeight).reversed());
```

## Comparator 연결
객체를 비교할 때 첫번째 비교자가 같을 경우 `thenComparing` 메서드를 활용할 두번째 Comparator을 만들 수 있다.
```java
inventory.sort(comparing(Apple::getWeight).thenComparing(Apple::getColor)));
```