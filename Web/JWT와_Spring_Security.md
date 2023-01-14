### **🦥  JWT (Json Web Token) ?**

JSON 객체를 사용해서 토큰 자체에 정보를 저장하여 사용자 인증에 사용되는 토큰

### **🦥  JWT 구성**

구분자는 점이고 세 부분으로 구성되어 있다

1. 헤더(Header) : 토큰의 타입과 암호 알고리즘으로 구성

2. 페이로드(Payload) : 토큰에 담을 정보가 들어있다. 페이로드에 담긴 정보 하나하나를  클레임(claim) 이라고 부르고 name-value 로 이루어져 있다. 클레임의 종류는 등록된 클레임, 공개 클레임, 비공개 클레임 3가지가 있다. 

-   등록된 클레임 : 미리 정의된 클레임의 집합
-   공개 클레임 : JWT 를 사용하는 사람들이 정의할 수 있음
-   비공개 클레임 : 클라이언트와 서버가 협의하여 사용

```
{ 
	"exp" : "14800000", // 등록된 클레임 
	"username" : "hello", // 비공개 클레임 
	"nickname" : "hello" // 비공개 클레임 
}
```

이렇게 정의된 페이로드는 Base64Url로 인코딩되어 JSON 의 두번째 부분을 구성한다.

3. 서명(Signature) : 헤더와 페이로드의 값을 각각 BASE64 로 인코딩 한 뒤 인코딩한 값을 비밀키와 헤더에서 정의한 암호화 알고리즘을 통해 해싱을 하고 이 값을 다시 BASE64 로 인코딩해서 생성한 값이다. 

이렇게 생성된 토큰은 Authorization의 value 로 사용되고 토큰 앞에 Bearer 가 붙는다. 

### **🦥  JWT 장단점**

▶︎ 장점 : Stateless 하기 때문에 세션처럼 별도의 인증 저장소가 필요하지 않음

▶︎ 단점 : Payload 정보가 많아지면 네트워크 사용량이 증가하고 데이터 설계를 고려할 필요가 있고, Payload 에 대해서 BASE64 인코딩만 진행하지 암호화를 진행하지 않기 때문에 중요한 데이터를 Payload 에 넣어선 안됨

### **🦥  Spring Security 인증 과정 (Spring Security 제공하는 form login 방식 사용)**
![[img_jwt.png]]

Security 설정파일에 formLogin 을 사용한다고 설정하면 UsernamePasswordAuthenticationFilter 를 통해 사용자의 ID/PW 추출 후 인증을 위해 토큰을 생성한다. 이에 대한 인증 요청은 AuthenticationManager 에게 넘어가고 Manager 는 해당 토큰 인증을 처리해줄 AuthenticationProvider 에게 인증을 위임한다. Provider는 UserDetailService 를 통해 사용자의 정보를 가져온 뒤 사용자의 요청 정보와 DB 정보를 비교하여 인증을 하고 성공하면 Authentication 객체를 반환한다. 

formlogin 방식이 아닌 JWT 를 사용하여 인증을 할 것이기 때문에 다음과 같은 클래스들이 필요할 것 같다. 

-   JwtAuthenticationFilter : JwtAuthentication 인증 처리 후 인증 결과를 Security Context 에 저장
-   JwtAuthenticationProvider : JWT 인증 처리 1
-   JwtDetailService : JWT 인증 처리 2