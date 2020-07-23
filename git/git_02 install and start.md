# git 02 Install and Start Using git

## git 설치하고 사용하기

### git 설치하기

git이 설치되어 있는지 확인하려면 터미널에 `git --version`이라고 입력하면 된다. 현재(2019년 09월) 기준으로 git의 버전은 2.23.0이다.

```shell
$ git --version
$ git version 2.23.0
```

설치되어 있지 않다면 git의 공식 페이지([링크](https://git-scm.com/downloads))로 가서 OS에 맞는 git을 받아 설치할 수 있다. git-scm 페이지의 git-book에는 cli로 설치하는 방법도 설명되어 있다.

### git 사용 시작하기

#### git 최초 설정

##### 사용자 설정

git을 설치하고 나면 사용환경을 설정해주어야 한다. 이는 `git config`라는 도구로 내용을 확인, 변경할 수 있다. 아래는 전역 사용자를 설정하는 방법이다.

```shell
$ git config --global user.name "Leo Kim"
$ git config --global user.email gh.leokim@gmail.com
```

만약 프로젝트마다 다른 사용자 정보를 설정하기 위해서는 `--global` 옵션을 빼고 명령을 실행하면 된다.

##### 편집기

git에서 사용할 텍스트 편집기를 설정하는 옵션은 다음과 같다.

```shell
$ git config --global core.editor vi
```

##### 설정 확인

`git config --list` 명령을 실행하면 설정한 내용을 볼 수 있다.

```shell
$ git config --list
```

### git의 기본 명령어

#### git init: git으로 관리하기

`git init` 명령어는 현재 디렉토리 및 하위 디렉토리를 git으로 관리하도록 초기화시켜준다.

```shell
$ git init
Initialized empty Git repository in /Users/leonardkim/dummy/.git/
```

`git init`을 실행하고 나면 `.git`이라는 하위 디렉토리를 만들어진다. `.git` 디렉토리에는 향후 저장되는 버전 관리 히스토리의 내용이 담긴다.

#### git add, commit, status: 스냅샷 찍기

git은 버전을 해당 순간의 스냅샷(파일의 변화를 기록한 상태)으로 기억한다. 이러한 스냅샷을 찍기 위해 준비하는 과정이 `git add`, 스냅샷을 찍는 과정이 `git commit`이다. git은 이렇게 찍어둔 스냅샷을 `commit`이라고 부른다.

`git status` 명령은 git의 현재 상태를 표시해준다. 가장 최근의 commit을 기준으로 어떤 파일이 변경, 생성 혹은 삭제되었는지를 파악하여 출력한다.

`git status`는 `git add`, `git commit` 과 함께 가장 많이 쓰게 되는 명령어이다. 현재 상태를 파악하는 것은 git 관리에 있어 매우 중요하기 때문에 습관적으로 `git status`를 입력하는 것이 좋다.

- 우선 파일을 하나 생성한다. 여기서는 `a.txt` 파일을 생성하였다.

```shell
$ touch a.txt
$ vi a.txt
```

- `git status`로 현재 상태를 출력해본다.

  `Untracked files:` 이후에 나오는 파일은 git이 관리하고 있지 않던 파일이라는 뜻이다. 만약 관리되던 파일이 변경되었다면 아래처럼 `modified:` 로 표시된다.

```shell
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	a.txt

nothing added to commit but untracked files present (use "git add" to track)
```

```shell
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   a.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

- `git add`로 파일을 staging area(스냅샷을 찍기 위한 공간)로 올려놓는다.

  이 때, 파일 하나만을 stage하거나 디렉토리, 또는 전체 공간을 staging area에 올려놓을 수 있다.

```shell
$ git add .
```

```shell
$ git add a.txt
```

- 다시 한 번 `git status`로 현재 상태를 출력한다.

```shell
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
	new file:   a.txt
```

- `git commit`으로 staged된 파일을 스냅샷으로 찍는다. 이 때 `-m` 옵션을 활용하면 commit message를 바로 삽입할 수 있다. `-m` 옵션을 사용하지 않는다면 commit messge를 텍스트 편집기에서 입력할 수 있다.

```shell
$ git commit -m 'First Commit'
[master (root-commit) aac8259] First Commit
 1 file changed, 1 insertion(+)
 create mode 100644 a.txt
```

#### git push-pull scenario

`git push`, `git pull`은 로컬로 관리되고 있는 git을 여러 공간에서 사용할 수 있도록 원격 저장소인 github, gitlab에 업로드/다운로드 할 수 있도록 설정할 수 있다.