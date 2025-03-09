[[Study/CS/https|https]]
[[ssl]]
## 1. AWS EC2 보안 그룹 설정

보안 규칙에 들어가서 433 포트 번호 연결 [[인바운드]] 규칙 추가하기

---
## 2. Nginx 및 Certbot(SSL 인증서) 설치, Let's Encrypt Certbot 설치

	**EC2에서 Nginx 설치 및 시작**
~~~
`sudo apt update sudo apt install nginx -y sudo systemctl enable nginx sudo systemctl start nginx`

`sudo apt install certbot python3-certbot-nginx -y`
~~~

---

## **3. `docker-compose.yml` 작성**

`docker-compose.yml` 파일 내용 추가 변경
nginx를 통해서 도커의 컨테이너로 접근하기 위한 네트워크 설정

(1) 클라이언트 요청 → (2) EC2 서버(Nginx) → (3) Docker 컨테이너 내부 앱(Node.js) → (4) 응답 반환

~~~
networks: - soulmate_network #Nginx와 연결할 네트워크 추가 

networks: soulmate_network: # Nginx와 연결할 네트워크 설정 driver: bridge
~~~

---
## ** 4. Nginx 설정**
[[proxy]]
🔹 **Nginx 설정 파일 편집**

`sudo nano /etc/nginx/sites-available/default`

🔹 **아래 내용 입력**
~~~
server {
    listen 80;
    server_name soulmate.kro.kr;
    return 301 https://$host$request_uri;
}
## 서버 리다이렉션 http 요청을 받았을때 자동으로 https서버로 변경해서 접속 시켜준다
server {
    listen 443 ssl;# 요청을 받은 도메인
    server_name soulmate.kro.kr;

	# 발급받은 인증서 키 위치
    ssl_certificate /etc/letsencrypt/live/soulmate.kro.kr/fullchain.pem;
    # ssl 개인 키 경로
    ssl_certificate_key /etc/letsencrypt/live/soulmate.kro.kr/privkey.pem;

	# 지원 할 ssl 버전 정보
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers HIGH:!aNULL:!MD5;

    location / {
	    # 요청 받은 정보를 컨테이너 서버로 전달 -> 사실 컨테이너 이름을 넣는게 좋음
        proxy_pass http://172.23.0.2:3000;## 컨테이너 명을 못 찾아서 퍼블릭 주소 입력
        # 원래 요청된 호스트 정보를 백엔드 서버(프록시 대상)로 전달
        proxy_set_header Host $host;
        
        # 클라이언트의 실제 IP 주소를 백엔드 서버로 전달.
        proxy_set_header X-Real-IP $remote_addr;

		# 클라이언트가 프록시를 거쳐 온 경우, 원본 IP 정보를 보존하여 전달 이게 없으면 nginx의 ip를 클라이언트로 인식.
		# 보통 로드 밸런서 뒤에 있을 때 사용됨. -> 무중단 cicd구축 생각으로 작성 추가
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        
	    # 클라이언트의 프로토콜(HTTP or HTTPS)을 백엔드 서버로 전달
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
~~~

---

## **5. SSL 인증서 발급 (Let's Encrypt)**

**Nginx 설정 테스트**

`sudo nginx -t`

~~~
`nginx: configuration file /etc/nginx/nginx.conf test is successful`
~~~

 **Nginx 재시작**
`sudo systemctl restart nginx`

🔹 **Let's Encrypt SSL 인증서 발급**

~~~
`sudo certbot --nginx -d soulmate.kro.kr`
~~~
**이메일 입력 후 약관 동의 (`Y`) → SSL 인증 완료!**

🔹 **SSL 자동 갱신 설정**

갱신 되는지 테스트
`sudo certbot renew --dry-run

-> Certbot has successfully simulated the renewal of your certificate.
갱신 성공

자동으로 갱신하는  설정 작성 

sudo crontab -e
0 3 * * * systemctl stop nginx && certbot renew && systemctl reload nginx



---

## **6. Docker 컨테이너 & HTTPS 최종 확인**

🔹 **HTTPS 접속 테스트**
https는 포트 번호 3000을 사용하지 않는다
[[port]]
`curl -I https://soulmate.kro.kr`
정상 출력은 아래와 같음

`HTTP/2 200 server: nginx ...`
