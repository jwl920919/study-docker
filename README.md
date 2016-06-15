# Linux 정리

Docker 설치(CentOS7)

## Docker Repository 등록
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
## docker 설치
```
yum install docker-engine
```
## 서비스 등록 / 시작 / 확인
```
systemctl enable docker
systemctl start docker
systemctl status docker
```

## docker-machine 설치
```
curl -L \
 https://github.com/docker/machine/releases/download/v0.7.0/docker-machine-`uname -s`-`uname -m` > \
 /usr/local/bin/docker-machine \
 && chmod +x /usr/local/bin/docker-machine \
```

## docker 실행 권한 부여
```
usermod -aG docker (계정명)
```

## docker 버전
```
docker version
```

## docker image 확인
```
docker images
```

# docker image 검색 / 다운 / 삭제
```
docker search (검색어)
docker pull (이미지명)
docker rmi (이미지명)
```
