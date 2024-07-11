# @Value
- `Value` 어노테이션은 애플리케이션 설정 파일, 환경변수에서 값을 주입하는데 사용된다.
- 주로 필드, 생성자 매개변수, 메서드 매개변수에 주입한다.

## 주의사항
- 필드에 값을 주입하는 시점이 빈 생성 시점보다 늦기 때문에 생성자가 호출되는 시점에는 필드 값이 null이다.
- 생성자에서 설정 파일이나 환경 변수 값을 사용하려면, 생성자 매개변수에 `@Value`를 붙여야 한다.