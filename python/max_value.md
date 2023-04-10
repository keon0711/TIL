## 파이썬에서 최대값 사용법
### 1. sys.maxsize()
sys.maxsize는 int형의 최대값인 2147483647을 값으로 가지기 때문에 int형 범위 내에서는 최대값으로 활용할 수 있다.
### 2. float('inf')
float('inf')를 통해 말 그대로 무한대 값을 얻을 수 있다. 무한대이기 때문에 1을 더해도 값이 변하지 않는다.  
float('-inf')로 음의 무한대를 구할 수도 있다.
```python
float('inf')
>>> inf

float('inf') + 1
>>> inf

float('inf') + 1 == float('inf')
>>> True

float('-inf')
>>> -inf
``` 
### 3. math.inf
math.inf를 이용해서 무한대를 얻을 수도 있다. float('inf')로 구한 무한대와 동일하다.
```python
float('inf') == math.inf
>>> True
```
