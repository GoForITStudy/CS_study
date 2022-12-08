### Private Network

- 보안을 위해 restrictions가 설정된 네트워크를 private network이라고 한다.
- 해당 private network 밖에있는 컴퓨터로부터 이 네트워크로 접근 불가능
- 오직 선별된 장치들만이 이 네트워크에 접근 가능하다. (network router에 설정된 값, access point에 따라 다르다.)
- 반면, Public Network는 외부의 어떤 장치든, 제약없이 접근 가능한 네트워크를 말한다.

![image](https://user-images.githubusercontent.com/47748246/206345359-26412e46-03fe-4b9a-a232-2bb21d6c3aa8.png)

- A 회사는 서버 2대에 기밀 자료를 보관하고 있고, 외부로부터 이 서버에 접근을 막기 위해 Private Network를 사용한다.
- 따라서 이 서버에 접근하기 위해서는 access point가 Private Network 내부에 있거나, VPN을 이용해야 한다.

### VPN

![image](https://user-images.githubusercontent.com/47748246/206345395-89d2297e-b335-479e-b426-1ea2e513b0d6.png)

- VPN은 주로 gateway router에 설치된다. 즉, gateway router가 VPN을 지원하고, 재택 근무자의 PC 역시 VPN client가 깔려있으면, 해당 사용자는 VPN으로 사내 Private Network에 접근이 가능해진다.

### VPN 상세 원리

![image](https://user-images.githubusercontent.com/47748246/206345430-227f83d5-88df-463e-94e6-67d76f3ab940.png)

- VPN Client를 설치하면,
    - 해당 컴퓨터는 **IP를 두 개** 갖게 된다. (하나는 ISP로부터 받은 IP, 그리고 secure gateway로부터 받은 IP)
        - 참고) 통상적으로 VPN Client를 IPSec VPN이라고 부른다. (IP 계층에서 보안한다는 뜻인듯)
    - **Virtual NIC Driver 가 생성**됨. (실제 NIC은 아니지만, OS에서 NIC으로 인식함)
        - 이 NIC 용 IP로 VPN IP가 되는 것.
- Client에서 회사 서버에 접속하려고함. 그럼 접속하려는 IP를 확인하고 이 IP가 회사 VPN server IP이면 Virtual NIC Driver가 IP(= VPN IP)를 세팅함. + destination IP는 회사 서버 IP로 세팅 → 1차 패킷 생성 → 암호화
- 그리고 그렇게 생성된 패킷을 다시 원래 host driver가 받아서 host IP로 다시 세팅 (= Outer IP) + destination IP는 secure gateway IP로 세팅→ 2차 패킷 생성
- secured gateway router가 해당 패킷을 받으면, outer IP 제거, 암호화된 패킷 decrypt → 해당 서버로 요청 포워딩함.
