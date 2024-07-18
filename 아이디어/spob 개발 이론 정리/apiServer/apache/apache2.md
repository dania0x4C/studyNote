Apache2는 Apache Software Foundation에서 개발한 오픈 소스 웹 서버입니다. 다음은 Apache2의 주요 특징과 구성 요소에 대한 설명입니다:

1. **기본 기능:**
    
    - Apache2는 HTTP 및 HTTPS 프로토콜을 지원하여 정적 및 동적 콘텐츠를 제공할 수 있습니다.
    - 모듈화된 구조로 다양한 기능을 추가하거나 제거할 수 있으며, 필요한 모듈만 로드하여 서버의 성능을 최적화할 수 있습니다
2. **모듈 시스템:**
    
    - Apache2는 모듈 기반 아키텍처를 가지고 있어, 필요한 기능을 쉽게 추가할 수 있습니다. 주요 모듈로는 `mod_ssl` (SSL/TLS 지원), `mod_rewrite` (URL 재작성), `mod_proxy` (프록시 기능) 등이 있습니다
3. **설정 파일:**
    
    - 주요 설정 파일은 `/etc/apache2/apache2.conf`와 `/etc/apache2/sites-available/` 디렉토리에 위치합니다. `httpd.conf` 파일을 통해 서버의 동작을 상세히 제어할 수 있습니다
    - 설정 파일에서는 서버의 루트 디렉토리, 접근 권한, 포트 설정 등을 지정할 수 있습니다
4. **보안:**
    
    - Apache2는 SSL/TLS를 통해 보안을 강화할 수 있으며, 접근 제어와 인증을 위한 다양한 모듈을 제공합니다. `mod_ssl`을 통해 HTTPS를 설정하여 데이터 전송 시 암호화를 적용할 수 있습니다
5. **운영체제 지원:**
    
    - Apache2는 대부분의 주요 운영체제에서 사용할 수 있습니다. 특히, Linux와 Unix 계열 시스템에서 널리 사용되며, Windows에서도 사용할 수 있습니다



apache 실행 방법(wls방식)

```
# apache library download
sudo apt install apache2

# apache module 활성화

# URL 재작성 규칙 정의, 활성화
sudo a2enmod rewrite

# SSL/TLS 암화화 사용하는 모듈 활성화
sudo a2enmod ssl

# apache 서버가 proxy서버로 동작 활성화 
# client와 server 사이에 중계 역할을 하는 서버
sudo a2enmod proxy

# http을 통한 proxy 연결 활성화
sudo a2enmod proxy_http

#HTML 컨텐츠를 proxy설정에 맞게 재작성 할 수 있게 활성
sudo a2enmod proxy_html

# http/2 protocol 활성화
sudo a2enmod http2

```

```
# 아파치 설정 기본 위치

cd /etc/apache2/sites-available

# 파일 만들기
sudo vim 

# apache2 설정 파일 활성화
sudo a2ensite (conf파일 명)

# 파일명 server.conf
sudo a2ensite server

# 설정유효성 검사
sudo apachectl configtest

# 실행 sudo systemctl start apache2 
# 중지 sudo systemctl stop apache2 
# 재실행 (stop후 start) sudo systemctl restart apache2 
# 리로드 sudo systemctl reload apache2

# HTTP 확인    http://server.com 
# HTTPS 확인   https://server.com 
# Express 확인 http://server.com:8080

# 서버 로그 확인
sudo tail -f /var/log/apache2/error.log
sudo tail -f /var/log/apache2/access.log

# 서버 작동 확인
curl -v http://spobweb.kro.kr
```

추가사항
1. port 포워딩
2. ssl인증서 발


DNS IP연결 공인 ip: curl ifconfig.me