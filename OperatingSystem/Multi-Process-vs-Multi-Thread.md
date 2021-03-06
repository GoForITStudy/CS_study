## 1. 주요 개념
<br/>

### **동시성 - Concurrent vs Parallel(프로세스 vs 멀티 프로세스)**

- **Concurrent** : 어떤 **Job 여러 개가 동시에** 처리된다는 개념
- **Parallel** : 어떤 **하나의 Job을 쪼개**서 여러 sub-job으로 나누고, 이를 물리적으로 분리된 구조에서 동시에 처리해서 완성하는 개념
    - Ex. 자동차 조립을 여러 사람이 동시에 하는 것, **CPU의 Core 여러 개** 표현

멀티 프로세서나 멀티 코어 구조가 발전하기 전에는 싱글 프로세서로 재빠르게 프로세스를 전환하여 **concurrent하게 동작하지만 parallel하게 동작하는 것처럼 보이도록 하였다.**

![https://github.com/Seogeurim/CS-study/raw/main/contents/operating-system/materials/%EB%8F%99%EC%8B%9C%EC%84%B1.jpeg](https://github.com/Seogeurim/CS-study/raw/main/contents/operating-system/materials/%EB%8F%99%EC%8B%9C%EC%84%B1.jpeg)

### **컨텍스트 스위칭 (Context-Switching)**

Context-Switching 이란, **CPU 코어를 다른 프로세스로 전환하기 위해 현재 프로세스의 상태 저장 및 다른 프로세스의 상태 복원을 수행하는 작업**을 말한다.

- **Context란 CPU가 프로세스를 실행하기 위한 정보**를 말하며, **PCB에 저장**되는 정보들이 해당된다.
- Context-Switching이 발생하면 **커널이 이전 프로세스의 context를 그 프로세스의 PCB에 저장**하고 **새롭게 실행할 (스케줄링으로 예약) 프로세스의 저장된 context를 불러**오게 된다.
- **Context-Switching 수행 중에는 CPU의 자원이 어떤 프로세스에 할당된 상태가 아니기 때문에 CPU가 아무 작업도 할 수 없다.** (따라서 Context-Switching time은 pure **overhead**)

<br/><br/>

## 2. 멀티 프로세스

하나의 컴퓨터에 여러 CPU 장착 → 여러 개의 프로세스를 동시에 수행하는 것이다.(병렬)

> 프로세스는 부모-자식 관계라고 해도 **자신만의 메모리 영역**을 가지게 되며, 공유되는 메모리 영역 없이 **독립적**인 구조를 가진다.

### **크롬 브라우저의 멀티 프로세스 구조**

대부분의 브라우저는 탭 브라우징을 지원한다. 만일 **브라우저가 멀티 프로세스 구조를 가지지 않는다면, 어떤 탭의 웹 어플리케이션이 비정상 종료되었을 때 다른 모든 탭을 포함한 전체 프로세스가 종료**될 것이다.

구글의 크롬 브라우저는 멀티 프로세스 구조를 가지고 있다. 브라우저의 각 탭은 **(Renderer) 프로세스**이며, 이들은 각자 **독립적으로 실행**된다. 하나의 웹 사이트가 비정상 종료되어도 **다른 (Renderer) 프로세스는 영향을 받지 않는다.**

크롬은 다음과 같은 3가지 유형의 프로세스를 지원한다.

- **브라우저 프로세스** : 사용자 인터페이스와 디스크 및 네트워크 I/O를 관리한다. 크롬이 시작되면 새 브라우저 프로세스가 생성된다.
- **Renderer 프로세스** : 웹 페이지 **렌더링을 위한 로직(HTML, JavaScript, 이미지 등 처리)**을 포함한다. 이 때, 새 탭에서 열리는 각 웹 사이트에 대해 새 Renderer 프로세스가 생성되므로 여러 프로세스가 동시에 활성화될 수 있다.
- **플러그인 프로세스** : Flash 또는 QuickTime과 같은 **각 플러그인 유형에 대해 플러그인 프로세스가 생성**된다. 플러그인 프로세스에는 **플러그인에 대한 코드와 연관된 Renderer**, 브라우저 프로세스와 통신할 수 있도록 하는 추가 코드가 포함되어 있다.

### **멀티 프로세스의 통신 방법**

독립적인 메모리 영역을 가지는 프로세스끼리도 통신하는 방법이 있다. 데이터를 교환하기 위해서 **IPC (Inter-Process Communication)** 메커니즘이 필요하다. IPC에는 **공유 메모리 (shared memory)** 와 **메시지 전달 (message passing)** 의 두가지 모델이 있다.

- 공유 메모리 : 프로세스가 공유하는 메모리 영역이 설정되며, 각 프로세스는 공유 영역에서 데이터를 읽고 쓰는 방식으로 정보를 교환할 수 있다.
- 메시지 전달 : 프로세스 간 **메시지를 교환하며 통신**한다.

### **멀티 프로세스의 장점**

- 높은 안정성 : 메모리 침범 문제를 OS차원(독립된 구조 가짐)에서 해결
- 하나의 프로세스가 비정상적으로 종료되어도 자식 프로세스 이외의 다른 프로세스들은 아무런 영향을 받지 않는다. 위 장점에 대한 근거와 같은 이유에서 비롯한다.

### **멀티 프로세스의 단점**

- 독립된 메모리 영역을 가지고 있기 때문에 Context Switching을 위한 오버헤드(캐시 초기화 등)가 발생한다.
- Context Switching이 빈번하게 일어나면 성능 저하를 유발할 수 있다.

<br/><br/>

## 3. 멀티 스레드 **(Multi-Thread)**

한 프로세스(=하나의 응용 프로그램)에서 여러 개의 쓰레드를 동시에 수행하여 하나의 작업을 처리하는 것

![https://github.com/Seogeurim/CS-study/raw/main/contents/operating-system/materials/%EB%A9%80%ED%8B%B0%EC%8A%A4%EB%A0%88%EB%93%9C.png](https://github.com/Seogeurim/CS-study/raw/main/contents/operating-system/materials/%EB%A9%80%ED%8B%B0%EC%8A%A4%EB%A0%88%EB%93%9C.png)

### **멀티 스레드를 적용한 어플리케이션의 예시 (Multi-threaded applications example)**

- **웹 서버** 프로세스는 **클라이언트 요청**이 들어오면 그 요청을 처리하기 위한 **별도의 스레드**를 생성한다. 이는 새 프로세스를 생성하는 것보다 비용적인 측면에서 훨씬 효율적이다.
- **운영체제의 커널은 멀티 스레드**이다. **Linux 시스템 부팅 시 여러 커널 스레드가 생성**되고, 각 스레드는 장치 관리, 메모리 관리 또는 인터럽트 처리와 같은 특정 작업을 수행한다. (`ps -ef` 명령을 사용하여 실행 중인 Linux 시스템에서 커널 스레드를 표시 할 수 있다.)
- **웹 브라우저**는 하나의 스레드에서는 이미지나 텍스트를 보여주고, 또다른 스레드에서는 네트워크를 통해 데이터를 검색할 수 있다.(참고로 웹 서버는 서버, 웹 브라우저는 클라이언트라 이해하면 된다.)
- **워드 프로세서**는 그래픽을 표시하기 위한 스레드, 사용자의 키 입력에 응답하기 위한 스레드, 백그라운드에서 맞춤법 및 문법 검사를 수행하기 위한 스레드를 가질 수 있다.

### **멀티 스레드의 장점**

- **응답성**이 좋아진다. 단일 스레드를 사용하면 그 작업이 완료될 때까지 응답을 기다려야 한다. 멀티 스레드를 사용함으로서 응답성을 향상시킬 수 있다.
- **자원을 공유**할 수 있다. 프로세스는 공유 메모리 및 메시지 전달과 같은 기술을 통해서만 자원을 공유할 수 있다. 하지만 스레드는 **기본적으로 자신이 속한 프로세스의 자원을 공유**하기 때문에 동일한 주소 공간 내에서 여러 스레드를 가질 수 있다.
- **비용**이 적다. 스레드는 자신이 속한 프로세스의 자원을 공유하므로 **스레드 생성과 Context-Switching 비용(프로세스 간 변환 시 드는 비용)**이 더 적다.

### **멀티 스레드의 단점**

- 스레드는 프로세스 내 자원을 공유하기 때문에 **스레드 하나에서 오류가 발생하면 같은 프로세스 내의 모든 스레드가 종료**될 수 있다.
- 공유 자원에 대한 동기화 문제를 고려해야 한다.
    - 서로 다른 스레드가 **데이터와 힙 영역을 공유**하기 때문에 어떤 스레드가 다른 스레드에서 사용중인 변수나 자료구조에 접근하여 **엉뚱한 값을 읽어오거나 수정할 수** 있다.
    - 그렇기 때문에 멀티스레딩 환경에서는 **동기화 작업이 필요**하다. 동기화를 통해 **작업 처리 순서를 컨트롤 하고 공유 자원에 대한 접근을 컨트롤** 하는 것이다. 하지만 이로 인해 병목현상이 발생하여 성능이 저하될 가능성이 높다. 그러므로 과도한 락으로 인한 병목현상을 줄여야 한다.
        - [Critical Section](https://genesis8.tistory.com/154) 기법 통해 문제 발생에 대한 대비 가능

<br/><br/>

## 4. **멀티 스레드 vs 멀티 프로세스**

이 두 가지는 **동시에 여러 작업을 수행**한다는 점에서 같지만 적용해야 하는 시스템에 따라 적합/부적합이 구분된다. 따라서 **대상 시스템의 특징에 따라 적합한 동작 방식을 선택**하고 적용해야 한다.

---

- 참고자료

[프로세스 & 스레드 | 👨🏻‍💻 Tech Interview](https://gyoogle.dev/blog/computer-science/operating-system/Process%20vs%20Thread.html#%E1%84%86%E1%85%A5%E1%86%AF%E1%84%90%E1%85%B5%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%89%E1%85%A6%E1%84%89%E1%85%B3)

[Interview_Question_for_Beginner/OS at master · JaeYeopHan/Interview_Question_for_Beginner](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/OS#%EB%A9%80%ED%8B%B0-%EC%8A%A4%EB%A0%88%EB%93%9C)

[CS-study/contents/operating-system at main · Seogeurim/CS-study](https://github.com/Seogeurim/CS-study/tree/main/contents/operating-system#%EB%A9%80%ED%8B%B0-%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4%EC%99%80-%EB%A9%80%ED%8B%B0-%EC%8A%A4%EB%A0%88%EB%93%9C)

[크리티컬 섹션(Critical Section)](https://genesis8.tistory.com/154)