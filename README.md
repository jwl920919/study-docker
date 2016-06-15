# Linux 정리

## Docker 설치(on CentOS7)

### Docker Repository 등록
```
vi /etc/yum.repos.d/docker.repo
```
```
[dockerrepo]
name=Docker Repository
baseurl=https://yum.dockerproject.org/repo/main/centos/$releasever/
enabled=1
gpgcheck=1
gpgkey=https://yum.dockerproject.org/gpg
```
### docker 설치
```
yum install docker-engine
```
### 서비스 등록 / 시작 / 확인
```
systemctl enable docker

systemctl start docker

systemctl status docker
```

### docker-machine 설치
```
curl -L \
 https://github.com/docker/machine/releases/download/v0.7.0/docker-machine-`uname -s`-`uname -m` > \
 /usr/local/bin/docker-machine \
 && chmod +x /usr/local/bin/docker-machine
```

### docker 실행 권한 부여
```
usermod -aG docker (계정명)
```

### docker 버전
```
docker version
```

### 저장된 이미지 확인
```
docker images
```

### 이미지 검색/다운/삭제
docker search (검색어)
docker pull (이미지명)
docker rmi (이미지명)

```
docker search centos

docker pull centos

docker rmi centos
```

### 이미지 file 저장 / 읽기
docker save -o (파일명) (이미지명)
또는
docker save (이미지명) > (파일명)
```
docker save -o myCentos.tar myCentos
또는
docker save myCentos > myCentos.tar
```
docker load -i (파일명)
또는
docker load < (파일명)
```
docker load -i myCentos.tar
또는
docker load < myCentos.tar
```

### 컨테이너 확인
docker ps
 -a : exit된 컨테이너 포함
 -s : 컨테이너 Size 출력
```
docker ps -a
```

### 컨테이너 실행
docker run <옵션> <이미지 이름,ID> <명령> <매개변수>

주요 Options
--name (이름)     : 컨테이너 이름
-d, --detach      : 데몬으로 실행 (백그라운드 )
-a, --attach      : 표준 출력, 표준 에러 연결 (ex. --attach="stdin" )
-c, --cpu-shares  : CPU 자원 분배, 기본 1024 (ex. --cpu-shares=2048)
--add-host        : /etc/hosts에 호스트 이름, IP 추가 (ex. --add-host=devserver:192.168.0.80 )
--dns             : dns 서버 설정 (ex. dns="8.8.8.8")
-e, --env         : 컨테이너 환경 변수 설정 (ex. -e MYSQL_ROOT_PASSWORD=password)
-h, --hostname    : 컨테이너의 호스트 이름 설정
-i, --interactive : 표준 입출력 활성화, 컨테이너와 연결되지 않아도 표준 입력을 유지, 보통 Bash 명령어 입력
-t, --tty         : TTY 모드 사용 여부, Bash를 사용하려면 이 옵셜 설정 필요. 입력 않으면 셸에 표기 X
-P, --publish-all : 컨테이너의 모든 포트를 외부에 노출
-p, --publish     : 특정 포트를 위부에 노출 <host포트>:<컨테이너포트>  (ex. -p 8080:80)
--restart         : 프로세스 종료 시 재시작 정책 설정
-v, --volume      : 데이터 볼륨 설정. :ro, :rw 읽기 쓰기 설정 <host 디렉토:<컨테이너디렉토리> (ex. -v /data:/data:ro)
--volume-from     : 데이터 볼륨 컨테이너 연결 (ex. --volume-from = "data:rw" )
-w, --workdir     : 컨테이너
--restart always  : 재시작
```
docker run \
 --name myRedis \
 -d \
 -p 6379:6379 \
 -e REDIS_PASS="pwd123" \
 --restart always \
 redis
```
 
