## 파이썬 heapq 모듈
- 파이썬의 heapq 모듈은 부모 노드가 자식 노드보다 작거나 같은 최소 힙(min heap)이다. 따라서 heap[0]가 가장 작은 값을 보장한다.

## iterable 우선순위 비교
- heapq에 리스트나 튜플같은 iterable 객체를 추가하면, iterable 객체의 앞 요소부터 크기를 비교한다.
- 예를 들어 `heappush(heap, (1, 2))`와 `heappush(heap, (1, 3))`는 첫번째 요소는 같지만, 두번째 요소는 2가 3보다 작기 때문에 (1, 2)가 최소임을 보장한다.

## 유용한 함수
### heapq.nlargest(_n_, _iterable_, _key=None_)[¶](https://docs.python.org/ko/3/library/heapq.html#heapq.nlargest "이 정의에 대한 퍼머링크")
- iterable에서 n개의 가장 큰 요소를 리스트로 반환한다.
### heapq.nsmallest(_n_, _iterable_, _key=None_)[¶](https://docs.python.org/ko/3/library/heapq.html#heapq.nsmallest "이 정의에 대한 퍼머링크")
- iterable에서 n개의 가장 작은 요소를 리스트로 반환한다.
