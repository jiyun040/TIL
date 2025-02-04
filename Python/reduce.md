반복 가능한 객체(iterable object)내 각 요소를 연산한 뒤 이전 연산 결과들과 누적해서 반환해주는 함수

반복 가능한 객체는 말 그대로 반복이 가능한 객체로써 요소가 하나의 객체에 여러개가 들어있고, 한 번에 하나의 요소씩 사용할 수 있는 객체를 말한다. 대표적으로는 문자열, 리스트, 딕셔너리, 세트가 있다. 

파이썬3 부터는 내장 함수가 아니기 때문에 `functools`모듈에서 불러와야 한다. 

- 기본형

```python
from functools import reduce
reduce(함수, 반복가능객체)
```

**연습문제**

리스트에 1부터 20까지의 정수가 있을 때, 해당 리스트의 모든 요소의 합을 출력하는 코드

1.  reduce 사용하지 않았을 때

```python
# 함수 만들기
def sumf(x, y):
	return x + y

# 1부터 20까지의 리스트 만들기
target = list(range(1, 21))
result = 0
# 반복문을 돌면서 곱하기
for value in target:
	result = sumf(result, value)
print(result)
```

2. reduce 사용했을 때

```python
from functools import reduce
def sumf(x, y):
	return x + y

target = list(range(1, 21))
print(reduce(sumf, target)) # target이 반복 가능한 객체(리스트)
```

3. reduce와 lambda를 사용했을 때

```python
from functools import reduce
target = list(range(1, 21))
print(reduce(lambda x, y: x + y, target)) # target을 돌면서 모두 더함
```

3개 코드의 실행결과는 모두 같지만 reduce와 lambda를 사용함으로써 더 간단해진다.

이해하기 조금 어렵고 해석하는 데 시간이 걸리긴 하지만 좋은 함수들인거 같다.
