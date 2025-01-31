# 마이크로커널에서의 성능 이슈

## 마이크로커널
### 마이크로커널의 특징

1. ==**사용자와 커널모드**==
	마이크로커널에서는 사용자와 커널모드 두가지로 분리되어있다. 커널에는 운영체제에 필요한 최소한의 기능 (메모리 관리, 스케줄링, 기본 IPC)만 두고, 나머지는 사용자 모드에 두었다. 두개의 커널모드로 분리함으로써 하드웨어 의존성을 줄일 수 있고, 커널의 크기를 줄일 수 있다. *(L4 마이크로커널의 경우 12KB크기의 코드 크기를 가진다.)*^[1]

2. **독립된 주소공간을 가진 사용자 프로세스**
	하나의 프로세스가 타 시스템에 미치는 영향 최소화할 수 있다. (Fault Tolerance)

3. ==**IPC를 통한 통신**==
	사용자 프로세스는 독립된 주소공간에 있으며 커널과 다른 주소 공간에 있기 때문에, IPC를 통한 통신이 필수적이다. 특히 독립된 주소 공간을 사용하는 사용자 프로세스, 커널 사이의 통신은 모놀리식커널(Monolithic kernel)과 비교해 자주 발생한다. 따라서 효율적인 IPC 메커니즘은 중요하다.
## Inter-process Communication

### 1. 메시지패싱 (Message passing)
IPC메커니즘 중 하나인 메시지 패싱 방식을 알아보자. L4 마이크로커널의 파생 중 하나인 OKL4에서 사용하는 메시지 패싱 방식을 다룬다.^[2]

- I**PC Operation**
	1. **Send operation**: 호출 쓰레드에서 종점 쓰레드로 메시지 전달시 사용한다.
	2. **Receive operation**: 다른 쓰레드로부터 메시지 수신 요청을 하기 위해 사용한다.

- **IPC Operation Mode**
	1. **Blocking IPC operation**: 요청한 IPC operation이 수행될 때까지 쓰레드 중지, 대기한다.
	2. **Non-blocking IPC operation**: 타깃 쓰레드가 현대 메시지 교환 참여 가능 상태가 아니면 실패(abort)한다.

- **Message**
	기본적으로 ==메시지데이터레지스터==를 이용해 전달한다. 해당 레지스터가 정보를 전달하기에 충분하지 않으면 공유메모리 방식을 사용해 전달. 메시지는 동기적으로 전달된다. (비동기방식도 지원)
	1. Message Tag![[MessageTag structure.png]]
		S: Send operation
		r: Receive operation
		n: Asynchronous notification operation
		m: Memory copy operation
		U: Number of words in message
	1. Message Data
## Performance Issue

---
[^1]: [Asif Iqbal, Nayeema Sadeque and Rafika Ida Mutia. "Microkernels"](https://www.eit.lth.se/fileadmin/eit/project/142/microkernels.pdf)
[^2]: [Rafika Ida Mutia, “Inter-Process Communication Mechanism in Monolithic Kernel and Microkernel”, 2010](https://www.semanticscholar.org/paper/Inter-Process-Communication-Mechanism-in-Monolithic-Mutia/8dc070438481e132a989654c53b6be6c1922ad13)