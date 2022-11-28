# CPU 스케줄러

다중 프로그램 OS의 기본으로 여러 프로세스들이 CPU를 교환하며 보다 생산적으로 동작한다. CPU를 선점한 프로세스가 대기하는 시간을 보다 효율적으로 사용하기 위하여 사용한다. </br>
기본적으로 다음에 실행 될 프로세스를 정하고 프로세스들을 실행 가능한 상태로 만든다.

</br>

# 목차

- [Scheduling Criteria (스케줄링 성능 척도)](#scheduling-criteria-스케줄링-성능-척도)
- [FCFS (First Come First Served)](#fcfs-first-come-first-served)
- [SJF (Shortest-Job-First)](#sjf-shortest-job-first)
- [Priority Scheduling](#priority-scheduling)
- [Round Robin (RR)](#round-robin-rr)
- [Multilevel Queue](#multilevel-queue)
- [Multilevel Feedback Queue](#multilevel-feedback-queue)
- [Multiple-Processor Scheduling](#multiple-processor-scheduling)
- [Real-Time Scheduling](#real-time-scheduling)
- [Thread Scheduling](#thread-scheduling)
- [Algorithm Evaluation](#algorithm-evaluation)
- [스케줄링 알고리즘 한 눈 비교](#스케줄링-알고리즘-한-눈-비교)

</br>

## Scheduling Criteria (스케줄링 성능 척도)

* 시스템 입장에서의 성능 척도 (= CPU 하나 가지고 일을 많이 시키면 좋은 것)
    * CPU Utilization (이용률)
    * Thoroughput (처리량)
* 프로그램 입장에서의 성능 척도 (= 내가 CPU를 빨리 얻어서 빨리 끝내면 좋은 것)
    * Turnaround time (소요시간, 반환시간)
    * Waiting time (대기 시간)
    * Response time (응답 시간)

</br>
</br>

> ### 성능 척도에 대한 지표를 자세히 살펴보자.

* CPU Utilization
    * CPU가 놀지 않고 일한 시간의 비율을 나타낸다.
    * 즉 CPU는 비싼 자원이기 때문에 놀게하지 말고 최대한 바쁘게 일을 많이 시키라는 의미로 볼 수 있다.
* Thoroughput
    * 주어진 시간 동안에 몇 개의 작업을 완료했는 지를 나타낸다.
    * CPU가 주어진 시간에 많은 일을 처리할 수 있다면 시스템 입장에서는 성능이 좋다라고 판단할 수 있을 것
* Turnaround time
    * CPU를 사용하려고 들어와서 다 쓰고 나갈 때까지 걸리는 시간을 의미한다.
    * 기다린 시간, CPU 쓴 시간 등을 다 더한 시간이다.
* Waiting time
    * CPU를 ready queue에서 기다리는 시간을 의미한다. 줄을 서서 기다리는 순수한 시간만을 의미한다.
    * preemptive인 경우에는 계속 CPU를 얻었다가 뺏겼다가 반복되기에 기다리는 시간이 누적될 것이다. 이를 다 합치면 waiting time이 되는 것이다.
* Response time
    * ready queue에 CPU를 쓰겠다고 들어와서, CPU를 얻고 최초로 응답을 얻기까지 걸린 시간을 의미한다.

</br>
</br>

> ### 시간과 관련된 성능 척도를 3개 씩이나 나누어서 이야기를 하는 이유는 무엇일까?

그 중에서 응답시간이라는 것을 두는 이유는 타임 쉐어링, 즉 CPU를 얻었다 뺏었다 하는 환경에서는 어쨌든 처음으로 CPU를 얻는 시간이 사용자 응답과 관련해서 굉장히 중요한 시간의 개념이라는 것이다. 그래서 대기시간도 있고 반환시간도 있지만 응답시간을 별도로 도입해서 이야기를 하는 것이다. 

![](https://i.imgur.com/fhHzTzn.png)

</br>
</br>

## FCFS (First Come First Served)

![](https://i.imgur.com/LQDwRFN.jpg)

> 프로세스의 도착 순서는 P1, P2, P3
> 스케줄 순서를 Gantt Chart로 나타내면 위와 같다.

* 대기 시간은 P1 = 0, P2 = 24, P3 = 27
* 따라서 평균 대기 시간은 (0 + 24 + 27) / 3 = 17초이다.

> 비선점형 스케줄링이라서 그렇게 효율적이지는 못하다.

![](https://i.imgur.com/Xk7itQX.png)

> 하지만 반대로 처리 시간이 짧은 프로세스가 먼저 도착했다고 하면 평균 대기 시간이 매우 짧아질 수도 있다.
> 즉, 처음 도착한 프로세스의 처리 시간에 상당한 영향을 받게 되는 것이다.
> 이와 같이 처리 시간이 긴 프로세스로 인해 짧은 처리 시간을 가지는 프로세스들이 오래 기다려야하는 상황을 `convoy Effect`라고 한다.

</br>
</br>

## SJF (Shortest-Job-First)

CPU Burst time이 가장 짧은 프로세스들을 먼저 처리해줌으로써 ready queue가 짧아지게 되고, 전체적인 평균 대기 시간(waiting time) 또한 짧아진다.

* `Nonpreemptive`
    * 일단 CPU를 잡으면 이번 CPU Burst가 완료될 때까지 CPU를 선점(preemptive) 당하지 않는다.
    * 즉 중간에 더 짧은 프로세스가 도착하더라도 계속 기존의 프로세스를 처리한다.
* `Preemptive`
    * 중간에 더 짧은 프로세스가 도착하면 CPU를 뺏어서 전달해준다. 이를 SRTF(Shortes-Remaining-Time-First)라고도 부른다. 
    * 즉, 남은 처리 시간이 짧은 프로세스에게 우선적으로 CPU를 주는 것이다.

> `SJF is optimal` : 주어진 프로세스들에 대해 minimum average waiting time을 보장한다. (= Preemptive 버전)

#

* Example of Nonpreemptive SJF

![](https://i.imgur.com/eSTNmZJ.png)

> 예시를 보면 제일 먼저 도착한 P0이 먼저 실행되고, 실행이 끝난 후 도착한 프로세스 중에서 제일 실행시간이 짧은 순서로 P3가 먼저 실행이 되는 것을 확인할 수 있다.

#

* Example of Preemptive SJF

![](https://i.imgur.com/5bupbcL.png)

> 0초에는 P1 혼자 도착했기 때문에 CPU를 P1에게 준다. 2초 시점이 되면은 P1이 다 실행되기도 전에 P2의 실행시간이 더 짧기 때문에 P2에게 CPU를 뺏기는 상황이 된다. 4초 때에는 도착한 P3의 사용시간이 더 짧기 때문에 P3가 CPU를 독점하게 되고, 이런식으로 계속 CPU가 뺏기는 상황이 번복되면서 프로세스들을 실행한다.

이 알고리즘의 경우 두 가지 문제점을 가지고 있다.

* `Starvation (기아 현상)`
    * 짧은 처리 시간을 가지는 프로세스를 우선적으로 처리해주기에 처리 시간이 긴 프로세스를 계속 무한 대기하게 된다.
    * 즉 처리 시간이 긴 프로세스는 영원히 처리되지 않을 수도 있다는 것이다.
* `다음 CPU Burst Time의 예측이 어렵다.`
    * 프로세스가 어느 정도의 처리 시간을 가지게 되는지 파악하기가 어렵다.
    * 그래서 추정을 통해 처리시간을 예측하게 되는데, 과거에 CPU를 얼마나 썼는지에 기반하여 계산하는데 정확하지는 않다고 한다.

</br>
</br>

## Priority Scheduling

우선순위가 제일 높은 프로세스에게 CPU를 주는 개념을 의미한다.

* Nonpreemptive
    * 일단 CPU를 줬으면 더 높은 우선순위의 프로세스가 도착하더라도 다 쓸 때까지는 보장해주는 것
* Preemptive
    * 우선 순위가 더 높은 프로세스가 도착했을 때 CPU를 빼앗을 수 있는 것

> 앞서 이야기했던 SJF는 priority scheduling의 일종이다.

마찬가지로 우선순위 스케줄링도 `Starvation(기아 현상)`이 단점이다. 마찬가지로 우선순위가 낮은 프로세스는 평생 CPU를 받지 못할 수 있는 상황이 발생할 수 있기 때문이다. 이러한 기아 현상 문제를 극복하기 위해 `Aging`이라는 기법을 사용한다.

* Aging
    * 아무리 우선순위가 낮은 프로세스라도, 시간이 지날 수록 우선 순위를 올려주어 끝내는 CPU를 받게 해주는 방식이다. (=경로 사상)

</br>
</br>

## Round Robin (RR)

현대적인 컴퓨터 시스템에서 사용하는 CPU 스케줄링은 Round Robin을 기반으로 하고 있다. 

* 각 프로세스는 동일한 크기와 할당 시간(time quantum)을 가진다.
    * 일반적으로 10-100 millisecaonds
* 할당 시간이 지나면 프로세스는 선점(preempted)당하고 ready queue의 제일 뒤에가서 다시 줄을 선다.
* n개의 프로세스가 ready queue에 있고 할당시간이 q time unit인 경우 각 프로세스는 최대 q time unit 단위로 CPU 시간의 1/n을 얻는다.
    * 어떤 프로세스도 (n-1)q time unit 이상 기다리지 않는다.

>* Performance
>* q large -> FCFS
>* q small -> context switch 오버헤드가 커진다.

Round Robin의 가장 큰 장점은 응답 시간이 빨라진다는 점이다.
* CPU를 최초로 얻기까지 걸리는 시간이 빠르다는 것이다. 
* 조금씩 CPU를 줬다 뺏었다 반복하기 때문에 누구든지 조금만 기다리게 되면 CPU를 한번 맛볼 수 있는 기회가 생기기 때문이다.
* CPU를 오래 쓸지 모르는 상황에서 구지 예측할 필요 없이 CPU를 짧게 사용하는 프로세스가 빨리 CPU를 쓰고 나갈 수 있게 해주는 것이다.

#

* Example: RR with Time Quantum = 20

![](https://i.imgur.com/B1EaBlm.png)

> RR은 SJF보다 average turnaround time은 길지만, response time은 더 짧게 가져갈 수 있다는 것이 장점이다.

</br>
</br>

## Multilevel Queue

프로세스들이 기다리는 줄(ready queue)이 여러개 있음을 의미한다.

![](https://i.imgur.com/IRI114A.png)

> 밑으로 갈 수록 우선순위가 낮은 프로세스 줄이다. 

우선순위가 다섯 가지로 나누어져 있는데, 우선 순위가 높은 큐에 프로세스가 존재한다면 우선적으로 해당 프로세스에게 CPU가 우선적으로 부여된다. 

즉 계층 구조처럼 위에 queue가 비워져야 아래 queue에게 CPU가 할당되는 구조인 것이다.

* foreground (interactive)
* background (batch - no human interaction)

각 큐는 독립적인 스케줄링 알고리즘을 가진다.

* foreground - RR
* background - FCFS

큐에 대한 스케줄링이 필요하다.
* 우선 순위 스케줄링으로 고정한다.
    * 우선 순위가 높은 큐에 모두 제공한 다음 우선 순위가 낮은 큐로 제공한다.
    * 하지만 starvation이 발생할 가능성이 존재한다.
* 따라서 이러한 starvation 발생을 줄이기 위해서 Time slice 방식을 사용해볼 수 있다.
    * 각 큐에 CPU time을 적절한 비율로 할당한다.
    * Eg., 전체 CPU 시간의 80%는 우선순위가 높은 줄에 부여하고, 20%는  우선순위가 낮은 줄에다가 부여하고...


</br>
</br>

## Multilevel Feedback Queue

![](https://i.imgur.com/78TpPu5.png)

프로세스가 다른 큐로 이동이 가능하다. 에이징(aging)을 이와 같은 방식으로 구현할 수 있다.

* Multilevel-feedback-queue scheduler를 정의하는 파라미터들
    * Queue의 수
    * 각 큐의 scheduling algorithm
    * 프로세스를 상위 큐로 보내는 기준
    * 프로세스를 하위 큐로 내쫓는 기준
    * 프로세스가 CPU 서비스를 받으려 할 때 들어갈 큐를 결정하는 기준

> 하지만 이 방법도 구현이 복잡하다는 것과 아직까지 이 방법도 starvation을 해결하지 못한다는 점, 스케줄링 오버헤드가 발생한다는 점을 단점으로 이야기할 수 있다.

</br>
</br>

## Multiple-Processor Scheduling

CPU가 여러 개인 경우 스케줄링은 더욱 복잡해진다.

* Homogeneous processor인 경우
    * Queue에 한줄로 세워서 각 프로세서가 알아서 꺼내가게 할 수 있다.
    * 반드시 특정 프로세서에서 수행되어야하는 프로세스가 있는 경우에는 문제가 더 복잡해진다.
* Load sharing
    * 일부 프로세서에 job이 몰리지 않도록 부하를 적절히 공유하는 메커니즘이 필요하다.
    * 별개의 큐를 두는 방법 vs 공동 큐를 사용하는 방법
* Sysmmatric Multiprocessing (SMP)
    * 각 프로세서가 각자 알아서 스케줄링을 결정한다.
* Asymmetric multiprocessing
    * 하나의 프로세서가 시스템 데이터의 접근과 공유를 책임지고 나머지 프로세서는 거기에 따른다.

</br>
</br>

## Real-Time Scheduling

* Hard real-time systems
    * Hard real-time task는 정해진 시간 안에 반드시 끝내도록 스케줄링해야 한다.
    * 데드라인이 존재하는 스케줄링을 의미한다. 정해진 시간안에 반드시 실행이 되야한다.
* Soft real-time computing
    * Soft real-time task는 일반 프로세스에 비해 높은 priority를 갖도록 해야한다.

</br>
</br>

## Thread Scheduling

* Local Scheduling
    * User level thread의 경우 사용자 수준의 thread library에 의해 어떤 thread를 스케줄할지 결정한다.
    * 운영체제가 아닌 사용자 프로세스가 어떤 스레드에 CPU를 줄지 결정하는 방법이다.
* Global Scheduling
    * Kernel level thread의 경우 일반 프로세스와 마찬가지로 커널의 단기 스케줄러가 어떤 thread를 스케줄 할지 결정한다.
    * 실제 프로세스 스케줄링 알고리즘에 근거하여 CPU를 분배한다.

</br>
</br>

## Algorithm Evaluation

어떤 알고리즘이 좋은지 평가할 수 있는 방법에 대해서 알아보자.

크게 3가지로 나누어볼 수 있다.

![](https://i.imgur.com/0Zted8l.png)

* Queueing models
    * 굉장히 이론적인 방법이다. 이론가들이 주로 하는 방법
    * 확률 분포로 주어지는 arrival rate와 service rate 등을 통해 각종 performance index 값을 계산한다.
* Implementation (구현) & Measurement (성능 측정)
    * 실측하는 방법이다.
    * 실제 시스템에 알고리즘을 구현하여 실제 작업(workload)에 대해서 성능을 측정 비교
* Simulation (모의 실험)
    * 실제로 돌리는 게 아니라 여러 예제들을 테스트해보는 방법이다.
    * 알고리즘을 모의 프로그램으로 작성후 trace를 입력으로 하여 결과 비교

</br>
</br>

## 스케줄링 알고리즘 한 눈 비교

![](https://i.imgur.com/j0TUlqY.jpg)

---

# Reference

- [반효경 교수님의 운영체제 강의(2014)](https://core.ewha.ac.kr/publicview/C0101020140328151311578473?vmode=f)
