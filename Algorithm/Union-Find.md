# Union-Find

## Union-Find란

- Union-Find 연산은 서로소 집합을 다루는 알고리즘
- 크루스칼 알고리즘을 구현할 때 두 노드가 사이클을 이루는지 확인할 수 있다
- 주요 연산
    - makeSet: 원소 x를 포함하는 새로운 집합을 생성
    - union: 두 집합을 합침
    - find: 각 원소가 어떤 집합에 속해 있는지 확인
    

## 구현

### Union 연산

```python
def union(x, y):
    x = find(x)
    y = find(y)

    root[y] = x

```

### Find 연산

```python
def find(x):
    if root[x] != x:
	    # 재귀적으로 탐색하면서 root 값을 최상위 부모노드로 수정
        root[x] = find(root[x])
		
    return root[x]
```