# Context Switch

상대적으로 비싼 작업이다. 여러 개의 프로세스를 사용하는 시스템에서 context switch로 인한 오버헤드는 성능에 중요한 요소이다.

시분할 시스템에서는 보통 초당 100~1000개의 context switch를 수행한다.

*time quantum = time slice*

Context Switch는 프로세스가 교체되는 과정이라고 생각하면 된다. 프로세스를 CPU에 올리고 내리는 과정이 이에 해당한다.

- OS는 프로세스를 CPU에 올릴 때 ready 상태의 프로세스를 PCB로부터 로드한다.
- 프로세스가 도는 동안, CPU는 Program counter, Stack Pointer, 레지스터 등을 조작한다.
- OS가 프로세스를 CPU에서 내릴 때, CPU는 현재 값들을 PCB에 저장한다.

참고

[2014 UMass Operating System CS377 수업](https://www.youtube.com/playlist?list=PLacuG5pysFbDQU8kKxbUh4K5c1iL5_k7k)