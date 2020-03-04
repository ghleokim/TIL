# 1-2 Hello World

## 프로젝트 디렉토리 만들기

rust는 projects/ 디렉토리 내에 프로젝트를 위한 디렉토리를 만드는 것을 제안한다.

```bash
$ mkdir ~/projects
$ cd ~/projects
$ mkdir hello_world
$ cd hello_world
```

## 러스트 프로그램 작성 및 실행

`main.rs` 라는 새로운 소스 파일 생성.

러스트 파일은 항상 .rs 확장자로 끝난다. 여러 단어를 사용한다면 단어 구분을 위해 _를 사용한다. (예: _hello_world.rs_)

- 파일 작성

Filename: main.rs

```rust
fn main() {
    println!("Hello, World!");
}
```

- 파일 컴파일, 실행

```bash
$ rustc main.rs
$ ./main # window의 경우 .\main.exe 실행
Hello, World!
```

## 내용 살펴보기

- 함수 정의

```rust
fn main() {

}
```

이 라인은 러스트의 함수를 정의한다. `main` 함수는 모든 실행가능한 러스트 프로그램 내에서 **가장 먼저 실행되는 코드**이다. 현재 `main` 함수는 파라미터가 없고, 아무것도 반환하지 않도록 만들어져 있다.

함수는 중괄호 `{}`로 감싸져있다. 모든 러스트 함수는 중괄호로 감싸져 있도록 정의되어 있다. `fn main() {` 와 같이 여는 중괄호를 함수 정의부와 한칸 띄워 작성하는 것이 컨벤션.

참고: `rustfmt`라고 불리는 자동 포맷팅 도구가 개발되어 있다.

- 내부 코드

```rust
    println!("Hello, World!");
```

여기서 봐야 할 중요한 디테일 네 가지가 있다.

1. 러스트는 탭(tab)이 아닌 4-space 들여쓰기를 사용한다.

2. `println!`은 러스트 매크로(macro)라고 불린다. 이는 보통의 함수 대신 매크로를 호출하고 있음을 의미한다.

3. `"Hello, World!"`의 스트링 인자를 println에 넘긴다.

4. 대다수의 러스트 코드라인은 세미콜론 `;`으로 끝난다.

## 컴파일과 실행의 단계

러스트는 컴파일-실행의 두 단게로 나누어져 있고, 이는 각각 개별적인 단계이다.

컴파일을 하기 위해서는 `rustc` 커맨드에 소스코드를 넘겨야 한다. `gcc` 와 `clang`과 유사.

```bash
$ rustc main.rs
```

이 과정을 통해 러스트는 실행 가능한 바이너리 파일을 출력한다.

- Linux, macOS, PowerShell의 경우

```bash
$ ls
main    main.rs
```

- Windows의 cmd환경

```cmd
> dir /B
main.exe
main.pdb
main.rs
```

**러스트는 _ahead-of-time compiled_ 언어이다.** 이는 프로그램을 컴파일하고 실행파일을 넘기면, 러스트를 설치하지 않고도 이를 실행할 수 있다는 의미이다.
