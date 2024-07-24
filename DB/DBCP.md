#DB #ConnectionPool #DBCP 
# DBCP
- DataBase Connection Pool
- DB connection은 backend server와 DB server 사이의 연결을 의미한다.

## DB (MySQL)
### max_connection
- 데이터베이스가 클라이언트와 맺을 수 있는 최대 connection 수
- 백엔드 서버가 추가되었을 때 max_connection 수가 부족하면 connection을 맺지 못할 수 있다.
### wait_timeout
- connection이 inactive 할 때 얼마나 기다린 후 close 할지를 결정

## DBCP (HikariCP)
### minimumIdle
- pool에서 유지하는 최소한의 idle connection 수
- connection이 active되면 minimumIdle에 맞게 추가 connection을 만든다.
- 기본값은 maximumPoolSize와 동일 (= pool size 고정)
### maximumPoolSize
- pool이 가질 수 있는 최대 connection 수 
- idle과 active(in-use) connection을 합친 최대 수를 의미한다.
### maxLifeTime
- pool에서 connection의 최대 수명
- maxLifeTime을 초과할 경우 **idle connection**은 즉시 제거하고, **active connection**은 close된 후 제거한다.
- pool로 반환되지 않으면 maxLifeTime은 동작하지 않음
- DB로 요청이 전송되는 시점에 DB의 connection이 close 될 수 있기 때문에 DB의 wait_timeout보다 몇 초 짧게 설정해야 함
### connectionTimeout
- pool에서 connection은 얻기 위한 대기 시간

## 적절한 connection 수를 찾는 법

### 준비
- 모니터링 환경 구축
- 부하 테스트 (ex. nGrinder)
- **request per second**, **avg response time**  을 확인

### 테스트 방법
- thread per request 모델일 경우 acitve thread 수 확인 (스레드 풀의 수가 너무 적지 않은지?)
- DBCP의 active connection 수 확인
- DBCP의 **maximumPoolSize**를 증가했다면, DB의 **max_connection**도 그에 맞게 증가시킨다.

### 해결 방법
1. secondary DB 추가
2. cache layer 추가
3. sharding
4. ...


## 참고자료
[쉬운코드님 DBCP 강의](https://www.youtube.com/watch?v=zowzVqx3MQ4&t=789s)
