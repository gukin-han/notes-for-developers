# 05 CPU Scheduling

## CPU and I/O Bursts in Program Execution

![스크린샷 2023-03-01 오전 9.24.55.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-01_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_9.24.55.png)

- 프로그램 관점 → CPU 사용과 I/O 실행을 반복하며 실행
- 빈번도가 다름 → 사람이 주로 사용하는 프로그램이 I/O burst가 빈번함 (interactive)

## CPU-burst Time의 분포

> ❓ CPU 스케쥴링이 필요한 이유?
> 

![스크린샷 2023-03-01 오전 9.27.34.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-01_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_9.27.34.png)

- 여러 종류의 job(= proecss)이 섞여있기 때문에 CPU 스케줄링이 필요하다.
    - interactive job에게 적절한 response 제공 요망
    - CPU와 I/O 장치 등 시스템 자원을 골고루 효율적으로 사용

> 📌 대부분의 CPU 시간은 CPU bound job이 많이 사용하나? No. 해석은 필요 없고 섞여있음을 알아야한다. interactive 한 프로세스에 우선적으로 부여하는게 유리하다.
> 

## 프로세스의 특성 분류

- 프로세스는 그 특성에 따라 다음 두 가지로 나눔
    - I/O - bound process
        - CPU를 잡고 계산하는 시간보다 I/O에 많은 시간이 필요한 job
        - many short CPU burst)
    - CPU-bound process
        - 계산 위주의 job (과학용 계산기)
        - few very long CPU bursts

## CPU Scheduler & Dispatcher

- 📌 CPU Scheduler (OS내 code)
    - Ready 상태의 프로세스 중에서 이번에 CPU를 줄 프로세스를 **고른다**
- Dispatcher (code)
    - CPU의 제어권을 CPU scheduler에 의해 선택된 프로세스에게 **넘긴다**
    - 이 과정을 context switch (문맥 교환)라고 한다
- CPU 스케쥴링이 필요한 경우는 프로세스에게 다음과 같은 상태 변화가 있는 경우
    - Running → Blocked (예: I/O 요청하는 시스템 콜)
    - Running → Ready (예: 할당 시간만료로 timer interrupt)
    - Blocked → Ready (예: I/O 완료 후 인터럽트)
    - Terminate

> 📌 1, 4에서의 스케쥴링은 **nonpreemtive** (= 강제로 빼앗기지 않고 자진 반납, 비선점형)
📌 All other scheduling is **preemptive** (= 강제로 빼앗음, 선점형)
> 

## 📌 Scheduling Criteria - Performance Index (measure) 성능 척도

**CPU 관점**

- **CPU utilization** (이용률)
    - keep the **CPU as busy as possible**
    - 전체 시간에서 CPU가 일한 시간의 비율
- **Throughput** (처리량)
    - # of processes that **complete** their execution per time unit
    - 주어진 시간동안 몇개의 작업을 처리했나

**프로세스 관점**

- **Turnaround time** (소요시간, 반환시간)
    - amount of time to execute a particular process
    - excute후 I/O해서 나갈때 까지의 총 시간
- **Waiting time** (대기 시간)
    - amount of time a process has been **waiting in the ready queue**
    - 총 기다린 시간
- **Response time** (응답 시간)
    - amount of time it takes **from when a request was submitted until the first response is produced**, not output (for time-sharing environment)
    - 처음으로 CPU를 얻기 까지 걸린 시간

> 중국집 주방장을 고용했다. 주방장이 쉬지 않고 일한 시간 비율을 높이는게 좋다 (CPU utilization). 손님이 첫 음식을 받기까지 (Responsive time)
> 

## Scheduling Algorithms

- FCFS (First-come First-serve)
- SJF (Shortest-Job-First)
- SRTF (Shortest-Remaining-Time-First)
- Priority Scheduling
- RR (Round Robin)
- Multilevel Queue
- Multilevel Feedback Queue

## FCFS (First-Come First-Serve)

> 먼저 온 순서대로 처리함 - non Preemptive
> 

![스크린샷 2023-03-01 오전 10.25.23.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-01_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_10.25.23.png)

![스크린샷 2023-03-01 오전 10.28.28.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-01_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_10.28.28.png)

- interative job은 앞에 사용시간이 긴 프로세스가 있는 경우 response time이 길기 때문에 비효율적
- Convoy effect: short process behind long process (= 호위효과)
- The convoy effect is the slow down of traffic due to queuing caused by slow processing.

## SJF (Shortest-Job-First)

> ❓ Non preemptive 와 preemptive의 차이?
❓ 문제점 그리고 왜 현실에서 사용하기 힘든지?
> 
- 각 프로세스의 다음번 CPU burst time을 가지고 스케쥴링에 활용
- CPU burst time이 가장 짧은 프로세스를 제일 먼저 스케쥴
- Two schemes:
    - Non preemptive
        
        ![스크린샷 2023-03-01 오전 10.33.49.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-01_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_10.33.49.png)
        
        - 일단 CPU를 잡으면 이번 CPU burst가 완료될 때까지 CPU를 선점 (preemptive) 당하지 않음
    - Preemptive
        
        ![스크린샷 2023-03-01 오전 10.35.52.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-01_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_10.35.52.png)
        
        - 현재 수행중인 프로세스의 남은 burst time 보다 더 짧은 CPU burst time을 가지는 새로운 프로세스가 도착하면 CPU를 빼앗김
        - 이 방법을 Shortest-Remaining-Time-First (SRTF) 이라고도 부른다
        - P1 입장 → P2가 burst time이 P1보다 짧기 때문에 CPU를 빼앗긴다.
        - 📌 burst time (사용시간)이 긴 프로세스는 기아현상(starvation)이 발생할 수 있다.
- SJF is **optimal**
    - 주어진 프로세스들에 대해 📌 **minimum average waiting time**을 보장 (Preemptive)
- 📌 SFJ 문제점 두 가지
    - burst time 이 긴 프로세스는 기아현상이 발생할 수 있다.
    - 사용 시간을 정확히 예측하기 힘들어 스케쥴링이 불가능 (과거 내역을 통해 추정은 가능)

## 다음 CPU Burst Time의 예측

- 다음번 **CPU burst time**을 어떻게 알 수 있는가? (input data, branch, user…)
- 추정 (**estimate**)만이 가능하다
- 과거의 CPU burst time을 이용해서 추정 (exponential averaging)
    
    ![스크린샷 2023-03-01 오전 10.52.25.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-01_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_10.52.25.png)
    
    ![스크린샷 2023-03-01 오전 10.53.41.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-01_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_10.53.41.png)
    
    - 최근은 가중치를 높게, 오래될 수록 가중치 낮게

> 📌 여러 곳에 자주 사용되는 알고리즘
> 

## Priority Scheduling

- A priority number (integer) is associated with each process
- highest priority를 가진 프로세스에게 CPU할당 (smallest integer = highest priority)
    - preemptive
    - nonpreemptive
- SJF는 일종의 priority scheduling이다.
    - priority = predicted next CPU burst time
- problem
    - Starvation (기아 현상): low priority processes may **never execute**
- solution
    - Aging: as time progresses increase the priority of the process.

## Round Robin (RR)

- 현대 CPU 스케쥴링은 RR을 기반으로 함 (preemptive)
- 각 프로세스는 동일한 크기의 할당 시간 (**time quantum**)을 가짐 (일반적으로 10-100ms)
- 할당 시간이 지나면 프로세스는 선점 (preempted) 당하고 ready queue의 제일 뒤에 가서 다시 줄을 선다
- n개의 프로세스가 ready queue에 있고 할당 시간이 **q time unit**인 경우 각 프로세스는 최대 q time unit 단위로 CPU 시간의 1/n을 얻는다 ⇒ 어떤 프로세스도 (n-1)q time unit 이상 기다리지 않는다.
- Performance (📝 극단적인 경우)
    - q large ⇒ FIFO
    - q small ⇒ context switch 오버헤드가 커진다 (📝 지나치게 잘게 자르면)
- Example: RR with time Quantum = 20
    
    ![스크린샷 2023-03-01 오전 11.05.47.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-01_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_11.05.47.png)
    

![스크린샷 2023-03-01 오전 11.09.04.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-01_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_11.09.04.png)

> 📌 Context switch 기능이 있기 때문에 RR이 가능
> 

## Multi level Queue

![스크린샷 2023-03-01 오전 11.37.15.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-01_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_11.37.15.png)

- Ready queue를 여러 개로 분할
    - **foreground** (interative)
    - **background** (batch - no human interaction) 📝 응답 시간이 빠를 필요 없음
- 각 큐는 독립적인 스케쥴링 알고리즘을 가짐
    - **foreground** - RR
    - **background** - FCFS
- 큐에 대한 스케쥴링이 필요
    - Fixed priority scheduling
        - serve **all from foreground then from background**
        - possibility of **starvation**
    - Time slice (📝 starvation 예방)
        - 각 큐에 CPU time을 적절한 비율로 할당
        - E.g., **80% to foreground in RR, 20% to background in FCFS**

## Multi level Feedback Queue

- 프로세스가 다른 큐로 이동 가능
- 에이징(aging)을 이와 같은 방식으로 구현할 수 있다.
- Multi level Feedback Queue scheduler를 정의하는 파라미터들
    - Queue의 수
    - 각 큐의 scheduling algorithm
    - Process를 상위 큐로 보내는 기준
    - Process를 하위 큐로 내쫓는 기준
    - 프로세스가 CPU 서비스를 받으려 할 때 들어갈 큐를 결정하는 기준

![스크린샷 2023-03-01 오전 11.44.55.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-01_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_11.44.55.png)

- Three queues:
    - Q0 - time quantum 8ms
    - Q1 - time quantum 16ms
    - Q2 - FCFS
- Scheduling
    - new job이 queue Q0로 들어감
    - CPU를 잡아서 할당 시간 8ms 동안 수행됨
    - 8ms동안 다 끝내지 못했으면 queue Q1으로 내려감
    - Q1에 줄서서 기다렸다가 CPU를 잡아서 16ms 동안 수행됨
    - 16ms에서 끝내지 못한 경우 queue Q2로 쫓겨남

> 처음 들어오는 프로세스는 우선순위를 높게준 RR에 넣어 한번 맛보게 해준다. 긴 프로세스는 아래로 이동하고 할당 시간은 많이 받지만, 위의 큐에서의 프로세스가 모두 끝날때까지 기다려야함. 즉, CPU 사용시간이 짧은 프로세스가 우선순위를 많이 받게 된다.
> 

## Multi processor Scheduling

- CPU가 여러 개인 경우 스케쥴링은 더욱 복잡해짐
- **Homogeneous processor인 경우**
    - Queue에 한줄로 세워서 각 프로세서가 알아서 꺼내가게 할 수 있다.
    - 반드시 특정 프로세서에서 수행되어야 하는 프로세스가 있는 경우에는 문제가 더 복잡해짐
- **Load sharing**
    - 일부 프로세서에 job이 몰리지 않도록 부하를 적절히 공유하는 메커니즘 필요
    - 별개의 큐를 두는 방법 vs. 공동 큐를 사용하는 방법
- **Symmetric Multi processing (SMP)**
    - 각 프로세서가 각자 알아서 스케쥴링 결정
- **Asymmetric multi processing**
    - 하나의 프로세서가 시스템 데이터의 접근과 공유를 책임지고 나머지 프로세서는 거기에 따름

## Real-Time Scheduling

- **Hard real-time systems**
    - Hard real-time task는 정해진 시간 안에 반드시 끝내도록 스케쥴링해야 함
- **Soft real-time computing**
    - soft real-time task는 일반 프로세스에 비해 높은 priority를 갖도록 해야 함
    - 예) 영화를 보는것 → deadline은 존재하지만 반드시 지킬 필요는 없다.

> 📌 데드라인을 고려해서 job 배치를 함, 주기적으로 사용되어야하는 제약조건이 있는 경우
> 

## Thread Scheduling

- **Local Scheduling**
    - User level thread의 경우 사용자 수준의 thread library에 의해 어떤 thread를 스케쥴할지 결정
    - 📝 OS는 스레드를 모름
- **Global Scheduling**
    - Kernel level thread의 경우 일반 프로세스와 마찬 가지로 커널의 단기 스케쥴러가 어떤 thread를 스케쥴할지 결정
    - 📝 OS는 스레드의 존재를 앎

## Alrorithm Evalution

![스크린샷 2023-03-01 오후 12.02.43.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-01_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_12.02.43.png)

- **Queuing models**
    - 확률분포로 주어지는 **arrival rate**와 **service rate**등을 통해 각종 **performance index**값을 계산
    - 📝 이론적인 방법 → 위에서 server == cpu
- **Implementation (구현) & Measurement (성능 측정)**
    - 실제 시스템에 알고리즘을 구현하여 실제 작업 (workload)에 대해서 성능을 측정 비교
    - 📝 예) 리눅스 소스코드를 수정해서, 어느쪽이 빨리 끝나는지 비교 → 오래 걸림
- **Simulaion (모의 실험)**
    - 알고리즘을 모의 프로그램으로 작성후 **trace** (input data)를 입력으로 하여 결과 비교
    - 📝 예) SJF에서 모델링 → 복잡하면 → 시뮬레이션 프로그램 이용
        
        ![스크린샷 2023-03-01 오후 12.07.23.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-01_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_12.07.23.png)