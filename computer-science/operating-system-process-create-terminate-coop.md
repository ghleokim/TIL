### Creating a Process

프로세스는 트리 구조를 가진다. 만약 브라우저를 클릭해 실행하면 그것은 GUI 프로세스의 자식이 된다.

최상단 프로세스(pid 0)는 macOS의 kernel_task, linux의 init 등이 있다.

한 프로세스는 다른 프로세스를 생성해 일을 시킬 수 있다.

프로세스를 생성하는 주체는 parent process, 생성된 프로세스는 child process

parent process는 나누어줄 자원과 권한을 정의한다.

**example: Unix-Fork**

- unix에서는 child process를 생성하기 위해 *fork* system call을 사용한다.

    현재 프로그램의 복사본을 생성한다. 하지만 fork call은 프로세스의 결과로 리턴하는 값만 다르다.

    C에서 fork()를 실행하면 child process id를 구할 수 있다. `int cid = fork();`

- 쉘에서 입력하는 모든 명령어는 현재 쉘 프로세스의 child process 이고, fork와 exec의 쌍으로 이루어져 있다. 예를 들어 vim 명령어를 치고 나면 OS는 새로운 프로세스를 *fork*를 통해 생성하고, vim을 *exec*하는 것이다.
- 만약 커맨드 다음 & 을 입력하면, unix는 해당 프로세스를 현재 프로세스(쉘)와 병렬적으로 프로세스를 실행한다.

만약 `while(1) { fork(); }` 를 실행하면..? fork bomb이 생긴다. 이는 exponentially기하급수적으로 갯수가 늘어난다.

```c
#include <unistd.h>
#include <sys/wait.h>
#include <stdio.h>
main() {
	int parentID = getpid();    /* get process id of current process */
	char prgname[1024];
	gets(prgname);    /* read the name of program we want to start */
	int cid = fork();
	if(cid == 0) {   /* I'm the child process */
		execlp(prgname, prgname, 0);    /* load the program */
    /* If the program named prgname can be started,
			 we never get to this line, because child program is replaced by prgname */
		printf("I didn't find program %s\n", prgname);
	} else {    /* I'm the parent process */
		sleep(1);    /* Give child time to start */
		waitpid(cid, 0, 0);     /* wait for my child to terminate */
		printf("Program %s is finished\n", prgname);
	}
}
```

- 위 코드 설명

    **fork()** 현재 프로세스의 복사본인 새로운 child process를 생성한다.

    **execlp()** 현재 프로세스의 프로그램을 지정한 이름의 프로그램과 교체한다.

    **sleep()** 특정 시간동안 실행을 미룬다. 

    **waitpid()** 프로세스가 끝나기(terminated)를 기다린다. child process를 넣을 수도 있지만 다른 유효한 프로세스는 아무것이나 넣을 수 있다.

    **gets()** 파일로부터 한 줄을 읽어온다. get string from stdin

### Process Termination

프로세스 종료하는 과정에서, OS는 자동으로 프로세스에 할당된 자원을 회수한다.

C 프로그래밍에서 프로세스를 수동으로 종료할 수 있지만, main 함수에서 return을 한다면 자동으로 불리게 된다. 또한 kill을 이용해 프로세스를 종료시킬 수도 있다. 프로세스는 스스로를 종료시킬 수 있다.

**example : Unix-process ternimation**

```c
#include <signal.h>
#include <unistd.h>
#include <stdio.h>
main() {
	int parentID = getpid();    /* get process id of current process */
	int cid = fork();
	if(cid == 0) {   /* I'm the child process */
		sleep(5);    /* I'll exit myself after 5 seconds. */
		printf( "Quitting child\n" );
		exit(0);
		printf( "Error! After exit call.!" );    /* should never get here */
	} else {    /* I'm the parent process */
		printf( "Type any character to kill the child.\n" );;
		char answer[10];
		gets(answer);
		if( !kill(cid, SIGKILL) ) {
			printf("Killed the child.\n");
		}
	}
}
```

### Cooperating Processes

**Producers and Consumers:**

예를 들어 producer가 버퍼에 아이템을 채워넣으면 consumer가 아이템을 꺼내 작업을 처리하는 방식

이는 서로 다른 프로세스이기 때문에 프로세스 간 데이터를 주고 받을 수 있는 방법이 필요하다. 또한 동기화도 필요하다.

message passing 또는 shared memory를 사용할 수 있다.

**Message Passing :** send method, receive method를 이용

```
EXAMPLE
**producer**
	repeat
		**...**
		produce item nextp
		...
		send(nextp, consumer)

**consumer**
	repeat
		****receive(nextc, producer)
		...
		consume item nextc
		...
```

**Shared Memory**

프로세스의 명시된 메모리 공간을 mapping을 통해 메모리 객체로 만든다.

mmap()을 통해 이를 지정할 수 있다.

자료구조를 공유하기 위해 fork process가 필요하다.

```
main()
	...
	mmap(..., in, out, PROT_WRITE, PROT_SHARED, ...);
	in = 0;
	out = 0;
	if (fork != 0) producer();
	else consumer();
```

참고

[2014 UMass Operating System CS377 수업](https://www.youtube.com/playlist?list=PLacuG5pysFbDQU8kKxbUh4K5c1iL5_k7k)