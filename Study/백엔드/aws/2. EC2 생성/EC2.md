가상 서버를 구축하고 보안, 네트워크 구성, 스토리지 관리를 하는 서비스 갑작스러운 트래픽 변화에 대한 예측을 하지 않아도 됨

## 웹 서버 인스턴스 생성

1. AMI 선택(Amazon Machine Image)
	- 가상머신(VM) 템플릿
	- 운영체제(OS), 서버, 사전 구성 소프트웨어가 포함

2. 인스턴스 유형(VM)
	- 각 인스턴스는 독립적으로 작동함 즉, 유연한 사용이 가능

3. Key 생성
	- SSH를 통해서 안전하게 EC2에 로그인

4. 네트워크 설정
	- VPC, SubNet, 보안규칙(인 바운드, 아웃 바운드)을 작성해서 사용
	
5. 스토리지 구성
	-  데이터를 안전하고 효율적으로 보관하며, 필요할 때 쉽게 검색할 수 있게 하는 것

7. 고급세부설정(예시)
```
#!/bin/sh
        
# Install a LAMP stack
sudo amazon-linux-extras install -y lamp-mariadb10.2-php7.2 php7.2
sudo yum -y install httpd php-mbstring
sudo yum -y install git
        
# Start the web server
sudo chkconfig httpd on
sudo systemctl start httpd
        
# Install the web pages for our lab
if [ ! -f /var/www/html/aws-boarding-pass-webapp.tar.gz ]; then
    cd /var/www/html
    wget -O 'techcamp-webapp-2024.zip' 'https://ws-assets-prod-iad-r-icn-ced060f0d38bc0b0.s3.ap-northeast-2.amazonaws.com/600420b7-5c4c-498f-9b80-bc7798963ba3/techcamp-webapp-2024.zip'
    unzip techcamp-webapp-2024.zip
    sudo mv techcamp-webapp-2024/* .
    sudo rm -rf techcamp-webapp-2024
    sudo rm -rf techcamp-webapp-2024.zip
fi
        
# Install the AWS SDK for PHP
if [ ! -f /var/www/html/aws.zip ]; then
    cd /var/www/html
    sudo mkdir vendor
    cd vendor
    sudo wget https://docs.aws.amazon.com/aws-sdk-php/v3/download/aws.zip
    sudo unzip aws.zip
fi
        
# Update existing packages
sudo yum -y update
```

## AMI

인스턴스를 시작하는데 필요한 정보를 제공

작업에서 이미지 생성하기를 통해 AMI 생성 가능

![[Pasted image 20240627100442.png]]

**AMI** 메뉴에서 상태 변경이 완료되면 AMI로 인스턴스 시작을 통해서 생성이 가능
동일한 설정을 해주고 subnet을 C와 연결하면 아래와 같은 구성이 됨

![[Pasted image 20240627100747.png]]


## ELB

애플리케이션 트래픽을 Amazon EC2 인스턴스, 컨테이너, IP 주소, Lambda 함수, 가상 어플라이언스와 같은 여러 대상에 자동으로 분산
- Application Load Balancer - http, https 사용시
- Network Load Balancer 
- Gateway Load Balancer
- Classic Load Balancer
을 제공함

1. VPC 설정
2. 사용할 서브넷 지정
3. 보안 그룹 생성
4. 리스너, 라우팅
	- 대상 그룹 생성
	- 유형: 인스턴스, 프로토콜: http
5. 인 바운드 규칙을 편집해서 ELB만 가능하게 만들어줌

![[Pasted image 20240627102329.png]]

##  S3

 객체 스토리지 서비스이다
즉, 다양한 데이터를 원하는 만큼 저장하고 보호 할 수 있는 서비

1. 이름은 Amazon S3 내에 중복될 수 없다
2. s3에 파일을 넣고 객체 URL을 복사하면 사용이 가능하다
3. 권한-기본적으로 퍼블릭에 대한 엑서스 차단 됨
	- 업로드한 객체에 대해서 퍼블릭으로 설정
4. 버킷 속성에서 정적 웹 사이트 호스팅 활성화, 인덱스 문서 입력시 웹 사이트에 접속이 가능
5. 엔드 포인트를 사용해서 접속







