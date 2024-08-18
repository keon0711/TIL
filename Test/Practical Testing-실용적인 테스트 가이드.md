## Spring Data JPA 기반 테스트
- 테스트 코드에 `@Transactional`, `@SpringBootTest`나 `@DataJpaTest` 를 사용할 때 서비스 코드에 `@Transactional` 유무를 알 수 없기 때문에 이 점을 숙지하고 사용해야 한다.
- 이 점 때문에 `@AfterEach` 메서드를 사용해 직접 초기화하는 방식을 사용하기도 한다.

## Presentation Layer Test
- Presentation Layer는 `@WebMvcTest` 로 테스트할 수 있다.
- 이 때 Service Layer는 `@MockBean`으로 주입하자


### `@Transactional`
- CQRS (Command and Query Responsibility Segregation)
	- Read와 CUD 를 구분하자!
	- Wrtie DB와 Read DB 의 엔트리 포인트가 된다.
- `@Transactional(readOnly = true)`를 사용하면 쿼리와 커멘드를 쉽게 구분할 수 있다.
- `readOnly = true`를 사용하면 Entity Manager가 1차 캐시에 스냅샷을 저장하거나, 더티체킹을 하는 오버헤드를 방지할 수 있다.
> ✅ 클래스 레벨에 `@Transactional(readOnly = true)`를 추가하고 CUD 동작을 하는 메서드에 `@Transactional`를 추가하자
- 메일 전송과 같이 긴 작업이 있는 서비스에서는 트랜잭션을 걸지 않는게 좋다.

## Test Double
Test Double은 소프트웨어 테스트에서 실제 객체 대신 사용되는 가짜 객체를 의미한다.
주로 단위 테스트를 수행할 때 사용되며, 테스트 대상 코드를 격리시켜 독립적으로 테스트할 수 있게 한다.

#### **✔️ Dummy**
  아무것도 하지 않는 깡통 객체  
#### **✔️ Fake**
  단순한 형태로 같은 동작을 하는 객체 (ex. FakeRepository)
#### **✔️ Stub**
  테스트에서 요청한 것에 대해 미리 준비한 결과를 제공하는 객체. 그 외에는 응답 X
#### **✔️ Spy**
  Stub이면서 호출된 내용을 기록해 보여줄 수 있는 객체. 일부는 실제 동작하고 일부는 stubbing 할 수 있다.
#### **✔️ Mock**
  행위에 대한 기대를 명세하고, 그에 따라 동작하도록 만들어진 객체  
  

> ### Stub vs Mock
> #### ✔️ Stub
> 상태 검증
> 
> #### ✔️ Mock
> 행위 검증

  