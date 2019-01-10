[Back](https://github.com/songagi/study-docker/blob/master/README.md)

# Docker 주요 Container 정리

#### Redis 설치

 * redis.conf 설정
  
```
mkdir -p /redis/conf
vi /redis/conf/redis/conf
```
```
# By default, if no "bind" configuration directive is specified, Redis listens
# for connections from all the network interfaces available on the server.
#
# Examples:
# bind 192.168.1.100 10.0.0.1
# bind 127.0.0.1 ::1

# Accept connections on the specified port, default is 6379 (IANA #815344).
port 6379

# TCP keepalive
tcp-keepalive 300

# Password
requirepass shinwoo123!
```

 * Redis 설치
```
docker pull redis

docker run \
 --name myRedis \
 -d \
 -p 6379:6379 \
 -v /redis/conf:/usr/local/etc/redis \
 --restart always \
 redis \
 redis-server /usr/local/etc/redis/redis.conf
```

#### RabbitMQ 설치
 
```
docker pull rabbitmq
```
```
docker run \
 --name=myRabbitmq \
 -d \
 -p 5672:5672 \
 -p 15672:15672 \
 -e RABBITMQ_DEFAULT_USER=$userid \
 -e RABBITMQ_DEFAULT_PASS=$password \
 --restart always \
 rabbitmq:3-management
```

#### Oralce 설치 (2019-01-10) - working

```
docker pull sath89/oracle-12c
```

* 실행
```
mkdir /oraData

docker run \
  --name=oracle \
  -d \
  -p 8080:8080 -p 1521:1521 \
  -v /oraData/data:/u01/app/oracle \
  --restart always \
  sath89/oracle-12c
  
docker run \
  --name=oracle \
  -d \
  -p 8080:8080 -p 1521:1521 \
  -e WEB_CONSOLE=true \
  -v /oraData/data:/u01/app/oracle \
  --restart always \
  sath89/oracle-12c
```

* Memory 설정
```
docker run -d -p 8080:8080 -p 1521:1521 -v /oraData/data:/u01/app/oracle -e DBCA_TOTAL_MEMORY=1024 sath89/oracle-12c
```

* 기본 연결 정보
sqlplus system/oracle@//localhost:1521/xe
```
hostname: localhost
port: 1521
sid: xe
service name: xe
username: system
password: oracle
```

* Password for SYS & SYSTEM:
```
oracle
```

*  Oracle Enterprise Management 콘솔 연결
```
http://localhost:8080/em
user: sys
password: oracle
connect as sysdba: true
```

* Oracle Application Express 콘솔 연결
```
http://localhost:8080/apex
workspace: INTERNAL
user: ADMIN
password: 0Racle$
```

#### PostgresSQL 설치

```
docker pull postgres
```
```
docker run \
 --name=myPostgres \
 -d \
 -p 5432:5432 \
 -e POSTGRES_PASSWORD=$password \
 -v /db_data/pgdata:/pgdata \
 --restart always \
 postgres
```
  
* Container 중지
  
```
docker stop myPostgres
```
 
* postgresql.conf 수정
 
```
vi /db_data/pgdata/postgresql.conf
```
```
# "local" is for Unix domain socket connections only
local   all             all                                     trust
# IPv4 local connections:
host    all             all             127.0.0.1/32            trust
# IPv6 local connections:
host    all             all             ::1/128                 trust
# Allow replication connections from localhost, by a user with the
# replication privilege.
#local   replication     postgres                                trust
#host    replication     postgres        127.0.0.1/32            trust
#host    replication     postgres        ::1/128                 trust
```
 
* postgresql.conf 수정
 
```
vi /db_data/pgdata/postgresql.conf
```
```
# optionally set the following
# it will tighten security by only allowing the host to connect
# listen_addresses = '*'
```

* 재시작
 
```
docker start myPostgres
```

#### cAdvisor 설치

* cAdvisor : Docker Monitoring Tool

```
docker pull google/cadvisor
```
```
docker run \
 --name=cAdvisor \
 -d \
 -p=8080:8080 \
 -v=/:/rootfs:ro \
 -v=/var/run:/var/run:rw \
 -v=/sys:/sys:ro \
 -v=/var/lib/docker/:/var/lib/docker:ro \
 --restart always \
 google/cadvisor:latest
```

* 접속
```
http://{SERVER-IP}:8080
```
 
[Back](https://github.com/songagi/study-docker/blob/master/README.md)
