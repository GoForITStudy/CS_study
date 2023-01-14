# **Browser에서 Google.com을 검색하면 무슨 일이 발생하나요?**

## 1. 브라우저의 URL 파싱

URL을 입력받은 브라우저는 일단 이 URL을 파싱합니다. 

- 어떤 프로토콜로 어느 도메인 URL으로 어떤 포트로 보낼지 해석합니다.
    - https 는 443 포트, http는 80 포트를 기본으로 활용합니다.
- 기본적인 URL의 구조

![https://k.kakaocdn.net/dn/dfjElB/btqDH2sKASg/236MABv8iJyBDUkm4TdhkK/img.png](https://k.kakaocdn.net/dn/dfjElB/btqDH2sKASg/236MABv8iJyBDUkm4TdhkK/img.png)

## 2. **DNS에서 IP주소 가져오기**

- 도메인 이름 주소를 IP주소로 변환하는 작업을 수행합니다.
- DNS 는 어플리케이션 레벨에서 동작합니다.

![image](https://user-images.githubusercontent.com/62924471/196605910-a42b008a-e67e-464d-b559-9a515380fa69.png)

### **Browser DNS Cache - Chrome**

- 브라우저는 도메인이 캐시에 들어있는지 확인합니다.
- `chrome://net-internals/#dns`

### **OS DNS Cache - Mac**

- 만약 Browser Cache에서 못 찾으면, OS에 저장된 DNS Cache를 찾습니다.
    - 호스트 파일은 컴퓨터 네트워크에서 네트워크 노드 주소 지정을 지원하는 여러 시스템 기능 중 하나입니다.
    - **운영 체제의 인터넷 프로토콜 (IP) 구현 의 공통 부분이며 호스트 이름을 IP 주소로 변환하는 기능을 제공합니다.**

### **Router Cache**

- 만약 OS Cache에서도 못 찾으면, Router Cache에서 조회합니다.

### **ISP Cache**

- ISP란 인터넷 사용자들을 지칭합니다. 예) KT, SKT

### **DNS Query**

- **DNS query**의 목적은 여러 다른 DNS 서버들을 검색해서 해당 사이트의 IP 주소를 찾는 것입니다.
    - 이러한 검색을 **recursive search**라고 부릅니다.
    - IP 주소를 찾을 때 까지 DNS 서버에서 다른 DNS 서버를 오가면서 반복적으로 검색하며, 결과를 찾지 못해 에러가 발생할 때 까지 검색을 수행합니다.
- Root DNS부터 Recursive Query를 진행하게 됩니다. ( 좀 더 확인해 볼 부분 )
    - 도메인 이름은 계층적 트리 구조를 갖습니다.
    
    <img width="600" src="https://nesoy.github.io/assets/posts/20180617/4.png">
    
- **DNS 서버로 요청하는 과정**
    1. 미리 설정 된 Local DNS에 해당 URL 주소의 IP주소를 요청합니다.
    2. Local DNS에 해당 IP주소가 존재한다면 이를 응답하고, 없다면 다른 DNS 서버와 통신합니다.
    root DNS 서버에 해당 URL의 IP주소를 요청합니다.
    3. root DNS서버에 해당 IP주소가 없다면 하위 DNS 서버에 요청하라고 응답합니다.
    이 응답을 받은 Local DNS는. com 도메인을 관리하는 DNS 서버에 같은 내용을 요청합니다.
    4. .com DNS 서버에 해당 IP주소가 없다면 하위 DNS 서버에 요청하라고 응답합니다.
    이 응답을 받은 Local DNS는 [google.com](http://google.com/) 도메인을 관리하는 DNS 서버에 같은 내용을 요청합니다.
    5. [google.com](http://google.com/) DNS 서버에서 IP주소를 응답받은 Local DNS는 해당 IP주소를 캐싱하고 응답합니다.

## 3. 라우터를 통해 해당 서버의 게이트웨이까지 이동

- IP 프로토콜을 이용해서 통신을 요청하면 라우터를 통해서 전송이 이루어집니다.
    - 라우터는 라우팅 테이블을 활용해서 통신 작업을 수행합니다.
    - 라우팅 프로토콜(알고리즘)을 바탕으로 생성된 라우팅 테이블을 활용해서 패킷의 전송을 효율적으로 수행합니다.

## 4. Server와 TCP Socket 열기

- (HTTP 2.0 이하 기준)
- 접속, 통신하고자 하는 Server의 IP주소로 TCP Socket을 열어 Server와 통신 준비를 진행합니다.
- 이 과정에서 HTTPS 요청이라면  TLS(Transport Layer Security) handshake 과정을 통해 세션키를 생성합니다.
    - 서버와의 세션이 종료되면 4-way handshake(transport layer)로 연결을 종료합니다.

### **TCP 3 Way-Handshake**

1. Client > Server : TCP SYN
2. Server > Client : TCP SYN ACK
3. Client > Server : TCP ACK

<img width="600" src="https://nesoy.github.io/assets/posts/20180617/5.png">
<img width="600" src="https://user-images.githubusercontent.com/62924471/196605982-dea63501-f86a-490a-aa27-aca76a437263.png">

### **TCP Socket 연결 상태 확인하기**

> $ netstat
<img width="600" src="https://nesoy.github.io/assets/posts/20180617/6.png">

## 5. HTTP(HTTPS) 프로토콜로 요청, 응답

- TCP Socket을 통해 HTTP Protocol로 원하는 Resource를 요청합니다.
    - 서버에서 해당 요청을 받고, 이 요청을 수락할 수 있는지 검사합니다. 그리고 서버는 이 요청에 대한 응답을 생성하여 브라우저에게 전달합니다.
    - 세션이 유지되는 동안 서버에게 요청(Request)하고 응답(Response)을 받는 과정을 반복합니다.

## 6. Browser Page Rendering

- Server에서 받은 Resource를 Browser에서 Rendering하는 작업이 필요합니다.

#### 참고 자료

- [https://kimbangg.tistory.com/m/259](https://kimbangg.tistory.com/m/259)
- [https://deveric.tistory.com/97](https://deveric.tistory.com/97)
- [https://nesoy.github.io/articles/2018-06/What-happens-search-google](https://nesoy.github.io/articles/2018-06/What-happens-search-google)
- [https://velog.io/@eesiwoo/www.google.com을-검색하면-어떤-일이-일어날까](https://velog.io/@eesiwoo/www.google.com%EC%9D%84-%EA%B2%80%EC%83%89%ED%95%98%EB%A9%B4-%EC%96%B4%EB%96%A4-%EC%9D%BC%EC%9D%B4-%EC%9D%BC%EC%96%B4%EB%82%A0%EA%B9%8C)
- [https://oizys.tistory.com/36](https://oizys.tistory.com/36)
