# 03 Process

## 📌 프로세스의 개념

> 프로세스란? 프로세스의 문맥을 나타낼 수 있는 세 가지?
> 

![스크린샷 2023-02-28 오후 2.17.24.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-02-28_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_2.17.24.png)

- 📌 “Process is **a program in execution**”
- 프로세스의 문맥 (📌 **context**)을 크게 세 가지로 설명 가능 ▼
    - (1) CPU 수행 상태를 나타내는 하드웨어 문맥 (CPU로 부터)
        - **Program Counter** (PC) 가 특정 code를 가리킴
        - 각종 register가 어떤 값을 가지고 있는지?
    - (2) 프로세스의 주소 공간 (독자적) (자신으로 부터)
        - code, data, stack
    - (3) 프로세스 관련 커널 자료 구조 (운영체제로 부터)
        - PCB (Process Control Block)
        - Kernel stack 📝 시스템 콜을 통해 CPU가 커널의 함수를 호출하여 특정 프로세스만의 스택에 저장

> 📝 **memo**
- 메모리에 무엇을 담고 있는가? 코드 어느 부분을 가리키는가? 지금 변수의 값은 얼마이며? 레지스터에 어떤 값을 넣고, 어떤 명령어를 실행하고 있는가 (현재 상태를 나타내는데 필요한 요소들 → 문맥)
- 커널 스택 또한 프로세스의 스택 처럼 프로세스만의 독자적인 공간을 가지고 있다.
- 다음에 CPU를 잡았을때, 이전의 문맥을 파악하지 못하면 비효율적으로 처음부터 실행할 수 밖에 없다.
> 

## 프로세스의 상태 (Process State)

> ❓프로세스의 상태를 크게 세 가지로 구분?
❓blocked와 suspended의 차이점?
> 

![스크린샷 2023-02-28 오후 2.48.45.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-02-28_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_2.48.45.png)

- 프로세스는 상태 (state)가 변경되며 수행된다 (📌 CPU 관점) → 📝 프로세서가 하나이기 때문
    - **Running**
        - CPU를 잡고 instruction을 수행중인 상태
    - **Ready**
        - CPU를 기다리는 상태 (메모리 등 다른 조건을 모두 만족하고) → in memory
    - **Blocked (wait, sleep)**
        - CPU를 주어도 당장 instruction을 수행할 수 없는 상태
        - Process 자신이 요청한 event(예: I/O)가 즉시 만족되지 않아, 이를 기다리는 상태
        - 예) 디스크에서 file을 읽어와야 하는 경우
    - **New**: 프로세스가 생성중인 상태
    - **Terminated**: 수행 (execution)이 끝난 상태
    - + **Suspended** (Stopped) ← 📝 중기 스케쥴러의 등장으로 새로 생긴 상태
        - 외부적인 이유로 프로세스의 수행이 정지된 상태
        - 프로세스는 통째로 디스크에 swap out 된다 (메모리를 빼앗음)
        - 예) 사용자가 프로그램을 일시 정지시킨 경우 (break key), 시스템이 여러 이유로 프로세스를 잠시 중단시킴 (메모리에 너무 많은 프로세스가 올라와 있을 때)

> Blocked: 자신이 요청한 event가 만족되면 Ready
Suspended: 외부에서 resume해 주어야 Active
> 

## 프로세스 상태도

![스크린샷 2023-02-28 오후 2.36.04.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-02-28_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_2.36.04.png)

- Ready, Running, Waiting
    - CPU를 오래 사용하는 경우: ready → running → ready → running을 반복하다 terminated
    - 중간에 I/O 인터럽트가 발생하는 경우 running → waiting (blocked) → ready

![스크린샷 2023-02-28 오후 3.44.04.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-02-28_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_3.44.04.png)

- Ready, Running (2), Blocked, suspended(2; blocked, ready)
    - user mode와 kernel mode
    - 운영체제가 running을 하는게 아님 → monitor mode로 running을 하고 있다고 표현해야함
    - suspended 상태에서도 I/O같은 작업에 의해 suspended ready로 넘어갈 수 있다.

## Process Control Block (PCB)

![스크린샷 2023-02-28 오후 2.50.05.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-02-28_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_2.50.05.png)

- PCB (**포인터가 있어서 queue에서 linked**)
    - 운영체제가 각 프로세스를 관리하기 위해 프로세스당 유지하는 정보
    - 다음의 구성 요소를 가진다 (구조체로 유지)
        - (1) OS가 관리상 사용하는 정보
            - Process State, Process ID (PID)
            - scheduling information, priority (First come First serve는 아님)
        - (2) CPU 수행 관련 하드웨어 값
            - Program counter, registers
        - (3) 메모리 관련
            - Code, data, stack의 위치 정보
        - (4) 파일 관련
            - Open file descriptors…

## Context Switch (문맥 교환)

![스크린샷 2023-02-28 오후 2.55.58.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-02-28_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_2.55.58.png)

- **CPU를 한 프로세스에서 다른 프로세스로 넘겨주는 과정**
- CPU가 다른 프로세스에게 넘어갈 때 운영체제는 다음을 수행
    - CPU를 내어주는 프로세스의 상태를 그 프로세스의 PCB에 저장
    - CPU를 새롭게 얻는 프로세스의 상태를 PCB에서 읽어옴 (복원)

![스크린샷 2023-02-28 오후 3.07.46.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-02-28_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_3.07.46.png)

- System call이나 Interrupt 발생시 반드시 context switch가 일어나는 것은 아님
    - CPU의 점유가 프로세스 (A) → 프로세스 (B)으로 이동 → context switch
- timer interrupt는 사용 시간이 끝나 CPU를 다른 프로세스로 넘기기 위함임 → context switch

> (1)의 경우에도 CPU 수행 정보 등 context의 일부를 PCB에 save해야 하지만, 문맥교환을 하는 (2)의 경우 그 부담이 훨씬 큼 (e.g., cache memory flush)
> 

## 프로세스를 스케쥴링하기 위한 큐

![스크린샷 2023-02-28 오후 3.15.37.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-02-28_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_3.15.37.png)

![스크린샷 2023-02-28 오후 3.17.41.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-02-28_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_3.17.41.png)

- Job queue (Ready + Device)
    - 현재 시스템 내에 있는 모든 프로세스의 집합
- Ready queue (Job - Device)
    - 현재 메모리 내에 있으면서 CPU를 잡아서 실행되기를 기다리는 프로세스의 집합
- Device queues (Job - Ready)
    - I/O device의 처리를 기다리는 프로세스의 집합
- 프로세스들은 각 큐들을 오가며 수행된다

> 운영체제가 위를 관리한다. PCB의 linked queue 자료구조를 가지고 있다.
> 

## 📌 스케줄러 (Scheduler)

> ❓ 스케쥴러는 하드웨어인가 소프트웨어인가?
> 
- **Long-term scheduler** (장기 스케쥴러 or job scheduler)
    - 시작 프로세스 중 어떤 것들을 **ready queue(in memory)**로 보낼지 결정
    - 프로세스에 **memory (및 각종 자원)**을 주는 문제
    - **degree of Multiprogramming**을 제어 → 📝 메모리에 여러 프로그램이 동시에 올라가는 것
    - time sharing system에는 보통 자기 스케줄러가 없음 (무조건 ready)
- **Short-term scheduler** (단기 스케줄러 or CPU scheduler)
    - 어떤 프로세스를 다음번에 **running** 시킬지 결정
    - 프로세스에 **CPU**를 주는 문제
    - 충분히 빨라야 함 (ms 단위)
- **Medium-term scheduler** (중기 스케쥴러 or Swapper)
    - **여유 공간 마련을 위해 프로세스를 통째로 메모리에서 디스크로 쫓아냄**
    - 프로세스에게서 **memory**를 뺏는 문제
    - **degree of Multiprogramming**을 제어
    - 📝 중기 스케쥴러에 의해 추가된 상태가 있음 → Suspended

> new → ready에서 admitted는 메모리에 올라가는 것을 허락 (← 장기 스케쥴러가 결정).
메모리에 너무 적은, 너무 많은 프로세스가 동시에 올라가 있으면 성능이 좋지 않다.
요즘은 프로세스가 시작되면 메모리에 무조건 올라감 → 문제가 있음 → 중기 스케쥴러로 조정
> 

## 📌 Thread

![스크린샷 2023-02-28 오후 4.07.55.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-02-28_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_4.07.55.png)

- CPU 수행 단위가 여러개 있는 경우에 thread
- “A thread (or **lightweight process**) is a baic unit of CPU utilization”
- 📌 Thread의 구성
    - program counter
    - register set
    - stack space
- Thread가 동료 thread와 공유하는 부분 (=task) 프로세스 내에서
    - code section
    - data section
    - OS resources
- 전통적인 개념의 heavyweight process는 하나의 thread를 가지고 있는 task로 볼 수 있다.
- 장점
    - 다중 스레드로 구성된 태스크 구조에서는 하나의 서버 스레드가 blocked (waiting) 상태인 동안에도 동일한 태스트 내의 다른 스레드가 실행 (running)되어 빠른 처리를 할 수 있다.
        - 예) 웹브라우저를 여러개 스레드를 사용하면, 하나의 스레드는 네이버 웹서버에서 그림을 불러오는 동안에 프로세스를 blocked하지 않고, 다른 스레드는 display를 하면 사용자 입장에서는 더 빨리 화면을 볼 수 있다 (빠른 응답성).
    - 동일한 일을 수행하는 다중 스레드가 협력하여 높은 처리율 (throughput)과 성능 향상을 얻을 수 있다
    - 스레드를 사용하면 병렬성을 높일 수 있다

> 📝 프로세스가 생성되면 독자적인 주소공간을 그림의 오른쪽과 같이 가지게 되며, PCB에 그 정보들을 저장한다.
📝 같은 일을 하는 여러 프로세스들을 실행하고 싶지만, 공간 낭비가 있기 때문에 → thread 개념이 생김. 즉, PC를 여러개 (CPU 수행 단위를 여러개)를 가지는 것 + 레지스터 값을 별도로 유지.
📝 스레드 간에 메모리 주소와 같은 각종 자원들을 공유하지만, CPU 수행과 관련된 (stack, PC, 레지스터 값)은 별도로 가지게 된다.
> 

![스크린샷 2023-03-01 오전 1.05.21.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-01_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_1.05.21.png)

- 프로세스는 하나이기 때문에 PCB는 하나 생성.
- 여기서 스레드는 CPU 사용 정보만 별도로 가지게 된다.
    - Program counter, register
- 주소 공간에서 데이터는 공유, stack은 따로

## Single and Multi-thread processes

![스크린샷 2023-03-01 오전 1.07.11.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-01_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_1.07.11.png)

## 📌 Benefits of Threads

- Responsiveness (응답성)
    - 예) multi-threaded Web - if one thread is blocked (eg network) another thread continues (eg display) 📝 이미지 파일을 불러오는 동안 (I/O) 텍스트라도 먼저 보여주기 (비동기식 입출력)
- Resouce sharing (자원 공유)
    - n threads can share binary code, data, resource of the process
    - 📝 별도의 프로세스를 생성하는 것은 자원 낭비
- Economy (경제성)
    - **creating** & **CPU switching** **thread** (rather than a **process**)
    - Solaris의 경우 위 두 가지 overhead가 각각 30배, 5배 📝 문맥 교환은 오버헤드가 크다.
- Utilization of Multi Processor Architectures
    - each thread may be running in **parallel** on a **different processor**

## 스레드 구현 (Implementation of Threads)

- Some are supported by kernel → kernel threads
    - Windows 95/98/NT
    - Solaris
    - Digital Unix, Mach
- others are supported by library → User threads
    - POSIX Pthreads
    - Mach C-threads
    - Solaris threads
- Some are real-time threads