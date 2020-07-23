# git 03 Branching

## git 브랜치로 작업하기

### Branch 개념

브랜치는 버전관리 시스템을 이용하는데 있어 아주 중요한 개념이다. 개발을 하다보면 항상 동작하고 있어야 하는 상태의 버전이 있고, 이 버전과는 별개로 코드를 복사하여 독립적으로 작업해야 하는 버전이 있을 것이다. 이렇게 독립적으로 개발하는 형태 또는 버전이 브랜치이다.

### HEAD

HEAD는 현재 작업중인 디렉토리(또는 브랜치)를 가리킨다. 

### git log

git log는 현재 상태를 기준으로 git의 history를 보여주는 명령이다. 아래는 가장 많이 확인하는 옵션이다.

- `git log`: 모든 히스토리 확인하기

- `git log --graph`모든 히스토리를 그래프로 확인하기
- `git log --oneline`: 각 히스토리를 한 줄로 축약하여 확인하기
- `git log --oneline --graph`: 각 히스토리를 한 줄로 축약하고, 이를 그래프로 확인하기

### git branch, checkout, switch

`git branch`는 현재 존재하는 브랜치를 확인하거나, 새로 만들거나, 지우는 등의 역할을 할 수 있는 명령이다.

- 모든 브랜치 확인하기

```shell
$ git branch
* master
```

- 브랜치 생성하기

  브랜치를 생성하고 모든 브랜치를 확인한다. 이 때 astrerisk`*`는 현재 HEAD가 가리키는 브랜치를 뜻한다.

```shell
$ git branch test
$ git branch
* master
  test
```

- 브랜치 삭제하기

```shell
$ git branch -d test
Deleted branch test (was f0895eb).
```

`git checkout`는 커밋 노드 사이를 이동할 수 있는 명령이다. 브랜치명 / 커밋 해쉬를 가지고 이동할 수 있다.

`git switch`(v2.23.0 new)는 git 2.23.0버전에 새롭게 등장한 명령이다. checkout과는 다르게 작업중인 브랜치 사이를 이동하는데, 이 때는 커밋 해시가 아닌 브랜치명으로 이동해야 한다.

**커밋을 기준으로 이동하는 `checkout`은 `switch`처럼 쓰일 수 있지만, 반대로 `switch`는 `checkout`처럼 쓰일 수는 없다.**