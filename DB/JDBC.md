# JDBC
- 자바에서 데이터베이스에 접속할 수 있도록 하는 표준 인터페이스
- JDBC 인터페이스를 각각의 DB에 맞게 구현한 라이브러리가 JDBC 드라이버 (MySQL 드라이버 등)
- JDBC 제공 기능
	- java.sql.Connection: 연결
	- java.sql.Statement: SQL을 담은 내용
	- java.sql.ResultSet: SQL 요청 응답
- `DriverManager`가 라이브러리에 등록된 DB 드라이버를 관리하고, 커넥션을 획득하는 기능을 제공

# 커넥션 풀
- 매번 DriverManager를 사용해 커넥션을 생성하게 되면 많은 시간이 소요된다.
- 커넥션을 미리 생성해 두고 사용하기 위해 **커넥션 풀**을 사용한다.