Map을 사용하다보면 key나 value를 기준으로 정렬하거나 최대, 최소를 찾아야하는 경우가 있다.

## Value를 기준으로 최대 값 찾기
`Map.Entry`에 `comparingByKey()`, `comparingByValue()` 메서드가 존재한다. 이 메서드들은 각각 키와 값을 기준으로 정렬하는 Comparator을 반환한다.
각 메서드에 람다 표현식으로 정렬 기준을 정해줄 수도 있다.
```java
Collections.max(map.entrySet(), Map.Entry.comparingByValue()).getKey();
```
