# CPU 스케줄링

## CPU Scheduler
### Preemptive Scheduling
실행상태 -> 대기상태 또는 종료상태에서만 스케쥴링이 발생한 경우
프로세스가 CPU를 방출할 때까지 점유, 즉 문맥교환전 해당 프로세스가 봉쇄되기를 대기함
### Nonpreemptive Scheduling
비선점 스케쥴링 외의 발생하는 스케줄링
경쟁조건 유발 -> 공유 커널 데이터 구조에 액세스시 경쟁 조건 방지를 위한 기법 필요