# 1-3 Hello, Cargo !

## Cargo

Cargo는 러스트의 **빌드 시스템 및 패키지 매니저**이다. 대부분의 러스트를 사용하는 사람들이 이 도구를 이용하여 러스트 프로젝트를 관리한다.

Cargo는 코드를 빌드하고, 코드가 의존하고 있는 라이브러리를 다운로드하고, 라이브러리를 빌드하는 등 많은 작업을 다룬다.

Cargo가 설치되어있는지 확인하려면 다음을 입력하면 된다.

```bash
$ cargo --version
cargo 1.41.0 (626f0f40e 2019-12-03)
```

만약 정상적으로 버전이 출력되지 않는다면, 설치되어있지 않은 것이다. 이 경우 Cargo를 개별적으로 설치하면 된다.

## Cargo를 사용하여 프로젝트 생성

Cargo를 사용하여 마치 `npm init`을 하듯이 프로젝트를 생성할 수 있다.

```bash
$ cargo new hello_cargo --bin
$ cd hello_cargo
```

hello_cargo 디렉토리에는 `Cargo.toml`과 `src/main.rs`가 생성되어 있다. 또한 새로운 git 저장소를 생성하고, _.gitignore_ 파일을 생성한다.

```
hello_cargo/
    .gitignore
    Cargo.toml
    src/
        main.rs
```

Cargo.toml에는 다음과 같은 내용이 적혀 있다.

Filename: Cargo.toml

```toml
[package]
name = "hello_cargo"
version = "0.1.0"
authors = ["Leo Kim <gh.leokim@gmail.com>"]
edition = "2018"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]
```

이 파일은 TOML 포맷의 파일이다. 이는 Cargo의 환경설정 포맷이다.

`[package]` 부분은 패키지의 환경 설정, `[dependencies]` 부분은 이 프로젝트의 의존성 리스트를 관리하는 섹션이다.

러스트에서는 코드 패키지를 _crate 크레이트_ 라고 부른다. 

`src/main.rs` 에는 hello world 프로젝트에서 작성했던 것과 같은 내용의 코드가 들어 있다.

Filename: src/main.rs

```rust
fn main() {
    println!("Hello, world!");
}
```

다른 점은 _src_ 디렉토리 내에 코드가 존재하고, 최상위 디렉토리에는 환경 파일을 가지게 된다는 점이다. Cargo는 소스 파일이 _src_ 디렉토리 내에 있을 것으로 예상한다.

## Cargo 프로젝트를 빌드하고 실행하기

**`cargo build`**

*hello_cargo* 디렉토리에서 `cargo build` 커맨드를 입력하여 프로젝트를 빌드할 수 있다.

```bash
$ cargo build
   Compiling hello_cargo v0.1.0 (.../TIL/rust/rust-lang-book-ko/projects/hello_cargo)
    Finished dev [unoptimized + debuginfo] target(s) in 1.32s
```

이 커맨드는 *target/debug_hello_cargo* 에 실행파일을 생성한다. 다음 커맨드로 빌드한 파일을 실행한다.

```bash
$ ./target/debug/hello_cargo # or .\target\debug\hello_cargo.exe on Windows
```

또한 `cargo build`를 실행하면 최상위 디렉토리에 *Cargo.lock* 이라는 파일을 생성한다.

**`cargo run`**

앞선 내용에서는 `cargo build`로 프로젝트를 빌드하고 `./target/debug/hello_cargo` 로 이를 실행했지만, `cargo run`을 사용하면 한 번의 커맨드로 코드를 컴파일하고 실행파일을 바로 실행할 수 있다.

```bash
$ cargo run
    Finished dev [unoptimized + debuginfo] target(s) in 0.00s
     Running `target/debug/hello_cargo`
Hello, world!
```

위 실행 결과에서는 컴파일 중이라는 출력이 나오지 않는다. 코드가 수정된 적이 없기 때문에 Cargo는 변경사항을 확인하고 필요한 경우 다시 빌드하기 때문이다.

만약 코드를 수정한 적이 있다면 아래와 같이 컴파일 후 실행되고 있다는 명령이 나왔을 것이다.

```bash
$ cargo run
   Compiling hello_cargo v0.1.0 (.../TIL/rust/rust-lang-book-ko/projects/hello_cargo)
    Finished dev [unoptimized + debuginfo] target(s) in 0.34s
     Running `target/debug/hello_cargo`
Hello, world!
```

**`cargo check`**

이외에도 `cargo check` 커맨드가 있다. 이는 코드가 컴파일되는지만 확인해주는 빠른 커맨드이다.

```bash
$ cargo check
    Checking hello_cargo v0.1.0 (.../TIL/rust/rust-lang-book-ko/projects/hello_cargo)
    Finished dev [unoptimized + debuginfo] target(s) in 0.13s
```

단, 이 커맨드는 `cargo build` 처럼 실행 파일을 만들지는 않는다. 실행 파일을 사용하지 않고 단지 코드를 작성하는 동안 검사하는 것이 목적이라면, `cargo check` 를 이용하여 컴파일 되는지 확인할 수 있다.

## 릴리즈 빌드

릴리즈를 위한 준비가 되었다면 `cargo build --release`를 이용하여 최적화와 함께 컴파일 할 수 있다. 이 커맨드는 *target/debug* 대신 *target/release* 디렉토리에 실행 파일을 생성한다. 

## Cargo에 대해 더 알아보기

https://doc.rust-lang.org/cargo/