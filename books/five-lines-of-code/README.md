# 파이브 라인스 오브 코드 정리
## 리팩터링
> 우선 변경하기 쉽게 만든 후 변경하라  - 켄트 벡 -
- 리팩터링: 기능을 변경하지 않고 코드를 변경하는 것
### 안전한 리팩터링 방법
- 애플리케이션을 정기적으로 실행해서 손상되지 않았는지 검증
- 검증할 때마다 문제가 없으면 커밋
- 애플리케이션이 손상되면 마지막 커밋으로 돌아감
### 리팩터링의 핵심
1. 의도를 전달함으로써 가독성 향상
2. 불변속성의 범위제한을 통한 유지보수성 향상
3. 범위 밖의 코드에 영향을 주지 않고 1항과 2항을 수행
### 속도, 유연성 및 안정성 확보
- 상속보다는 콤포지션(객체 참조)을 사용
- 수정이 아니라 추가로 코드를 변경 (OCP)


## 규칙
### 1. 다섯 줄 제한
#### 정의
메서드는 '{'와 '}'를 제외하고 5줄 이상이 되어서는 안 된다.
#### 참조
[[#메서드 추출]]

### 2. 호출 또는 전달, 한 가지만 할 것
#### 정의
함수 내에서는 **객체에 있는 메서드를 호출**하거나 **객체를 매개변수로 전달**할 수 있지만 **둘을 섞어 사용해서는 안된다**.
#### 설명
함수의 내용은 **동일한 추상화 수준**에 있어야 한다. 변수 옆의 '.'(점)의 개수로 추상화 수준을 알 수 있다.
추상화 수준이 다르다면 메서드 추출을 통해 리팩터링한다.
#### 예시
```java
// arr를 매개변수로 전달하기도 하고, arr.length를 호출하기도 함
int average(int[] arr) {
	return sum(arr) / arr.length;
}
```
```java
// 배열 길이 구하는 코드를 추출해 추상화 수준을 동일하게 맞춤
iint average(int[] arr) {
	return sum(arr) / size(arr);
} 
```

### 3. if 문은 함수의 시작에만 배치
#### 정의
if 문이 있는 경우 if 문은 함수의 첫 번째 항목이어야 한다.

#### 설명
함수는 한 가지 일만 해야한다. **무언가를 확인하는 일**도 한 가지 일이다.
따라서 함수에 if 문이 있는 경우 함수의 첫 번째 항목이어야 한다. 또한 그 이후에는 아무것도 해서는 안 된다.

if 문과 이어지는 else-if는 분리할 수 없는 원자 단위로 본다.

#### 예시
```java
// 반복과 소수 확인 2가지 일을 하고있다.
void someFunction() {
	for (int i = 2; i < n; i++) {
		if (isPrime(n))
			system.out.println(n + "은 소수입니다.");
	}
}
```
```java
// 반복만 한다.
void reportPrime() {
	for (int i = 2; i < n; i++) {
		reportIfPrime(i);
	}
}

// 소수 확인만 한다.
void reportIfPrime(int n) {
	if (isPrime(n))
		system.out.println(n + "은 소수입니다.");
}
```

### 4. if문에서 else를 사용하지 말 것
#### 정의
프로그램에서 이해하지 못하는 타입인지를 검사하지 않는 이상 if 문에서 else를 사용하지 말아야 한다.

#### 설명
if-else는 결정이 내려지는 지점을 고정한다. 그럴 경우 if-else 이후 위치에서는 변형을 도입할 수 없기 때문에 코드의 유연성이 떨어진다.
if-else가 반드시 필요한 경우는 제어 불가능한 타입일 때이다. 이때는 외부의 데이터 타입을 내부에서 제어 가능한 데이터 타입으로 매핑해야 한다.

독립된 if문은 **검사**(check)로 간주하고, if-else문은 **의사결정**(decision)으로 간주한다.
#### 예시
```typescript
// e라는 외부 데이터를 내부에서 제어 가능한 데이터 타입으로 매핑
// 이런 경우를 제외하면 if-else문을 사용하면 안됨
window.addEventListener("keydown", e => {  
  if (e.key === LEFT_KEY || e.key === "a") inputs.push(Input.LEFT);  
  else if (e.key === UP_KEY || e.key === "w") inputs.push(Input.UP);  
  else if (e.key === RIGHT_KEY || e.key === "d") inputs.push(Input.RIGHT);  
  else if (e.key === DOWN_KEY || e.key === "s") inputs.push(Input.DOWN);  
});
```
#### 의도
if는 조건 연산자로 흐름을 제어한다. 그러나 객체지향 프로그래밍에는 객체라는 더 강력한 제어 흐름 연산자가 있다. 인터페이스를 사용한 두 가지 다른 구현이 있는 경우 **인스턴스화하는 클래스에 따라 실행할 코드를 결정**할 수 있다.
#### 참조
[[#클래스로 타입 코드 대체]], [[#전략 패턴의 도입]]



## 리팩터링 패턴
### 메서드 추출
- 한 메서드의 일부를 취해서 자체 메서드로 추출한다.
- 여러 매개변수가 필요하거나, 여러 조건에서 전체가 아닌 일부 조건에서만 리턴할 경우 복잡해질 수 있다.
- if의 일부 분기만 return문을 가지고 있을 경우 메서드의 끝에서 시작해 위로 작업하는게 좋다.

### 클래스로 타입 코드 대체
#### 설명
- 열거형을 인터페이스로 변환하고, 열거형의 값들은 클래스가 된다.
- [[#클래스로 코드 이관]]과 함께 사용해서 **추가를 통한 변경**으로 이어진다.
- 모든 값에 대해 메서드를 갖는 것도 하나의 스멜이다. 이후 is로 시작하는 메서드를 줄일 수 있다.
```typescript
// 리팩터링 전
enum Input {
	Left, Right, Up, Down
}
```
```typescript
// 리팩터링 후
interface Input {
	isRight(): boolean;  
	isLeft(): boolean;  
	isUp(): boolean;  
	isDown(): boolean;
}
class Right { ... }
class Left { ... }
class Up { ... }
class Down { ... }
```


### 클래스로 코드 이관
- [[#클래스로 타입 코드 대체]]의 연장선에 있다.
- if-else의 모든 조건이 특정 클래스와 연관이 있다면, 이것은 코드가 해당 클래스에 있어야 함을 의미한다.
- 함수를 객체 메서드로 이관함으로써 조건문 대신 인스턴스마다 어떤 행동을 할지 스스로 결정하게 한다.
- 아래와 같이 리팩터링할 경우 is 메서드도 더 이상 필요없다.
```typescript
// 모든 조건이 input과 관련이 있다. 즉, 코드가 Input 존재해야 한다!!
function handleInput(input: Input) {  
  if (input.isLeft())  
    moveHorizontal(-1);  
  else if (input.isRight())  
    moveHorizontal(1);  
  else if (input.isUp())  
    moveVertical(-1);  
  else if (input.isDown())  
    moveVertical(1);  
}
```
```typescript
// handleInput 함수를 Input 인터페이스의 메서드로 이관하고, 각 클래스에서 메서드 구현
interface Input {
	...
	handle(): void;
}
class Right implements Input{  
  handle() {  
      moveHorizontal(1);
  }  
}
```


### 메서드의 인라인화
#### 설명
- 더 이상 가독성에 도움이 되지 않는 코드를 제거한다.
- 메서드에서 자신을 호출하는 모든 곳으로 코드를 옮긴다. 이러면 더 이상 메서드가 사용되지 않는다.
- 메서드가 짧을 경우 이 리팩터링 패턴을 고려해본다.
- 메서드가 복잡할 경우 (낮은 수준의 연산, 가독성이 안 좋을 경우 등) 인라인화를 하지 않는 것이 좋다.

### 전략 패턴의 도입



## 좋은 함수 이름의 속성
- 함수 의도를 설명해야 한다.
- 함수가 하는 모든 것을 담아야 한다.
- 도메인에서 일하는 사람이 이해할 수 있어야 한다.

