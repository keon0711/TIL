## Specification
- **Specification**: 검색 조건을 생성하는 인터페이스
- and() / or() 로 검색 조건 조합 가능

### Criteria
- **Criteria**를 이용해 검색 조건을 생성
	- Criteria: JPA에서 제공하는 API로, 코드로 쿼리를 작성할 수 있다.
- **Predicate**: Criteria API의 핵심 개념으로, 쿼리의 조건을 표현
<br>
### Specification 인터페이스
```java
public interface Specification<T> extends Serializable {
	@Nullable
	Predicate toPredicate(Root<T> root, CriteriaQuery<?> query, CriteriaBuilder criteriaBuilder);
}
```
<br>

### Repository
- 다음과 같이 Specification 인터페이스를 받아 검색 조건을 지정
```java
public interface UserRepository extends Repository<User, String> {
	List<User> findAll(Specification<User> spec);
}
```

<br>

### 구현 방법
1. Specification 구현체를 만들어 Repository에 넘겨줌
	- 매번 구현체를 정의하기 번거로움
2. **Static 메서드로 람다식을 이용해 Specification을 생성**
```java
public Class UserSpecs {
	public static Specification<User> nameLike(String value) {
		return (root, cq, cb) -> cb.like(root.get("name"), "%" + value + "%");
	}
}
```
```java
Specification<User> spec = UserSpecs.nameLike("이름");
List<User> users = userRepository.findAll(spec);
```
