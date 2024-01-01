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
