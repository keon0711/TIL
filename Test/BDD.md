# BDD
- Behavior-Driven Development (행동 주도 개발)
- 시나리오 기반의 테스트를 통해 요구사항을 정확히 만족하는지 검증할 수 있다.
- 코드보다 인간의 언어와 유사하게 구성되어야 한다.
- 모든 테스트 문장은 Given / When / Then 으로 나눠 작성할 수 있어야 한다.

## BDDMockito
- Mockito 프레임워크의 확장 기능
- BDD 스타일의 테스트 작성을 지원
	- **Given**: 테스트의 사전 조건을 설정합니다.
	- **When**: 특정 행동을 수행합니다.
	- **Then**: 결과를 검증합니다.

> **💡 @InjectMocks, @Mock, @MockBean 차이점**  
> - @InjectMocks, @Mock는 Mockito에서 ‘유닛 테스트’에 사용되고 @MockBean는 Spring Boot에서 ‘통합 테스트’에서 사용이 됩니다.@Mock는 특정 클래스나 인터페이스에 대해 ‘모의 객체를 생성’하는 역할을 수행합니다.  
> - @InjectMocks는 테스트 대상 객체에 ‘모의 객체를 주입’하는 역할을 수행합니다.

```java
@ExtendWith(MockitoExtension.class)
public class UserServiceTest {

    @Mock
    private UserRepository userRepository;

    @InjectMocks
    private UserService userService;

    @Test
    public void givenValidUserId_whenGetUserById_thenUserShouldBeReturned() {
        // Given
        Long userId = 1L;
        User user = new User();
        user.setId(userId);
        user.setName("John Doe");
        given(userRepository.findById(userId)).willReturn(user);

        // When
        User foundUser = userService.getUserById(userId);

        // Then
        assertThat(foundUser).isNotNull();
        assertThat(foundUser.getId()).isEqualTo(userId);
        assertThat(foundUser.getName()).isEqualTo("John Doe");
        then(userRepository).should().findById(userId);
    }

    @Test
    public void givenInvalidUserId_whenGetUserById_thenUserShouldNotBeReturned() {
        // Given
        Long userId = 1L;
        given(userRepository.findById(userId)).willReturn(null);

        // When
        User foundUser = userService.getUserById(userId);

        // Then
        assertThat(foundUser).isNull();
        then(userRepository).should().findById(userId);
    }
}
```

1. **Given**: `given(userRepository.findById(userId)).willReturn(user);`
	- 테스트 사전 조건을 설정
2. **When**: `User foundUser = userService.getUserById(userId);`
	- 특정 행동을 수행하는 부분
3. **Then**: `assertThat(foundUser).isNotNull();`
	- 결과를 검증하는 부분

### 메서드 파라미터 예시
- `any()`: 모든 객체
- `any(String.class)` = `anyString()`: 모든 String
- `any(Long.class)` = `anyLong()`: 모든 Long
- `eq(value)`: 정확히 해당 값을 나타낸다.
- `anyList()`: 임의의 리스트

### 반환 값
- `willReturn()`: 반환값을 설정
- `willThrow()`: 예외를 던지도록 설정

### 목 객체 행동 검증
- Mockito의 `then` 메서드는 BDD 스타일로 **목 객체의 행동을 검증할 때 사용**된다.
- `should` 메서드와 함께 사용됨
- then 메서드 사용법
	1. **then().should()**: 특정 메서드가 호출되었는지 검증합니다.
	2. **then().should(times(n))**: 특정 메서드가 n번 호출되었는지 검증합니다.
	3. **then().should(never())**: 특정 메서드가 전혀 호출되지 않았는지 검증합니다.
	4. **then().shouldAtLeast(n)**: 특정 메서드가 최소 n번 호출되었는지 검증합니다.
	5. **then().shouldAtMost(n)**: 특정 메서드가 최대 n번 호출되었는지 검증합니다.


