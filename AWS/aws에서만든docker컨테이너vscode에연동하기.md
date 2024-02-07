# vscode에서 ec2에서 생성한 docker images,container보기
- 우선 vscode에서 extension 'ssh','docker'를 설치해준다.

- 로컬에서 /.ssh/config에 아래코드를 추가해준다.
```
Host ec2
  HostName [ec2 public address]
  User ubuntu
  IdentityFile [aws에서 인증키가 있는 곳에 주소]
```

- 접속이 잘되는 지 확인
```
ssh ec2
```

- vscode에서 연결해주기
1. command + shift + p 를 치고 ec2와 연결을 해준다.

2. 그다음에 docker로 이동해서 images와 contianer가 다 있는지 확인
