# 주요 Docker Container 정리

#### Redis

 * Redis image 다운 / 실행
  
 ```
 docker pull redis
 ```
  
 ```
 docker run \
  --name myRedis \
  -d \
  -p 6379:6379 \
  -e REDIS_PASS="shinwoo123!" \
  --restart always \
  redis
 ```

#### RabbitMQ
 
 * Rabbit image 다운 / 실행
 
 ```
 docker pull rabbitmq
 ```

 ```
 docker run \
  --name=myRabbitmq \
  -d \
  -p 5672:5672 \
  -p 15672:15672 \
  -e RABBITMQ_DEFAULT_USER=admin \
  -e RABBITMQ_DEFAULT_PASS=shinwoo123! \
  --restart always \
  rabbitmq:3-management
 ```
