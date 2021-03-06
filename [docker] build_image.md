[Back](https://github.com/songagi/study-docker/blob/master/README.md)

# Docker Image build

#### 1.Entrypoint 파일 생성

  * 해당 Container root에 docker-entrypoint.sh 파일 생성

```
Container$ vi /docker-entrypoint.sh
```

  Sample:
```
#!/bin/bash

java -jar /ipm/swt-collector-0.0.1.jar --spring.config.location=/ipm/app.properties
```

  - 실행 권한 추가

```
Container$ chmod 755 /docker-entrypoint.sh
```

#### 2. Commit

 * 대상 Container 중지
 
```
$ docker stop {Target-Container}
```

 * commit 
 
 * 대상 Container를 Image로 저장
  
```
$ docker commit {Target-Container} {ImageName}:{Tag}
```

  sample:
```
docker myContainer myImage:0.5.10
```

  * Image 생성 확인
```
$ docker images
```

#### 2. Image Build

  * Dockerfile 파일 생성

```
$ vi Dockerfile
```

  Sample:
```
FROM myImage:0.5.10
MAINTAINER songagi <songagi@gmail.com>

RUN yum update

VOLUME ["/logs", "/logs", "/logs"]

EXPOSE 80
EXPOSE 514/udp

ENTRYPOINT ["/docker-entrypoint.sh", "-D", "FOREGROUD"]
```

  * Build (Dockerfile 기준으로 신규 Image 생성)

  docker build --tag {ImageName}:{Tag} {Dockerfile경로}
  
```
$ docker build --tag myImage:0.6 .
```

```
Step 1 : FROM myImage:0.5.10
 ---> 5f3d29f0c533
Step 2 : MAINTAINER songagi <songagi@gmail.com>
 ---> Using cache
 ---> a9b67f8fc8e0
Step 3 : RUN yum update
 ---> Using cache
 ---> 62801b1caade
Step 4 : VOLUME /logs /logs /logs
 ---> Using cache
 ---> 05fb1415113c
Step 5 : EXPOSE 514/udp
 ---> Using cache
 ---> b622d75d425f
Step 6 : ENTRYPOINT /docker-entrypoint.sh -D FOREGROUD
 ---> Using cache
 ---> 6b7f5d94188c
Successfully built 6b7f5d94188c
```

  * 신규 Image 생성 확인
```
$ docker images
```

#### 3. 신규 이미지 실행

  * Container 실행
```
$ docker run -d --name newContainer -v /logs:/logs -p 80:80 -p 514:514/udp myImage:0.6
```

  * Container 접속
```
$ docker exec -it newContainer /bin/bash
```

[Back](https://github.com/songagi/study-docker/blob/master/README.md)
