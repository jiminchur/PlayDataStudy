# hive

## hive 설치하기
1. 설치하기
```
wget https://dlcdn.apache.org/hive/hive-3.1.3/apache-hive-3.1.3-bin.tar.gz
```

2. 압축해제
```
tar xvfz apache-hive-3.1.3-bin.tar.gz
```

3. 폴더이름 변경
```
mv apache-hive-3.1.3-bin hive
```

## hive란??
- 페이스북에서 개발한 정보 플랫폼 중 가장 중요한 구성요소가 바로 아파치 하이브(apache hive)
- 하이브는 하둡 기반의 데이터 웨어하우징 프레임윅크
- 빠른 속도로 성장하는 페이스북의 소셜 네트워크에서 매일 같이 생산되는 대량의 데이터를 관리하고 학습하기 위해 개발 
- 페이스북의 제프 해머바처가 속한 팀은 데이터를 저장하고 처리하는 용도에 맞는 시스템을 고민하다가  비용이 저렴하고 확장성도 높은 하둡을 선택 
- 하이브는 자바 프로그래밍 기술은 부족하지만 강력한 SQL 기술을 가진 분석가가 페이스북의 HDFS에 저장된 대량의 데이터를 분석할 수 있도록 개발 

## 설치하는 법
- 하이브는 사용자의 워크스테이션에서 실행 
- 작성된 SQL 쿼리는 일련의 맵리듀스 잡으로 변환되어 하둡 클러스터에서 구동 
- 하이브는 HDFS에 저장된 데이터(디렉토리/파일)에 구조(스키마)를 입히는 방식으로 데이터를 테이블로 구조화 
- 테이블 스키마와 같은 메타데이터는 메타스토어(metastore)라 불리는 데이터베이스에 저장 

## mariadb에서 테이블 생성쿼리
```
# 데이터베이스 생성 쿼리
create database hivedb;

# 계정생성 쿼리
create user hiveuser@localhost identified by 'hivepw';

# 권한부여 쿼리
grant all privileges on hivedb.* to hiveuser@localhost;
grant all privileges on hivedb.* to hiveuser@'client' 

# 사용자의 암호를 설정하는 쿼리
identified by 'hivepw';

# 데이터베이스에서 권한 변경을 즉시 적용하기 위해 권한 캐시를 리로드하는 명령어
flush privileges;
```

## 마리아DB root계정으로 접속하기
```
# u : 사용자 p : 비밀번호
mysql -uroot -p123
```

## 하둡폴더생성
```
# 하둡에 warehouse폴더 생성
hdfs dfs -mkdir -p /user/hive/warehouse
# warehouse폴더 쓰는 권한 부여
hdfs dfs -chmod g+w /user/hive/warehouse

# 하둡에 tmp폴더 생성
hdfs dfs -mkdir /tmp
# tmp폴더 쓰는 권한 부여
hdfs dfs -chmod g+w /tmp
```

## 설정
1. bashrc에 접속
```
vim ~/.bashrc
```

2. 해당 코드 입력
```
export HIVE_HOME=/home/hadoop/hive
export PATH=$PATH:$HADOOP_HOME/sbin:$HADOOP_HOME/bin:$HIVE_HOME/bin
```

3. 변경사항 저장하기
```
source ~/.bashrc
```

4. Hive 메타스토어 스키마를 초기화하는 작업
```
schematool -initSchema -dbType mysql
```
- 이 명령어는 Apache Hive에서 제공하는 schematool 도구를 사용하여 Hive 메타스토어 스키마를 초기화하는 작업을 수행합니다. -initSchema 옵션은 스키마를 초기화하고, -dbType mysql은 데이터베이스 유형을 MySQL로 설정하는 것을 의미합니다.
### *** schemaTool failed *** 이 오류가 뜰경우!!!!
1. 마리아db연결상태 확인!!
```
# 활동중이여야 한다!!
sudo service mysql status 
```

2. 설정된시간 확인!!
```
timedatectl status

date

# 두개 코드를 입력해서 시간이 현재위치에 맞는지 확인한다.
```

- 만약 틀릴 경우
```
### mysql time zone 설정 ###

select @@global.time_zone, @@session.time_zone;
show global variables like '%time_zone%';
SET GLOBAL time_zone='Asia/Seoul';
SET time_zone='Asia/Seoul';

# 위의 SET에서 에러가 발생된다면 Asia/Seoul이 설치가 되어 있지 않는 것 

# 그럼 아래처럼 설정
SET GLOBAL time_zone='+09:00';
SET time_zone='+09:00';

# 시스템을 재가동하면 리셋이 되기 때문에 
sudo vi /etc/my.cnf 

# 위 파일에 보면 해당 경로에 수정해야한다고 !dir 이런 식으로 적혀 있음 
cd /etc/my.cnf.d 

# 이동해서 mariadb-server 파일 수정 

# 아래 내용을 mysqld 쪽에서 입력 Asia/Seoul이 설치가 되어 
[mysqld]
default-time-zone='Asia/Seoul'

# timezone 설치가 안되어 있어서 오류가 발생한다면... 아래 입력 
default-time-zone='+09:00'
-> insert place (입력할 위치)
# 이 폴더위치로 이동
cd /etc/mysql/mariadb.conf.d

# 파일 확인
sudo vim 50-server.cnf

# 저장하고 나와서 sql 재실행
sudo service mysql restart 
```

5. 제대로 실행된 경우 hive 실행
```
hive
```

## 주식데이터로 실습하기
1. hive에서 해당 쿼리 실행
```
# 전에 hadoop에 넣어두었던 주식정보 데이터를 사용하기위해 테이블생성, 컬럼생성 쿼리
CREATE EXTERNAL TABLE IF NOT EXISTS stocks (
day STRING,
open INT,
high INT,
low INT,
close INT,
trade INT,
alien INT,
ma05 FLOAT,
ma20 FLOAT,
ma60 FLOAT,
ma90 FLOAT,
ma120 FLOAT,
ticker STRING)
# hadoop폴더 어디에서 가져올건지 설정
ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
LOCATION '/stock'
tblproperties ("skip.header.line.count"="1");
```

2. 밑에 쿼리를 입력해서 잘 들어 왔는지 확인
 select * from stocks limit 10;

3. 이후에 원하는 쿼리문 작성