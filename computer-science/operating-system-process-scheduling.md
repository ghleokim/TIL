# 프로세스 스케줄링 알고리즘

## 요약

- FCFS: 불공평, 평균 wait time이 나쁘다.
- Round Robin: 공평, 평균 wait time이 나쁘다.
- SJF: 불공평, CPU time을 정확하게 예측할 수 있다는 전제 하에 average wait time이 최소화된다. starvation이 일어날 수 있다.
- Multilevel Queueing: SJF의 근사 구현체(?)
- Lottery Scheduling: 조금 더 공평, 조금 더 낮은 평균 wait time. 하지만 예측성이 떨어진다.

⇒ 이 모델링은 context switch에 걸리는 시간이 없다는 가정을 바탕으로 함. 현실은 그렇지 않음.


## CPU 스케줄링 알고리즘을 비교하는 조건

- CPU Utilization : CPU가 일하고 있는 시간의 비율
- Throughput : 일정 시간동안 처리하는 프로세스의 갯수 → batch process에서 유용
- Turnaround Time : 한 프로세스를 초기화부터 종료까지 수행하는데 걸리는 시간, waiting time 포함
- Waiting Time : 프로세스가 ready queue에 있는 총 시간
- Response Time : process가 ready 상태에 있는 상태에서 다음 IO 요청까지의 시간

→ 이는 유용한 수치이지만, 모든 것을 충족하는 것은 불가능. 일부 tradeoff가 있다.

## 스케줄링 정책Scheduling Policies

- minimize avg response time
- minimize variance of response time

    interactive 시스템에서는 평균이 조금 더 높더라도 분산이 더 적은 것이 나을 경우가 있다. 마우스 작업의 응답속도가 [10ms 평균&가끔 3초 걸림 vs 15ms 평균& 크게 안벗어남] 조건에서는 후자가 더 나을 수 있기 때문.

- Maximize throughput - two components
    1. minimize overhead(OS overhead, context switching)
    2. efficient use of system resources(CPU, IO devices)

- Minimize waiting time

    각 프로세스에게 공평한 시간을 나누어주는 것. 이것은 평균 응답 시간을 조금 높이는 결과를 낳을 수 있다.

프로세스 스케줄링 알고리즘은 다음의 가정을 바탕으로 한다.

- 1 프로세스당 1 유저
- 1 프로세스당 1 쓰레드
- 프로세스들은 서로 독립적

알고리즘이 만들어졌을 당시(70년대)에는 이 가정들이 더욱 현실에 가까웠다. 이 가정을 완화하는 방법은 아직 열린 문제이다.

FCFS, RR, SJF, MLQ, Lottery Scheduling

## FCFS

non-preemptive scheduler비선점형.

현재 작업중인 프로세스가 끝나기를 기다려야한다.

- 장점

    단순하고 간단하다.

- 단점

    평균 wait time이 매우 들쭉날쭉할 수 있다. 오래 걸리는 작업이 나중에 도착할 수도 있기 때문

    IO와 CPU가 둘 다 잘 활용될 수 없을 가능성이 있다. poor overall system utilization

## Round Robin Scheduling

Round Robin의 변형이 대부분의 시분할 시스템에서 사용된다.

타이머를 추가. preemptive선점형

각 time slice가 지난 후에, 실행하는 쓰레드를 큐의 마지막으로 보낸다.

time slice를 선택하여야 한다.

→ 너무 크게 정하면 - waiting time이 너무 길어진다.

→ 너무 작게 정하면 - contect switch로 인한 오버헤드로 throughput이 낮아진다.

⇒ 이 tradeoff를 조절해서 context switch가 time slice의 대충 1%정도가 되도록 맞춘다.

요즘 : typical timeslice = 10 ~ 100ms, context switch time = 0.1 ~ 1 ms

- 장점

    공평한 일 분배. 모든 작업이 CPU에 동등하게 접근

- 단점

    평균 wait time이 낮아질 수 있다.

## SJF/SRJF: Shortest Job First

다음 IO 요청 또는 프로세스 종료까지 예상되는 CPU time이 가장 적은 것 부터 스케줄링한다.

하지만 이 시간을 어떻게 측정할 것인가?

- 장점

    평균 wait time을 최소화하는 측면에서 최적인 알고리즘

    선점형, 비선점형 스케줄러 모두에 적용 가능

    선점형 SJF는 SRJF라고 부른다: Shortest **Remaining Time** First

    IO-bound job은 CPU-bound job보다 높은 우선순위를 가진다 : IO-bound는 CPU를 많이 먹지 않는다.

- 단점

    남은 CPU time을 예측하는 것은 불가능하다. 실질적으로 적용 불가능

    long running cpu bound jobs can starve. 

## Multilevel Feedback Queue(MLFQ)

멀티레벨 피드백 큐는 이전의 행동을 바탕으로 작업의 우선순위를 결정하는데 사용한다.

→ SJF의 예측 문제를 해결할 수 있다.

이전 실행 때 IO-bound였던 프로세스는 앞으로도 IO-bound일 가능성이 크다.

이를 통해 스케줄러는 CPU time을 적게 사용했던 작업을 조금 더 우선할 수 있게 된다.

이 규칙은 이전 행동을 바탕으로 하여 앞으로의 행동을 결정하기 때문에 **적응형**이라고 볼 수 있다.

### Approximating SJF: Multilevel Feedback Queues

서로 다른 우선순위를 가진 여러 개의 큐.

각 우선순위 레벨에서는 round robin을 사용하여 돌린다.

높은 우선순위 큐에 있는 작업을 우선 처리.

앞선 작업이 끝나면 그 다음 우선순위 큐를 처리한다. 하지만 이는 starvation으로 이어질 수도

round robin time slice는 낮은 우선순위에서 배로 증가하게 설정한다. (cpu bound job을 더 낮은 우선순위로 둔다고 가정하고)

### MLFQ에서 우선순위 조정하는 방법

- 작업은 우선 높은 우선순위에서 처음 실행된다.
- 작업의 time slice가 expire한다면, 우선순위를 1레벨 낮춘다. ( 이는 CPU bound job일 가능성에 더 무게를 둔다. )
- 작업의 time slice가 expire되지 않는다면, 우선순위를 1레벨 높인다.
- CPU bound 작업은 우선순위가 빠르게 떨어지지만, IO bound 작업은 높은 우선순위에서 유지된다.

### improving fairness

SJF는 최적화를 하지만 공평하지는 않다. 더 짧은 작업이 있더라도 긴 작업에 일부 CPU를 할당하는 것은 이 형평성을 조금 높일 수 있을 것이다.

가능한 해결책 :

- 각 큐에 일정한 비율의 CPU time을 준다. 이 방법은 각 큐 사이 적당한 분포를 가지고 있을 때만 공평하다.
- (유닉스의 오리지널 해결법) 서비스되지 않는 작업의 우선순위를 조정한다.

    이 해결법은 starvation 문제를 해결하지만, 시스템이 오버로드되었을 때는 평균 wait time이 높아지게 된다. 모든 작업들의 우선순위가 올라가기 때문.

## Lottery Scheduling

모든 작업에 일정한 갯수의 로또 티켓을 발급.

각 time slice마다, 랜덤으로 선정된 티켓을 하나 정한다.

평균적으로 CPU time은 각 작업에 할당된 티켓의 양에 비례한다.

단순히 더 많은 티켓을 발급하는 것으로 더 치중할 작업을 지정할 수 있다: 시간이 적게 걸리는 작업에 더 많은 티켓을, 오래 걸리는 작업에 더 적은 티켓을 발급(approximating SJF). starvation을 피하기 위해서 모든 작업은 적어도 하나의 티켓을 가진다.

더 많은 작업이 추가될 수록, 성능이 **점진적으로(천천히)** 낮아진다. 작업을 더하고 빼는 일은 모든 작업에 할당된 티켓의 비율대로 영향을 미친다.

기존의 다른 스케줄링이 deterministic한 반면(각 시간마다 어떻게 동작할지 정확히 안다), 이 스케줄링은 확실히 결정되어있는 것이 없다.


참고

[2014 MIT Operating System CS377 수업](https://www.youtube.com/playlist?list=PLacuG5pysFbDQU8kKxbUh4K5c1iL5_k7k)