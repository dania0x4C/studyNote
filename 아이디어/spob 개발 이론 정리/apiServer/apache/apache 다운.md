
apache 다운로드
```
sudo apt install apache2
```

서비스 상태 확인
```
sudo systemctl status apache2
```

/etc/apache2/sites-available/ : 가상 호스트 설정 파일 저장

/etc/apache2/sites-enabled/: 활성화된 가상 호스트 설정 파일의 symbolic 링크가 저장

/var/www/html: 기본 루트 디렉토리 설정

apache2를 이용한 local -> http -> https api통신을 위한 conf 코드

기본 설정
```
# HTTP (HTTP -> HTTPS 리디렉션)

<VirtualHost *:80>

    ServerName spobweb.kro.kr

    # ServerAlias www.spobweb.kro.kr

    RewriteEngine On

    RewriteCond %{HTTPS} off

    RewriteRule ^/?(.*) https://%{SERVER_NAME}/$1 [R=301,L]

</VirtualHost>

# HTTPS (HTTPS -> Express)

<IfModule mod_ssl.c>

    <VirtualHost *:443>

        ServerName spobweb.kro.kr

        ServerAdmin admin@spobweb.kro.kr

        DocumentRoot /var/www/html

        ErrorLog ${APACHE_LOG_DIR}/error.log

        CustomLog ${APACHE_LOG_DIR}/access.log combined

        SSLEngine on

        SSLCertificateFile /etc/ssl/certs/apache-selfsigned.crt

        SSLCertificateKeyFile /etc/ssl/private/apache-selfsigned.key

        ProxyRequests Off

        ProxyPreserveHost On

        ProxyPass /api http://localhost:3000/api

        ProxyPassReverse /api http://localhost:3000/api

        <Proxy *>

            Require all granted

        </Proxy>

    </VirtualHost>

</IfModule>
```

1. domain 생성
2. 