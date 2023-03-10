# CPU Scheduling

**cpu scheduling 필요한 이유**: i/o를 많이 사용하는 job, cpu를 많이 사용하는 job이 섞여있기 때문에 cpu scheduling 이 필요하다.

각 프로그램마다 CPU burst와 i/o burst가 차지하는 비율이 균일하지 않다. 빈번한 기준으로 프로세스를 크게 CPU 바운드 프로세스, I/O 바운드 프로세스로 나눔

### CPU burst

사용자 프로그램이 CPU를 직접 가지고 빠른 명령을 수행하는 단계

### I/O burst

커널에 의해 입추력 작업을 진행하는 비교적 느린 단계

## CPU Scheduling

- 자원을 어떤 프로세스에 얼마나 할당할지

<aside>
💡 컴퓨터 시스템 내에서 수행되는 프로세스의 CPU 버스트를 분석해보면 대부분 짧은 CPU 버스트. 극히 일부만 긴 CPU 버스트. 즉, 대부분 CPU를 한 번에 잠깐 사용하고 I/O 작업을 수행하는 프로세스가 더 많다는 것. 사용자에 빠른 응답을 위해서는 해당 프로세스에게 우선적으로 CPU를 할당하는 것이 바람직함.

</aside>

## CPU Scheduling 알고리즘

### 비선점형 (non-preemptive)

- 강제로 CPU를 빼앗지 않는 방법
- 프로그램이 자진 반납을 할 때 까지 보장

### 선점형 (preemptive)

- 대부분 선점형
- 강제로 빼앗음

### 성능척도 (Scheduling Criteria)

시스템 입장

- 이용률 (utillization)
    - 전체 사용 시간 중에 놀지 않고 일을 한 비율
- 처리량 (Throughput)
    - 몇개의 작업을 완료했는지

프로세스 입장

- 소요시간 (Turnaround time)
    - cpu를 쓰고 빠져나갈 때 까지의 시간
- 대기시간 (Waliting time)
    - cpu를 쓰려고 ready queue에서 기다리는 시간
- 응답시간 (Response time)
    - 최초로 cpu를 얻기까지 기다리는 시간

# CPU Scheduling 알고리즘

## FCFS (First-Come First-Served)

- 비선점형
- cpu를 먼저 선점하면 뒤에 cpu를 아주 짧게 사용하는 경우도 기다려야 하기때문에 효율적이지 못한 편

## SJF (Shortest-Job-First)

- CPU burst time 이 가장 짧은 프로세스를 제일 먼저 스케줄
- 큐가 전체적으로 짧아짐
- **average waiting time 최소 보장 (선점형)**
- 두 가지 경우
    - 비선점형
    - 선점형: 더 짧은 새로운 프로세스가 도착하면 CPU를 빼앗음
        - **Shortest-Remaining-Time-First** (SRTF) 이라고도 부름
- 비선점형

    ![%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-01-10_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_11 34 00](https://user-images.githubusercontent.com/48097396/211521305-3ae5e82f-8dce-4382-a3b5-387c1d937254.png)
    
- 선점형
    
    ![%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-01-10_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_11 35 18](https://user-images.githubusercontent.com/48097396/211521423-217db9c4-57c7-4c05-b70b-d51d5ee1f36c.png)
    

### SJF 문제

**starvation** (기아상태)

→ cpu 사용시간이 긴 프로세스는 영원히 사용하지 못할 수도 있다.

### SJF 문제 해결책

→ Aging (노화): 오래됐으면 우선순위를 조금씩 올려주자

## Round Robin (RR)

현대의 컴퓨터들은 대부분 라운드 로빈

각 프로세스는 동일한 크기의 할당 시간(time quantum)을 가짐 (일반적으로 10~100 밀리세컨드)

할당 시간이 지나면 프로세스는 선점 당하고 레디큐의 제일 뒤에 가서 다시 줄을 선다.

(할당시간 = q)

**Prerformance**

- q large = FCFS
- q small = context switch 오버헤드가 커진다

## Multilevel Queue

![%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-01-10_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4 02 53](https://user-images.githubusercontent.com/48097396/211521547-96055aab-2924-46de-b3b3-86023f00fe92.png)

- Ready Queue가 여러개가 있고 우선순위가 정해져있는 스케줄링
- 공정성이 떨어짐

## Multilevel Feedback Queue

- 큐의 수
- 각 큐의 알고리즘
- 최상위(Q0) = 퀀텀 짧게 → 다음큐(Q1) = 퀀텀 좀더 길게 → 최하위(Q2) = FCFS
- 새로운 잡은 최상위에 들어감

## 멀티 프로세서 스케줄링 (Multiple-Processor Scheduling)

![%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-01-10_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6 44 57](https://user-images.githubusercontent.com/48097396/211521600-dcddf366-b6dc-4136-b2fb-08cd92e049a1.png)

- CPU가 여러개인 경우 스케줄링은 더욱 복잡해짐

## Real-Time Scheduling

정해진 시간 안에 반드시 끝내도록 스케줄링 해야함 (데드라인 보장)

**Hard real-time systems**

- 정해진 시간 안에 반드시 끝내도록 해야함. (데드라인 무조건 보장) 이런 경우 시스템에 매우 심각한 문제가 발생할 소지가 있을 수 있음

**soft real-time computing**

- 조금 어겨도 일반 프로세스에 비해 높은 우선순위를 갖도록 해야함. (시간을 지키지 않으면 매우 심각한문제가 발생하는 것이 아닌 경우, 영화 방영 등)

## Thread Scheduling

**Local Scheduling**

- User level thread
- 사용자의 수준의 thread library에 의해 어떤 thread를 스케줄할지 결정
- 운영체제는 스레드를 모른다. 프로세스만 결정한다. 프로세스 내부에서 스레드를 결정한다.

**Global Scheduling**

- Kernel level thread
- 커널의 단기 스케줄러가 어떤 스레드를 스케줄할지 결정
- 운영체제가 스레드의 존재를 안다. 운영체제의 알고리즘에 기반하여 운영체제가 어떤 스레드에게 cpu를 줄지 결정

# 어떤 알고리즘이 좋은지 판단하는 성능측정 방법

1. Queueing models (큐잉 모델)
    - 확률 분포로로 주어지는 arrival rate,  service rate 등을 통해 퍼포먼스 인덱스 값을 계산(측정)
    - 실제 시스템에서 돌려보는 것을 더 의미있어 하기 때문에 많
2. implementation (구현) & Measurement (성능 측정)
    - 실제 시스템에 알고리즘을 구현하여 실제 작업에 대해서 성능을 측정 비교
    - 실제 두 개 이상의 컴퓨터를 상호 비교
3. Simulation (모의실험)
