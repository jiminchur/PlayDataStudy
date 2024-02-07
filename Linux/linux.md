# linux 기초

## linux terminal를 iterm에 연동하기

'''ssh [사용자명]@[각 컴퓨터에 할당된 주소] '''
> 이걸 linux에서 실행 시킨뒤에 iterm에 입력해주면 연동이 가능하다.

## Linux 명령어 모음집

### 현재 사용중인 계정
```
whoami
```

### 현재 폴더에 있는 파일을 숨겨진거 까지 모두 보여줘
```
ls -al
```
- ls만 하면 숨겨진 파일까진 보여주진 않는다 

### sudo vim /etc/hosts
```
sudo vim /etc/hosts
```

### bashrc
```
vim ~/.bashrc

# 변경사항 저장하기
source ~/.bashrc
```

### apt 업데이트
```
sudo apt update
```

### 프로그램 위치 확인
```
whereis java
```

### 실제 실행되는 프로그램 위치 확인
```
which java
```

### 현재 계정에서 나가기
```
exit
```
### 스케일 업 / 아웃

- 스케일 업
    
    - 버전 업그레이드

- 스케일 아웃
    
    - 병렬처리

### drwxrwxrwx의 의미

> 'd'는 폴더라는 의미 이고 'r' read ,'w' write, 'x' 실행이라는 뜻 이다.
> 3등분으로 나눌수가 있는데 "소유자/그룹/others"로 나눈다.
> 보통 숫자로 바꿔서 많이 부른다 r-4,w-2,x-1 이고 같이 있으면 더해서 부르면 된다.

1. drwxr-x--- : 폴더 750
2. -rw-r--r-- : 644

### 권한변경

> 위에를 이해 했다면 권한 변경이해도 금방 될거다 원래 750인걸 700으로 권한을 변경한다면 'chmod'를 사용하면 된다.
```
chmod 700 [바꿀 폴더명]
```

### 포트 확인 하는 방법
```
netstat

# net-tools 설치
sudo apt install net-tools -y

# 특정 포트(여기서는 8888)와 관련된 네트워크 연결 및 프로세스 정보를 조회
netstat -nao | grep 8888

netstat -lntp | grep 8888
```






## vim

> Vim 설치하기 
```
vim/home/[계정]/.jupyter/jupyter_notebook_config.py
```

- 수정모드 : i
- 명령어모드 : ':q' 나가기 / ':q!' 강제로 나가기






## wget missingURL오류
1. ping오류 인지확인한다.
```
ping 8.8.8.8
```
2. 오류가 나면 다음부터 순서대로 진행한다.
```
# etc/netplan으로 이동
cd /etc/netplan/

# ip 주소 설정 폴더로 이동 
sudo vim 00-installer-config.yaml

# 안에서 false로 되어있는 값을 true로 변경

# 변경사항 저장
sudo netplan apply

# 다시 핑 확인
ping 8.8.8.8
```
> 8.8.8.8은 크롬 인터넷 주소이고 연결되어 있는지 확인해야 한다. 보통 Mac에서 많이 발생한다.





## Mac에서 windows powertoys의 호스트파일관리와 같은 기능구현 하는법
1. Mac terminal에서 아래 코드 입력
```
sudo vim /etc/hosts
```

2. password 입력 후에 접속하기

3. namenode/secondnode/datanode의 ip을 넣고 뒤에 name입력해주기
```
000.00.000.000 namenode
000.00.000.000 secondnode
000.00.000.000 datanode
```

4. 저장 후 빠져나오기






# Miniconda

## miniconda 설치 주소 (arm용)
```
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-aarch64.sh
```

## minconda 설치하기
1. 권한 변경하기
```
chmod 700 ./Miniconda3-latest-Linux-aarch64.sh
```

2. 실행하기
```
./Miniconda3-latest-Linux-aarch64.sh
```
> 주의할점이 enter을 쭉 누르다보면 yes입력 창이 뜬다 이걸 입력해줘야 한다. 중간에 끊으면 안됨

3. 변경사항 저장하기
```
source ~/.bashrc
```

- 이제 자동적으로 (base)라는 가상환경에 들어가게 된다.
```
# 가상환경 빠져나오는 코드
conda deactivate

# 다시 들어가는 코드
conda activate

# 안되면 base 가상환경 이름까지 입력
conda activate base
```





# jupyter
> 모든 진행은 가상환경 base에서 진행한다.

## jupyter 설치하기
```
pip3 install jupyter
```

## jupyter server password 생성
```
jupyter server password

cat /home/gen2/.jupyter/jupyter_server_config.json
-> after 나오는 "argon..." 를 따로 저장해둔다

# 아래에 들어가서 위에서 저장한걸 입력하면 된다.
vim /home/[계정]/.jupyter/jupyter_notebook_config.py
# 927번에 입력
:927 로 이동후 입력모드한 후에 입력
```

## 서버 실행
1. workspace폴더 생성 및 이동
```
mkdir workspace

cd workspace
```

2. 실행
```
jupyter lab --ip=0.0.0.0

# 만약 백그라운드로 실행하고 싶으면 nohup
nohup jupyter lab --ip=0.0.0.0 &
```

3. 접속
```
[hadoop ip 주소]:8888
```

## 운영체제 batch job 설정
- 예시
```
# 아래에 접속하기
crontab -e

# 자동화 코드
## 월요일 부터 금요일까지 18시 정각에 day.sh를 실행 시키겠다
00 18 * * 1-5 /home/ec2-user/workspace/batch/day.sh 
## 금요일까지 18시 15분에 week.sh를 실행 시키겠다
15 18 * * 5 /home/ec2-user/workspace/batch/week.sh 
```

- 실습하기
1. hadoop계정에서 a.py 생성 및 접속
```
vim a.py
```

2. 실행 시킬 파이썬 코드 작성
```
# 현재시간을 띄워주는 코드 #

from datetime import datetime

# 현재 시간을 YYYY-MM-DD HH:MM:SS 형식의 문자열로 가져옴
now = datetime.now().strftime("%Y-%m-%d %H:%M:%S")

# 현재 디렉터리에 있는 test.txt 파일을 쓰기 모드로 열기
f = open("./test.txt", "w")

# 현재 시간을 파일에 씀
f.write(now)

# 파일 닫기
f.close()

# 저장 후 빠져나오기
```

3. a.py 실행하기
```
python a.py 
```

4. 스크립트 파일 만들기 및 접속
```
vim test.sh 
```

5. 아래코드 전체 입력 후 저장하고 빠져나오기
```
#!/bin/bash
/home/hadoop/miniconda3/bin/python /home/hadoop/a.py
```
6. sh파일에 실행 권한 부여
```
chmod +x ./test.sh
```

7. crontab -e에 접속 및 자동화 코드 작성
```
crontab -e

# 매일 5시 46분에 test.sh파일을 실행 시킨다.
46 05 * * * /home/hadoop/test.sh 
```




## linux에 'ssh client'로 바로 들어가기

1. 맥에서 사용자/.ssh 터미널로 접속
```
cd .ssh/
```

2. config 파일이 없다면 생성 있다면 밑에다가 작성
```
Host client
  HostName 000.00.000.000
  User hadoop
  IdentityFile /Users/[사용자]/.ssh/hadoop.pem
```

3. client에서 private키 복사 후 밑에 진행
```
# .ssh 에서 진행

# hadoop.pem 만들기
vim hadoop.pem 접속후 붙여넣기

# 권한 변경
chmod 400 hadoop.pem
```

4. ssh client로 로그인 없이 접속가능한지 확인!!





## File 전송 프로그램 scp
```
scp  [대상파일]   [계정]@[ip]:[경로]

ex : scp ./dog_model_service.7z gen2@192.168.57.129:/home/gen2/
```




## find 명령어 사용하기
```
find [위치] -name [검색이름]

# 예시

# 이름에 jar로 끝나는 파일 찾기
find ./ -name "*.jar"

# 이름에 jar로 끝나는 파일 찾기 + dev파일 안에 있는
find / -name "*.jar" 2>/dev/null

# 이름에 jar로 끝나는 파일 찾기 + dev파일 안에 있는 + result.txt에 저장하기
find ./ -name "*.jar" 2>/dev/null > result.txt

# result.txt 확인하기
vim result.txt
```

## chown 파일 소유자 변경
```
# 소유자를 hadoop:hadoop으로 변경하는데 sqoop2파일 부터 이하 파일 모두 변경하기
sudo chown -R hadoop:hadoop /var/lib/sqoop2/
```