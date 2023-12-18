## 기존의 null 처리
- 자바에서 NullPointerException을 줄이기 위해 null 확인 코드를 추가해야 한다.
- 그러나 null 확인에 의해 코드의 들여쓰기 수준이 증가하거나, 너무 많은 출구가 생겨 유지보수가 어려워진다.

## null에 의해 발생하는 문제
- 에러의 근원
- 코드를 어지럽힘
- 아무런 의미가 없음
- 자바 철학에 위배됨 (자바는 개발자로부터 모든 포인터를 숨김. null 포인터만이 예외)
- 형식 시스템에 구멍을 만듬 (모든 참조 형식에 null을 할당할 수 있기 때문)

## Optional 클래스
- 자바 8은 `java.util.Optional<T>`이라는 새로운 클래스를 제공한다.
- Optional은 선택형 값을 캡슐화하는 클래스
- 사람이 차를 소유하고 있지 않다면, Person 클래스의 car 변수가 null을 가져야한다. 그러나 Optional을 사용한다면 `Optional<car>`을 가지도록 할 수 있다.
- 값이 없을 경우 `Optional.empty` 메서드로 Optional을 반환한다.
```java
public class Person {
	// 사람이 차를 소유할 수도, 소유하지 않을 수도 있으므로 Optional로 정의한다.
	private Optional<Car> car;
	public Optional<Car> getCar() { return car; }
}

public class Car {
	// 자동차가 보함에 가입되어 있을 수도, 가입되어 있지 않을 수도 있으므로 Optional로 정의한다.
	private Optional<Insurance> insurance;
	public Optional<Insurance> getInsurance() { return insurance; }
}

public class Insurance {
	private String name;
	public String getName() { return name; }
}
```
- 모든 null을 Optional로 대치해야하는 것은 아님
- Optional은 메서드의 시그니처만 보고 선택형값인지 여부를 구별할 수 있도록 한다.

## Optional 적용 패턴
### Optional 객체 생성
#### 빈 Optional
`Optional<Car> optCar = Optional.empty();`
#### null이 아닌 값으로 Optional 만들기
`Optional<Car> optCar = Optional.of(car)`
- 정적 팩토리 메서드 `Optinal.of`로 null이 아닌 값을 포함하는 Optional을 만들 수 있다.
- 만약 car이 null이라면 즉시 nullPointerException이 발생한다. (기존에는 car 프로퍼티에 접근하려고할 때 에러가 발생)
#### null값으로 Optional 만들기
`Optional<Car> optCar = Optional.ofNullable(car)`
- `Optional.ofNullable`로 null값을 저장할 수 있는 Optional을 만들 수 있다.
- car이 null이면 빈 Optional 객체가 반환됨

### 맵으로 Optional 값 추출&변환
```java
Optional<Insurance> optInsurance = Optional.foNullable(insurance);
Optional<String> name = optInsurance.map(Insrance::getName);
```
- Optional의 map 메서드는 스트림의 map 메서드와 개념적으로 비슷하다. Optional 객체를 최대 요소 개수가 1개 이하인 데이터 컬렉션으로 생각할 수 있다.
- Optional이 값을 포함하면 map의 인수로 제공된 함수가 값을 바꾼다. Optional이 비어있으면 아무 일도 일어나지 않는다.
- 위 코드에서 map 메서드를 실행하면 `Optional<Insurance> -> Optinal<String>`
### flatMap으로 Optional 객체 연결
- `Optional<Optional<Car>>` 같은 형식이 되는 경우를 위해 flatMap 메서드가 존재한다
- flatMap 메서드로 전달된 함수의 결과를 Optional 객체 감싸는 것이 아니라, Optional 객체의 값을 Optional 객체로 감싼다.
- 즉, `getInsurance`메서드가 `Optional<Insurance>`를 반환함에도 `optCar.flatMap(Car::getInsurance)`의 결과는 `Optional<Optional<Insurance>>`가 아니라 `Optional<Insurance>`가 된다.

## Optional 언랩
- `get()`은 가장 간단하면서, 가장 안전하지 않은 메서드이다. 래핑된 값이 없을 경우 `NoSuchElementException`을 발생시킨다. 따라서 Optional에 값이 반드시 존재한다고 생각할 때만 사용이 권장된다. 결국 중첩된 null 확인 코드를 넣는 상황과 크게 다르지 않다.
- `orElse(T other)`을 사용하면 Optional이 값을 가지지 않을 때 기본값을 제공할 수 있다.
