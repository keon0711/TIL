# 스프링 컨테이너
## 목차
1. [스프링 빈으로 등록하는 방법](#스프링-빈으로-등록하는-방법)
2. [스프링 빈 주입 방법](#스프링-빈-주입-방법)
3. [Reference](#Reference)
<br/>

## 스프링 빈으로 등록하는 방법
1. @RestController, @Service, @Repository
   - 컨트롤러, 서비스, 레포지토리를 스프링 빈으로 등록
2. @Configuration, @Bean
   - @Configuration
     - 클래스에 붙이는 어노테이션
     - @Bean과 함께 사용해야 함
   - @Bean
     - 메소드에 붙이는 어노테이션
     - 메소드에서 리턴되는 객체를 스프링 빈으로 등록
   - 외부 라이브러리를 스프링 빈으로 등록할 때 사용
3. @Component
   - 개발자가 직접 작성한 클래스를 스프링 빈으로 등록할 때 사용  
<br>

## 스프링 빈 주입 방법
1. 생성자를 이용 (가장 권장)
```java
public UserController(UserService userService) {
    this.userService = userService;
}
```
2. setter와 @AutoWired 사용
- 누군가 setter을 사용해서 오작동할 여지가 있다.
```java
@AutoWired
public void setUserService(UserService userService) {
    this.userService = userService;
}
```
1. 필드에 직접 @AutoWired 사용
- 테스트를 어렵게 만드는 요인이다.
```java
@AutoWired
private JdbcTemplate jdbcTemplate
```

## Reference
- [인프런 - 자바와 스프링 부트로 생애 최초 서버 만들기, 누구나 쉽게 개발부터 배포까지!](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-%EC%84%9C%EB%B2%84%EA%B0%9C%EB%B0%9C-%EC%98%AC%EC%9D%B8%EC%9B%90/dashboard)