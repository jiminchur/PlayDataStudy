# sqoop1

## sqoop1 설치하기
```
wget https://archive.apache.org/dist/sqoop/1.4.7/sqoop-1.4.7.bin__hadoop-2.6.0.tar.gz

# 압축해제하기
tar xvfz sqoop-1.4.7.bin__hadoop-2.6.0.tar.gz

# 파일 이름 변경
mv ./sqoop-1.4.7 ./sqoop
```

## 강사님에게 받은 파일 아래 위치에 넣기
```
~/download/sqoop/ 에 있는 3개 jar파일

/home/hadoop/sqoop2/lib
```

## mysql 접속 
```
# sql 접속
mysql -uroot -p123 

# 권한 부여
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '123';
```

# sqoop db 목록 확인 
```
sqoop list-databases --connect jdbc:mysql://192.168.56.121:3306/ --username root --password 123

sqoop list-databases --connect jdbc:mysql://client:3306/ --username root --password 123

sqoop import --connect jdbc:mysql://client:3306/[데이터베이스] --username root --password 123 --table [테이블명] -m 1 --target-dir /sqoop/subway7

sqoop import --connect jdbc:mysql://client:3306/[데이터베이스] --username root --password 123 --table [테이블명] -m 1 --hive-import
```

# sqoop2

## sqoop2설치하기
```
https://archive.apache.org/dist/sqoop/1.99.7/sqoop-1.99.7-bin-hadoop200.tar.gz
```

## mysql 권한 확인
```
# mysql 접속하기
mysql -uroot -p123

# 권한 확인하기
select user,host from mysql.user;

# 권한 확인하기 + 비밀번호(암호화된) 까지
select user,host,password from mysql.user;
```

## sqoop2압축해제하기
```
tar xvfz sqoop-1.99.7-bin-hadoop200.tar.gz

# 파일명 변경하기
mv ./sqoop-1.99.7-bin-hadoop200 ./sqoop2
```

## 강사님에게 받은 파일 아래 위치에 넣기
```
~/download/sqoop/ 에 있는 3개 jar파일

/home/hadoop/sqoop2/tools/lib
```

## bashrc 추가하기
```
# bashrc에 접속하기
vim ~/.bashrc

export SQOOP_HOME=/home/hadoop/sqoop
export PATH=$PATH:$HADOOP_HOME/sbin:$HADOOP_HOME/bin:$HIVE_HOME/bin:$HIVE_HOME/bin:$Z    EPPELIN_HOME/bin:$SQOOP_HOME/bin

# 변경사항 저장하기
source ~/.bashrc
```

## ???
```
# 위치로 이동
cd /home/hadoop/sqoop/conf

# 안에 들어가서 뭐했지???
vim sqoop.properties
```

## hadoop core-site.xml 수정
```
<property>        
    <name>hadoop.proxyuser.hadoop-user.hosts</name>       
    <value>hadoop</value>
</property>
<property>       
    <name>hadoop.proxyuser.hadoop-user.groups</name>        
    <value>hadoop</value>
</property>
```

## container-executor 수정
```
allowed.system.users=hadoop
```

## sqoop2폴더 아래 위치에 생성
```
sudo mkdir /var/lib/sqoop2
```

## bashrc 추가하기
```
# bashrc에 접속하기
vim ~/.bashrc

export SQOOP_SERVER_EXTRA_LIB=/var/lib/sqoop2/

# 변경사항 저장하기
source ~/.bashrc
```

## chown 파일 소유자 변경
```
# 소유자를 hadoop:hadoop으로 변경하는데 sqoop2파일 부터 이하 파일 모두 변경하기
sudo chown -R hadoop:hadoop /var/lib/sqoop2/
```

## 파일 복사하기
```
cp -R [복사할곳] [옮길곳]

cp -R /home/hadoop/hadoop/share/hadoop/client/*.jar /var/lib/sqoop2/
cp -R /home/hadoop/hadoop/share/hadoop/hdfs/*.jar /var/lib/sqoop2/
cp -R /home/hadoop/hadoop/share/hadoop/hdfs/lib/*.jar /var/lib/sqoop2/
cp -R /home/hadoop/hadoop/share/hadoop/common/*.jar /var/lib/sqoop2/
cp -R /home/hadoop/hadoop/share/hadoop/common/lib/*.jar /var/lib/sqoop2/
cp -R /home/hadoop/hadoop/share/hadoop/mapreduce/*.jar /var/lib/sqoop2/
cp -R /home/hadoop/hadoop/share/hadoop/yarn/lib/*.jar /var/lib/sqoop2/
cp -R /home/hadoop/hadoop/share/hadoop/yarn/*.jar /var/lib/sqoop2/
```


