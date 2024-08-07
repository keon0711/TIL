charsyam@naver.com

## Redis
- In-Memory Cache
- In-Memory DB
- 디스크보다 메모리가 빨라서 사용
- 성능 최적화를 위해서는 디스크 접근을 최소화
- 메모리와 SSD 속도 차이는 1000배
- 카프카(디스크)는 왜 빠를까? -> Random Access보다 Sequential Access가 빠르기 때문에
- 빠르면서 다양한 자료구조 제공 (Sorted Set 등)
- 데이터 Cache나 세션 스토어로 활용
- Cache: 더 빠른 매체에 저장하는 것
- key-value store
- 처음 접근하거나, 없는 데이터를 요청할 때 성능이 가장 안 좋음

## 접근 전략
### Cache Aside (Look Aside)
1. 캐시 읽기
2. 캐시 미스
3. DB 읽기
4. 데이터 얻기
5. 캐시 업데이트
### Read Through (주로 사용)
1. 캐시 읽기
2. 캐시 미스
3. 캐시가 DB에 접근
4. 캐시가 데이터 얻음
5. 캐시 업데이트
### Write Back
1. 캐시에 씀
2. 캐시에 저장된 데이트를 한번에 DB에 저장
DB 접근을 최소화
캐시가 날라가면 데터이가 사라질 위험이 있음 -> 중요하지 않은 데이터를 다룰 때 사용할 수 있음
### Write Through
1. 캐시에 쓰기
2. 캐시가 DB에 바로 쓰기
쓸 때마다 DB에 접근하기 때문에 쓰기가 느림
읽기를 강조하고 싶을 때 사용
데이터가 사라질 위험이 없음

- DB도 내부적으로 캐시가 있기 때문에 생각만큼 차이가 나지 않음


## 활용 예시
### Session Store
- 서버가 여러 대일 때 세션 스토어로 활용
### Distributed Lock
- 실 서비스에서는 서버가 무조건 1대 이상
### Rate Limiter
- 잦은 업데이트가 필요한 경우 저장소로 사용되는 케이스
- View Count 등을 저장할 때도 사용


## Redis is Single Thread
- 한번에 하나의 명령밖에 처리하지 못하기 때문에 오래 걸리는 태스크`O(n)`를 요청하지 않는게 좋다.

## 사용하는 이유
- 속도의 균일성