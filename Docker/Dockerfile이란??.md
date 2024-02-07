# Dockerfile이란??
> dockerfile이란 기존 환경을 구성하기 위해 패키지를 설치하고 환경설정하는 과정을 반복하였다. 이런 문제를 간소화하기 위해서 Dockerfile에 설치해야하는 패키지 소스코드 명령어 환경변수설정을 기록하여 빌드하면 자동으로 이미지가 생성하게 된다. 앞으로 애플리케이션 빌드 및 배포를 자동화 할 수 있게된다.

## 예시
```
FROM ubuntu:14.04
LABEL "purpose"="practice"
RUN apt-get update
RUN apt-get install apache2 -y
ADD test.html /var/www/html
WORKDIR /var/www/html
RUN ["/bin/bash", "-c", "echo hello >> test2.html"]
EXPOSE 80
CMD apachectl -DFOREGROUND
```
