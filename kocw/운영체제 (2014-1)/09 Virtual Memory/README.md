# 09 Virtual Memory

## Demand Paging

- 실제로 필요할 때 page를 메모리에 올리는 것
    - I/O 양의 감소 (방어적 코드가 대부분을 차지)
    - Memory 사용량 감소
    - 빠른 응답 시간
    - 더 많은 사용자 수용
- valid / invalid ibt의 사용
    - 📌 invalid의 의미
        - 사용되지 않는 주소 영역인 경우
        - 페이지가 **물리적 메모리에 없는** 경우
    - 처음에는 모든 page entry가 invalid로 초기화
    - address translation시에 invalid bit이 set되어 있으면 ⇒ page fault

## Memory에 없는 page의 page table

![스크린샷 2023-03-02 오후 1.59.42.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-02_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_1.59.42.png)

- 논리적 메모리가 페이지로 구성
- disk (= backing store), swap area → invalid 로 표시
- 만약 CPU가 요청한 논리 주소를 주소 변환하려 할때 invalid일 경우 → page fault → I/O작업을 통해 (커널, trap) disk로

## 📌 Page Fault

- invalid page를 접근하면 MMU가 trap을 발생시킴 (page fault trap)
- kernel mode로 들어가서 page fault handler가 invoke됨
- 다음과 같은 순서로 page fault를 처리한다
    1. invalid reference? (예 - bad address, protection violation) ⇒ abort process
    2. get an empty page frame (없으면 뺏어온다: replace)
    3. 해당 페이지를 disk에서 memory로 읽어온다 (느린 작업이라서 다음 과정들을 수행함)
        1. disk I/O가 끝나기까지 이 프로세스는 CPU를 preempt 당함 (block)
        2. disk read가 끝나면 page tables entry 기록, valid/invalid bit = “valid”
        3. ready queue에 process를 insert → dispatch later
    4. 이 프로세스가 CPU를 잡고 다시 running
    5. 아까 중단되었던 instruction을 재개

## Steps in Handling a Page Fault

![스크린샷 2023-03-02 오후 2.11.38.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-02_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_2.11.38.png)

## Performance of Demand Paging

![스크린샷 2023-03-02 오후 2.13.03.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-02_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_2.13.03.png)

- 대부분의 경우 page fault 없음 → 만약 발생 → 엄청난 시간 오버헤드

## Free frame이 없는 경우

- page replacement (OS의 작업)
    - 어떤 frame을 빼앗아올지 결정해야 함
    - 곧바로 사용되지 않을 page를 쫓아내는 것이 좋음
    - 동일한 페이지가 여러 번 메모리에서 쫓겨났다가 다시 돌아올 수 있음
- Replacement Algorithm
    - **page-fault rate을 최소화**하는 것이 목표
    - 알고리즘의 평가
        - 주어진 page reference string에 대해 page fault를 얼마나 내는지 조사
    - reference string의 예
        - 1, 2, 3, 4, 1, 2, 5, 1, 2, 3, 4, 5

## Page Replacement

![스크린샷 2023-03-02 오후 2.23.57.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-02_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_2.23.57.png)

- 만약 write가 발생했다면? 그냥 지울 수 없다
- 어떤 page를 쫒아낼까?

## Optimal Algorithm

> 실제로 사용할수 없음
> 
- MIN (OPT): 가장 먼 미래에 참조되는 page를 replace
- 4 frames example
    
    ![스크린샷 2023-03-02 오후 2.26.15.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-02_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_2.26.15.png)
    
- 미래의 참조를 어떻게 아는가? → Offline algorithm
    - reference string을 미리 알고 있다는 가정하에 알고리즘 운영
- 다른 알고리즘의 성능에 대한 📌 **upper bound** 제공 (실제 시스템에서 사용하는 대신)
    - Belady’s optimal algorithm, MIN, OPT 등으로 불림

> 1~4번은 page fault 이후 4번이 가장 먼 미래에 참조되는 page이기 때문에 5번과 교환
> 

## FIFO (First in fisrt out) algorithm

> FIFO 먼저 들어온 것을 먼저 내쫓음
> 

![스크린샷 2023-03-02 오후 2.32.43.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-02_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_2.32.43.png)

- FIFO Anomaly (Belady’s Anomaly)
    - more frames ⇒ less page falut ?
- 1번이 가장 처음 들어왔기 때문에 최근의 참조 사실과 관계없이 쫓아낸다.

## LRU (Least Recently Used) Algorithm

> 가장 오래 전에 참조된 것을 지움
> 

![스크린샷 2023-03-02 오후 2.35.05.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-02_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_2.35.05.png)

- 과거를 가지고 victim을 선정 → 5를 위해 (2 → 1 → 4 → 3) 3번을 쫓아내고 5번을 넣어준다.

## LFU (Least Frequently Used) Algorithm

> 참조 횟수 (reference count)가 가장 적은 페이지를 지움
> 
- 최저 참소 횟수인 page가 여럿 있는 경우
    - LFU 알고리즘 자체에서는 여러 page중 임의로 선정
    - 성능 향상을 위해 가장 오래 전에 참조된 page를 지우게 구현할 수 도 있음
- 장단점
    - LRU 처럼 직전 참조 시점만 보는 것이 아니라 장기적인 시간 규모를 보기 때문에 page의 인기도를 좀 더 정확히 반영할 수 있음 (장점)
    - 참조 시점의 최근성을 반영하지 못함 (단점)
    - LRU보다 구현이 복잡합 (단점)

## LRU 와 LFU 알고리즘 예제

![스크린샷 2023-03-02 오후 2.40.47.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-02_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_2.40.47.png)

## 📌 LRU와 LFU 알고리즘의 구현

- LRU
    
    ![스크린샷 2023-03-02 오후 2.46.41.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-02_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_2.46.41.png)
    
    - 참조 시점을 기준으로 LinkedList를 만들어 관리
    - O(1) → 비교를 할 필요 없다 (쫓아낼 page는 가장 오래된것이기 때문에)
- LFU
    - 한 줄로 줄 세우기를 할 수없다 → 참조가 되면 비교 연산을 해야함 O(n)
    - 참조횟수에 따라 자리 바꿈을 해야하기 때문
        
        ![스크린샷 2023-03-02 오후 2.48.25.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-02_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_2.48.25.png)
        
    - heap을 이용해 구현 → 최악의 경우 O (log n)
        
        ![스크린샷 2023-03-02 오후 2.50.13.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-02_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_2.50.13.png)
        

## 📌 다양한 캐슁 환경

- 캐슁 기법
    - 한정된 빠른 공간 (=캐쉬)에 요청된 데이터를 저장해 두었다가 후속 요청시 캐쉬로 부터 직접 서비스를 하는 방식
    - paging system외에도 cache memory, buffer cache memory, buffer caching, web caching 등 다양한 분야에서 사용
        - buffer와 paging의 매체는 동일 → 다음 챕터에서 자세히 설명됨
        - web caching은 지리적으로 멀리 떨어져있는 컴퓨터로 부터 불러오기 때문
- 캐쉬 운영의 시간 제약
    - 교체 알고리즘에서 📌 **삭제할 항목을 결정하는 일에 지나치게 많은 시간이 걸리는 경우** (< `O(n)`) 실제 시스템에서 사용할 수 없음
    - buffer caching이나 web caching의 경우
        - O(1)에서 O(log n) 정도까지 허용
    - paging system인 경우
        - page fault인 경우에만 OS가 관여함
        - 페이지가 이미 메모리에 존재하는 경우 참조 시각 등의 정보를 OS가 알 수 없음
        - O(1)인 LRU의 list 조작 조차 불가능

## Paging System에서 LRU, LFU 가능한가?

![스크린샷 2023-03-02 오후 3.12.13.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-02_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_3.12.13.png)

- 주소 변환은 하드웨어적 작업 → 만약 invalid인 경우 → page fault → disk 접근 (I/O) trap 발생 → kernel 모드
- 📌 이때 OS가 쫓아낼 대상을 찾는다 → 참조 횟수나 참조시기에 대한 정보를 알 수 있나? → 없다 → 페이지가 이미 메모리에 존재하기 때문
- 운영체제에게 반쪽 정보만 주어짐

## 📌 Clock algorithm

![스크린샷 2023-03-02 오후 3.30.10.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-02_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_3.30.10.png)

- clock algorithm ⇒ 시계 바늘이 움직이면서 작동하는 것 처럼 보임
    - LRU의 근사 (approximation) 알고리즘
    - 여러 명칭으로 불림
        - second chance algorithm
        - NUR (not used recently) 또는 NRU (not recently used)
    - reference bit을 사용해서 교체 대상 페이지 선정 (circular list)
    - refererence bit가 0인 것을 찾을 때까지 포인터를 하나씩 앞으로 이동
    - 포인터가 이동하는 중에 reference bit 1은 모두 0으로 바꿈
    - reference bit이 0인 것을 찾으면 그 페이지를 교체
    - 한 바퀴 되돌아와서도 (=second chance) 0이면 그때에는 replace 당함
    - 자주 사용되는 페이지라면 second chance가 올 때 1
- clock algorithm의 개선 (+ 하드웨어)
    - **reference bit**과 modified bit (dirty bit)을 함께 사용
        - refererence bit = 1 : 최근에 참조된 페이지
        - modified bit = 1: 최근에 **변경된** 페이지 (I/O를 동반하는 페이지)
            - 그냥 지우면 안되고 backing store(disk)에 변경 사항을 반영해야한다

## Page Frame의 Allocation

> 실제로는 프로그램 여러개가 동시에 올라와 있음 → 특정 프로그램이 장악할 수 있음
> 
- Allocation problem: 각 process에 얼마 만큼의 page frame을 할당할 것인가?
- Allocation의 필요성
    - 메모리 참조 명령어 수행시 명령어, 데이터 등 여러 페이지 동시 참조
        - 📌 명령어 수행을 위해 최소한 할당되어야 하는 frame의 수가 있음
    - Loop를 구성하는 page들은 한꺼번에 allocate되는 것이 유리함
        - 최소한의 allocation이 없으면 매 loop마다 page fault
- Allocation scheme
    - **equal allocation**: 모든 프로세스에 똑같은 갯수 할당
    - **propotional allocation**: 프로세스 크기에 비례하여 할당
    - **priority allocation**: 프로세스의 priority에 따라 다르게 할당 (CPU 우선순위)

## Global vs. Local Replacement

**📂 Global replacement**

> 미리 frame을 할당하지 않고 그때그때 할당하면 알아서 조절?
> 
- replace 시 다른 process에 할당된 frame을 빼앗아 올 수 있다
- Process별 할당량을 조절하는 또다른 방법임
- FIFO, LRU, LFU 등의 알고리즘을 global replacement로 사용시에 해당
- Working set, PFF 알고리즘 사용

**📂 Local replacement**

- 자신에게 할당된 frame 내에서만 replacement
- FIFO, LRU, LFU 등의 알고리즘을 process 별로 운영시

## Thrashing

> 프로세스의 원활한 수행에 필요한 최소한의 page frame 수를 할당 받지 못한 경우 발생
> 
- Page fault rate이 매우 높아짐
- CPU utilization이 낮아짐
- OS는 MPD (Multi programming degree)를 높여야 한다고 판단
- 또 다른 프로세스가 시스템에 추가됨 (higher MPD)
- 프로세스 당 할당된 frame의 수가 더욱 감소
- 프로세스는 page의 swap in / swap out으로 매우 바쁨
- 대부분의 시간에 CPU는 한가함
- low throughput

⇒ MPD를 조절해야함 ⇒ Working-Set model, PFF

### 📂 **Diagram**

![스크린샷 2023-03-02 오후 3.51.13.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-02_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_3.51.13.png)

- I/O를 하러 간 상황에 CPU 이용률이 낮아짐
- MPD를 올리는 방식으로 utilization을 높임
- 어느 순간 이용률이 drop

## Working-Set Model

- Locality of reference
    - 프로세스는 **특정 시간 동안 일정 장소만을 집중적으로 참조**한다
        - 특정 함수의 실행
    - 집중적으로 참조되는 해당 page들의 집합을 locality set이라고 함
- working-set model
    - locality에 기반하여 프로세스가 일정 시간 동안 원활하게 수행되기 위해 한꺼번에 메모리에 올라와 있어야 하는 page들의 집합을 📌 **working set**이라고 정의함
    - working set 모델에서는 process의 working set 전체가 메모리에 올라와 있어야 수행되고 그렇지 않을 경우 모든 frame을 반납한 후 swap out (suspend)
    - Thrashing을 방지함
    - multi programming degree를 결정함

## Working-Set Algorithm

- 과거를 통해 Working set의 추정
    - working set window를 통해 알아냄
    - window size 가 𝚫인 경우
        
        ![스크린샷 2023-03-02 오후 4.06.54.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-02_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_4.06.54.png)
        
        - (즉, 참조된 후 𝚫 시간 동안 해당 page를 메모리에 유지한 후 버림)

![스크린샷 2023-03-02 오후 4.07.10.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-02_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_4.07.10.png)

- 현재 시점에서 윈도우 사이즈 만큼의 과거 참조 페이지 까지 이 프로그램의 working set이므로 메모리에 올려 놓아야한다.
- working set의 크기가 그때그때 바뀐다 (t1과 t2일때를 비교)

### **📂 Working-Set Algorithm**

- process들의 working set size의 합이 page frame의 수보다 큰 경우
    - 일부 process를 swap out 시켜 남은 process의 working set을 우선적으로 충족시켜 준다 (MPD를 줄임)
- working set을 다 할당하고도 page frame이 남는 경우
    - swap out 되었던 프로세스에게 working set을 할당 (MPD를 키움)

### **📂 Window size 𝚫**

- working set을 제대로 탐지하기 위해서는 window size를 잘 결정해야함
- 𝚫 값이 너무 작으면 locality set을 모두 수용하지 못할 우려
- 𝚫 값이 너무 크면 여러 규모의 locality set 수용
- 𝚫 값이 ∞이면 전체 프로그램을 구성하는 page를 working set으로 간주

## PFF (page-fault frequency) scheme

![스크린샷 2023-03-02 오후 4.15.53.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-02_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_4.15.53.png)

- page-fault rate의 상한값과 하한값을 둔다
    - page fault rate이 상한값을 넘으면 frame을 더 할당한다
    - page fault rate이 하한값 이하이면 할당 frame 수를 줄인다
- 빈 frame이 없으면 일부 프로세스를 swap out

## Page Size의 결정

- Page size를 감소시키면
    - 페이지 수 증가
    - 페이지 테이블 크기 증가
    - internal fragmentation 감소
    - disk transfer의 효율성 감소
        - seek/rotation vs. transfer
    - 필요한 정보만 메모리에 올라와 메모리 이용이 효율적
        - locality의 활용 측면에서는 좋지 않음
- Trend
    - larger page size (> 4KB)