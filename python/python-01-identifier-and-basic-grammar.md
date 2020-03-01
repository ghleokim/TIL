# 1. 기초: 식별자, 기초 문법

## 1-1. 식별자(이름, name)

파이썬에서 식별자는 변수, 함수, 모듈, 클래스 등을 식별하는데 사용되는 이름이다.

식별자는 대소문자를 구분하며, 첫 글자에 숫자가 올 수 없다.

### 식별자와 예약어

식별자(변수명)를 만들 수 없는 예약어가 있는데, 예약어를 확인하는 방법은 다음과 같다.

```
# 식별자를 직접 확인
import keyword
keyword.kwlist

# output
['False', 'None', 'True', 'and', 'as', 'assert', 'async', 'await', 'break', 'class', 'continue', 'def', 'del', 'elif', 'else', 'except', 'finally', 'for', 'from', 'global', 'if', 'import', 'in', 'is', 'lambda', 'nonlocal', 'not', 'or', 'pass', 'raise', 'return', 'try', 'while', 'with', 'yield']
```

내장함수 또는 모듈의 이름으로 만들면 안된다.

- 예시 `str` : 인자를 문자열로 바꿔주는 내장함수

  ```
    # 숫자 3을 string으로 바꾼다.
    str(3)
    
    # output
    '3'
  ```

- `str` 에 값을 할당 후 함수를 다시 호출하면 오류가 발생한다.

  ```
    # str에 값을 할당 후 다시 함수 호출
    str = 'hello'
    str(5)
    
    # output
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    TypeError: 'str' object is not callable
  ```

- `del` 로 지워주면 다시 사용할 수 있다.

  ```
    del str
    str(3)
    
    # output
    '3'
  ```

## 1-2. 기초 문법

### 1-2-1 변수형 및 기타

**인코딩 선언**

```
# -*- coding: utf-8 -*-
```

파이썬 2.x에서는 기본적으로 ASCII를 사용하기 때문에 인코딩 선언을 해주어야 한다. 그러나 파이썬 3.0부터는 기본 인코딩이 utf-8로 지정되어 있기 때문에 이는 필요 없다.

**주석**

```
#
```

**docstring** : multi-line comment

```
"""something"""
```

docstring은 여러 줄을 한번에 주석 처리 할 수 있는 방법이다. C/JavaScript 등에서 볼 수 있는 `/* ... */`와 같은 개념이라고 볼 수 있다.

또한, 함수 또는 클래스를 정의할 때 이를 설명하는 docs로써의 역할을 할 수 있다.

```
# 함수 정의시 docstring 사용
def mysum(a,b):
	"""Sum Function
	this is a sum function.
	"""
	return a + b

# docstring 내용 출력
print(mysum.__doc__)
```

**출력**

```
print()
```

**리스트, 딕셔너리**

list `[]`

dictionary `{}`

```
menu = [
    "짬뽕",
    "순두부찌개",
    "닭도리탕"
]

restaurant = {
    "백짬뽕": "베이징코야",
    "햄버거": "바스버거",
    "돼지고기": "백운봉 막국수"
}
```

**할당, 바인딩**

```
=
```

보통 `=` 연산자는 변수를 '할당'하는 개념으로 알고 있다. 하지만 이를 '바인딩'의 개념으로 이해하는 것이 필요할 때가 온다.

만약 두 변수가 **진짜** 같은 값인지 알고 싶으면, id()함수를 실행해 출력해보면 알 수 있다. 주소가 같다면 두 변수는 같은 변수이다.

같은 값 동시에 할당하기

```
y=z=1000
print(id(y)) # xxxxxxxxxx *id: 해당 값이 저장되어 있는 메모리 주소(숫자) 출력
print(id(z)) # xxxxxxxxxx 위와 같은 방법으로 할당하면 같은 메모리 공간에 저장된다.
```

할당된 값 바꾸기

```
x, y = 5, 10

print(id(x), id(y)) # aaaaaaaaaa bbbbbbbbbb

x, y = y, x

print(id(y), id(x)) # bbbbbbbbbb aaaaaaaaaa 주소값이 서로 바뀐 것을 알 수 있다.
```

### 1-2-2 숫자 : int, float, complex

### 1-2-3 String

single quote

escape string interpolation

## 1-3. 연산자