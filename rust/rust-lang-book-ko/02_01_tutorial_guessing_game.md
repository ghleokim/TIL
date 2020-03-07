# 2 Tutorial: Guessing Game

추리 게임을 통해 러스트 사용해보기 : `let`, `match`, 메소드, associated functions_연관함수, external crates_외부 크레이트를 어떻게 사용하는지 배울 수 있다.

추리게임 : 1~100 사이의 임의의 정수를 생성한다. 사용자는 추리한 정수를 입력한다. 프로그램은 입력받은 추리 갑보다 정답이 높은지, 낮은지를 알려준다. 정답을 맞히면 프로그램은 종료된다.

## 새로운 프로젝트 준비

```bash
$ cargo new guessing_game --bin
$ cd guessing_game
```

`cargo new`는 *Cargo.toml* 파일과 *src/main.rs* 파일을 생성한다. Cargo가 생성한 이 프로젝트의 소스에는 기본적인 Hello, world 예제가 들어있다.

Filename: src/main.rs

```rust
fn main() {
    println!("Hello, world!");
}
```

`cargo run`을 이용하여 Hello world문을 컴파일하고 실행해본다.

```bash
$ cargo run
   Compiling guessing_game v0.1.0 (.../TIL/rust/rust-lang-book-ko/projects/02/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 0.44s
     Running `target/debug/guessing_game`
Hello, world!
```

## 추리값 처리하기

프로그램의 첫 부분은 사용자의 입력을 받고, 입력값을 처리한 후 입력값이 원하는 형식인지 검증한다. 아래 내용을 *src/main.rs* 에 작성한다.

Filename: src/main.rs

```rust
use std::io;

fn main() {
    println!("Guess the number!");
    
    println!("Please input your guess.");
    
    let mut guess = String::new();

    io::stdin().read_line(&mut guess)
        .expect("Failed to read line");
    
    println!("You guessed: {}", guess);
}
```

이 코드는 사용자의 입력을 받아 그대로 출력하는 코드이다. 사용자의 입력을 받기 위해서는 `io` 라이브러리를 사용해야 한다. `io` 라이브러리는 표준 라이브러리 `std`에 속해 있다.

```rust
use std:io;
```

러스트는 모든 프로그램의 스코프에 *prelude* 내 타입들을 가져온다. 만약 원하는 타입이 prelude에 없다면, `use`문을 이용하여 명시적으로 그 타입을 가져와야 한다. `std::io`는 사용자의 입력을 받는 것을 포함, `io`와 관련된 기능을 제공한다.

* prelude: The Rust Prelude.
prelude는 러스트가 모든 프로그램에 불러오는 리스트이다. 러스트는 모든 크레이트의 root에

```rust
extern crate std;
```

를 삽입하고, 모든 모듈에는

```rust
use std::prelude::v1::*;
```

를 삽입한다.


### 값을 변수에 저장하기

코드에서 새로운 부분이 있다. 바로 변수를 생성하는 `let`문이다.

```rust
let mut guess = String::new();
```

변수는 다음과 같이 선언할 수도 있다.

```rust
let foo = bar;
```

이는 `foo`라는 변수를 선언하고 `bar`라는 값과 **바인딩** 한다. 러스트에서 **변수는 기본적으로 불변**이다. `let` 다음에 오는 `mut`는 이를 mutable variable가변변수로 만들어준다.

```rust
let foo = 5; // immutable
let mut bar = 5; // mutable
```

이제 이 라인에서는 `guess`라는 이름의 가변변수가 선언되었음을 알 수 있다. 이 라인의 등호 오른쪽의 내용은 `String::new()` 함수의 결과값인 새로운 `String` 인스턴스가 바인딩의 대상이 된다. `String`은 표준 라이브러리에서 제공하는 확장가능한(growable) UTF-8 인코딩 문자열 타입이다.

`::new`에 있는 `::`는 `new`가 `String` 타입의 연관함수임을 나타낸다. 연관함수는 하나의 타입을 위한 함수이고, 이 경우에는 생성된 `String` 인스턴스 하나가 아니라 `String` 타입을 위한 함수이다. 이는 정적 메소드라고 생각하면 된다.

`new` 함수는 새로운 빈 `String` 인스턴스를 생성한다. `new` 함수는 새로운 값을 생성하기 위한 일반적인 이름이므로 많은 타입에서 찾아볼 수 있다.

다음 줄에서, `io`의 연관함수인 `stdin`을 호출한다.

```rust
io::stdin().read_line(&mut guess)
    .expect("Failed to read line");
```

만약 프로그램 시작점에 `use std::io`가 없다면 함수 호출시 `std::io::stdin` 처럼 작성해야 한다. 이 함수는 터미널 표준 입력의 핸들 타입인 `std::io::Stdin` 인스턴스를 반환한다.

이 코드의 `.read_line(&mut guess)`는 사용자로부터 입력을 받기 위해 표준 입력 핸들에서 `.read_line` 메소드를 호출한다. 또한 이 함수에 `$mut guess`를 인자로 넘긴다.

`read_line`은 사용자가 표준 입력에 입력할 때마다 입력된 문자를 문자열에 저장하므로, 인자로 값을 저장할 문자열이 필요하다. 문자열 인자는 사용자의 입력을 추가하면서 변경되므로 가변이어야 한다.

`&`는 코드의 데이터를 여러번 메모리로 복사하지 않고 접근하기 위한 방법을 제공하는 *Reference참조자*임을 나타낸다. 참조자는 매우 복잡한 특성인데, 러스트의 대표적인 장점 중 하나가 바로 이 참조자를 얼마나 쉽고 안전하게 사용할 수 있는가에 있다.
([이 부분](https://rinthel.github.io/rust-lang-book-ko/ch02-00-guessing-game-tutorial.html)은 번역에서 고쳐도 괜찮을 것 같다.)
뒤에 참조자에 대한 자세한 설명이 나온다. 참조자는 변수처럼 기본적으로 불변이다.
따라서 가변으로 바꾸기 위해 `&guess`가 아닌 `&mut guess`라고 적어야 한다.

### `Result` 타입으로 잠재적인 오류 다루기

이 다음 라인의 딸려오는 부분은 메소드이다.

```rust
.expect("Failed to read line");
```

`.foo()`의 형태로 메소드를 호출할 경우 가독성이 떨어지기 때문에, 긴 라인을 나누기 위해 newline과 공백을 넣는 것이 낫다.

```rust
io::stdin().read_line(&mut guess).expect("Failed to read line");
```

`read_line` 함수는 앞서 설명되어 있는 인자로 넘긴 문자열에 사용자의 입력을 저장하는 것 이외에도 하나의 값을 돌려준다. 이 돌려준 값은 `io::Result` 이다. 러스트는 표준 라이브러리에 여러 종류의 `Result` 타입을 가지고 있다. Generic `Result` 또는 `io::Result`가 그 예시이다.

`Result` 타입은 *enumerations열거형* 으로써 *enums*라고 부르기도 한다. 열거형은 정해진 값들만을 가질 수 있으며, 이 값들은 열거형의 *variants*라고 부른다. 뒤에 더 자세한 설명이 나온다.

이러한 `Result`는 에러 처리를 위한 정보를 표현하기 위해 사용된다. `Result`타입의 값들은 다른 타입들처럼 메소드를 가지고 있는데, 이 중 하나가 바로 `expect` 메소드이다.
만약 `io::Result` 인스턴스가 `Err`일 경우 `expect` 메소드는 프로그램을 멈추고 `expect`에 인자로 넘겼던 메세지를 출력하도록 한다.
만약 `read_line`이 `Err`를 반환했다면 이는 운영체제의 에러일 수 있다. 만약 `io::Result`의 결과가 `Ok`라면 expect는 `Ok`가 가지고 있는 결과값을 반환해 이 값을 사용할 수 있도록 한다. 이 때 `Ok`의 결과값은 사용자가 표준 입력으로 입력한 것의 바이트수(string의 경우 글자 수 + 1)를 가지고 있다.

만약 expect를 호출하지 않으면 컴파일은 가능하지만 경고가 나타난다.

```bash
$ cargo build
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
warning: unused `std::result::Result` which must be used
  --> src/main.rs:10:5
   |
10 |     io::stdin().read_line(&mut guess);
   |     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
   |
   = note: #[warn(unused_must_use)] on by default
```

러스트는 `read_line`의 반환값인 `Result`를 사용하지 않았다는 것을 경고하며 에러를 처리하지 않았다는 것을 알려준다. 뒤에 에러 처리에 대한 방법 또한 나온다.

### `println!`의 *placeholder변경자*를 이용한 값 출력

마지막으로 입력한 줄에는 다음과 같은 코드가 있다.

```rust
println!("You guessed: {}", guess);
```

이 코드는 사용자가 입력한 값을 저장함 문자열 `guess`를 출력한다. `{}`는 변경자로써 값이 표시되는 위치를 나타낸다. `{}`를 이용하여 하나 이상의 값을 표시할 수도 있다. 다음과 같이 여러 개의 값을 표시할 수 있다.

```rust
let x = 5;
let y = 10;

println!("x = {} and y = {}", x, y);
```

이 코드는 `x = 5 and y = 10`을 출력한다.

### 작성한 코드 테스트하기

여기까지 작성한 코드를 테스트한다. `cargo run`을 이용한다.

```bash
$ cargo run
    Compiling guessing_game v0.1.0 (.../TIL/rust/rust-lang-book-ko/projects/02/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 0.62s
     Running `target/debug/guessing_game`
Guess the number!
Please input your guess.
10
You guessed: 10
```

## 비밀 숫자 생성하기

사용자가 추리하기 위한 비밀 숫자를 하나 생성해야 한다. 게임을 다시 실행하면 비밀번호는 매번 바뀌어야 한다. 1에서 100 사이의 임의의 수를 하나 사용하도록 한다. 러스트에는 표준 라이브러리에 아직 랜덤 기능을 포함하지 않는다. 하지만 `rand` 크레이트를 사용할 수 있다.

### *Crate크레이트*를 이용하여 더 많은 기능 가져오기

크레이트는 러스트 코드의 패키지이다. 현재 작성하고 있는 프로젝트는 실행이 가능한 *binary crate* 이다. `rand` 크레이트는 다른 프로그램에서 사용하기 위한 *library crate* 이다.

외부 크레이트를 사용하기 위해서는 *Cargo.toml*을 수정하여 `rand` 크레이트를 의존성 리스트에 추가해야 한다.

Filename: Cargo.toml

```toml
[dependencies]

rand = "0.5.5"
```

*Cargo.toml* 파일에서 헤더 뒤에 나오는 부분은 하나의 섹션으로, 이 섹션은 다음 섹션이 시작하기 전(새로운 헤더가 나오기 전)까지 같은 섹션으로 본다. `[dependencies]` 섹션은 Cargo에게 현재 프로젝트가 어떤 크레이트의 어느 버젼에 의존하고 있는지를 적는 부분이다. Cargo는 버전 기록의 표준인 Semantic Versioning(SemVer)을 지원한다. `0.5.5`라고 적은 것은 사실 `^0.5.5`의 축약으로, "0.5.5 버전과 호환 가능한 버전을 사용한다"는 뜻이다.

다른 것을 바꾸지 않은 채, 프로젝트를 빌드하면 다음과 같이 실행된다.

```bash
$ cargo build
    Updating crates.io index
  Downloaded rand v0.5.6
  Downloaded rand_core v0.3.1
  Downloaded libc v0.2.67
  Downloaded rand_core v0.4.2
   Compiling libc v0.2.67
   Compiling rand_core v0.4.2
   Compiling rand_core v0.3.1
   Compiling rand v0.5.6
   Compiling guessing_game v0.1.0 (.../TIL/rust/rust-lang-book-ko/projects/02/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 53.48s
```

다른 버젼이나 라인의 순서가 다를 수 있다. 하지만 이는 SemVer가 관리해주는 부분이기 때문에 현재 코드와 호환된다.

이로써 외부 의존성이 생겼고, Cargo는 Crates.io의 복사본인 registry에서 모든 것을 가져온다. Crates.io는 러스트의 오픈소스 공유 공간이다.

레지스트리를 업데이트하면 Cargo는 `[dependencies]`의 내용을 확인하고 현재 가지고 있지 않은 것을 다운로드 받는다. `rand`는 `libc`에 의존하기 때문에 `libc`도 다운받게 된다.

러스트는 이것들을 다운받고 컴파일한 다음, 이제 의존성이 해결된 프로젝트를 컴파일한다.

이 때 만약 아무 것도 변경하지 않고 `cargo build`를 실행했다면 아무 것도 출력되지 않는다. Cargo는 의존 패키지를 컴파일했고, *Cargo.toml*에 변경사항이 생기지 않은 것을 알고 있다. 코드에도 변경사항이 없으므로 아무 것도 컴파일하지 않고 바로 종료되는 것이다.

만약 *src/main.rs*에 사소한 변경을 한 후 다시 `cargo build`를 실행하게 되면 Cargo는 의존 패키지를 컴파일하는 과정 없이 바로 코드만 컴파일하게 된다. 이는 Cargo가 이미 다운 받은 의존 패키지를 재사용한 다는 것을 알려준다.

### 재현 가능한 빌드를 보장하는 *Cargo.lock*

Cargo는 작성자 뿐만 아니라 어느 누구라도 코드를 빌드할 경우, 같은 산출물이 나오도록 보장하는 방법을 가지고 있다. Cargo는 다른 의존성을 추가하기 전까지는 명시된 의존 패키지만을 사용한다.

하지만, 만약 `rand`의 `v0.5.5` 버전에서는 잘 동작하던 코드가 `v0.5.6`에서는 오류가 나도록 `rand` 크레이트가 업데이트 되었다면 어떻게 될까?

처음 프로젝트를 빌드할 때, Cargo는 *Cargo.lock*에 모든 패키지의 버전을 기록한다. 만약 의존하는 패키지가 업데이트 되었더라도, Cargo는 *Cargo.lock*이 있는지 우선 확인하고, 이 안에 명시된 버전을 사용한다. *Carco.lock*은 사용자가 명시적으로 업그레이드 하지 않는 이상 `v0.5.5`를 사용하게 된다.

### 크레이트 새로운 버전으로 업그레이드하기

정말 업그레이드가 필요한 경우를 위해 Cargo는 `update` 명령어를 제공한다. 이 명령어는 *Cargo.lock* 파일을 무시하고, *Cargo.toml*에 명시된 요구사항에 맞는 최신 버전을 확인하고, 이 버전으로 문제가 없다면 Cargo는 해당 버전을 *Cargo.lock*에 기록한다.

현재 `rand v0.5.5`를 가지고 있는 상태에서 `cargo update`를 실행하면, *Cargo.toml*에 설정된 내용(`rand = "0.5.5"`)에 따라 Cargo는 `0.5.5` 보다 크고 `0.6.0` 보다 작은 버전을 찾게 될 것이다. 만약 `rand` 크레이트가 새로운 버젼 `0.5.6`과 `0.6.0`을 릴리즈했다면 `cargo update`의 실행 결과는 다음과 같을 것이다.

```bash
$ cargo update
    Updating crates.io index
      Adding fuchsia-cprng v0.1.1
    Removing fuchsia-zircon v0.3.3
    Removing fuchsia-zircon-sys v0.3.3
    Updating rand v0.5.5 -> v0.5.6
    Removing rand_core v0.2.2
```

이 때 *Cargo.lock* 파일의 `rand` 크레이트 버전이 `0.5.6`으로 수정된 것을 볼 수 있다.

만약 `rand`의 `0.5.x`를 사용하려고 한다면, *Cargo.toml*의 내용을 다음과 같이 바꾸면 된다.

```toml
[dependencies]
rand = "0.5.0"
```


### 임의의 숫자 생성하기

Filename: src/main.rs

```rust
extern crate rand;

use std::io;
use rand::Rng;

fn main() {
    println!("Guess the number!");

    let secret_number = rand::thread_rng().gen_range(1,101);

    println!("The secret number is : {}", secret_number);
    
    println!("Please input your guess.");
    
    let mut guess = String::new();

    io::stdin().read_line(&mut guess)
        .expect("Failed to read line");
    
    println!("You guessed: {}", guess);
}
```

`extern crate rand;`를 추가하여 외부에 의존하는 크레이트가 있음을 알린다. 이는 `usr rand`로도 표기할 수 있다. 이제 `rand::`를 앞에 붙여 `rand`내의 모든 것을 호출할 수 있다.

다음으로, `use rang::Rng`를 추가한다. `Rng`는 정수 생성기가 구현한 메소드를 정의한 trait이며 이 메소드를 이용하기 위해서는 반드시 스코프 내에 있어야 한다. trait는 10장에서 다뤄진다.

`rand::thread_rng` 함수는 OS가 시드를 정하고 **현재 스레드에서만 사용되는 정수 생성기**를 돌려준다.

그 다음 `.get_range()` 메소드를 호출하는데, 이는 `Rng` trait에 정의되어 있으므로 `use rand:Rng`를 통해  두 개의 숫자를 인자로 받은 후 두 숫자 사이에 있는 임의의 숫자를 생성한다. `1`과 `101`을 넘기면 1~100 사이의 임의의 숫자를 생성한다.

    Note: 크레이트에서 어떤 trait를 사용할지, 어떤 함수와 메소드를 사용할지는 그냥 알 수 없을 것이다. 각 크레이트에는 어떻게 사용해야 하는지에 대한 문서가 존재하는데, 이를 손쉽게 볼 수 있는 방법을 Cargo가 제공한다. `cargo doc --open` 명령어를 사용하면 **현재 작업중인 프로젝트를 포함**하여 의존성이 있는 모든 크레이트에 대해 문서를 빌드하여 로컬 브라우저에 띄워준다.

코드에 추가한 두 번째 라인은 비밀 숫자를 표시한다. 이 라인은 개발 중에만 사용하도록 할 것이다.

이제 프로그램을 몇 번 실행해본다.

```bash
$ cargo run
    Finished dev [unoptimized + debuginfo] target(s) in 0.01s
     Running `target/debug/guessing_game`
Guess the number!
The secret number is : 20
Please input your guess.
4
You guessed: 4

$ cargo run
    Finished dev [unoptimized + debuginfo] target(s) in 0.01s
     Running `target/debug/guessing_game`
Guess the number!
The secret number is : 50
Please input your guess.
4
You guessed: 4
```

매 실행 때마다 다른 숫자가 나오는 것을 확인할 수 있다.

## 비밀 숫자와 추리값 비교하기

이제 우리는 입력값`guess`와 임의의 정수`secret_number`를 가지고 있으므로, 비교가 가능하다.

```rust
P
```

`std::cmp::Ordering`을 통해 새로운 요소를 스코프로 가져온다. `Ordering`은 `Result`와 같은 *enumeration열거형*이지만, `Ordering`의 값은 `Less`, `Greater`, `Equal`이다. 이들은 두 값을 비교했을 때 나올 수 있는 결과이다.

다음으로 `Ordering` 타입을 사용하는 다섯 줄을 추가하였다.

```rust
match guess.cmp(&secret_number) {
    Ordering::Less     => println!("Too small !"),
    Ordering::Greater  => println!("Too Big !"),
    Ordering::Equal    => println!("You Win !"),
}
```

`cmp` 메소드는 두 값을 비교하고, *비교 가능한 모든 것*에 대해 호출할 수 있다. 이 메소드는 비교하고 싶은 것들의 참조자를 받는다. 여기서는 `guess` 와 `secret_number`를 비교하고 있다. `cmp`는 `Ordering` 열거형을 돌려준다. `match` 표현문을 이용하면 `cmp`가 `guess`와 `secret_number`를 비교한 결과인 `Ordering`의 값에 따라 무엇을 할지 결정할 수 있다.

`match` 표현식은 *arms*로 이루어져 있다. 하나의 arm은 *pattern패턴*과 코드로 구성되어 있는데, 만약 `match` 표현식의 시작에 주어진 값이 해당 arm의 패턴과 맞는다면 코드가 실행된다. 러스트는 `match`에 주어진 값이 arm의 패턴에 맞는지 순서대로 확인한다. `match` 생성자와 패턴들은 코드가 마주칠 다양한 상황을 표현할 수 있도록 하고, 모든 경우의 수를 처리했음을 확신할 수 있도록 도와주는 특성이다. 는 6장, 18장에서 더 자세히 다뤄진다.

예제에서 사용된 `match` 표현식의 흐름을 따라가 보자. `guess`는 50, `secret_number`는 38이라고 가정한다. 50과 38을 비교하는 `cmp` 메소드의 결과는 `Ordering::Greater`이다. `match` 표현식은 `Ordering::Greater`를 값으로 받을 것이다. 이 `match` 표현식은 `Less`, `Greater`, `Equal` 순으로 되어 있으므로 첫 번째 arm의 패턴 `Less`부터 검사를 하게 된다.
첫 번째 arm의 패턴  `Ordering::Less`는 앞선 메소드 `cmp`의 결과인 `Ordering::Greater`와 같지 않으므로, 무시하고 다음 arm으로 넘어간다.
두 번째 arm의 패턴과 `Ordering::Greater`는 들어온 값인 `Ordering::Greater`와 정확히 매칭되므로, 두 번째 arm의 코드가 실행되고 `Too big !`가 출력될 것이다.
마지막 세 번째 arm은 확인할 필요가 없으므로(매칭되는 결과를 앞에서 찾았으므로) `match` 표현식은 끝이 난다.

**하지만, 이 코드는 컴파일 되지 않는다.** 컴파일을 해서 에러를 확인해 본다.

```bash
$ cargo run
   Compiling guessing_game v0.1.0 (.../TIL/rust/rust-lang-book-ko/projects/02/guessing_game)
error[E0308]: mismatched types
  --> src/main.rs:23:21
   |
23 |     match guess.cmp(&secret_number) {
   |                     ^^^^^^^^^^^^^^ expected struct `std::string::String`, found integer
   |
   = note: expected reference `&std::string::String`
              found reference `&{integer}`

error: aborting due to previous error

For more information about this error, try `rustc --explain E0308`.
error: could not compile `guessing_game`.

To learn more, run the command again with --verbose.
```

이 에러는 *mismatched types 일치하지 않는 타입*이 있다고 알려준다. 러스트는 **강한 정적 타입 시스템**을 가지고 있다. 하지만 러스트는 타입 추론 또한 수행한다. 만약 `let guess = String::new()`를 작성한다면, 러스트는 `guess`가 `String`타입임을 추론할 수 있으므로 타입을 따로 명시하지 않아도 된다. 반대로 `secret_number`는 정수형이다. 몇몇 숫자 타입이 1~100 사이 값을 가질 수 있다. `i32`-int32, `u32`-unsigned int32, `i64`-int64 등이 있다. 러스트는 기본적으로 다른 정수형임을 추론할 수 있는 타입 정보를 제공하지 않는다면 숫자를 `i32`로 간주한다. 이 에러의 원인은 러스트가 문자열과 정수형을 비교하지 않기 때문이다.

따라서 우리는 추리 값을 정수형으로 바꾸기 위해 입력으로 받은 `String`을 정수로 바꿔야 할 것이다. 이는 `main`함수 내에 다음 코드를 추가하여 구현할 수 있다.

Filename: src/main.rs

```rust
extern crate rand;

use std::io;
use std::cmp::Ordering;
use rand::Rng;

fn main() {
    println!("Guess the number!");

    let secret_number = rand::thread_rng().gen_range(1,101);

    println!("The secret number is : {}", secret_number);
    
    println!("Please input your guess.");
    
    let mut guess = String::new();

    io::stdin().read_line(&mut guess)
        .expect("Failed to read line");

    let guess: u32 = guess.trim().parse()
        .expect("Please type a number!");
    
    println!("You guessed: {}", guess);

    match guess.cmp(&secret_number) {
        Ordering::Less     => println!("Too small !"),
        Ordering::Greater  => println!("Too Big !"),
        Ordering::Equal    => println!("You Win !"),
    }
}
```

```rust
let guess: u32 = guess.trim().parse()
    .expect("Please type a number!");
```

이 코드에서 `guess` 변수를 생성하였다. 하지만 기존에 `guess`라는 이름의 변수가 이미 생성되어 있었는데도 실행되는 것은 러스트가 이전에 있던 변수 `guess`의 값을 *가리는 것shadowing*을 허락하기 때문이다. 이 특성은 하나의 값을 현재 타입에서 다른 타입으로 변환하고 싶을 경우에 사용한다. `guess_str`, `guess`처럼 고유의 변수명을 만드는 대신, shadowing을 이용하면 `guess`를 재사용 가능하다. (3장에서 더 자세하게 다룬다.)

이 코드에서 우리는 `guess`를 `guess.trim().parse()` 표현식과 바인딩한다. 표현식 내의 `guess`는 원래의 `String` 타입 `guess`를 참조하고 있다.

`String` 인스턴스의 `trim`메소드는 처음과 끝 부분의 빈칸을 제거한다. `u32`는 정수형 글자만을 가져야 하지만, 사용자는 `read_line`을 끝내기 위해 enter키를 눌러야 한다. enter키가 눌리는 순간 문자열에는 개행문자(newline character)가 추가된다. 만일 5 + enter를 누른 경우 `guess`는 `5\n`과 같은 값이 된다. `trim` 메소드는 `\n`을 제거하고 `5`만 남도록 처리한다.

문자열에서 `parse` 메소드는 문자열을 숫자형으로 파싱한다. 이 메소드는 다양한 종류의 정수형을 반환하므로, `let guess: u32`와 같이 정확한 타입을 명시해야 한다. `guess` 뒤의 콜론(`:`)은 변수의 타입을 명시했음을 의미한다.  `u32`는 작은 양수를 표현하기 좋은 선택이다. 이 때 명시한 `u32`와 `secret_number`와의 비교는 러스트가 `secret_number`의 타입 또한 `u32`로 추론할 것이라는 것을 의미한다. 이제 이 비교는 같은 타입을 가진 두 값의 비교가 된다.

`parse` 메소드는 에러가 발생하기 쉽다. `A👍%` 등의 문자가 포함되어 있다면 정수로 바꿀 방법이 없다. `read_line`과 비슷하게 `parse` 메소드는 실패할 경우를 대비해 `Result` 타입을 결과로 반환한다. 만약 `parse`가 파싱을 실패하여 `*Err* Result` variant를 반환한다면, `expect`의 호출은 게임(프로그램)을 종료하고 주어진 메세지를 출력하도록 한다. 만약 `parse` 메소드가 문자열을 정수로 바꾸는데 성공하였다면, `Result`의 `Ok` variant를 돌려받으므로, `expect`는 그 `Ok` 값에서 우리가 원하는 숫자를 반환할 것이다.

프로그램을 실행해본다.

```bash
$ cargo run
   Compiling guessing_game v0.1.0 (.../TIL/rust/rust-lang-book-ko/projects/02/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 0.65s
     Running `target/debug/guessing_game`
Guess the number!
The secret number is : 7
Please input your guess.
   10
You guessed: 10
Too Big !
```

## 반복문으로 여러 번 반복하기

`loop` 키워드는 무한루프를 제공한다.

Filename: src/main.rs

```rust
extern crate rand;

use std::io;
use std::cmp::Ordering;
use rand::Rng;

fn main() {
    println!("Guess the number!");

    let secret_number = rand::thread_rng().gen_range(1,101);

    println!("The secret number is : {}", secret_number);
    
    println!("Please input your guess.");
    
    let mut guess = String::new();

    io::stdin().read_line(&mut guess)
        .expect("Failed to read line");

    let guess: u32 = guess.trim().parse()
        .expect("Please type a number!");
    
    println!("You guessed: {}", guess);

    match guess.cmp(&secret_number) {
        Ordering::Less     => println!("Too small !"),
        Ordering::Greater  => println!("Too Big !"),
        Ordering::Equal    => println!("You Win !"),
    }
}
```

단, 이렇게만 적는다면 답을 맞춰도 계속 반복문이 실행된다는 것을 볼 수 있다. 이를 `parse` 메소드가 실패하도록 만들면(숫자가 아닌 문자를 넣어주면) 무한 반복의 늪에서 빠져나올 수는 있다.

```bash
$ cargo run
   Compiling guessing_game v0.1.0 (.../TIL/rust/rust-lang-book-ko/projects/02/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 0.58s
     Running `target/debug/guessing_game`
Guess the number!
The secret number is : 24
Please input your guess.
25
You guessed: 25
Too Big !
Please input your guess.
23
You guessed: 23
Too small !
Please input your guess.
24
You guessed: 24
You Win !
Please input your guess.
quit
thread 'main' panicked at 'Please type a number!: ParseIntError { kind: InvalidDigit }', src/libcore/result.rs:1188:5
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace.
```

### 정답을 맞출 때 종료하기

정답이 맞으면 종료할 수 있도록 `break`문을 추가한다.

```rust
extern crate rand;

use std::io;
use std::cmp::Ordering;
use rand::Rng;

fn main() {
    println!("Guess the number!");

    let secret_number = rand::thread_rng().gen_range(1,101);

    println!("The secret number is : {}", secret_number);
    
    println!("Please input your guess.");
    
    let mut guess = String::new();

    io::stdin().read_line(&mut guess)
        .expect("Failed to read line");

    let guess: u32 = guess.trim().parse()
        .expect("Please type a number!");
    
    println!("You guessed: {}", guess);

    match guess.cmp(&secret_number) {
        Ordering::Less     => println!("Too small !"),
        Ordering::Greater  => println!("Too Big !"),
        Ordering::Equal    => {
            println!("You Win !");
            break;
        },
    }
}
```

`break`문을 `Ordering::Equal` 후 처리되는 함수의 마지막 부분에 추가함으로써 정답을 맞힌 후 반복문이 종료될 수 있도록 해주다.

### 잘못된 입력값 처리하기

사용자가 숫자가 아닌 값을 입력했을 때, 종료되는 동작을 더욱 다듬어 숫자가 아닐 경우 사용자가 계속 입력할 수 있도록 해보자. `guess`가 `String`에서 `u32`로 변환되는 라인을 수정하면 가능하다.

```rust
let guess: u32 = match guess.trim().parse() {
    Ok(num) => num,
    Err(_) => continue,
};
```

`expect` 메소드 호출을 `match` 표현식으로 바꾸는 것은 에러 발생시 종료에서 에러 처리로 바꾸는 일반적인 방법이다. `parse` 메소드가 `Result` 타입을 돌려주는 것과 `Result`는 `Ok` 또는 `Err` variants를 가진 열거형임을 기억하자. 

`parse`가 입력값을 문자열에서 정수로 변환하는데 성공하였다면 `Ok`를 돌려줄 것이고, 첫 번째 arm의 패턴과 매칭이 되고, `match` 표현식은 `parse`가 생성한 `num` 값을 돌려줄 것이다. 이 값은 이 위 라인의 새로운 `guess`와 바인딩 될 것이다.

만약 `parse`가 문자열을 정수로 바꾸지 못했다면, 에러 정보를 가진 `Err`를 돌려준다. `Err`는 첫 번째 arm의 패턴과 매칭하지 않지만, 두 번째 패턴 `Err(_)`과 매칭됩니다. `_`는 모든 값과 매칭될 수 있다. 이 예시에서는 `Err` 내에 어떤 값이 있던 간에 모든 `Err`를 매칭하도록 했다. 따라서 이 프로그램은 해당 arm의 코드인 `continue`를 실행하며, 이 코드는 `loop`의 다음 반복으로 가서 다른 추리값을 요청하도록 한다. 프로그램은 `parse`에서 가능한 모든 에러를 효과적으로 무시한다.

이제 마지막으로 비밀 숫자를 출력하는 `println!` 라인을 삭제하면 최종프로그램은 완성이다.

```rust
extern crate rand;

use std::io;
use std::cmp::Ordering;
use rand::Rng;

fn main() {
    println!("Guess the number!");

    let secret_number = rand::thread_rng().gen_range(1,101);

    loop {
    
        println!("Please input your guess.");
    
        let mut guess = String::new();

        io::stdin().read_line(&mut guess)
            .expect("Failed to read line");

        let guess: u32 = match guess.trim().parse() {
            Ok(num) => num,
            Err(_) => continue,
        };
        
        println!("You guessed: {}", guess);

        match guess.cmp(&secret_number) {
            Ordering::Less     => println!("Too small !"),
            Ordering::Greater  => println!("Too Big !"),
            Ordering::Equal    => {
                println!("You Win !");
                // break;
            },
        }
    }   
}
```

## 정리

이 프로젝트에서는 `let`, `match`, 메소드, 연관함수, 외부 크레이트 사용 등의 새로운 러스트 개념을 소개하기 위한 실습이었다. 3장에서는 대부분의 프로그래밍 언어가 가지는 변수, 데이터타입, 함수를 소개하고 러스트에서의 사용법을 다룬다. 4장에서는 다른 프로그래밍 언어와 차별화된 러스트만의 특성인 소유권을 다룬다. 5장에서는 구조체외 메소드 문법을, 6장에서는 열거형에 대해 다룬다.
