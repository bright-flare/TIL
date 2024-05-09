
### aws cli install
```bash
curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"

sudo installer -pkg ./AWSCLIV2.pkg -target /

# 설치 위치 체크
which aws

# version 확인
aws --version
```

### aws configure 
- (cli는 권장하지 않음) server만 접속하면 key획득가능하기때문.
```bash
aws configure

AWS Access Key ID [None]: <Key id>
AWS Secret Access Key [None]: <Secret Access Key>
Default region name [None]: 
Default output format [None]:
```

### ecr docker login
---
```bash
aws ecr get-login-password --region <region> | docker login --username AWS --password-stdin <ecr-repo>
```

### ecr docker pull
---
```bash
docker pull <account>.dkr.ecr.<region>.amazonaws.com/<repository-name>:<tag>
```

