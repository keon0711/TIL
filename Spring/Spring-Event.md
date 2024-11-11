## Spring Event
• Spring Event는 모듈 간 결합도를 낮추는 데 유용하다.
• Event 객체 외에는 public으로 선언할 필요가 없다.
• Event가 애플리케이션 간의 API 역할을 한다.
• Java 클래스의 기본 접근 제한자는 package-private이라 모듈 간 결합도를 줄이는 데 도움이 된다.
• Event를 통해 객체지향적인 설계를 할 수 있어 객체 간의 독립성을 높일 수 있다.

### Spring Event 사용 예시
```java
// Event 정의
public record OrderCreatedEvent (String orderId) {}

// Event Publisher
@Service
class OrderService {
    private final ApplicationEventPublisher eventPublisher;
    public OrderService(ApplicationEventPublisher eventPublisher) {
        this.eventPublisher = eventPublisher;
    }
    public void placeOrder(String orderId) {
        // 주문 처리 로직
        eventPublisher.publishEvent(new OrderCreatedEvent(orderId));
    }
}

// Event Listener
@Component
class NotificationListener {
    @EventListener
    public void onOrderCreated(OrderCreatedEvent event) {
        System.out.println("Order created with ID: " + event.getOrderId());
    }
}
```

  

## @ApplicationModuleListener (Spring Modulith)

• `@ApplicationModuleListener`는 `Spring Modulith`에서 제공하며, `@Async`, `@EventListener`, `@Transactional`의 기능을 결합한 어노테이션이다.
• **비동기 처리**를 통해 이벤트가 별도의 스레드에서 실행되며, 트랜잭션 완료 후에만 이벤트 리스너가 호출되므로 데이터의 일관성을 유지한다.
• 애플리케이션이 중단되더라도 재시작 시 이벤트 처리를 이어받을 수 있어 안정성을 높인다.
• `Spring Modulith`는 **이벤트 다이어그램을 자동 생성**하여 모듈 간 이벤트 흐름을 시각적으로 파악할 수 있게 해준다.
• 다른 모듈이 public 클래스에 의존하지 않도록 강제해 모듈 간 독립성을 유지한다.

### @ApplicationModuleListener 예시 코드
```java
// @ApplicationModuleListener 사용 예시
@Component
class NotificationListener {

    @ApplicationModuleListener
    public void onOrderCreated(OrderCreatedEvent event) {
        // 알림 로직 처리
        System.out.println("Handling order event for ID: " + event.getOrderId());
    }
}
```

---

`Spring Event`와 `@ApplicationModuleListener`를 함께 사용하면 모듈 간 비동기 통신이 가능하고, 독립적인 작업을 수행하면서도 이벤트 기반으로 데이터를 교환할 수 있다.
특히 `Spring Modulith`의 `@ApplicationModuleListener`는 모듈 간 결합도를 낮추고, 모듈화된 설계를 더욱 강화하는 데 유용하다.