# JVM 구조

## JVM이란?

JVM은 **Java Virtual Machine**의 약자로, 자바 가상 머신이라고 부릅니다.  
그리고 **자바와 운영체제 사이에서 중개자 역할을 수행**하며, 자바가 운영체제에 구애 받지 않고 프로그램을 실행할 수 있도록 도와줍니다.   
또한, **가비지 컬렉터**를 사용한 메모리 관리도 자동으로 수행하며, 다른 하드웨어와 다르게 레지스터 기반이 아닌 **스택 기반**으로 동작합니다.  
아래는 자바 프로그램의 실행 단계입니다.

<img width="400" src="https://user-images.githubusercontent.com/62924471/211124876-e4b2293b-2567-4050-97da-ff798e8b4ef4.png">

- 먼저, 자바 컴파일러에 의해 자바 소스 파일은 바이트 코드로 변환됩니다. 
- 이러한 **바이트 코드를 JVM에서 읽어 들인 다음에, 여러 과정을 거쳐서 어떤 운영체제든 간에 프로그램을 실행**할 수 있게 됩니다.
    - JVM이 해당 운영체제에 맞게 변환하여 동작하기 때문에 운영체제에서 자유롭습니다.
    - 즉, JVM 은 자바 프로그램이 **어느 기기 또는 어느 운영체제에서도 실행될 수 있게 해줍니다.**
- 다만, JVM 은 운영체제에 종속적이기 때문에 **운영체제에 맞는 JVM을 필요**로 합니다. 
    - 만약, 자바 소스 파일은 리눅스에서 만들었고 윈도우에서 이 파일을 실행하고 싶다면, 윈도우용 JVM을 설치합니다.
    
## 자바 가상 머신(JVM)의 동작 방식

<img width="600" src="https://blog.kakaocdn.net/dn/cQRqku/btru0vJ6Ixx/9qCTW7ChXc80fGfQUrT4B0/img.png">
<img width="600" src="https://user-images.githubusercontent.com/62924471/197547921-f3fcae65-77f7-46cc-b790-4d6b1c8ede77.png">


**1.** 자바 프로그램을 실행하면 **JVM이 OS로부터 메모리를 할당**받습니다.  
**2.** 자바 컴파일러(javac)가 자바 소스코드(.java)를 자바 바이트코드(.class)로 컴파일합니다.  
**3. Class Loader**를 통해 **JVM Runtime Data Area**로 로딩합니다.  
**4.** Runtime Data Area에 로딩 된 .class들은 **Execution Engine**을 통해 해석합니다.  
**5.** 해석된 바이트 코드는 Runtime Data Area의 각 영역에 배치되어 수행하며 이 과정에서 Execution Engine에 의해 GC의 작동과 스레드 동기화가 이루어집니다.

---

## **JVM의 구조**

JVM의 구조는 크게 **Garbage Collector, Execution Engine, Class Loader, Runtime Data Area** 4가지로 나눌 수 있습니다.

### (1) **클래스 로더(Class Loader)**
![image](https://user-images.githubusercontent.com/62924471/211125311-cec0108b-5cc6-4584-9e99-f15f9bcace9b.png)

- 자바는 동적으로 클래스를 읽어오므로, 프로그램이 실행 중인 런타임시 동적으로 클래스를 로드하여 모든 코드가 자바 가상 머신과 연결됩니다. 
- 이렇게 **동적으로 클래스를 로딩해주는 역할**을 하는 것이 바로 클래스 로더(class loader)입니다. 
- 클래스 로더는 이처럼 **JVM 내로 클래스 파일을 로드**하고, 링크를 통해 배치하는 작업을 수행하는 모듈입니다.

- 자바에서 소스를 작성하면 .java파일이 생성되고 .java 파일을 컴파일하면 .class파일이 생성됩니다. 
- 클래스 로더는 .class 파일을 묶어서 JVM이 운영체제로부터 할당받은 메모리 영역인 Runtime Data Area로 적재합니다.

### (2) **실행 엔진(Execution Engine)**

- 클래스 로더에 의해 JVM으로 로드된 .class 파일(바이트코드)들은 Runtime Data Areas의 Method Area에 배치됩니다.
- **JVM은 Method Area의 바이트 코드를 실행 엔진(Execution Engine)을 이용**하여, 바이트 코드를 해석하고 실행시킵니다. 
- 이 때, **로드된 바이트코드를 실행하는 런타임 모듈**이 실행 엔진(Execution Engine)입니다. 
  - 실행 엔진은 바이트코드를 **명령어 단위로 읽어서 실행**합니다.

- 최초 JVM이 나왔을 당시에는 **인터프리터 방식**이었기때문에 속도가 느리다는 단점이 있었지만 **JIT 컴파일러** 방식을 통해 이 점을 보완하였습니다. 
  - JIT(Just In Time)는 바이트 코드를 어셈블러 같은 네이티브 코드로 바꾸는 역할을 수행하며, 실행이 빠르지만 역시 변환하는데 비용이 발생하였습니다. 
  - 이 같은 이유로 JVM은 모든 코드를 JIT 컴파일러 방식으로 실행하지 않고, 인터프리터 방식을 사용하다가 일정한 기준(반복 사용)이 넘어가면 JIT 컴파일러 방식으로 실행합니다.

### (3) **가비지 컬렉터(Garbage Collector)**

![image](https://user-images.githubusercontent.com/62924471/211125325-1560be3f-d72b-4e54-8fca-690c7b27bb0b.png)

- 자바 가상 머신은 **가비지 컬렉터**(garbage collector)를 이용하여 더는 사용하지 않는 메모리를 자동으로 회수해 줍니다. 
  - **Heap 메모리 영역에 생성(적재)된 객체들 중에 참조되지 않은 객체들을 탐색 후 제거**하는 역할을 하며 해당 역할을 하는 시간은 정확히 언제인지를 알 수 없습니다. 
  - **GC역할을 수행하는 스레드를 제외한 나머지 모든 스레드들은 일시정지상태가 됩니다. (stop-the-world)**
  - ***가비지 컬렉터에 대해 더 자세한 내용 링크 →*** [[Java] 가비지 컬렉션(GC, Garbage Collection) 총정리](https://coding-factory.tistory.com/829)

---

### (4) **런타임 데이터 영역 (Runtime Data Area)**

- **JVM의 메모리 영역**으로 자바 애플리케이션을 실행할 때 사용되는 데이터들을 적재하는 영역입니다. 
- 이 영역은 크게 Method Area, Heap Area, Stack Area, PC Register, Native Method Stack로 나눌 수 있습니다.

<img width="600" src="https://user-images.githubusercontent.com/62924471/197548269-241e74a3-05de-4a7c-a57c-e11b92fe0438.png">
<img width="600" src="https://user-images.githubusercontent.com/62924471/211125393-889ba4c7-c90b-410d-95e2-cd2168756508.png">

- **런타임 데이터 영역은 JVM이 운영체제로부터 할당받은 메모리 영역**으로, **자바 애플리케이션을 실행할 때 사용되는 데이터들을 적재하는 영역**입니다.

1. **모든 스레드가 공유해서 사용하는 영역**
    - Method Area(메서드 영역)
    - Heap Area(힙 영역)
2. **스레드(Thread) 마다 하나씩 생성**
    - Stack Area(스택 영역)
    - PC Register(PC 레지스터)
    - Native Method Stack(네이티브 메서드 스택)

### 1. 모든 스레드가 공유해서 사용하는 영역

#### 1-1. **메서드 영역 (Method Area)**

1. **모든 쓰레드가 공유**하며, 클래스, 인터페이스, 메소드, 필드, Static 변수 등의 바이트 코드를 보관합니다.
    - 클래스 멤버 변수의 이름, 데이터 타입, 접근 제어자 정보와 같은 각종 필드 정보들과 메서드 정보, 데이터 Type 정보, Constant Pool, static변수, final class 등이 생성되는 영역입니다.
    - jvm 벤더(별 구현체)마다 이 메서드 영역이 존재하는 위치가 다를 수 있다고 합니다.
    - Java 8이후에는 PermGen이 완전히 제거 되어 **metaspace**로 대체되었으며, metaspace(Method Area)는 Native Heap에 할당됩니다.(힙 영역에서 다시 설명합니다.)
   
#### 1-2. **힙 영역 (Heap Area)**

1. **모든 쓰레드가 공유**하며, new 키워드로 생성된 **객체와 배열이 생성되는 영역**입니다.
    - 메소드 영역에 로드된 클래스만 생성이 가능하고 Garbage Collector가 참조되지 않는 메모리를 확인하고 제거하는 영역입니다.
2. 주기적으로 **GC가 제거하는 영역**입니다.
3. Perm Gen
  - Perm Gen 영역은 자바 8 이후 **metaspace**로 대체되었으며, 더 이상 힙 영역이 아닌 네이티브 메모리 영역으로 옮겨졌습니다.
    - PermGen에서는 Default로 Maximum size를 할당 하였는데 만약 해당 사이즈에 도달 하면 OutOfMemoryErr 가 생성되는 이슈가 존재했기 니다.
    - 과거의 PermGen은 현재 Metaspace 로 불리우는데 Method Area는 Meta 정보를 저장하기 때문입니다. 

<img width="600" src="https://user-images.githubusercontent.com/93559998/196344165-1fae6d5b-1310-4455-a142-d227aca2e910.png">

![image](https://user-images.githubusercontent.com/62924471/211125638-9078c9e4-c280-4e92-a216-d40328d31765.png)

- Heap Area는 효율적인 GC를 위해 위와 같이 크게 3가지의 영역으로 나뉘게 됩니다.
- **Young Generation 영역**은 자바 객체가 생성되자마자 저장되고, 생긴지 얼마 안되는 객체가 저장되는 공간입니다.
    - Heap 영역에 객체가 생성되면 최초로 Eden 영역에 할당됩니다.
    - 그리고 이 영역에 데이터가 어느정도 쌓이게 되면 참조정도에 따라 Servivor의 빈 공간으로 이동되거나 회수됩니다.
    - **Young Generation(Eden+Servivor) 영역**이 차게 되면 또 참조정도에 따라 Old영역으로 이동 되게 되거나 회수됩니다.
    - 이렇게 Young Generation과 Tenured Generation 에서의 GC를 **Minor GC** 라고 합니다.
    - minor GC에서도 다른 스레드의 동작이 멈추는 현상은 짧게나마 발생한다고 합니다.
- **Old Generation 영역**에 할당된 메모리가 허용치를 넘게 되면, Old 영역에 있는 모든 객체들을 검사하여 참조되지 않는 객체들을 한꺼번에 삭제하는 GC가 실행됩니다.
    - 시간이 오래 걸리는 작업이고 이 때 **GC를 실행하는 쓰레드를 제외한 모든 스레드는 작업을 멈추게 됩니다. 이를 'Stop-the-World'** 라 합니다.
    - **Old영역의 메모리를 회수하는 GC를 Major GC**라고 합니다.

### 2. 스레드(Thread)마다 하나씩 생성

#### 2-1. **스택 영역 (Stack Area)**
![image](https://user-images.githubusercontent.com/62924471/211125565-31d6b15c-c87f-41b7-84e3-6fbb1a1f1cc5.png)
- 지역변수, 파라미터, 리턴 값, 연산에 사용되는 임시 값 등이 생성되는 영역입니다.
- 메서드 호출 시마다 각각의 스택 프레임(그 메서드만을 위한 공간)을 생성하고, 메서드 안에서 사용되는 값들을 저장하고, 호출된 메서드의 매개변수, 지역변수, 리턴 값 및 연산 시 일어나는 값들을 임시로 저장합니다. 
- 메서드 수행이 끝나면 프레임별로 삭제합니다.

#### 2-2. **PC 레지스터 (PC Register)**

1. 쓰레드가 시작하고 생성될 때마다 생성되는 공간으로 쓰레드마다 하나씩 존재합니다. 
    - 프로그램 카운터, 즉 현재 스레드가 실행되는 부분의 주소와 명령을 저장하고 있는 영역입니다.
2. 쓰레드가 어떤 부분을 무슨 명령으로 실행해야할 지에 대한 기록을 하여 현재 수행중인 JVM 명령의 주소를 갖습니다.

#### 2-3. **네이티브 메서드 스택 (Native Method Stack)**

1. 자바 이외의 언어(C, C++, 어셈블리 등)로 작성된 네이티브 코드를 실행할 때 사용되는 메모리 영역으로, Java가 아닌 C, C++로 작성된 메서드를 실행하는 스택입니다.
2. 자바 네이티브 인터페이스(JNI: Java Native Interface)는 자바 가상 머신위에서 실행되고 있는 자바코드가 네이티브 응용 프로그램 그리고 C, C++ 그리고 어셈블리 같은 다른 언어들로 작성된 라이브러리들을 호출하거나 반대로 호출되는 것을 가능하게 하는 프로그래밍 프레임워크입니다.
    - 네이티브 메소드 스택은 네이티브 네이티브 인터페이스(JNI)와 연결되어 있는데, JNI가 사용되면 네이티브 메서드 스택에 저장됩니다.
    - 참고로, 네이티브 메소드 라이브러리는 JNI를 통해 쓸 수 있습니다. 
    네이티브 메소드 라이브러리는 네이티브 메소드 인터페이스의 구현체이기 때문입니다.
3. 만약 native 메소드가 JVM에 의해 로드되지 않으면 스택 또한 필요 없게 됩니다.
4. 메모리 크기 또한 JVM의 스택처럼 고정적이거나 유동적입니다.

---

### 출처
- [https://steady-coding.tistory.com/305](https://steady-coding.tistory.com/305)
- [https://coding-factory.tistory.com/828](https://coding-factory.tistory.com/828)
- [https://www.tcpschool.com/java/java_array_memory](https://www.tcpschool.com/java/java_array_memory)
- [https://velog.io/@dev_isaac/JVM#runtime-data-areas](https://velog.io/@dev_isaac/JVM#runtime-data-areas)
- [https://thisisprogrammingworld.tistory.com/m/179](https://thisisprogrammingworld.tistory.com/m/179)

### 추가 참고 링크
- [JVM에 관하여 - Part 3, Run-Time Data Area](https://tecoble.techcourse.co.kr/post/2021-08-09-jvm-memory/)
- [[JVM구조]JVM으로 보는 java 프로그램의 실행 과정](https://technote-mezza.tistory.com/72)
- [JVM Runtime Data Area- Heap, Method Area](https://mia-dahae.tistory.com/m/101)
- [[Java] 자바 메타스페이스(Metaspace)에 대해 알아보자.](https://jaemunbro.medium.com/java-metaspace%EC%97%90-%EB%8C%80%ED%95%B4-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90-ac363816d35e)

---
- 원글 링크 : https://github.com/Growth-Collectors/Interview-study/blob/main/Java/JVM%20%EA%B5%AC%EC%A1%B0.md

