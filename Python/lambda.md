익명함수(anonymous function)이라도고 부른다. 

이름이 없는 함수로 일반적으로 함수를 한번만 사용하거나 함수를 인자로 전달해야 하는 경우 주로 사용한다. 

*함수를 인자로 전달해야 하는 경우: **어떤 함수의 매개변수로 또 다른 함수를 전달하는 것**

#### 람다(lambda)함수

`lambda 인자 : 표현식`

def 키워드를 사용해 함수를 정의하는 것 보다 간결하고 간편하게 함수를 정의할 수 있다. 

```python
def add(x, y):
	return x * y
```

이 함수를 람다 함수로 바꾼다면 

```python
add = lambda x, y = x * y
```

이제 add 변수는 람다 함수를 참조한다. 

#### 활용 예제

1. map() 함수
    
    시퀀스의 모든 요소에 함수를 적용한 결과를 반환한다. 
    
    ```python
    mylist = [1, 2, 3, 4, 5]
    # 위의 리스트에 2를 곱하는 것을 람다를 사용한다면
    mylist2 = list(map(lambda x: x * 2, mylist))
    print(mylist2)
    ```
    
    [2, 4, 6, 8, 10]
    
2. filter() 함수
    
    시퀀스의 모든 요소 중에서 조건에 맞는 요소만을 반환한다.
    
    ```python
    mylist = [1, 2, 3, 4, 5]
    # 위의 리스트에서 홀수만 추출
    mylist2 = list(filter(lambda x: x % 2 == 1, mylist))
    print(mylist2)
    ```
    
    [1, 3, 5]
    
3. sorted() 함수
    
    시퀀스의 요소를 정렬한 결과를 반환한다. 
    
    ```python
    mylist = ['apple', 'banana', 'cherry']
    # 위 리스트를 길이 순으로 정렬
    mylist2 = sorted(mylist, key = lambda x: len(x))
    print(mylist2)
    ```
    
    [’apple’, ‘cherry’, ‘banana’]
    
    *key = 정렬 기준 지정 함수
    
4. reduce() 함수
    
    시퀀스의 모든 요소를 누적적으로 계산한 결과를 반환
    
    ```python
    from functools import reduce
    mylist = [1, 2, 3, 4, 5]
    # 위 리스트의 모든 요소를 곱
    result = reduce(lambda x, y: x * y, mylist)
    print(result)
    ```
    
    120
