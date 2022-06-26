# HTTP/1.0~ 3

Last Edited: February 4, 2022 11:21 PM

# HTTP/1.0
![스크린샷_2022-02-04_오후_11 13 12](https://user-images.githubusercontent.com/41617388/152545460-11f6ad84-8925-4190-9144-5ffd1224d05d.png)

- 하나의 TCP 연결에 하나의 요청
- 느림
- 보안 문제
- 버퍼링

# HTTP/1.1

![스크린샷_2022-02-04_오후_10 46 09](https://user-images.githubusercontent.com/41617388/152545559-b9a2774d-17cf-4dd5-908e-ce2b73c107b6.png)

- 하나의 연결에 여러 개의 요청
    - keep-alive 헤더
    - 기존 연결에 대해서 handshake 생략 가능
- Chunk된 응답 지원

![Untitled](https://user-images.githubusercontent.com/41617388/152545507-7e8cad66-e0dc-4e4a-8544-5be999620faa.png)

- Pipelining 추가 → Latency를 낮춤
    - 하나의 연결에서 순차적인 여러 요청을 연속적으로 받아오는 기법
- 컨텐츠 전송

### 단점

- Head Of Line (HOL) Blocking
    - 먼저 받은 요청이 끝나지 않으면, 다음 요청이 처리가 안됨
    - 모던 브라우저들은 대부분 해당 기능을 막아놓음
    - client별로 최대 6-8개의 요청만 할 수 있음
- RTT (Round Trip Time)
    - 보낸 요청을 서버가 처리한 후 다시 클라이언트로 응답해주는 사이클
    - handshake가 반복적으로 일어나고, 불필요한 RTT 증가와 네트워크 지연 초래
- 무거운 헤더
    - 헤더에 매우 많은 정보들이 담겨 있어서 전송값보다 헤더가 더 큰 경우도 있음

# HTTP/2

![Untitled 1](https://user-images.githubusercontent.com/41617388/152545499-bf87556e-631c-47a9-b765-808824276db0.png)

- 헤더 압축
    - HPack (허프만 coding 기반)
- Multiplexing
    - 하나의 연결로 데이터를 병렬로 요청
    - 응답은 순서에 상관 없이 Stream으로 주고받는다
- Resource Priority
    - 리소스간의 우선순위 설정
    - CSS, 이미지 동시 요청 시 CSS 수신이 늦어지면 렌더링이 안되는 경우가 해결됨
- Server Push
    - client의 요청이 없어도 서버에서 push
- Secure by default
    - HTTPS가 기본

<aside>
✅ Head of line blocking 해결, 무거운 헤더 해결

</aside>

<aside>
🚫 TCP이기 때문에 RTT 시간은 아직도 지연됨

</aside>

# HTTP/3

![Untitled 2](https://user-images.githubusercontent.com/41617388/152545504-abdb7e22-7958-446f-8316-a57cae36183d.png)

- QUIC (Quick UDP Internet Connections)
    - UDP를 사용함 → TCP 필요 없음
    - 속도 향상
- 보안 유지
    - TLS 사용
    - 재전송 (Stop and Wait ARQ)
        - 일정 시간 후에도 응답이 없다면 재전송
- Connection ID
    - 네트워크 환경이 바뀌어도 unique ID를 통해 그대로 통신 가능
