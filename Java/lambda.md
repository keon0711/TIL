# 람다 표현식
   1. [람다 표현식이란?](#람다-표현식이란)
   2. [메소드를 전달하는 방식의 발전](#메소드를-전달하는-방식의-발전)
      1. [1.인터페이스와 익명 클래스 활용](#1인터페이스와-익명-클래스-활용)
      2. [2.람다 함수와 Predicate(2) 인터페이스 활용](#2람다-함수와-predicate2-인터페이스-활용)
      3. [3. 메소드 레퍼런스(3)와 스트림을 활용](#3-메소드-레퍼런스3와-스트림을-활용)
   3. [각주](#각주)
   4. [Reference](#reference)


## 람다 표현식이란?
자바에서 람다 표현식은 메서드를 간결하게 표현하기 위한 방법으로, 익명 함수를 만들어 사용할 수 있다.  
람다를 사용하면 함수를 변수처럼 다루거나, 파라미터로 전달하는 것처럼[(1)](#참조) 쓸 수 있다.  
다음과 같이 '->'를 사용해 표현한다.
```java
(parameter1, parameter2, ...) -> { 코드 실행 };
```

<br>

## 메소드를 전달하는 방식의 발전
### 1.인터페이스와 익명 클래스 활용
```java
private List<Fruit> filterFruits(List<Fruit> fruits, FruitFilter fruitFilter) {
    List<Fruit> results = new ArrayList();
    for (Fruit fruit : fruits) {
        if (fruitFilter.isSelected(fruit)) {
            results.add(fruit);
        }
    }
    return results;
}
```
익명 클래스를 통해 메소드를 전달했지만, 익명 클래스는 사용이 번거로움  
<br>

### 2.람다 함수와 Predicate[(2)](#각주) 인터페이스 활용
```java
filterFruits(fruits, fruit -> fruit.getName().equals("사과"));
```
```java
private List<Fruit> filterFruits(List<Fruit> fruits, Predicate<Fruit> fruitFilter) {
    List<Fruit> results = new ArrayList();
    for (Fruit fruit : fruits) {
        if (fruitFilter.test(fruit)) {
            results.add(fruit);
        }
    }
    return results;
```
<br>

### 3. 메소드 레퍼런스[(3)](#각주)와 스트림을 활용
```java
filterFruits(fruits, Fruit::isApple);
```
```java
private List<Fruit> filterFruits(List<Fruit> fruits, Predicate<Fruit> fruitFilter) {
    return fruits.stream().
        .filter(fruitFilter)
        .collect(Collectors.toList());
```

<br>

## 각주
(1) 자바에서 함수는 변수로 할당되거나, 파라미터로 전달할 수 없다. (2급 시민)

(2) Predicate 인터페이스는 함수형 인터페이스로, 하나의 인수를 받아 boolean 값을 반환하는 추상 메소드 **test()** 를 가지고 있다.  
Predicate는 다음과 같이 작성할 수 있다.
```java
Predicate<String> isEmpty = (str) -> str.isEmpty();
if (isEmpty.test(someString))
    ...
```

(3) 메소드 레퍼런스는 메소드를 참조하는 기능으로 '::' 연산자를 사용해 표현한다. static 메소드, 인스턴스 메소드, 생성자 등을 참조할 수 있다.
```java
클래스명::메소드명
```

## Reference
[[인프런] 자바 개발자를 위한 코틀린 입문 - 최태성님](https://www.inflearn.com/course/java-to-kotlin)
