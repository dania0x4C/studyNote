### **포트(Port) 개념**

- **네트워크에서 특정 서비스를 구분하는 번호**
- 하나의 서버에서 여러 개의 서비스를 운영할 때 **각 서비스마다 다른 포트를 사용**
- 포트는 **0~65535번**까지 있으며, 특정 포트는 **표준(Well-known) 포트**로 정해져 있음

### ** HTTP 요청 시 (포트 80)**

1. `http://soulmate.kro.kr`로 요청하면, 브라우저는 **기본적으로 80번 포트**로 요청을 보냄  
2.  하지만 **내부 서버가 3000번 포트에서 실행 중이면**, Nginx가 **80번에서 받은 요청을 3000번으로 전달해야 함**  
3.  이때 **프록시(proxy_pass)** 설정이 필요함

- **즉, HTTP를 사용하면 내부 3000번으로 프록시해야 하므로, 직접 요청하는 경우 `http://soulmate.kro.kr:3000`처럼 포트를 적어줘야 함**
### ** HTTPS 요청 시 (포트 443)**
1.  `https://soulmate.kro.kr`로 요청하면, 브라우저는 **기본적으로 443번 포트**로 요청을 보냄  
2.  **Nginx가 443번에서 SSL 인증을 처리하고 내부 서버(3000번)로 요청을 프록시**  
3.  이 과정에서 **Nginx가 내부 3000번 포트로 알아서 요청을 넘겨주므로, 클라이언트가 3000번 포트를 신경 쓸 필요가 없음**

-  **즉, HTTPS를 사용하면 어차피 Nginx가 내부에서 프록시하기 때문에, 3000번 포트를 직접 입력할 필요가 없음!**

# HTTP(80)와 HTTPS(443)의 역할

| 포트 번호    | 프로토콜  | 역할                         |
| -------- | ----- | -------------------------- |
| **80번**  | HTTP  | 보안 없이 데이터를 전송              |
| **443번** | HTTPS | SSL/TLS를 사용하여 암호화된 데이터를 전송 |
