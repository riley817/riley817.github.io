# [AWS EKS] 4. Amazon ECR 에 이미지 올리기


> _☁️  **Amazon EKS 웹 애플리케이션 구축하기** 워크샵을 실습한 내용입니다._\
> ☁️  AWS Workshop 링크 : https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/ko-KR/ 

## Amazon ECR 리포지토리 생성 및 이미지 올리기

**Amazon ECR(Elastic Container Registry)** 은 도커 컨테이너의 이미지를 저장하는 Repository 서비스이다. Docker hub의 기능과 동일하다.

### AWS CLI로 이미지 리포지토리 생성

```bash
aws ecr create-repository \
--repository-name demo-flask-backend \
--image-scanning-configuration scanOnPush=true \
--region ap-northeast-2
```
명령어가 수행되면 리포지토리에 대한 정보가 출력되며 [Amazon ECR  콘솔창](https://console.aws.amazon.com/ecr/home)에서도 생성된 리포지토리를 확인할 수 있다.

{{< figure src="/categories/images/devops/20220507231626.png#center">}}

### 이미지를 리포지토리로 푸시
이미지를 푸시하려는 리포지토리 선택 후 오른쪽 상단에 **푸시 명령 보기(View push commands)** 버튼을 클릭하면 푸시 명령어를 확인 할 수 있다.
{{< figure src="/categories/images/devops/20220507232820.png#center">}}

#### AWS 인증정보를 검색하여 도커 클라이언트를 인증한다.
아래 파라미터는 환경 변수로 지정하거나 직접 입력한다.

- `${AWS_REGION}` : 사용 리전 
- `$ACCOUNT_ID` : IAM 계정 ID(숫자 12자리)

```bash
aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com
```

#### 도커 이미지 빌드
(AWS 워크샵 샘플 이미지 이용)
```bash
cd ~/environment/amazon-eks-flask

docker build -t demo-flask-backend .
```

#### 이미지 빌드 완료 후 **docker tag** 태그 지정
```bash
docker tag demo-flask-backend:latest $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/demo-flask-backend:latest
```

#### 이미지를 리포지토리에 푸시
```bash
docker push $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/demo-flask-backend:latest
```

### 푸시 된 리포지토리 확인
Amazon ECR > 해당 리포지토리 클릭 하면 빌드된 이미지를 확인 할 수 있다.
{{< figure src="/categories/images/devops/20220507234210.png#center">}}



