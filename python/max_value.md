## 파이썬에서 최대값 사용법
### 1. sys.maxsize()
sys.maxsize는 int형의 최대값인 2147483647을 값으로 가지기 때문에 int형 범위 내에서는 최대값으로 활용할 수 있다. sys.maxsize를 사용하기 위해서 먼저 ```import sys```를 해줘야 한다.
```Python
sys.maxsize
>>> 2147483647
```
그러나 다른 환경에서 sys.maxsize의 값을 확인하면 9223372036854775807가 나오기도 한다.
```Python
sys.maxsize
>>> 9223372036854775807
```
확인해보니 sys.maxsize는 Py_ssize_t 데이터 유형의 변수가 저장할 수 있는 가장 큰 값을 가져온다고 한다.  
따라서 아키텍처에 따라 2^31-1을 값으로 할 때도 있고, 2^63-1을 값으로 가질 때도 있다.  
<br>

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
<br>

### 3. math.inf
math.inf를 이용해서 무한대를 얻을 수도 있다. float('inf')로 구한 무한대와 동일하다.
```python
import math

float('inf') == math.inf
>>> True
```
