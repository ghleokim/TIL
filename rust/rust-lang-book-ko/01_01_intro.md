# 1-1 intro

## 시작하기

rust 설치 : `rustup`을 활용하여 설치한다.

```bash
$ curl https://sh.rustup.rs -sSf | sh
```

설치 스크립트는 다음 로그인 이후 러스트를 자동으로 시스템 환경변수에 추가한다.
만약 터미널을 재시작하지 않고 실행하려면 환경변수를 직접 추가해야 한다.

```bash
$ source ~/.cargo/env
```

또는

```bash
$ export PATH="~/.cargo/bin:$PATH"
```

## 업데이트 및 설치 제거

업데이트

```bash
$ rustup update
```

러스트와 rustup 제거

```bash
$ rustup self uninstall
```

## 러스트 버젼 확인하기

```bash
$ rustc --version
```

### references

https://rinthel.github.io/rust-lang-book-ko/ch01-01-installation.html