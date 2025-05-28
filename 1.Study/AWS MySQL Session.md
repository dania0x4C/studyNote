qarameter에 대한 튜닝이 쿼리 튜닝

??? 파라미터가 뭐임

파라미터 튜닝의 종류
1. global 수준의 변경
2. session 수준의 변경

conf 파일은 autoConfig, myConfig가 있는데 auto는 demon으로 관리

- innodb_buffer_pool_size: 

rdbms
데이터는 영구관리를 위해서 디스크에 저장이 되어있고 -> 버퍼 메모리에 이동 -> 사용

temp 설정이 v.8에서는 좀 중요하다

config 파일에서도 innodb_flush_log_at_trx_commit으로 로그를 남기는 설정 가능

Upgrade

in-place: 엔진만 바꾸는 upgrade

logical: 자체 application을 가지고 상위로 올리는//////// 

architecture

storageEngine, serverEngine 나누어서 사용하는 방법
![[Pasted image 20250523104015.png]]



thread
- foreground thread
- backgroud thread


https://jonghoonpark.com/2024/08/24/mysql-architecture

redo를 이용해서
