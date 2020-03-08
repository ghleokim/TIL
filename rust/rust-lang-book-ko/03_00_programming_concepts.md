# 03 common programming concepts

이 장은 모든 프로그래밍 언어가 대부분 가지고 있는 개념에 대해 러스트는 어떻게 대하는지를 다룬다. 많은 프로그래밍 언어가 보편적인 핵심 요소를 갖는다.

이 장에서는 변수, 기본 데이터타입, 함수, 주석, 제어문에 대해서 다룰 것이다.

## Keywords

러스트는 다른 프로그래밍 언어와 마찬가지로 러스트만이 사용하는 _keywords-키워드 또는 예약어_ 가 있다. 이 단어들은 변수나 함수명으로 사용될 수 없다. 대부분의 키워드는 특별한 의미가 있고, 이들은 러스트 프로그램에서 다양한 작업을 처리하기 위해 사용될 것이다.

몇 단어들은 아무 기능이 없지만, 향후 러스트에 추가될 지도 모르는 기능을 위해 예약되어 있다.

### from Appendix A : 키워드/예약어의 목록

**Keywords currently in use**

```
as, break, const, continue, crate, dyn, else, enum, extern, false, fn, for, if, impl, in, let, loop, match, mod, move, mut, pub, ref, return, Self, self, static, struct, super, trait, true, type, unsafe, use, where, while
```

- `as` - primitive casting을 수행하거나, 특정 trait을 명확하게 하거나, `use`, `extern crate` 구문의 항목을 rename한다.
- `break` - loop에서 즉시 벗어난다.
- `const` - 상수 객체 또는 상수 기본 포인터(raw pointer)를 정의한다.
- `continue` - loop의 다음 iteration으로 넘어간다.
- `crate` - 외부 크레이트를 링크하거나, 매크로가 선언된 경우 해당 크레이트를 나타내는 매크로 변수.
- `dyn` - dynamic dispatch to a trait object
- `else` - 흐름 제어 구문에서 `if` 또는 `if let`에 대한 처리.
- `enum` - *enumertation열거형*을 정의한다.
- `extern` - 외부 크레이트, 함수, 변수를 링크한다.
- `false` - Boolean false literal
- `fn` - 함수 또는 그 함수의 포인터 타입을 정의한다.
- `for` - iterator의 항목에 대한 반복, trait을 채워넣거나, higher-ranked lifetime을 구체화한다.
- `if`
- `impl`
- `in`
- `let`
- `loop`
- `match`
- `mod`
- `move`
- `mut`
- `pub`
- `ref`
- `return`
- `Self`
- `self`
- `static`
- `struct`
- `super`
- `trait`
- `true`
- `type`
- `unsafe`
- `use`
- `where`
- `while`

**Keywords reerved for future use**

```
abstract, async, await, become, box, do, final, macro, override, priv, try, typeof, unsized, virtural, yield
```

- `abstract`
- `async`
- `await`
- `become`
- `box`
- `do`
- `final`
- `macro`
- `override`
- `priv`
- `try`
- `typeof`
- `unsized`
- `virtual`
- `yield`

### Raw Identifiers

*Raw Identifiers*란 보통 허용되지 않는 키워드를 사용할 수 있도록 만드는 식별자 문법이다. `r#`을 키워드에 붙여 사용하면 있는 그대로 함수 이름으로 사용할 수 있다.

예를 들어, `match`는 키워드이다. 만약 `match`라는 이름의 함수를 생성하려고 하면 다음과 같은 에러가 발생할 것이다.

Filename: src/main.rs

```rust
fn match(needle: &str, haystack: &str) -> bool {
    haystack.contains(needle);
}
```

에러메세지:

```bash
error: expected identifier, found keyword `match`
 --> src/main.rs:4:4
  |
4 | fn match(needle: &str, haystack: &str) -> bool {
  |    ^^^^^ expected identifier, found keyword
```

Filename: src/main.rs
