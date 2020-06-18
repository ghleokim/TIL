# This의 호출부

> 이 문서는 _You Don't Know JS : this와 객체 프로토타입_ 을 읽고 공부한 내용을 정리한 것입니다.

1장에서는 this는 호출부에서 함수를 호출할 때 바인딩된다는 사실을 이야기했다.

## 2.1 호출부

this 바인딩의 개념을 이해하려면 호출부, 즉 함수 호출 코드(함수 선언 부분X)부터 확인하고 'this가 가리키는 것'이 무엇인지 확인해야 한다.

호출부는 함수를 호출한 지점을 찾으면 금방 찾을 수 있을 것 같지만, 코딩패턴에 따라 찾기 어려울 때도 있다. 중요한 것은 콜 스택(현재 실행 지점에 오기까지 호출된 함수의 스택)을 생각해보면 된다. 이 호출부는 현재 실행중인 함수 '직전'의 호출 코드 내부에 있다.

다음 예제는 호출부와 호출 스택을 설명하기 위한 예제이다.

```javascript
function baz() {
    // call stack: 'baz'
    // 호출부는 전역 스코프 내부이다.

    console.log("baz");
    bar(); // <= bar()의 호출부
}

function bar() {
    // call stack : 'baz' -> 'bar'
    // 호출부는 'baz' 내부이다.

    console.log("bar");
    foo(); // <= foo()의 호출부
}

function foo() {
    // call stack: 'baz' -> 'bar' -> 'foo'
    // 호출부는 'bar' 내부이다.

    console.log("foo")
}

baz(); // 'baz'의 호출부
```