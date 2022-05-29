# Nginx 로드 밸런싱

Nginx는 리버스 프록시, 로드 밸런서, 웹 서버, API 게이트웨이 등 하나로 사용할 수 있는 도구다.

> **로드 밸런싱(Load Balancing)**
> 

서버 팜의 네트워크 트래픽을 체계적이고 효율적으로 분배하는 것. 클라이언트와 백엔드 사이에 위치하여 들어오는 요청을 기반으로 요청을 실행할 수 있는 UPSTREAM 서비스 또는 인스턴스로 요청을 전달한다. ( UPSTREAM 서버는 사용자의 요청을 받아 요청을 처리할 애플리케이션 서버를 선정하는 서버이고 클라이언트에게 서버의 응답을 전달해준다. ) 로드 밸런싱에서 아무 설정이 안되어있다면 기본 알고리즘은 라운드 로빈 알고리즘을 사용한다.

> **proxy_pass 지시어**
> 
1. 클라이언트 요청을 처리할 서버 풀과 그룹을 정의하고 관리한다.
2. 사용자의 요청을 proxy_pass가 서버에 전달해준다.
3. 서버들은 UNIX 소켓, 도메인 이름, IP와 PORT를 정의할 수 있다

> **upstream 지시어 (http 컨텍스트 내에 있음)**
> 

```sql
upstream backend_servers {
	server 10.0.0.1:9000;
    server 10.0.0.1:9001;
}

server {
	listen 80;
    location / {
    	proxy_pass	http://backend_servers;
    }
}
```

`proxy_pass`로 설정된 http://backend_servers 은 클라이언트로부터 받은 요청과 이에대한 응답을 수행한다.

> **로드 밸런싱 : 기본 알고리즘 (Round Robin)**
> 

라운드 로빈 알고리즘에 의해 백엔드 서비스로 요청이 전달된다. 

```sql
<-- 가중치를 두지 않은 로드밸런싱-->
upstream backend_servers {
	server 10.0.0.1:9000;
    	server 10.0.0.1:9001;
    	server 10.0.0.1:9002;
}

<-- 가중치를 둔 로드밸런싱-->
upstream backend_servers {
	server 10.0.0.1:9000 weight=2;
    	server 10.0.0.1:9001 weight=3;
    	server 10.0.0.1:9002 weight=5;
}
```

> **로드 밸런싱 : Server Fail 과 Timeouts**
> 

Max_fails은 서버가 명확하게 사용할 수 없게되기 전 연결 시도 실패 횟수이다. 특정 횟수만큼 실패하게 되면 서버에서 제외시킬 수 있다. 아래의 경우 9000포트로 들어온 요청은 2의 가중치를 가지며 90초이내에 10번 실패할 수 있게하는 설정이다. 실패를 하게 되면 90초동안 사용할 수 없다.

```sql
<-- 가중치를 둔 로드밸런싱-->
upstream backend_servers {
	server 10.0.0.1:9000 weight=2 max_fails=10 fail_timeout=90s;
  server 10.0.0.1:9001 weight=3 max_fails=4 fail_timeout=60s;
  server 10.0.0.1:9002 weight=5 max_fails=2 fail_timeout=30s;
}
```

> **로드 밸런싱 방법**
> 
1. hash
2. ip_hash
3. least_conn
4. least_time
5. random

> **hash를 이용한 로드 밸런싱**
> 

클라이언트를 키로 구분했을 때 특정 유형의 클라이언트 요청을 원래 연결했던 서버로 전달. 프론트엔드 요청보다 백엔드나 방화벽 뒤에서 자주 사용된다. 단점은 서버를 추가하거나 서버를 제거하면 키가 손실되거나 세션 정보가 무효가 된다. 클라이언트 요청 URI를 사용하여 해시 키를 빌드한다.

```sql
update backend_servers {
	hash $request_uri;
	server 10.0.0.1:9000;
  server 10.0.0.1:9001;
  server 10.0.0.1:9002;
}
```

> **ip hash를 이용한 로드 밸런싱**
> 

ip의 해시 값을 클라이언트를 구분하는 키로 사용한다. 주의해야 할 점은 만약 백엔드 서버 앞에 프록시 서버가 있다면 클라이언트의 요청은 프록시 서버를 거쳐 오기 때문에 백엔드 서버는 프록시 서버의 IP를 클라이언트 IP로오해할 수 있다. 그러면 모든 요청을 같은 백엔드 서버로 넘길 수 있기 때문에 주의해야한다.

```sql
update backend_servers {
	ip_hash;
    	server 10.0.0.1:9000;
    	server 10.0.0.1:9001;
    	server 10.0.0.1:9002;
}
```

> **least_conn 최소 연결 방법을 이용한 로드 밸런싱**
> 

연속 연결 수가 가장 적은 서버에게 요청을 보내는 방법이다.

```sql
update backend_servers {
	least_conn;
    	server 10.0.0.1:9000;
    	server 10.0.0.1:9001;
    	server 10.0.0.1:9002;
}
```

> **least_time 최소 시간 방법을 이용한 로드 밸런싱**
> 

평균 응답 시간이 가장 빠르고 활성 연결 수가 가장 적은 서버에게 요청을 보내는 방법이다.

```sql
update backend_servers {
	least_time header;
    	server 10.0.0.1:9000;
    	server 10.0.0.1:9001;
    	server 10.0.0.1:9002;
}
```

> **random 랜덤 로드 밸런싱**
> 

랜덤으로 선택 된 서버에 요청을 보내는 방법이다.

> **Nginx의 로드 밸런스 설정 파일 위치**
> 

```sql
/etc/nginx/conf.d/lb.conf
```