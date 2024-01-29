# jupyter설치하기 + spark까지 사용할 수 있는
```
docker run -p 8888:8888 --name myjupyter -e JUPYTER_ENABLE_LAB=yes -v /home/ubuntu/jupyter:/home/jovyan/work --restart always jupyter/all-spark-notebook
```
- 도커로mysql파트에서 사용했던 encore_backup.sql 이용하기

- 파일을 복사하여서 jupyter폴더로 이동
```
sudo cp encore_backup.dql ~/jupyter
```

- jupyter에 접속해서 work폴더안에 파일들을 만들수 없을 것이다. 이유는 work폴더의 권한인 root로 되어있기 때문이가 이를 유저로 바꿔주어야 한다. 그러면 aws로컬에서 아래 명령어 실행
```
sudo chown 1000:100 ~/jupyter/
```