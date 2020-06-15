# this

> 이 문서는 _You Don't Know JS : this와 객체 프로토타입_ 을 읽고 공부한 내용을 정리한 것입니다.

JS에서 this는 매우 햇갈리는 체계 중 하나이다. 우선 this를 _왜_ 사용하는지부터 알아 봅시다.

## 1.1 왜 this를 사용하는가?

this는 여러 객체가 각각 따로 함수를 작성할 필요 없이 _다중 콘텍스트 객체(후술)_ 에서 함수를 재사용할 수 있다는데 장점이 있다. 아래 예시에서 이를 확인할 수 있다.

```javascript
function identify() {
    return this.name.toUpperCase();
}

function speak() {
    var greeting = "Hello, I'm " + identify.call( this );
    console.log( greeting );
}

var me = {
    name: "Leo"
};

var you = {
    name: "Reader"
};

identify.call( me );    // LEO
identify.call( you );   // READER

speak.call( me );       // Hello, I'm LEO
speak.call( you );       // Hello, I'm READER
```

this를 안 쓰고 identify()와 speak() 함수에 콘텍스트 객체를 명시할 수도 있다.

```javascript
function identify(context) {
    return context.name.toUpperCase();
}

function speak(context) {
    var greeting = "Hello, I'm " + identify( context );
    console.log( greeting );
}

identify( you );    // READER
speak( me );        // Hello, I'm LEO
```

하지만 암시적인 객체 레퍼런스를 _함게 넘기는passing along_ this 체계가 API 설계상 조금 더 깔끔하고 명확하며 재사용하기 쉽다..고 한다.

## 1.2 헷갈리는 것들 : this에 대한 두 가지 오해

this라는 이름 자체가 사용하는 사람으로 하여금 그 정체를 헷갈리게 만드는 점이 있다. 사람들은 this에 대해 보통 아래 두 가지의 오해를 많이 하고 있다.

### 오해 1 : this는 자기 자신을 가리킨다.

첫 번째는 자기 자신을 가리킨다는 오해이다. 함수가 내부에서 자기 자신을 가리킬 필요가 있는 경우는 보통 재귀 로직, 또는 최초 호출시 이벤트에 바인딩된 자신을 _언바인딩unbinding_ 할 때도 사용한다.

자바스크립트 체계에 생소한 개발자는 보통 함수를 객체로 참조함으로써 함수 호출간 _상태state_ 를 저장할 수 있을 것이라고 생각한다. 그렇게 할 수는 있고 제한적으로 가능하지만, 함수 객체 말고도 더 좋은 장소에 상태를 저장하는 패턴을 이 책의 나머지 부분에서 설명한다.

아래 예시는 this가 자기 참조를 할 수 없다는 것을 보여준다. 함수 호출 횟수를 추적하는 예제이다.

```javascript
function foo(num) {
    console.log("foo: " + num);

    this.count++;
}

foo.count = 0;
var i;
for (i=0; i<10; i++) {
    if (i > 5) {
        foo( i );
    }
}

// foo: 6
// foo: 7
// foo: 8
// foo: 9

// foo는 몇 번 호출되었을까?
console.log( foo.count ); // 0
```

console.log에는 분명 foo() 함수의 호출 횟수가 출력되었지만 foo.count 값은 0이다.

foo.count = 0을 하면 foo라는 함수 객체에 count 프로퍼티가 추가된다. 하지만 this.count에서 this는 함수 객체를 바라보는 것이 아니다. 프로퍼티 명이 같아 헷갈리지만 근거지를 둔 객체 자체가 다르다.

> 이 때 실제 증가한 count는 전역 변수 count이다.

이 때 대부분의 개발자는 더 이상 답을 찾지 않고 우회책을 통해 문제를 해결한다.

```javascript
function foo(num) {
    console.log( "foo: " + num );
    data.count++;
}

var data = {
    count: 0
};

var i;
for (i=0; i<10; i++) {
    if (i > 5) {
        foo( i );
    }
}

// foo: 6
// foo: 7
// foo: 8
// foo: 9

// foo는 몇 번 호출되었을까?
console.log( data.count ); // 4
```

이렇게 해결할 수는 있지만 this가 무엇인지 모른 채 결국 _렉시컬 스코프lexical scope_ 를 사용하게 된 것이다. 함수가 내부에서 자신을 참조할 때, 일반적으로 this만으로는 부족하고 렉시컬 식별자(변수)를 거쳐 함수 객체를 참조한다.

다음 두 함수의 예시를 보자.

```javascript
function foo() {
    foo.count = 4;      // foo는 자기 자신을 가리킨다.
}

setTimeout( function() {
    // 익명함수는 자기 자신을 가리킬 방법이 없다.
}, 10);
```

_이름 있는 함수named function_ 이라고 불리는 foo 함수는 foo라는 함수명 자체가 내부에서 자신을 가리키는 레퍼런스로 쓰인다. 하지만 setTimeout()에 콜백으로 전달한 함수는 이름 식별자가 없으므로 함수 자신을 참조할 방법이 없다.

> arguments.callee도 실행중인 함수를 가리키지만, 현재는 권장하지 않는deprecated 레퍼런스이다. 자기 참조가 필요하다면 익명 함수보다 이름 붙은 함수를 사용하는게 최선이다.

### 오해 2 : this는 함수의 스코프를 가리킨다.

this가 함수의 스코프를 가리킨다는 말은 아주 흔한 오해이다. this는 어떤 식으로도 함수의 렉시컬 스코프를 참조하지 않는다. 내부적으로 스코프는 별개의 식별자가 달린 프로퍼티로 구성된 객체의 일종이지만, 스코프 객체는 자바스크립트 구현체인 엔진의 내부부품이기 때문에 일반 코드로는 접근하지 못한다.

다음과 같이 this가 함수의 렉시컬 스코프를 가리키도록 해보자.

```javascript
function foo() {
    var a = 2;
    this.bar();
}

function bar() {
    console.log( this.a );
}

foo(); // ReferenceError: a is not defined
```

이 코드는 실행조자 되지 않는다. 이 코드는 여러 곳에서 실수를 했다.

bar() 함수를 this.bar()로 참조하려고 했던 것 부터가 문제다. bar() 앞의 this를 빼고 식별자를 이름으로 참조하는 것이 가장 자연스러운 호출 방법이다.

이 코드를 작성할 때에는 foo()와 bar()의 렉시컬 스코프 사이에 특정 통로를 통해 bar()가 foo() 내부 스코프의 변수 a에 접근하려는 생각으로 작성했겠지만, 이는 절대 불가능한 일이다. 이러한 방식으로 this를 통해 렉시컬 스코프 내부의 대상을 참조하는 것은 불가능하다.

## 1.3 this는 무엇인가?

this 체계가 어떻게 작동하는지 알아보자. this는 작성 시점이 아닌 런타임 시점에 바인딩되며 함수 호출 당시 상황에 따라 컨텍스트가 결정된다고 설명했다. 함수 선언 위치와 상관 없이 this 바인딩은 오로지 함수를 어떻게 호출했는가에 따라 정해진다.

함수를 호출하면 _활성화 레코드Activation Record_ , 즉 실행 컨텍스트가 만들어진다. 여기엔 함수가 호출된 출처(_콜스택Call stack_)와 호출 방법, 전달된 인자 등의 정보가 담겨 있다. this 레퍼런스는 그 중 하나로, **함수가 실행되는 동안 이용할 수 있다.**

이 다음 장에서는 this 바인딩을 결정짓는 _함수 호출부Call site_ 를 설명한다.

## 1.4 정리하기

this 바인딩은 매우 애매한 주제이다. 대충 감으로 때려잡고 코드를 짜다가는 같은 실수를 계속 반복하게 된다. this에 대해 제대로 알고 싶다면 **this는 함수 자신, 함수의 렉시컬 스코프를 가리키는 레퍼런스가 아니라는 점을 분명히 인지해야 한다.**
