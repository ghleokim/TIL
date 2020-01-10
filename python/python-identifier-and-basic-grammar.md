# python-identifier-and-basic-grammar

## 식별자(이름, name)

파이썬에서 식별자는 변수, 함수, 모듈, 클래스 등을 식별하는데 사용되는 이름이다.

식별자는 대소문자를 구분하며, 첫 글자에 숫자가 올 수 없다.

식별자(변수명)를 만들 수 없는 예약어가 있는데, 예약어를 확인하는 방법은 다음과 같다.

```python
# 식별자를 직접 확인
import keyword
keyword.kwlist

# output
['False', 'None', 'True', 'and', 'as', 'assert', 'async', 'await', 'break', 'class', 'continue', 'def', 'del', 'elif', 'else', 'except', 'finally', 'for', 'from', 'global', 'if', 'import', 'in', 'is', 'lambda', 'nonlocal', 'not', 'or', 'pass', 'raise', 'return', 'try', 'while', 'with', 'yield']
```

내장함수 또는 모듈의 이름으로 만들면 안된다.

- 예시 `str` : 인자를 문자열로 바꿔주는 내장함수

```python
# 숫자 3을 string으로 바꾼다.
str(3)

# output
'3'
```

- `str` 에 값을 할당 후 함수를 다시 호출하면 오류가 발생한다.

```python
# str에 값을 할당 후 다시 함수 호출
str = 'hello'
str(5)

# output
Traceback (most recent call last): File "<stdin>", line 1, in <module> TypeError: 'str' object is not callable
```

- `del` 로 지워주면 다시 사용할 수 있다.

```python
del str
str(3)

# output
'3'
```

## 파이썬 스타일 가이드, PEP-8

ing...