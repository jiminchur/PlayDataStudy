# Cli 환경에서 aws EC2관리하기
- 다운로드하기
[최신 버전의 AWS CLI 설치 또는 업데이트 - AWS Command Line Interface (amazon.com)](https://docs.aws.amazon.com/ko_kr/cli/latest/userguide/getting-started-install.html)

- IAM > 사용자중 사용할 사용자 > 보안자격증명 탭 > 엑세스키 (없으면 발급하기) > csv파일로 저장

- 발급받은 엑세스키 등록하기
```
aws configure
```
- 로컬 환경에서 받은 엑세스키 등록하고 세번째에는 설정해둔 지역을 적으면된다.

- 현재 실행중인 인스턴스 확인하기
1. Json
```
aws ec2 describe-instances
```
2. Query
```
aws ec2 describe-instances --query 'Reservations[*].Instances[*].[InstanceId, State.Name, Tags[?Key==`Name`].Value | [0], PublicIpAddress, PrivateIpAddress, InstanceType, KeyName, Platform,InstanceType]' --output table
```

- aws cli 사용해서 인스턴스 시작, 중지, 삭제
```
aws ec2 start-instances --instance-ids <instance-id>
aws ec2 stop-instances --instance-ids <instance-id>
aws ec2 terminate-instances --instance-ids <instance-id>
```
