# 프로세스의 상태와 상태변이

![프로세스의 상태와 상태변이 흐름도](https://user-images.githubusercontent.com/52501513/86213114-2bb4d480-bbb4-11ea-8604-82d85344b815.png)

## Process State 프로세스의 상태

### Active Process State(활동 상태)

- New/Created

    프로세스의 최초 상태. 이 상태에서 프로세스는 ready 상태가 되기를 기다린다.

- Ready / Waiting

    프로세스가 우선순위에 의해 정렬된다. 메인 메모리에 로드된 상태이고, 실행되기를 기다리고 있다.

    특정 시점에서 시스템의 ready 또는 waiting인 상태의 프로세스는 많을 수 있다. 예를 들어 프로세서가 하나일 경우, 한 프로세스는 한 번에 하나씩만 실행될 수 있으므로 병렬적으로 실행되고 있는 다른 모든 프로세스는 실행을 waiting기다리고 있게 되는 것이다.

    ready queue 또는 run queue는 스케줄링에 사용된다. ready 상태의 프로세스는 큐에 넣어 관리한다.

- Running

    프로세스는 실행 대상이 되면 running 상태로 변경된다. 프로세스는 **커널 모드** 또는 **유저 모드**로 실행될 수 있다. 모드는 운영체제에 따라 더욱 세분화될 수 있으며, 예를 들어 x86에서 중간의 권한을 가진 모드는 가상화에 사용될 수 있다.

    1. 커널 모드

        커널 모드의 프로세스는 커널 주소, 유저 주소 두 가지에 모두 접근 가능하다.

        커널 모드는 제한 없이 하드웨어에 직접 접근할 수 있고, Priviliged Instructions 또한 실행할 수 있다.

        ***Priviliged Instructions***: 커널 모드로만 실행 가능한 명령어instructions이다. 반대로 유저 모드로만 실행 가능한 Instructions는 non-priviliged instructions라고 부른다.

        I/O,
        Halt,
        Turn off Interrupts,
        Set Timer,
        Context Switching,
        Clear memory or Remove Process from memory,
        Modify entries in Device-status table 등의 작업이 포함된다.

        참고→ [https://www.geeksforgeeks.org/privileged-and-non-privileged-instructions-in-operating-system/](https://www.geeksforgeeks.org/privileged-and-non-privileged-instructions-in-operating-system/) 

    2. 유저 모드

        유저 모드의 프로세스는 각각의 명령어와 데이터를 가지고 있지만 다른 프로세스 또는 커널의 명령어와 데이터는 가지고 있지 않다.

        시스템이 유저 애플리케이션의 이름으로(on behalf of) 실행할 경우, 시스템이 유저 모드에 있다고 말한다. 하지만, 유저 애플리케이션이 **시스템 콜**을 통해 OS에게 작업을 요청할 때에는, 시스템은 이 요청을 수행하기 위해 **반드시 유저 모드에서 커널 모드로** 변환해야 한다.

        유저 모드는 다음과 같은 다양한 문제로부터 분리되어 있다.

        - 각 유저 모드의 프로세스에는 독립된 **virtual address space가상 주소 공간**이 있다.
        - 유저모드는 각 프로세스가 독립적으로 실행됨을 보장하고, 서로 영향을 주지 않음을 보장한다.
        - 모든 하드웨어 디바이스로의 **직접 접근은 금지**되어있다.

- Blocked

    프로세스는 이벤트가 발생하거나, 외부에서 상태를 변경하기 전까지 더이상 진행할 수 없는 상태에서 Blocked 상태가 된다. 예로 I/O 디바이스를 호출하거나 기다리는 경우, 임계 영역(Critical Section)에 접근하기 전에 대기하는 상태일 때를 들 수 있다.

- End/Terminated

    프로세스는 running 상태에서 종료될 수 있다. 프로그램의 끝에 도달하여 실행이 종료되거나, 따로 종료 명령을 받아 killed될 수 있다. 이 두 가지 경우 모두 프로세스는 **terminated** 상태로 변경된다.

    프로세스는 실행되고 있지 않지만, 프로세스 테이블에는 **좀비 프로세스로** 남아 있으면서 부모 프로세스가 `wait` 시스템 콜을 통해 exit status를 읽는 것을 기다린다. 이후 프로세스는 테이블에서 내려가며 사라진다.

    만약 부모 프로세스가 `wait` 를 호출하지 않는다면, 이 프로세스는 프로세스 테이블에서  내려가지 않으며 PID를 가지고 있게 되며, resource leak를 유발하게 된다.

### Suspended Process State(지연 상태)

아래 두 가지 상태는 가상 메모리를 사용하는 시스템에서 사용한다. 이 두 가지 경우에, 프로세스는 보조기억장치(보통 하드디스크, ssd 등)에 저장된다.

suspended 상태의 프로세스는 스케줄링에서 제외된다. 프로세스가 메모리에서 옮겨져 나갔거나, 프로세스를 시작한 유저가 특정 조건에서 스케줄링되는 것을 막거나 하는 경우에 suspension이 이루어진다.

[이 링크](https://superuser.com/questions/377572/what-is-the-main-purpose-of-the-swapper-process-in-unix)에 따르면 유닉스 시스템에서는 swap을 1970년대 이후 거의 사용하지 않고 있다고 한다. 

- Swapped out and waiting, suspended and waiting

    가상 메모리를 지원하는 시스템에서 프로세스는 swapped-out될 수 있다. 이는 프로세스가 스케줄러에 의해 메인 메모리에서 지워지고 외부 스토리지에 저장될 수 있다는 것을 의미한다. 이 때 프로세스는 waiting 상태로 들어간다.

- Swapped out and blocked

    blocked 상태의 프로세스 역시 swapped-out될 수 있다. 이 때 프로세스는 swapped되고 blocked되며, swapped-out and waiting 상태의 프로세스와 똑같이 다시 swapped back in 될 수 있다.

## 프로세스의 상태전이(dispatch, wake-up 등)

### **ready → running**

프로세스가 Dispatched된다. CPU는 명령어의 실행을 시작하거나 복귀starts or resumes한다.

### **blocked → ready**

프로세스의 요구사항이 만족되거나, 발생하기를 기다리던 이벤트가 발생했다.

### **running → ready**

프로세스가 preemted(or timeout)당한다. OS가 다른 프로세스를 처리하도록 스케줄링하고 있기 때문. 이 전환은 **더 높은 우선순위**의 프로세스가 ready상태에 있거나, 프로세스가 가지고 있는 **유지시간time slice이** 다 했기 때문이다.

### **running → blocked**

실행되는 프로그램은 **시스템 콜을 통해** 자원에 대한 요청을 기다리거나, 시스템에 특정 이벤트가 발생하기를 기다리겠다는 것을 명시한다.

- blocking의 다섯 가지 주요 이유는 다음과 같다:

    프로세스가 I/O 작업을 요청한다.
    프로세스가 메모리 또는 다른 자원을 요청한다.
    프로세스가 특정 시간동안specified interval of time 기다리고자 한다.
    프로세스가 다른 프로세스로부터 메세지를 받는 것을 기다린다.
    프로세스가 다른 프로세스의 액션을 기다리고자 한다.

### **running → terminated**

프로그램의 실행은 종료되거나 종료당한다completed or terminated. 프로세스 종료의 다섯 가지 주요 이유는 다음과 같다:

- self termination

    프로그램이 수행하고자 하는 작업을 끝내거나, 하려는 작업을 수행하지 못 할 경우, 'terminate me' 시스템 콜을 부른다. 후자의 경우 예를 들어 잘못되었거나 일관성이 없는 데이터, 그리고 데이터를 원하는 방식으로 접근할 수 없는 경우(예: 파일 접근 권한이 잘못되었을 때)에 생긴다. 

- termination by a parent

    프로세스는 'terminate P_i' 시스템 콜을 통해 자녀 프로세스 P_i를 종료시킬 수 있다. 이는 자녀 프로세스가 더 이상 필요 없을 때 수행된다. (이와 동일한 결과를 나타내는 방법으로써, 부모 프로세스가 자녀 프로세스에게 'terminate now' 신호를 보내 자녀 프로세스가 스스로 'terminate me' 시스템 콜을 하도록 하는 방법 또한 있다.)

- exceeding resource utilization

    OS는 프로세스 하나가 소비할 수 있는 자원을 제한하고 있을 수 있다. 이러한 제한을 넘어가는 프로세스의 경우 커널에 의해 종료될 수 있다.

- abnormal conditions during execution

    커널은 프로그램이 실행되는 동안 비정상적인 조건이 발생하면 프로세스를 취소시켜 버린다. 예를 들어 유효하지 않은 명령어를 사용하거나, priviliged 명령어를 사용하거나, 오버플로우를 이르키는 연산을 하거나, 메모리 보호를 어기거나 등의 조건이 있을 것이다.

- incorrect interaction with other processes

    커널은 한 프로세스가 다른 프로세스와 잘못된 방법으로 상호작용하는 것을 발견하면 프로세스를 취소시킬 수 있다. 예를 들어, 프로그램이 데드락 상황에 관여될 때.


참고

[process state - wiki](https://en.wikipedia.org/wiki/Process_state)

[Operating Systems - google books](https://books.google.co.kr/books?id=kbBn4X9x2mcC&pg=PA99&lpg=PA99&dq=swapped+and+blocked&source=bl&ots=TnimA2gSxb&sig=ACfU3U395V9iXSFYGRsHETT9e6bST_wiWA&hl=en&sa=X&ved=2ahUKEwiSzLbtj6nqAhVdyIsBHVH_BF4Q6AEwCXoECAoQAQ#v=onepage&q=swapped%20and%20blocked&f=true)
