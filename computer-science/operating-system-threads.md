# 쓰레드

- what are threads?

    threads can communicate with each other. 주소 공간을 공유한다. - 장점

    parallelism을 적용하는 방법이 된다.

- where should we implement threads? in the kernel? in a user level threads package?
- how should we schedule threads(or processes) onto the cpu?

## 요약

- 쓰레드 : 프로세스 내부의 단일 실행 흐름
- 유저레벨 쓰레드간 스위칭이 커널 쓰레드간 스위칭보다 빠르다. - context switch가 필요 없다.
- 유저레벨 쓰레드는 커널의 스케줄링 판단을 어렵게 만들 수 있다. 커널 쓰레드를 사용하는 것보다 프로세스 실행이 더 느려질 수도 있다.
- 많은 스케줄링 알고리즘이 존재한다. 알고리즘을 선택하는 것은 스케줄링 정책 측면에서의 결정이며, 이는 프로세스의 성격과 OS에서 실행에 얻고자 하는 목표를 기준으로 결정해야 할 것이다. (응답시간을 낮추거나, throughput을 높이거나 등)


## Processes VS Threads

**프로세스**는 주소공간, 텍스트(코드), 자원 등을 정의한다.

**쓰레드**는 프로세스 내에서 하나의 실행 흐름(스트림)을 정의한다. (PC, stack, registers)

쓰레드는 *thread of control* 정보를 프로세스로부터 추출한다.

쓰레드는 한 프로세스 내부에 국한된다.

각 프로세스는 여러 개의 쓰레드를 가지고 있을 수 있다.

- 프로세스의 주소 공간은 모든 쓰레드에서 공유한다.
- 쓰레드간 협업을 위해 별도의 시스템 콜이 필요 없다.
- message passing, shared memory보다 단순하다.

## Single and Multithreaded Processes

![single-and-multithreaded-process](https://user-images.githubusercontent.com/52501513/86745499-5c45b400-c075-11ea-92a4-63ccc60a2c3b.png)

### Example: threaded program (psuedo code)

```c
main()
	global in, out, n, buffer[n];
	in = 0; out = 0;
	fork_thread(producer());
	fork_thread(consumer());
end

producer
	repeat
		nextp = produced item
		while (in+1) mod n = out do no-op
		buffer[in] = nextp; in = (in+1) mod n

consumer
	repeat
		while in = out do no-op
		nextc = buffer[out]; out = (out+1) mod n
		consume item nextc;
```

쓰레드를 만드는 것은 커널에 시스템 콜을 하는 방법과, 쓰레드 라이브러리로 프로시저 콜을 하는 방법이 있다.

### Kernel Threads

**커널 쓰레드**는 **lightweight process**라고도 한다. 이는 운영체제가 인지하고 있는 쓰레드이다.

동일 프로세스 내에서 수행하는 커널 쓰레드 스위칭은 비교적 작은 context switch를 요구한다.(더 적은 시간)

- 레지스터, 프로그램 카운터, 스택 포인터는 전부 변경되어야 한다.
- 메모리 관리 정보는 변경될 필요가 없다 : 쓰레드는 같은 주소 공간을 공유하기 때문에.

커널은 쓰레드 역시 프로세스처럼 스케줄링하고 관리해야 한다. 하지만 동일 프로세스 스케줄링 알고리즘을 사용할 수 있다.

⇒ 커널 쓰레드간 switch는 프로세스간 switch보다 빠르다.

### User-Level Threads

**유저레벨 쓰레드**는 운영체제가 인지하지 못하는 쓰레드이다.

OS는 이 쓰레드를 포함하고 있는 프로세스에 대한 정보만을 알고 있다.

OS는 프로세스를 스케줄링 하지만 프로세스 내부의 유저 레벨 쓰레드를 스케줄링하지는 않는다.

프로그래머는 *쓰레드 라이브러리*를 통해 쓰레드를 관리한다(쓰레드 **생성, 삭제, 동기화, 스케줄링**).

- 장점

    OS가 쓰레드간 switch에 관여하지 않기 때문에 시스템 콜로 인한 context switch 오버헤드가 없다.

    OS가 쓰레드를 지원하지 않아도 사용 가능하다. ex 임베디드 시스템

    유저레벨 쓰레드가 더 유연하다.

    → 독립적인 쓰레드 스케줄링 정책을 지정할 수 있다.

    → 각 프로세스는 각자의 쓰레드를 관리하기 위한 서로 다른 스케줄링 알고리즘을 사용할 수 있다.

    → 한 쓰레드는 CPU를 자발적으로 다른 쓰레드에게 넘겨줄 수 있다.

    ⇒ 유저레벨 쓰레드는 보통 커널 쓰레드보다 훨씬 빠르다.

- 단점

    **실질적인 병렬 처리가 아니다.** 멀티코어 프로세싱의 장점을 활용하기 어렵다.

    OS는 여러 개의 쓰레드가 있는 것도 모르기 때문

    → thread가 idle 상태인 경우에도 OS는 프로세스를 실행할 수도 있다. 이는 자원낭비

    → 한 쓰레드가 IO처리를 기다리고 있다면, 전체 프로세스가 blocked된 상태가 된다.

    → 이 문제를 해결하려면 커널과 유저레벨 쓰레드 매니저간 통신을 구현해야 한다.

    OS가 프로세스에 대한 정보만을 가지고 있기 때문에, 유저 쓰레드의 갯수에 상관 없이 다른 프로세스와 동일하게 처리한다.

    커널 쓰레드의 경우, 프로세스가 더 많은 쓰레드를 만들 수록 OS는 그 프로세스를 처리하기 위해 더 많은 시간을 들일 것이다.

### Example- Kernel and User-Level Threads in Solaris

Solaris에서는 커널 쓰레드와 유저 쓰레드 중간 개념인 lightweight process를 통해 유저 쓰레드와 커널 쓰레드 모두의 장점을 살린다.

### 쓰레딩 모델

- many-to-one - 커널 쓰레드 1 : N 유저 쓰레드
- one-to-one - 커널 쓰레드 1 : 1 유저 쓰레드
- many-to-many - 커널 쓰레드 M : N 유저 쓰레드

    커널 쓰레드의 갯수를 정해놓고, 유저 쓰레드를 할당하여 사용

- two-level - 커널 쓰레드 M : N 유저 쓰레드 | 커널 쓰레드 1 : 1 유저 쓰레드

## 쓰레드 라이브러리

쓰레드 라이브러리는 프로그래머에게 쓰레드를 만들고 관리하는 API를 제공한다.

두 가지 방법의 구현이 있다.

- library entirely in user space
- kernel-level library supported by the OS

### Pthreads

user-level or kernel-level

쓰레드 생성과 동기화에 있어 POSIX standard API이다.

API는 쓰레드 라이브러리의 명세만을 정의한다. 구현은 라이브러리의 개발에 달려있다.

Unix계열 OS(Solaris, Unix, Mac OS X)에서 흔하게 사용된다.

Win32 Threads : Posix와 비슷하지만 윈도 전용

### Java Threads

자바 쓰레드는 JVM에 의해 관리된다.

보통 OS의 쓰레드 모델을 사용하여 구현된다.

자바 쓰레드는 다음과 같이 생성될 수 있다:

- Thread Class를 extend하여 사용
- Runnable interface를 구현하여 사용

참고

[2014 UMass Operating System CS377 수업](https://www.youtube.com/playlist?list=PLacuG5pysFbDQU8kKxbUh4K5c1iL5_k7k)
