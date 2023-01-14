## JWT란?

- JSON Web Token (JWT)는 웹 표준으로서
- 두 개체에서 JSON 객체를 사용하여 가볍고 자가 수용적인 (self-contained) 방식으로 정보를 안전성 있게 전달해준다.
- 자가 수용적
    - JWT는 필요한 모든 정보를 자체적으로 지닌다.
    - JWT 시스템에서 발급된 토큰은 **토큰에 대한 기본 정보**와 **전달할 정보** (ex. 로그인 시스템에서는 유저 정보), 그리고 **토큰이 검증됐다는 것을 증명해주는 signature**를 포함한다.
- 쉽게 전달될 수 있다.
    - JWT는 자가수용적이므로 두 개체 사이에서 손쉽게 전달될 수 있다.
    - 웹 서버의 경우 HTTP의 헤더에 넣어서 전달할 수도 있고 URL의 파라미터로 전달할 수 있다.

## JWT가 사용되는 경우

**1) 회원 인증**

![image](https://user-images.githubusercontent.com/47748246/188250681-208af321-ed60-46c3-943f-d59232f92690.png)

- 유저가 로그인을 하면 서버는 유저의 정보에 기반한 **토큰을 발급**하여 유저에게 전달해준다.
- 그 후 유저가 서버에 **요청을 할 때마다 JWT를 포함하여 전달**한다.
- 서버는 클라이언트에게서 요청을 받을 때마다 해당 **토큰의 유효성을 검증**하고
    
    유저가 요청한 작업에 **권한이 있는지 확인**하여 작업을 처리한다.
    
- 또한 **서버 측에서는 유저의 세션을 유지할 필요가 없다.**
    
    즉, 유저가 로그인되어있는지 안되어있는지 신경 쓸 필요가 없고
    유저가 요청을 했을 때 토큰만 확인하면 되니 세션 관리가 필요 없어서 서버 자원을 많이 아낄 수 있다. 
    

**2) 정보 교류**

- JWT는 두 개체 사이에서 안정성 있게 정보를 교환하기에 좋은 방법이다.
- 그 이유는 정보가 sign이 되어있기 때문에 정보를 보낸 이가 바뀌진 않았는지 또 정보가 도중에 조작되지는 않았는지 검증할 수 있다.

+ 참고)

- HTTP는 무상태 → 사용자가 요청을 할 경우 과거에 로그인한 사실을 기억하지 못함.
- **세션 기반 인증 방식**
    - 사용자 정보를 DB에 담아두어, 요청할 때마다 세션에 담긴 값을 비교해 일치하는지 확인하는 세션 기반의 인증 방식을 사용함
    - 서버에 부담
    
    ![image](https://user-images.githubusercontent.com/47748246/188250687-46b02879-6812-45d1-a6c3-325ef319e16f.png)
    
- **JWT 기반 인증 방식**
    - 로그인 할 경우, 사용자를 위한 토큰을 발급해주고, 유저가 회원 정보 변경 같은 요청을 할 경우 토큰이 일치하는지 검증만 하면 된다.
    - 토큰은 사용자의 정보가 담겨있기 때문에 굳이 DB를 조회하지 않아도 누가 요청하는지 알 수 있다.
    
    ![image](https://user-images.githubusercontent.com/47748246/188250695-0b235283-955f-45ff-87d6-dad7b00c2a3e.png)
    

## JWT 구조

![image](https://user-images.githubusercontent.com/47748246/188250704-06f0e4d2-b9cc-4fa9-93d8-35c9dd4db516.png)

**1) 헤더 (header)**

- 헤더는 2가지 정보를 지니고 있다.
    - `typ` : 토큰의 타입을 지정한다.
    - `alg` : 해싱 알고리즘을 지정한다. 주로 사용되는 알고리즘으로는 `HMAC SHA256` 혹은 `RSA` 이며, 토큰을 검증할 때 사용되는 `signature` 부분에서 사용된다.

```json
{
 "alg": "HS256",
 "typ": JWT
}
```

**2) 정보 (payload)**

- payload 부분에는 토큰에 담을 정보가 들어있다.
- 여기에 담는 정보의 한 조각을 **클레임 (claim)**이라고 부르고, 이는 Name, Value의 한 쌍으로 이뤄져있다.
- 토큰에는 여러 개의 클레임들을 넣을 수 있다.
- 클레임에는 3가지 부류가 있다.
    - **등록된 클레임 (Registered Claim)**
        - 등록된 클레임들은 서비스에서 필요한 정보가 아닌, 토큰에 대한 정보들을 담기 위하여 이름이 **이미 정해진 클레임들**이다.
        - 등록된 클레임의 사용은 모두 선택적(optional)이며 이에 포함된 클레임 이름들은 다음과 같다.
            - iss : 토큰 발급자 (issuer)
            - sub : 토큰 제목 (subject)
            - aud : 토큰 대상자 (audience)
            - exp : 토큰의 만료 시간 (expiration)
            - nbf: Not Before를 의미하며 토큰의 활성 날짜와 비슷한 개념이다.
            - iat : 토큰이 발급된 시간 (issued at)
                - 이 값을 사용하여 토큰의 age가 얼마나 되었는지 판단할 수 있다.
            - jti: JWT의 고유 식별자
                - 주로 중복 처리를 방지하기 위하여 상요된다.
                - 일회용 토큰에 사용하면 유용하다.
    - **공개 클레임 (Public Claim)**
        - 공개 클레임들은 충돌이 방지된 (collision-resistant)이름을 가지고 있어야 한다.
        - 충돌을 방지하기 위해서는 클레임 이름을 URI 형식으로 짓는다.
        
        ```json
        {
            "https://goodgid.github.io/jwt_claims/is_admin": true
        }
        ```
        
    
    - **비공개 클레임 (Private Claim)**
        - 등록되거나 공개된 클레임이 아니며, 양 측 (서버와 클라이언트) 간의 협의 하에 사용되는 클레임 이름들이다. 이름이 중복되어 충돌될 수 있으니 주의해야 한다.
        
        ```json
        {
            "username": "goodgid"
        }
        ```
        
    
- 예제
    
    ```json
    {
        "iss": "goodgid.com",
        "exp": "1485270000000",
        "https://goodgid.github.io/jwt_claims/is_admin": true,
        "userId": "11028373727102",
        "username": "goodgid"
    }
    ```
    
    - 위 예제 payload는 2개의 등록된 클레임 1개의 공개 클레임 2개의 비공개 클레임으로 이뤄져있다.
    - 위 데이터를 base64로 인코딩하면 다음과 같다.

```jsx
const payload = {
    "iss": "goodgid.com",
    "exp": "1485270000000",
    "https://goodgid.github.io/jwt_claims/is_admin": true,
    "userId": "11028373727102",
    "username": "goodgid"
};

// encode to base64
const encodedPayload = new Buffer(JSON.stringify(payload))
                            .toString('base64')
                            .replace('=', '');

console.log('payload: ',encodedPayload);

/* 
payload:  eyJpc3MiOiJ2ZWxvcGVydC5jb20iLCJleHAiOiIxNDg1MjcwMDAwMDAwIiwiaHR0cHM6Ly92ZWxvcGVydC5jb20vand0X2NsYWltcy9pc19hZG1pbiI6dHJ1ZSwidXNlcklkIjoiMTEwMjgzNzM3MjcxMDIiLCJ1c2VybmFtZSI6InZlbG9wZXJ0In0
*/
```

**3) 서명 (signature)**

- JSON Web Token의 마지막 부분은 바로 서명(signature)이다.
- 이 서명은 **헤더의 인코딩 값과 정보의 인코딩 값을 합친 후 주어진 비밀 키로 해싱을 하여 생성**한다.
- 서명 부분을 만드는 pseudo code의 구조는 다음과 같다.

```jsx
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret)
```

- 이렇게 만든 해쉬를 base64 형태로 나타내면 된다.
- 문자열을 인코딩 하는게 아닌 hex → base64 인코딩을 해야한다.

예) 

- 헤더와 정보의 인코딩 값 사이에 `.`을 넣어주고 합친다.

![image](https://user-images.githubusercontent.com/47748246/188250712-5ed69b26-63e3-4b8d-87e0-e56f032abf7a.png)

- 이 값을 비밀 키의 값(secret)으로 해싱을 하고 base 64로 인코딩하면 다음과 같은 값이 나온다.

```jsx
const crypto = require('crypto');
const signature = crypto.createHmac('sha256', 'secret')
             .update(encodedHeader + '.' + encodedPayload)
             .digest('base64')
             .replace('=', '');

console.log('signature: ',signature);

/* 
signature: WE5fMufM0NDSVGJ8cAolXGkyB5RmYwCto1pQwDIqo2w
*/
```

- 그러면 지금까지 구한 값들을 `.`을 중간자로 하여 다 합쳐주면 하나의 토큰이 완성이 된다.

![image](https://user-images.githubusercontent.com/47748246/188250715-cbdc74ad-afe9-439e-8d30-ef3ef8415357.png)

- 위 토큰을 decode하면 다음과 같이 나온다.

![image](https://user-images.githubusercontent.com/47748246/188250728-996a4738-30e6-434f-ab08-802d290455d2.png)
```jsx
