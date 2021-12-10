# 개요

<br/><br/>

## API란?

- 정보를 주고 받는 데에 있어서 개발자들 사이에 널리 쓰이는 일종의 `형식`
- 어떤 기술이나 제품 ❌ 단지 형식 ⭕ → **어떤 프레임워크, 언어 등을 사용하든지 그 형식에 맞게 쓰면 되는 것!**

<br/><br/>

- 예시 : 기타 여러 어플이 기상청으로부터 날씨 정보를 가져오는 행위
    - 정보를 요청하는 지정된 형식이 있어야 함
        - 어플 —→  `date:211211|place:seoul|which:temperature`  —→ 기상청
        - 기상청 —→  `17degree`  —→ 어플
- 이처럼 소프트웨어가 다른 소프트웨어로 **지정된 형식**을 이용해 **요청, 명령을 받을 수 있는 수단**을 `API(Application Programming Interface)` 라 함
- api는 1️⃣네트워크 상 뿐만 아니라, 2️⃣로컬 프로그램인 브라우저도 web api를 통해 자바스크립트로부터 특정 동작들을 지시받기도 하고, 3️⃣프로그램 개발 시 보다 편리한 개발을 위해 api를 사용하기도 함

<br/><br/>

## REST API란?

<aside>
🗒️ 주요 특성<br/><br/>


> 각 요청이 어떤 동작이나 정보를 위한 것인지, 그 요청의 모습 자체만으로도 추론 가능하다는 점
> 

 → 다른 협업 개발자들을 위해, 누가봐도 그게 어떤 의미인지 알 수 있게 하는, 깔끔하고 직관적이고 효율적인 api 형식을 설계하려는 목적

</aside>

- 형식 : `API메소드 + URI`

<br/><br/>

## URI

> 자원을 구조와 함께 나타내는 형태의 구분자
> 

<aside>
🗒️ 예시

`https://xxit.com/studies/CS/members`

→ xxit사이트 > 스터디 > CS스터디 > 소속 멤버가 자원임을 유추할 수 있음

</aside>

이때 작업의 종류에 따라 CRUD의 4가지 작업 수행이 가능

- CRUD
    - Create 생성
    - Read 조회
    - Update 수정
    - Delete 삭제

<br/><br/>

## REST API를 이용한 정보 요청 및 전송 과정

- 서버에 REST API로 요청을 보낼 때 → `HTTP규약`을 따라 신호를 전송
- HTTP로 요청을 보낼 때 사용할 수 있는 여러 메소드가 존재
    - REST API의 경우, 사용하는 HTTP 메소드 → 보통 `GET(조회), POST(생성), DELETE(삭제), PUT(수정), (수정-PATCH)` 를 사용
- POST, PUT, PATCH에는 body 부분이 존재 (기존에 존재하지 않는, 새로 생성할 정보, 수정할 정보를 전달해주어야하므로) → GET, DELETE보다 많은 정보를, 비교적 안전하게 감춰서 전달 가능
- 하지만 이 메소드들이 특정 용도로만 쓰일 수 있는 건 ❌ → POST 메소드만으로 데이터를 읽고 쓰고 수정, 삭제 가능

<br/>

    
    🗒️ POST메소드로만 API 작성
    
    POST https://xxit.com/studies/CS/members/create
    POST https://xxit.com/studies/CS/members/read
    POST https://xxit.com/studies/CS/members/3/update
    POST https://xxit.com/studies/CS/members/3/delete

<br/>

뒷부분이 지저분하게 느껴지고, 메소드만으로 CRUD를 전달할 수 있다면 그렇게 하는 것이 더 직관적이고 깔끔하다!

→ **메소드에는 작업 종류**를, **URI에는 리소스**를 각각 담아 전달하는 것이 `RESTful한 API`인 것

    

- **누구나, API 형식만 보고도 요청의 목적을 파악할 수 있도록** 하기 위해서 `RESTful` 한 API를 만드려고 하는 것

<br/>

## PUT & PATCH

- PUT : 전면 대체
- PATCH : 일부 수정

<br/>

🗒️ 예시 : 특정 학생 정보에서 소속만 바꾸려할 때

기존 정보:
```
이름 : 루나
성별 : 여
소속 : 레번클로
```

**PUT메소드의 body**

```
이름 : 루나
성별 : 여
소속 : 그리핀도르
```

**PATCH메소드의 body**
```
소속 : 그리핀도르
```

<br/><br/>

# REST

> Representational State Transfer
> 

표현 가능한 상태 전송

<br/><br/>

## REST API 구성

- **자원(RESOURCE)** - URI
- **행위(Verb)** - HTTP METHOD
- **표현(Representations)**

<br/><br/>

## REST 특징


### 1) Uniform (유니폼 인터페이스)

URI로 지정한 리소스에 대한 **조작을 통일되고 한정적인 인터페이스로 수행**하는 아키텍처 스타일<br/>

⇒ HTTP를 사용할 수 있는 환경이라면 **플랫폼에 상관없이 사용**할 수 있으며 **리소스의 타입에 상관 없이 같은 형태의 요청**으로 처리<br/>

### 2) Stateless (무상태성)

작업을 위한 상태정보를 따로 저장하고 관리 ❌<br/>

→ 세션 정보나 쿠키정보를 별도로 저장 및 관리 ❌<br/>

→  API 서버는 **들어오는 요청만을 단순히 처리**하면 됨<br/>

→ 서비스의 자유도가 높아지고 서버에서 불필요한 정보를 관리하지 않음으로써 **구현이 단순**해짐<br/>

### 3) Cacheable (캐시 가능)

HTTP라는 기존 웹표준을 그대로 사용하므로 웹에서 사용하는 기존 인프라를 그대로 활용 가능<br/>

→ HTTP가 가진 캐싱 기능이 적용 가능. (HTTP 프로토콜 표준에서 사용하는 Last-Modified태그나 E-Tag를 이용하여 캐싱 구현)<br/>

### 4) Self-descriptiveness (자체 표현 구조)

REST의 또 다른 큰 특징 중 하나는 REST API **메시지만 보고도 이를 쉽게 이해 할 수 있는 자체 표현 구조**로 되어 있다는 것입니다.<br/>

### 5) Client - Server 구조

서버는 API 제공, 클라이언트는 유저에 대한 처리를 전담하는 구조를 가지기 때문에 **서버와 클라이언트의 역할을 분명하게 구분**할 수 있음<br/>

### 6) 계층형 구조

REST 서버는 다중 계층으로 구성될 수 있으며 보안, 로드 밸런싱, 암호화 계층을 추가해 구조상의 유연성을 둘 수 있고 PROXY, 게이트웨이 같은 네트워크 기반의 중간매체를 사용할 수 있게 함<br/>

<br/><br/>

# REST API 디자인 가이드

<br/>

## 1. 메소드에 작업 행위를, URI에 리소스를 작성

```
🗒️ 예시

POST https://xxit.com/studies/CS/members/3/delete   ❌
DELETE https://xxit.com/studies/CS/members/3        ⭕

```

<br/>

## 2. URI 설계 시 주의할 점

1. 슬래시 `/` 구분자는 계층 관계 의미
    
    예) `https://xxit.com/studies/CS/members/3`<br/>
    
    ⇒ xxit > 스터디들 > CS > 소속 멤버들 > 3번째 멤버<br/>
    
2. 슬래시 `/` 구분자를 맨 뒤에 사용하지 않음
    1. `https://xxit.com/studies/CS/members/3/`  ❌
    2. `https://xxit.com/studies/CS/members/3`  ⭕
3. 밑줄(`_`)보단 하이픈(`-`)을 사용
    1. 밑줄은 보기 어렵거나 밑줄 때문에 문자가 가려지기도 하므로 사용 지양
    2. 특히 URI가 길어질 땐 가독성을 높이기 위해 하이픈 사용 권장
4. URI는 소문자로 작성
    
    대소문자에 따라 다른 리소스로 인식하게 되기 때문에 소문자로만 작성 권장<br/>
    
    - RFC 3986(URI 문법 형식)은 URI 스키마와 호스트를 제외하고는 대소문자를 구별하도록 규정
5. 파일 확장자는 URI에 포함X
    
    예) `https://xxit.com/studies/CS/members/3/photo.jpg` ❌<br/>
    
    ⇒ 대신 Accept header를 사용 : `GET /studies/CS/members/3/photo HTTP/1.1 Host: xxit.com Accept: image/jpg` ⭕
    
<br/>

## 3. 자원을 표현하는 Colllection과 Document

- 도큐먼트 : 하나의 문서 객체, 컬렉션 : 도큐먼트의 집합
- 컬렉션과 도큐먼트는 모두 리소스로서 표현할 수 있으며 URI에 표현됨

```
https://xxit.com/studies/CS
```

- studies라는 컬렉션과 CS라는 도큐먼트(xxit에 스터디가 여러 개 있고, 그 중 CS스터디가 하나뿐이라 가정)

```
https://xxit.com/studies/CS/members/3
```

- 컬렉션 : studies, members → 복수형으로 표현
- 도큐먼트 : CS, 3(3번째 멤버) → 단수형으로 표현

<br/><br/>

## 4. HTTP 응답 상태코드

- 잘 설계된 REST API는 URI만 잘 설계된 것이 아닌 그 **리소스에 대한 응답을 잘 내어주는 것까지 포함**되어야 함 (→ 정확한 응답의 상태코드만으로도 많은 정보를 전달할 수가 있기 때문)

```
🗒️ 개요

- 10x : 정보 확인
- 20x : 통신 성공
- 30x : 리다이렉트
- 40x : 클라이언트 오류
- 50x : 서버 오류
```

<br/>

**200번대 : 통신 성공**

| 상태코드 | 이름 | 의미 |
| --- | --- | --- |
| 200 | OK | 요청 성공(GET) |
| 201 | Create | 생성 성공(POST) |
| 202 | Accepted | 요청 접수O, 리소스 처리X |
| 204 | No Contents | 요청 성공O, 내용 없음 |

<br/>

**300번대 : 리다이렉트**

| 상태코드 | 이름 | 의미 |
| --- | --- | --- |
| 300 | Multiple Choice | 요청 URI에 여러 리소스가 존재 |
| 301 | Move Permanently | 요청 URI가 새 위치로 옮겨감(응답 시 Location header에 변경된 URI를 적어 줘야 함) |
| 304 | Not Modified | 요청 URI의 내용이 변경X |

<br/>

**400번대 : 클라이언트 오류**

| 상태코드 | 이름 | 의미 |
| --- | --- | --- |
| 400 | Bad Request | API에서 정의되지 않은 요청 들어옴 |
| 401 | Unauthorized | 인증 오류(로그인 하지 않은 유저가 로그인한 유저만 요청 가능한 리소스를 요청했을 때) |
| 403 | Forbidden | 권한 밖의 접근 시도. 유저 인증상태와 관계 없이 응답하고 싶지 않은 리소스를 클라이언트가 요청했을 때 사용하는 응답 코드(403 보다는 400이나 404를 사용할 것을 권고. 403 자체가 리소스가 존재한다는 뜻이기 때문에) |
| 404 | Not Found | 요청 URI에 대한 리소스 존재X |
| 405 | Method Not Allowed | API에서 정의되지 않은 메소드 호출. 클라이언트가 요청한 리소스에서는 사용 불가능한 Method를 이용했을 경우 사용하는 응답 코드. |
| 406 | Not Acceptable | 처리 불가 |
| 408 | Request Timeout | 요청 대기 시간 초과 |
| 409 | Conflict | 모순 |
| 429 | Too Many Request | 요청 횟수 상한 초과 |

<br/>

**500번대 : 서버 오류**

| 상태코드 | 이름 | 의미 |
| --- | --- | --- |
| 500 | Internal Server Error | 서버 내부 오류 |
| 502 | Bad Gateway | 게이트웨이 오류 |
| 503 | Service Unavailable | 서비스 이용 불가 |
| 504 | Gateway Timeout | 게이트웨이 시간 초과 |

<br/><br/>

# REST API 사용 예시

[참고 링크](https://github.com/whhm312/tesks-demo-flo/blob/master/api_doc.md)

## **1. Playlist 생성 API**

```
POST /playlists
```

- 해당 사용자의 playlist를 생성할 수 있습니다.
- 한 사용자는 여러 개의 playlist를 가질 수 있습니다.
- playlist 이름을 지정할 수 있습니다.

| parameter | Type | description |
| --- | --- | --- |
| title | string | (필수)생성할 플레이리스트 이름 |

## **Response Code**

`200`

## **2. Playlist 노래, 앨범 추가 API**

```
POST /playlists/{playlist_id}
```

- 노래를 playlist에 추가할 수 있습니다.
- 특정 앨범에 포함된 모든 노래를 playlist에 추가할 수 있습니다.

## **Path Parameter**

| Parameter | Type | Description |
| --- | --- | --- |
| playlist_id | integer | (필수) 노래 혹은 특정 앨범에 포함된 모든 노래를 추가 할 playlist 아이디 |

## **Parameter**

| Parameter | Type | Description |
| --- | --- | --- |
| song_ids | array | 추가 할 노래 아이디들 |
| album_ids | array | 추가 할 앨범 아이디들 |

```json
{
    "song_ids": [ 1, 2 ],
    "album_ids": []
}
```

## **Response Code**

`200`

## **3. Playlist 목록 API**

- 특정 사용자의 playlist 목록을 조회할 수 있습니다.
- pagination은 하지 않습니다.

```
GET /playlists
```

## **Response Body**

```json
{
	"playlists": [
		{
			"playlist_id": 13,
			"title": "추억",
			"thumbnail_url": "http://xxxxx.xxx/xxxxxx",
			"created_date": "20200209222809"
		},
		{
			"playlist_id": 12,
			"title": "7080",
			"thumbnail_url": "http://xxxxx.xxx/xxxxxx",
			"created_date": "20200209222805"
		},
		{
			"playlist_id": 11,
			"title": "운동",
			"thumbnail_url": "http://xxxxx.xxx/xxxxxx",
			"created_date": "20200209222759"
		}
	]
}
```

## **4. Playlist 삭제 API**

- 사용자의 특정 playlist 1개를 삭제합니다.

```
DELETE /playlists/{playlist_id}
```

## **Response Code**

`200`

<br/><br/>

# REST API 생성 예시

<br/>

## Flask서버와 MongoDB를 이용한 REST API 생성
### 1. app.py

```python
# app.py

from flask import Flask, render_template, jsonify, request
app = Flask(__name__)

import requests
from bs4 import BeautifulSoup

from pymongo import MongoClient
client = MongoClient('localhost', 27017)
db = client.dbxxit

## HTML을 주는 부분
@app.route('/')
def home():
   return render_template('index.html')

@app.route('/memo', methods=['GET'])
def listing():
    articles = list(db.articles.find({}, {'_id':False}))
    return jsonify({'all_articles':articles})

## API 역할을 하는 부분
@app.route('/memo', methods=['POST'])
def saving():
    url_receive = request.form['url_give']
    comment_receive = request.form['comment_give']

    headers = {
        'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64)AppleWebKit/537.36 (KHTML, like Gecko) Chrome/73.0.3683.86 Safari/537.36'}
    data = requests.get(url_receive, headers=headers)

    soup = BeautifulSoup(data.text, 'html.parser')

    # meta tag 가져오기

    title = soup.select_one('meta[property="og:title"]')['content']
    image = soup.select_one('meta[property="og:image"]')['content']
    desc = soup.select_one('meta[property="og:description"]')['content']

    doc = {
        'title': title,
        'image': image,
        'desc': desc,
        'url': url_receive,
        'comment': comment_receive
    }

    db.articles.insert_one(doc)

    return jsonify({'msg':'저장이 완료되었습니다!'})

if __name__ == '__main__':
   app.run('0.0.0.0',port=5000,debug=True)
```

<br/>

### 2. index.html의 javascript부분

```jsx
<script>
      $(document).ready(function () {
          showArticles();
      });

      function openClose() {
          if ($("#post-box").css("display") == "block") {
              $("#post-box").hide();
              $("#btn-post-box").text("포스팅 박스 열기");
          } else {
              $("#post-box").show();
              $("#btn-post-box").text("포스팅 박스 닫기");
          }
      }

      function postArticle() {
          let url = $('#post-url').val()
          let comment = $('#post-comment').val()

          $.ajax({
              type: "POST",
              url: "/memo",
              data: {url_give:url, comment_give:comment},
              success: function (response) { // 성공하면
                  alert(response["msg"]);
                  window.location.reload()
              }
          })
      }

      function showArticles() {
          $.ajax({
              type: "GET",
              url: "/memo",
              data: {},
              success: function (response) {
                  let articles = response['all_articles']
                  for(let i = 0; i < articles.length; i++){
                      let title = articles[i]['title']
                      let image = articles[i]['image']
                      let url = articles[i]['url']
                      let desc = articles[i]['desc']
                      let comment = articles[i]['comment']

                      let temp_html = `<div class="card">
          <img class="card-img-top"
              src="${image}"
              alt="Card image cap">
          <div class="card-body">
              <a href="${url}" class="card-title">${title}</a>
              <p class="card-text">${desc}</p>
              <p class="card-text comment">${comment}</p>
          </div>
      </div>`
                      $('#cards-box').append(temp_html)
                  }
              }
          })
      }
</script>
```

### 참고

---

[REST API가 뭔가요?](https://www.youtube.com/watch?v=iOueE9AXDQQ)

[REST(Representational State Transfer)란?](https://tibetsandfox.tistory.com/19)

[REST API 제대로 알고 사용하기 : NHN Cloud Meetup](https://meetup.toast.com/posts/92)

[tesks-demo-flo/api_doc.md at master · whhm312/tesks-demo-flo](https://github.com/whhm312/tesks-demo-flo/blob/master/api_doc.md)
