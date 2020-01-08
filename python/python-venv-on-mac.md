# python-venv-on-mac

## python venv

> 맥은 `python 2.x`를 기본으로 가지고 있기 때문에 터미널상에서 `python 3.x`의 문법으로 작성된 `.py` 파일을 실행하면 정상적으로 실행되지 않는다. 때문에 `python 3.x` 버전을 실행하기 위해서는 `python3`로 실행해야 한다.

### 설치

- 파이썬 버전 확인하기

```bash
python3 -V # 또는 python3 --version
Python 3.7.3
```

- venv를 설치할 디렉토리 생성

```bash
mkdir venv
```

- venv 설치

```bash
python3 -m venv /path/to/new/virtual/environment
```

```bash
# or below
cd venv
python3 -m venv .
```

### 실행

```bash
# macOS
source ./venv/bin/activate

# bash in windows
source ./venv/Scripts/activate
```

extra) alias 설정으로 명령어 등록하기

- 터미널, bash, zsh(macOS), git bash(windows) 등의 환경에서 alias(별명)를 등록해 명령어처럼 실행할 수 있다.

- 방법 1 .bash_profile, .bashrc, .zshrc 등의 설정 파일을 열어 내용을 수정한다.

```bash
# 설정 파일 열기
# nano 등의 커맨드라인 편집기, sublime text, 메모장 등의 텍스트 편집기로 열어도 무방하다.
vim ~/.bash_profile
vim ~/.bashrc
vim ~/.zshrc # (기본 셸이 zsh일 경우)

# 맨 아래 줄에 내용 추가
# ~/.bash_profile(현재 파일)
...
alias venv="source ~/venv/bin/activate"
```

- 방법 2 echo 명령어로 .bash_profile, .bashrc, .zshrc의 내용을 추가한다.

```bash
echo 'alias venv="source ~/venv/bin/activate"' >> ~/.bash_profile
```