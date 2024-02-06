# aws로 website배포하기

# aws ip로 접속하기
- 새로운 인스턴스를 생성해서 접속한다.

## 키파일 권한 변경
```
chmod 600 키파일.pem 
```

## 접속하기
```
ssh -i 키파일.pem ubuntu@ip
```

## apt 업데이트 하기
```
sudo apt update

# python 설치하기
sudo apt install python3-pip
```

## repos 폴더 생성하기
```
mkdir ~/repos

# 들어가기
cd repos/
```

## git 설정하기
```
git init --bare .

# ls -al로 폴더 생성 되었는지 확인하기
```

## 간단하게 접속 설정하기
```
# 내 로컬서버 Public key 복사
id_rsa.pub

# aws서버
vim ~/.ssh/authorized_keys 
위에 위치에다가 붙여넣기

# 내 노트북에서 ec2 서버에 접속할때 
cmd -> ssh ubuntu@ip 
```

## gitignore 파일 만들기
기존 장고웹폴더안에 .gitignore파일이 없다면 생성해 줘야 한다.

1. https://www.toptal.com/developers/gitignore
에 접속

2. django 입력

3. 나온내용 복사후 .gitignore파일에 붙여넣기

## 장고웹폴더랑 aws서버 repos폴더랑 연결시키기
1. git init

2. 주소로 연동하기
git remote add origin ssh://ubuntu@ip:/home/ubuntu/repos

3. push 준비부터 푸쉬하기
```
git add .
git commit -m 'first'
git push origin master 
```

## authoized_keys 설정하기
1. aws서버에서 키젠 생성하기
```
ssh-keygen -t rsa
```
2. authorized_keys에 붙여넣기
```
cat ~/.ssh/id_rsa.pub >> authorized_keys

# 만약 들어가서 확인했을때 들어가 있지 않으면 직접들어가서 붙여넣어준다.
```
3. home에서 workspace폴더 만들기
```
mkdir ~/workspace && cd ~/workspace 
```
4. 연동하기
```
git clone ssh://ubuntu@localhost:/home/ubuntu/repos
```
5. pkg-config 설치하기
```
sudo apt-get install python3-dev default-libmysqlclient-dev build-essential pkg-config
```
6. requirements.txt 설치하기
```
pip install -r requirements.txt
```
7. 실행하기 
```
python manage.py runserver 0.0.0.0:8000
```
## AWS서버에서 도커설치 및 jupyter 실행
- 아래코드는 한꺼번에 실행하기

```
# Add Docker's official GPG key:

sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

```
# Add the repository to Apt sources:

echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update




sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

- docker 실행확인하기
```
sudo service docker status 
```

- 도커에서 jupyter를 실행하는데 spark가 있는!!
```
sudo docker run -p 8888:8888 -e JUPYTER_ENABLE_LAB=yes --restart always jupyter/all-spark-notebook
```

- 다시 실행해서 token값 확인하는 방법
```
# 실행중인지 확인
sudo docker ps

# logs로 토큰값확인
sudo docker logs reverent_cohen 

# docker를 멈추고 싶다면
sudo docker stop reverent_cohen

# 다시 실행할려면 start로 바꿔주면 된다.
```

## git clone 연결 해제 하는법
- 연결 해제
```
git remote remove origin
```

- 다시 연동하기
```
git remote add origin ssh://ubuntu@[]:/home/ubuntu/repos
```

## nginx
- 설치하기
```
sudo apt install nginx 
```

- 보안 그룹에 80포트로 등록

- ubuntu ip 로 실행하기