# Docker Image build

#### Entrypoint 생성

* 해당 Container에 접속 후 entry point 파일 생성

```
vi /docker-entrypoint.sh
```

  Sample:
```
#!/bin/bash

java -jar /ipm/swt-collector-0.0.1.jar --spring.config.location=/ipm/app.properties
```

  - 실행 권한 추가
```
chmod 755 /docker-entrypoint.sh
```

#### Commit

* 실행 중인 Container를 Image로 저장

  - commit
  
```
  docker commit {Target-Container} {ImageName}:{Tag}
```

  예제
```
docker myContainer:running newImage:0.5
```

#### 2. Build

* Dockerfile 파일 생성

```
vi Dockerfile
```

Sample:
```
FROM newImage:0.5
MAINTAINER songagi <songagi@gmail.com>

RUN yum update

VOLUME ["/logs", "/logs", "/logs"]

EXPOSE 80
EXPOSE 514/udp

ENTRYPOINT ["/docker-entrypoint.sh", "-D", "FOREGROUD"]
```

* Build

  docker build --tag {ImageName}:{Tag} {Dockerfile경로}
  
```
docker build --tag newImage:0.5.1 .
```

```
Step 1 : FROM newImage:0.5
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

* 생성된 Image 확인
```
docker images
```

#### 3. 신규 이미지 실행

```
docker run -d --name newContainer -v /logs:/logs -p 80:80 -p 514:514/udp newImage:0.5.1
```
