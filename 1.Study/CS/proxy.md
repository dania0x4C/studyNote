### ** 프록시 서버(Proxy Server) 개념**

- **클라이언트와 서버 사이에서 중개 역할을 하는 서버**
- 클라이언트가 직접 목적지 서버에 요청하지 않고, **프록시 서버를 통해 요청을 보내고 응답을 받음**
- 웹 프록시는 **보안, 성능 개선, 로드 밸런싱, 캐싱 등의 역할 수행**

### ** 현재 사용 중인 프록시 방식 (Reverse Proxy)**

- **Nginx가 Reverse Proxy 역할을 수행**
- 클라이언트 → `https://soulmate.kro.kr` 요청 → Nginx가 `proxy_pass`를 통해 컨테이너로 요청 전달  
~~~
    location / {     
	    proxy_pass http://172.23.0.2:3000;  # Docker 컨테이너 내부 IP로 요청 전달
		proxy_set_header Host $host;
		proxy_set_header X-Real-IP $remote_addr;     
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		 proxy_set_header X-Forwarded-Proto $scheme; 
	}
~~~

- 클라이언트는 Nginx가 직접 응답을 주는 것처럼 보이지만, **실제로는 Nginx가 내부 서버(Docker 컨테이너)와 통신 후 응답을 반환함**.

**Reverse Proxy를 사용하는 이유**

1. **보안 강화**

- **외부에서 직접 애플리케이션 서버(3000 포트)에 접근 불가능**
- 모든 요청이 **Nginx를 거쳐야 하므로 보안성이 증가**

2.  **로드 밸런싱**

- 여러 개의 컨테이너에 트래픽을 분산할 수 있음 (현재는 단일 서버지만 확장 가능)

3. **SSL 처리 (HTTPS 적용)**

- 애플리케이션 서버(Node.js)는 HTTPS를 몰라도 됨 → **Nginx가 SSL 암호화를 처리**