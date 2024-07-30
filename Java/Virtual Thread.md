## Virtual Thread
- JDK 21에 추가된 경량 스레드
- JVM 내부 스케줄링으로 수십만~수백만 개의 스레드를 동시에 사용할 수 있다.
- I/O 와 관련된 작업을 처리할 때 (Blocking 될 때) 이점이 크다.

## 전통적인 Java Thread
- **Platform Thread**: OS Thread 를 Wrapping 한 스레드
- Java 애플리케이션에서 Thread를 사용하면 실제 OS Thread를 사용한 것
- OS Thread는 생성 갯수가 제한적이고, 유지 비용이 비쌈
- 운영 체제가 제공하는 스케줄러를 사용하여 스레드를 관리하며, 우선순위와 스케줄링 정책은 OS에 의해 결정
- 플랫폼 스레드를 효율적으로 사용하기 위해 Thread Pool을 사용

![[../attached-file/Pasted image 20240724185853.png]]
> https://tech.kakaopay.com/post/ro-spring-virtual-thread/
## Thread Pool
![[../attached-file/Pasted image 20240724190158.png]]
> 이미지 출처 : Introduction to Thread Pools in Java . (2023). 
> www.baeldung.com/thread-pool-java-and-guava._  



## Throughput
- 기본적인 Web Request는 **Thread Per Request**
- 처리량을 높이려면 스레드 증가 필요, but 스레드를 무한정 늘릴 수 없다 (OS 스레드 제약)

## Blocking I/O
- Thread에서 I/O 작업 시 Blocking 발생
- 대기시간이 작업시간보다 길다.

## Virtual Thread
- 높은 처리량 확보
	- Blocking 이 발생하면 내부 스케줄링을 통해 다른 작업을 처리하기 때문
- 기존 스레드 구조를 그대로 사용
- 짧은 context switching 시간 (ns 단위)
- 전통적인 스레드가 Stack에 미리 할당된 메모리를 사용하는 것과 달리, **필요시 마다 Heap 메모리 사용**

![[../attached-file/Pasted image 20240724185912.png]]
> https://tech.kakaopay.com/post/ro-spring-virtual-thread/


## 사용법
```java
// Virtual Thread 방법 1
Thread.startVirtualThread(() -> {
    System.out.println("Hello Virtual Thread");
});

// Virtual Thread 방법 2
Runnable runnable = () -> System.out.println("Hi Virtual Thread");
Thread virtualThread1 = Thread.ofVirtual().start(runnable);

// Virtual Thread 이름 지정
Thread.Builder builder = Thread.ofVirtual().name("JVM-Thread");
Thread virtualThread2 = builder.start(runnable);

// 스레드가 Virtual Thread인지 확인하여 출력
System.out.println("Thread is Virtual? " + virtualThread2.isVirtual());

// ExecutorService 사용
try (final ExecutorService executorService = Executors.newVirtualThreadPerTaskExecutor()) {
    for (int i = 0; i < 3; i++) {
        executorService.submit(runnable);
    }
}
```

### Spring Boot 적용법 (3.2 이상)
- was에 대한 처리를 Virtual Thread가 처리하도록 설정
```yaml
# application.yml
spring:
	threads:
		virtual:
			enabled: true
```

## 유의사항
### 리소스라고 생각하지 말고, Task 별로 Virtual Thread 할당
- Virtual Thread는 OS의 자원이 아님
- Platform Thread 하나를 Virtual Thread로 바꾸면 이점이 없음
- 하나의 Task 라고 생각
### Thread Local 사용 시 주의
- Heap 메모리를 사용하기 때문에 Thrad Local 를 남발하면 메모리 사용이 늘어남
> #### Thread Local 이란?
> ThreadLocal은 Java에서 각 스레드가 독립적인 변수 값을 가질 수 있게 하는 방법을 제공합니다. 즉, 여러 스레드가 같은 변수를 사용하더라도 각 스레드마다 해당 변수의 독립적인 복사본을 가지게 되어, 스레드 간의 간섭 없이 변수 값을 사용할 수 있습니다.

### syncronized 사용 시 주의
- `syncronized` 사용 시 Virtual Thread에 연결된 Carrier Thread 가 Blocking 될 수 있음 (pinning)

## 성능 테스트 해석
- I/O Blocking 발생 시 Virtual Thread 가 더 높은 처리량을 보여준다.
- DB Connection을 가져오려다 Timout. 이런 경우를 Overwhelming이라고 부른다.
### Thread Pool 을 사용하는 이유
1. Platform Thread 의 비용이 비싸서
2. Throttle 역할도 수행

## 적합한 사용처
1. I/O Blocking 이 발생하는 경우
2. CPU Intensive 작업에는 적합하지 않음
3. Srping MVC 기반 Web API 제공 시 편리하게 사용 가능
	- 높은 Throughput을 위해 Webflux를 고려중이라면 대안이 될 수 있음

## Virtual Thread 에 대한 오해
1. Virtual Thread는 Platform Thread를 대체하는 것이 목적이 아님
2. Virtual Thread는 기다림에 대한 개선과 플랫폼 디자인과의 조화
3. 도입한다고 무조건 사용량이 늘어나지 않음
4. Virtual Thread 그 자체로 동시성을 완전히 개선했다고 보기는 어려움

## Virtual Thread의 제약
1. Thread Pool 에 적합하지 않음. Task 별로 Virtual Thread를 할당
2. Thread Local 사용 시 메모리 사용량 증가
3. syncronized 사용 시 주의
4. 제한된 리소스가 있을 경우 Semaphore 를 사용


## 참고자료

[JDK 21의 신기능 Virtual Thread 알아보기 (안정수 James)](https://www.youtube.com/watch?v=vQP6Rs-ywlQ)
[Virtual Thread에 봄(Spring)은 왔는가](https://tech.kakaopay.com/post/ro-spring-virtual-thread/)

