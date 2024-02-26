# CompletableFuture
- `CompletableFuture`는 자바8에서 제공하는 `Future` 인터페이스의 구현체이다.
- `CompletableFuture`의 `join` 메서드는 `Future`의 `get`과 같다. 그러나 예외를 발생시키지 않는다.
## Future
- `Future`는 비동기 계산을 위해 계산이 끝났을 때 결과에 접근할 수 있는 참조를 제공한다. (`get 메서드`)
- 메서드의 반환값을 `Future`로 하면, 메서드는 즉시 `Future`을 반환하고, 이후에 계산이 완료되면 반환받은 `Future` 객체의 `get` 메서드로 결과를 얻을 수 있다.

## CompletableFuture 만들기
### 객체를 직접 생성
```java
public Future<Double> getPriceAsync(String product) {
	CopletableFuture<Double> futurePrice = new CompletableFuture<>();
	new Thread( () -> {
		double price = calculatePrice(product);
		futurePrice.complete(price); // 오래 걸리는 계산이 완료되면 Future에 값을 설정
	}).start();

	return futurePrice;  //계산이 완료되길 기다리지 않고 Future을 반환
}
```

### 팩터리 메서드 사용 (supplyAsync)
- **supplyAsync** 메서드는 Supplier를 인수로 받아 **CompletableFuture**을 반환한다.
- **CompletableFuture**는 Supplier를 실행해서 비동기적으로 결과를 생성
	- 두번째 인수로 Executor를 선택적으로 전달할 수 있다.
```java
public Future<Double> getPriceAsync(String product) {
	return CompleatableFuture.supplyAsync(() -> caclucatePrice(product));
}
```

