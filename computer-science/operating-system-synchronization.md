## Synchronization

### Example : too much milk!

you and your roommate

```
0. 나- 집 도착
1. 나- 우유가 없다!
2. 나- 우유 사러 출발
3. 룸메이트- 집 도착
4. 나- 슈퍼 도착   룸메이트- 우유가 없다!
5. 나- 우유 삼     룸메이트- 우유 사러 출발
6. 나- 집 도착
7. 룸메이트 - 우유 삼
8. 룸메이트 - 집도착
9. 투머치밀크
```

### 동기화 용어 정리

- **동기화**

    쓰레드간 협업을 보장하기 위해 atomic operation을 사용.

- **Mutual Exclusion(Mutex)**

    특정 동작을 한 번에 한 쓰레드만 하고 있도록 보장하고, 나머지 쓰레드는 동시에 여기 접근하는 것을 막는 것

- **Critical Section**

    한 번에 한 쓰레드만 접근 가능하도록 설정한 코드의 영역.

- **Lock**

    한 프로세스가 접근하는 동안 다른 프로세스가 일을 하지 못하게 하는 메커니즘

    → 임계영역 또는 shared data에 접근하기 전 Lock

    → 임계영역 또는 shared data에 접근이 끝날 때 Unlock

    → 잠겨있는 상태일 때는 기다린다.

⇒ 모든 synchronization은 **waiting**을 수반한다.

### Too much milk: solutions

**solution 1**

```
# Thread A
if (noMilk && noNote) {
	leave note;
	buy milk;
	remove note;
}

# Thread B
if (noMilk && noNote) {
	leave note;
	buy milk;
	remove note;
}
```

이 방법은 제대로 동작하는가? **그렇지 않다.**

→ 양쪽 모두 우유와 노트가 없는 상태를 보고 우유를 사러 갈 가능성 존재. 스케줄러가 쓰레드 A에서 노트를 남기기 전에 쓰레드 B에 접근할 수도 있다.

⇒ safety

**solution 2**

두 개의 노트를 사용하는 방법

```
# Thread A
leave note A
if (noNote B) {
	if (noMilk) {
		buy milk;
	}
}
remove note;

# Thread B
leave note B
if (noNote A) {
	if (noMilk) {
		buy milk;
	}
}
remove note;
```

이 방법은 제대로 동작하는가? **이 역시 그렇지 않다.**

→ 양쪽 모두 노트를 보고 우유를 사지 못할 가능성이 존재. 우유가 있는지도 체크 못할 수 있다.

⇒ liveness

**solution 3**

separate program

```
# Thread A
leave note A
X : while (Note B) {
	do nothing;
}

if (noMilk) {
	buy milk;
}

remove note A;

# Thread B
leave note B
Y if (noNote A) {
	if (noMilk) {
		buy milk;
	}
}
remove note;
```

이 방법은 제대로 동작하는가? **제대로 동작은 할 것** 

⇒ safety와 liveness 모두 충족

**solution3이 좋은 해결책인가?**

- 너무 복잡하다 : 이것이 제대로 동작할지 여부를 판단하는 것은 어렵다.
- 비대칭적이다 : 쓰레드 A와 B는 다르다. 또한, 쓰레드가 여러개 추가될 경우 기존 쓰레드의 코드의 수정이 필요하다.
- A의 busy waiting : A는 일을 하고 있지 않지만 CPU 자원을 먹고 있다.

### 동기화를 위한 언어 지원

프로그래밍 언어가 동기화를 위한 atomic routine을 지원하도록 만들어야 한다.

*atomic이란? interrupt가 불가능한 작업.

- Locks : 프로세스가 특정 시점에 락을 걸고, 임계영역을 처리한 후 락을 푼다.
- Semaphotes : 조금 더 일반적인 버전의 lock
- Monitors : shared data를 동기화의 primitive로 사용

⇒ 이를 위해서는 일부 하드웨어 지원과 waiting이 필요하다.

**Locks**

Lock이란 두 개의 atomic routine을 가지고 mutual exclusion을 제공하는 것이다.

- Lock.Acquire - lock이 해제될 때까지 기다렸다가 가져간다.
- Lock.Release - unlock하고, acquire를 기다리고 있는 쓰레드가 있다면 깨운다.

**Lock을 사용하는 규칙 :**

- 공유데이터에 접근하기 전에는 항상 acquire
- 공유데이터와 작업이 종료되고 나면 항상 release
- Lock의 초기 상태는 free(해제)

하드웨어 지원을 통해 보장된다. 멀티프로세서 시스템에서 쓰레드가 동시에 처리되더라도, 두 개의 쓰레드가 동시에 접근한다면 한 쓰레드만 자원을 사용할 수 있게 된다. 

**Too much milk: Lock 구현**

```
# Thread A

Lock.Acquire();
if(noMilk) {
	buy milk;
}
Lock.Release();

# Thread B

Lock.Acquire();
if(noMilk) {
	buy milk;
}
Lock.Release();
```

이 해결책은 깔끔하고, 대칭적이다.

⇒ 어떻게 하면 Lock.Acquire과 Lock.Require를 atomic하게 만들 수 있을까?

### 동기화를 위한 하드웨어 지원

load / store, interrupt disable, test & set

## Semaphore & Monitors

### 세마포어

세마포어에는 두 가지 종류가 있다.

- Binary(Mutex) Semaphore : lock과 동일
- Counting Sempaphore

    복수 단위의 자원이 가용할 때 유용하다.

    세마포어의 카운트는 보통 가용한 자원의 수로 초기화한다.

**세마포어 - 핵심 개념**

```
S.wait(); // 기다린다

<Critical Section>

S.signal(); // 알린다
```

⇒ **wait()**과 **signal()**

- wait : 자원이 사용가능할 때까지 기다린다.

    세마포어가 0이 아닌 값인 경우(자원 사용가능), 코드 실행을 계속한다. 만약 0일 경우 OS는 이 프로세스를 세마포어를 위한 wait queue에 넣는다.

- signal : 세마포어가 이제 사용 가능하다는 것을 다른 프로세스에게 알린다.

    한 개의 프로세스를 세마포어의 wait queue로부터 가져온다.