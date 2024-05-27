# Entity와 VO
- Entity: 연속성과 식별성 맥락에서 정의되는 객체
- VO: 식별성 없이 속성만으로 정의되는 객체 (Money, Address 등)

# Aggregate로 Entity 간의 선 긋기
## Aggregate
- 하나의 단위로 취급되는 연관된 객체군, 객체망
	- 연관된 Entity와 VO의 묶음
	- 엄격한 데이터 일관성, 제약사항이 유지되어야 할 단위
	- Transaction, Lock의 필수 범위
- **Aggregate 1개당 Repository 1개**
	- **Aggregate root를 통해서 Aggregate 밖에서 Aggregate 안의 객체로 접근**
- 다른 Aggregate 객체를 참조하면 Aggregate간의 경계가 모호해질 수 있다.
	- 객체 자체가 아니라 ID 값을 참조해 경계를 유지한다.
	- Spring data JDBC에는 다른 Aggregate의 ID를 참조하기 위한 `AggregateReference`가 존재
## Aggregate root
애그리거트 루트 엔티티는 애그리거트의 핵심적인 역할과 책임을 가지고있으며, 애그리거트 내부의 모든 객체들은 애그리거트 루트 엔티티를 중심으로 관리된다.

**애그리거트 루트 엔티티의 주요 책임**
1. 애그리거트 내부의 모든 객체들을 관리하고 제어한다.
2. 애그리거트 루트 엔티티의 불변성을 보장한다.
3. 애그리거트 루트 엔티티와 다른 객체 간의 관계를 관리한다.
4. 애그리거트 루트 엔티티를 통해 애그리거트 전체를 제어한다.

## 여러 Aggregate에 걸친 조회
### Service 레이어에서 조합
- 쿼리가 2번 날라가지만, 쿼리가 단순해서 성능에 유리할 수 있다.
```java
MilestoneEntity milestone = milestoneRepository.findById(milestoneId);
int issueCount = issueRepository.countByMilestoneId(milestoneId);

val milestoneResponse = MilestoneResponse.builder()
		.name(milestone.getName())
		.endedAt(milestone.endedAt())
		.issueCount(issueCount)
		.build();
```

# Spring Data JDBC

- `CrudRepository.save(…)`는 
- Aggregate/Entity 단위로 접근이 어려운 부분은 `NamedParameterJdbcTemplate`을 직접 사용하면 됨
- 동일한 Aggregate 내에서는 객체 참조
- 다른 Aggregate가 필요하면 객체가 아니라 ID 값을 참조
- N대N 관계일 때는 외래키 방식인 ID 참조와 객체 참조 방식이 함께 사용
- 외래키가 존재해야 하는 곳에 `AggregateReference`를 넣어주고 참조할 객체와 아이디의 타입을 설정해준다.
	- ex) `AggregateReference<Member, Long> memberId`
- 연관 관계 클래스에 `@Id`는 선언하지 않아도 됨 (Table column만 존재하면 됨)
- 양방향 연관관계 매핑 금지