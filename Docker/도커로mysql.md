# mysql 설치 후 실행하기
```
docker run -d --name wordpressdb -e MYSQL_ROOT_PASSWORD=encore -e MYSQL_DATABASE=wordpress mysql:5.7
```
- usernaem : root / password : encore

```
docker run -d -e WORDPRESS_DB_USER=root -e WORDPRESS_DB_PASSWORD=encore -e WORDPRESS_DB_NAME=wordpress --name wordpress --link wordpressdb:mysql -p 90:80 wordpress
```
- 맨처음에 만들었던 wordpressdb를 바라보는 wordpress를 만들어 준것이다.

```
docker run -d --name encoredb -e MYSQL_ROOT_PASSWORD=encore -e MYSQL_DATABASE=encore -p 3306:3306 -v /home/ubuntu/data:/var/lib/mysql mysql:5.7
```
- 이후 해당 aws계정 ip주소로 dbeaver에 접속하여서 mysql에 연결해보면 새롭게 생성된걸 볼수있다.

- -v : 위에 코드는 aws계정 로컬에 연동이 되는 data폴더를 만들어 컨테이너환경에서 작업을하면 data쪽에도 똑같이 생기는 것을 알수있다.

## 백그라운드로 실행중인 컨테이너 안에서 bash 실행하기
```
docker exec -it encoredb bash
```

## 실습하기
- 선생님 공유폴더에서 'encore_backup.sql'파일 다운로드 후 aws계정으로 이동
- 다시 encoredb의 root폴더안에 복사해서 붙여넣기
```
docker cp encore_backup.sql encoredb:/root/
```

- bash 실행하기
```
docker exec -it encoredb bash
```

- mysql에 백업파일 넣기
```
mysql -uroot -pencore < encore_backup.sql
```

- dbeaver로 파일이 들어왔는지 확인하기

- 백업을 하고 있는 상태니까 컨테이너를 삭제하고 나서 다시 run 했을때 정보가 안지워지고 그대로 있는지 확인하는 실습하기
```
docker stop encoredb
docker rm encoredb

# 다시실행하기
docker run -d --name encoredb -e MYSQL_ROOT_PASSWORD=encore -e MYSQL_DATABASE=encore -p 3306:3306 -v /home/ubuntu/data:/var/lib/mysql mysql:5.7
```