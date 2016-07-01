# 주요 Docker Container 정리

#### Redis 설치

 ```
 docker pull redis
 ```
  
 ```
 docker run \
  --name myRedis \
  -d \
  -p 6379:6379 \
  -e REDIS_PASS=$password \
  --restart always \
  redis
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

  # optionally set the following
  # it will tighten security by only allowing the host to connect
  # listen_addresses = '*'
  ```
  
  * 재시작
  * 
  ```
  docker start myPostgres
  ```
  
#### cAdvisor (Docker Monitoring Tool)

  ```
  docker pull google/cadvisor

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
