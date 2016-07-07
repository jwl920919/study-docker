[Back](https://github.com/songagi/study-docker/blob/master/README.md)

# Container log 삭제

  * docker-logs-clean.sh 생성
```
vi docker-logs-clean.sh
```
```
#!/bin/bash

rm $(docker inspect $1 | grep -G '"LogPath": "*"' | sed -e 's/.*"LogPath": "//g' | sed -e 's/",//g');
```

  * 권한 수정
```
chmod 755 docker-logs-clean.sh
```

  * log 삭제
```
sudo ./docker-logs-clean <container>
```

[Back](https://github.com/songagi/study-docker/blob/master/README.md)
