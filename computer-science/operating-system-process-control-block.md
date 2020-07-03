# 프로세스 컨트롤 블록 Process Control Block

## Process Control Block(PCB)

프로세스 컨트롤 블록은 프로세스가 작업을 처리하는데 필요한 모든 정보를 가지고 있다. ID, 우선순위, 상태, PSW(program status word), CPU 레지스터의 내용, 또한 자원에 접근하고 다른 프로세스와 통신하는 등의 정보 또한 가지고 있다.

**우선순위**와 **상태 정보**는 스케줄러에 의해 사용된다. 스케줄러는 선택한 프로세스의 ID를 dispatcher에게 넘긴다.

*running* 상태가 **아닌** 프로세스는 **CPU 스냅샷**을 가지고 있는데, PSW와 CPU registers 항목 두 개를 바탕으로 알 수 있다. 이 때 CPU 스냅샷이란 프로세스가 blocked, timeout(preemted) 등의 **상태 또는 상태 전이를 이유로** CPU를 놓았을 당시의 (when CPU is last released by the process) 내용을 가지고 있게 된다. 프로세스의 재개resume는 스냅샷 정보를 단순히 PCB로부터 CPU에 옮기는 것으로 수행할 수 있다. 이 절차는 다음번 프로세스가 dispatched될 때 수행된다.

이벤트정보event information 항목은 시스템에서 적절한 상태 변이가 일어나는데 중요한 역할을 한다.

예를 들어, 프로세스 `P_i` 가 I/O 작업을 기다리느라 blocked 상태에 있다고 가정해보자. `P_i` PCB의 **이벤트정보** 항목에는 작업이 이루어질 I/O 장치의 id를 기록한다. 만약 이 장치의 I/O 작업이 완료(이벤트 발생!)되면, 커널은 이 이벤트를 기다리고 있는 프로세스를 찾아 ready 상태로 변경하는 작업을 수행할 것이다. PCB의 이벤트정보 항목을 찾은 후, 커널은 이 프로세스의 상태가 blocked에서 ready로 변경되어야 한다는 것을 알 수 있다.

## **PCB 자료구조의 항목과 내용**

- Process Id(PID)

    프로세스가 부여받는 고유한 id이다. 프로세스가 생성되는 시점에 결정된다.

- Child and Parent Ids

    이 id들은 프로세스 동기화에 사용된다. 특히 자녀 프로세스가 terminated 되었는지 여부를 판단하는데 쓰인다.

- Priority

    priority는 대개 숫자 값으로 나타낸다. 프로세스가 생성되는 시점에 할당된다.
 
    우선순위는 프로세스의 생성부터 종료까지의 과정 중에 바뀔 수 있다. 특성nature(CPU-bound이냐 I/O-bound이냐)에 따라, 프로세스의 나이age 또는 소비된 자원(보통 CPU time)에 따라 바뀐다.

- Process State

    프로세스의 현재 상태.

- Process Status Word-PSW

    이는 PSW의 스냅샷이다. 프로세스가 CPU를 놓았거나, 커널에 의해 CPU가 점유당하는 시점의 PSW의 스냅샷을 기록한다. 이 스냅샷을 PSW로 로딩하는 것은 프로그램의 실행을 재개resume시킨다.

- CPU regjsters

    프로세스가 CPU를 놓았거나, 커널에 의해 CPU가 점유당하는 시점의 CPU 레지스터 값(내용)이다.

- Event Information

    blocked 상태의 프로세스에 대해서, event information 필드는 이 프로세스가 **기다리고 있는 이벤트**에 대한 정보를 가지고 있다.

    특정 이벤트가 발생하면, 커널은 이 정보를 활용하여 그 이벤트를 기다리고 있는 프로세스를 찾아 blocked에서 ready 상태로 변화시킨다.

- Signal Information

    signal handlers에 대한 위치 정보이다.(다른 곳에 자세한 설명 있음)

- PCB pointer

    이 항목은 PCB들의 리스트를 구성하는데 사용된다. 커널은 ready 상태 프로세스의 리스트, blocked 상태 프로세스의 리스트 등 PCB의 리스트를 관리하고 있다.

참고 

[Operating Systems](https://books.google.co.kr/books?id=kbBn4X9x2mcC&pg=PA99&lpg=PA99&dq=swapped+and+blocked&source=bl&ots=TnimA2gSxb&sig=ACfU3U395V9iXSFYGRsHETT9e6bST_wiWA&hl=en&sa=X&ved=2ahUKEwiSzLbtj6nqAhVdyIsBHVH_BF4Q6AEwCXoECAoQAQ#v=onepage&q=swapped%20and%20blocked&f=true)