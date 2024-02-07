## spark 설치하기
```
wget https://dlcdn.apache.org/spark/spark-3.3.4/spark-3.3.4-bin-hadoop3.tgz

# 압축해제
tar xvzf ./spark-3.3.4/spark-3.3.4-bin-hadoop3.tgz

# 폴더이름 변경
mv ./spark-3.3.4-bin-hadoop3 ./spark
```

## 선생님이 주신 spark 파일 아래 경로로 이동
```
/home/hadoop/spark/conf
```

## bashrc 설정
```
vim ~/.bashrc

export SPARK_HOME=/home/hadoop/spark/
export PATH=$PATH:$HADOOP_HOME/sbin:$HADOOP_HOME/bin:$HIVE_HOME/bin:$SPARK_HOME/bin

source ~/.bashrc
```

## jupyter 연동
```
pip install jupyter 

jupyter notebook --generate-config
vim /home/hadoop/.jupyter/jupyter_notebook_config.py


c.NotebookApp.allow_origin = '*'
c.NotebookApp.open_browser = False
c.NotebookApp.password = "argon2:$argon2id$v=19$m=10240,t=10,p=8$JjOUUl+PkMyaeVWz34kP7g$gH7h20sz6D/JmgVtCUKm3RquEAcdCnbDQ4jRL2eFskI"


# password -> 123

vim ~/.bashrc

export PYSPARK_DRIVER_PYTHON=jupyter
export PYSPARK_DRIVER_PYTHON_OPTS='notebook --ip=0.0.0.0'

source ~/.bashrc
```

## pyspark으로 실행
```
pysparks
```

## 연습용 데이터 받기
```
git clone https://github.com/FVBros/Spark-The-Definitive-Guide.git

hdfs dfs -mkdir -p /data/flight

cd /home/hadoop/Spark-The-Definitive-Guide/data/flight-data/csv

hdfs dfs -put *.csv /data/flight/
```

## pyspark 작업자 3개로 실행하는 방법
```
pyspark --master yarn --num-executors 3
```

## jupyter로 pyspark 사용하는 방법
1. jupyter 접속
```
[ip주소]:8888
```

2. 새로운 ipynb파일 생성

3. spark 실행

4. 연습용 데이터를 미리 hadoop 에 넣어두고 불러온다.
```
df = spark.read.csv("/data/bicycle/*", encoding='cp949', inferSchema = True, header=True)
```

### 전체 컬럼 확인하기
```
df.printSchema()

# output
root
 |-- 자전거번호: string (nullable = true)
 |-- 대여일시: timestamp (nullable = true)
 |-- 대여 대여소번호: integer (nullable = true)
 |-- 대여 대여소명: string (nullable = true)
 |-- 대여거치대: integer (nullable = true)
 |-- 반납일시: timestamp (nullable = true)
 |-- 반납대여소번호: string (nullable = true)
 |-- 반납대여소명: string (nullable = true)
 |-- 반납거치대: string (nullable = true)
 |-- 이용시간(분): integer (nullable = true)
 |-- 이용거리(M): double (nullable = true)
 |-- 생년: string (nullable = true)
 |-- 성별: string (nullable = true)
 |-- 이용자종류: string (nullable = true)
 |-- 대여대여소ID: string (nullable = true)
 |-- 반납대여소ID: string (nullable = true)

```
* 스키마
    * 스키마는 DataFrame의 컬럼명과 데이터 타입을 정의합니다. 데이터소스에서 스키마를 얻거나 직접 정의할 수 있습니다. 
    * 운영 환경에서 추출(Extract), 변환(Transform), 적재(Load)를 수행하는 ETL 작업에 스파크를 사용한다면 직접 스키마를 정의해야 합니다. 
    * ETL 작업 중에 데이터 타입을 알기 힘든 CSV 파일이나 JSON 등의 데이터소스를 사용하는 경우 스키마 추론 과정에서 읽어 들인 샘플 데이터의 타입에 따라 스키마를 결정해버릴수 있습니다. 

### pyspark에서 사용하는 함수 import
```
from pyspark.sql.functions import *
```
기존 파이썬에서 사용하는 코드랑 동일한 경우가 있다 나중에 현업에 가게되면 이렇게 진행하다보면 오류가 생길 위험이 있으니 사용할 함수만 임포트하는게 좋다.

### 표현식
* 표현식은 DataFrame 레코드의 여러 값에 대한 트랜스포메이션 집합을 의미
* 표현식은 expr 함수로 가장 간단히 사용할 수 있습니다. 
* 이 함수를 사용해 DataFrame의 컬럼을 참조할 수 있습니다. 
```
from pyspark.sql.functions import expr
expr("(((someCol + 5) * 200) - 6) < otherCol")

# output
Column<'((((someCol + 5) * 200) - 6) < otherCol)'>
```

### 첫번째 메서드로 로우를 확인하기
```
df.first()

# output
Row(DEST_COUNTRY_NAME='United States', ORIGIN_COUNTRY_NAME='Romania', count=15)
```

### 로우 생성하기
```
from pyspark.sql import Row
myRow = Row("Hello", None, 1, False)

myRow[0]

# outputs
'Hello'

myRow[2]

# output
1
```

### 데이터베이스안에 null값이 있는지 확인하기
```
df2 = df.select([count(when(col(c).contains('None') | \
                            col(c).contains('NULL') | \
                            (col(c) == '' ) | \
                            col(c).isNull(), c 
                           )).alias(c)
                    for c in df.columns])
```

- 위에 식이 실행되는 과정을 보여주는 코드
```
df2.explain()
```

## join
- inner join
```
empDF.join(deptDF,empDF.emp_dept_id ==  deptDF.dept_id,"inner") \
     .show(truncate=False)
```
- outer join
```
empDF.join(deptDF,empDF.emp_dept_id ==  deptDF.dept_id,"outer") \
    .show(truncate=False)
```
- fullouter
```
empDF.join(deptDF,empDF.emp_dept_id ==  deptDF.dept_id,"fullouter") \
    .show(truncate=False)
```
- full
```
empDF.join(deptDF,empDF.emp_dept_id ==  deptDF.dept_id,"full") \
    .show(truncate=False)
```
- right outer join
```
empDF.join(deptDF,empDF.emp_dept_id ==  deptDF.dept_id,"right") \
   .show(truncate=False)
empDF.join(deptDF,empDF.emp_dept_id ==  deptDF.dept_id,"rightouter") \
   .show(truncate=False)
```
- left semi
```
empDF.join(deptDF,empDF.emp_dept_id ==  deptDF.dept_id,"leftsemi") \
   .show(truncate=False)
```