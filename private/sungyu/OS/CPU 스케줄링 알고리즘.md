# CPU 스케줄링 알고리즘  

### 비선점 알고리즘(Non-Preepmtive)  
> 한 프로세스가 CPU를 할당받아 실행 중일 경우, 다른 프로세스들이 **CPU를 강제로 뺏을 수 없는** 방식  

<br>

**FCFS**(First Come, First Served)  
> 먼저 **도착한 순서대로** CPU를 할당하여 처리(큐 방식)  
<img width="341" alt="스크린샷 2026-02-14 21 24 20" src="https://github.com/user-attachments/assets/9dcdcbe9-bfa3-4fe4-9065-adb8ebbe0f47" />  

- P1 -> P2 -> P3 순서로 처리  
- 만약 처리시간이 긴 작업이 먼저 도착한다면 다른 프로세스의 대기시간이 길어지는 문제 발생 가능  

<br>

**SJF**(Shortest Job First)  
> 프로세스의 처리시간(실행시간)이 가장 짧은 프로세스부터 실행  
<img width="341" alt="스크린샷 2026-02-14 21 23 39" src="https://github.com/user-attachments/assets/41fcc3e0-6ad6-4478-9257-e3806741dcba" />

- P1 -> P3 -> P2 순서로 처리  
- 처리시간이 긴 프로세스는 우선순위가 뒤로 밀려 CPU를 영원히 할당받을 수 없는 문제 발생 가능  
- (현실적으로 프로세스가 얼마나 작업을 할당받는지 프로세스들의 작업량을 알 수 없음)  

<br>

**HRN**(Highest Response Ratio Next)  
> 대기 시간과 처리 시간을 고려하여 CPU를 할당  
> **우선순위 = (대기 시간 + 처리 시간) / 처리 시간**  
<img width="341" alt="스크린샷 2026-02-14 21 36 12" src="https://github.com/user-attachments/assets/e6f0fcbe-471c-4141-81b1-1511b9f83569" />

- P1 -> P2 -> P3 순서로 처리 (P2 = 1.9, P3 = 1.5)  
- 우선순위 가 높은 프로세스부터 처리  

<br>

### 선점 알고리즘(Preepmtive)  
> 한 프로세스가 CPU를 할당받아 실행 중이라도 다른 프로세스가 **CPU를 할당받을 수 있는** 방식  

<br>

**라운드로빈**(Round Robin, RR)  
> 프로세스 별로 할당 시간(Time Slice)동안 작업을 진행하다 작업을 완료하지 못하면 **대기 큐**의 맨 뒤로 가서 자기 차례를 기다리는 방식  
<img width="425" alt="스크린샷 2026-02-14 21 45 19" src="https://github.com/user-attachments/assets/2fa2c807-7f8a-494e-8a30-53eb35b84d14" />

- 현대 컴퓨터의 스케줄링인 우선순위 스케줄링 방식의 일종  
- Time Slice = 4 로 가정  
- P1 -> P2 -> P3 -> P1 -> P2 순서로 처리  

<br>

**SRF**(Shortest Remaining Time First)  
> 프로세스의 남은 시간이 가장 짧은 프로세스부터 실행  
> = SJF(Shortest Job Time) + 선점형 방식  
<img width="343" alt="스크린샷 2026-02-14 21 48 29" src="https://github.com/user-attachments/assets/7aabd916-01aa-4fcc-9233-44191621195e" />

*작업 중간에 더 짧은 작업이 들어오면 수행하던 프로세스 중지  

- P1 -> P2 -> P1 -> P3 -> P1 순서로 처리  
- 처리시간이 긴 프로세스는 지속적으로 다른 프로세스에게 CPU를 뺏기는 문제 발생 가능  

<br>

**다단계 피드백 큐**(Multi Level Feedback Queue)  
> 여러 개의 대기 큐를 두고 각 큐에 우선순위를 부여해, 우선순위가 높은 큐에 있는 프로세스에 CPU를 할당  

1. 우선순위가 높은 큐(Queue)에 있는 프로세스가 먼저 실행  
2. 우선순위가 같은 큐(Queue) 내부에서는 라운드로빈(Round Robin) 방식을 이용  
3. 프로세스가 주어진 CPU Time Slice 를 모두 소진하면, 우선순위기 한 단계 낮은 큐로 이동   
4. 프로세스가 Time Slice 안에 CPU를 양도하면, 현재 큐에 그대로 있음  

<img width="1070" height="421" alt="스크린샷 2026-02-14 21 55 34" src="https://github.com/user-attachments/assets/27b4dd97-48a0-4dd7-9d1d-41eeb19dbb75" />
<img width="1057" height="397" alt="스크린샷 2026-02-14 21 57 16" src="https://github.com/user-attachments/assets/c11d83b8-2f57-489f-99ce-b82d66bfe602" />

- P1 -> P2 -> P1 순서로 처리  
