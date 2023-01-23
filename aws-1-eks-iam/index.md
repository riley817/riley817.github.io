# [AWS EKS] 1. IAM 계정 생성


> ☁️  [Amazon EKS 웹 애플리케이션 구축하기](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/ko-KR/)
워크샵을 실습한 내용입니다.

## IAM 계정 생성
1. AWS 계정의 루트사용자로 로그인한다.
2. [IAM 대시보드](https://us-east-1.console.aws.amazon.com/iamv2/home#/home)에서 **액세스 관리 > 사용자 > 사용자 추가**를 선택한다.

{{< figure src="/posts/images/devops/Pasted image 20220505142124.png#center">}}

3. 사용자 이름을 입력 후 **Access type**에서 **암호-AWS 관리 콘솔 액세스** 선택 **사용자 지정 비밀번호**로 비밀번호 생성한다. 

{{< figure src="/posts/images/devops/Pasted image 20220505142328.png#center">}}

4. **기존 정책 직접 연결(Attach existing policies directly)** 선택 후 부여하려는 정책을 선택하여 **다음:태그(Next:Tags)** 버튼을 클릭.

{{< figure src="/posts/images/devops/Pasted image 20220505142354.png#center">}}

5. 태그 추가(선택 사항) 단계 후 최종 생성 정보를 확인하고 **사용자 만들기(Create User)** 클릭하여 생성한다.

6. 사용자가 추가되면 로그인 URL이 생성된다!
    - `https://<your_aws_account_id>.signin.aws.amazon.com/console`

7. 루트 사용자에서 로그아웃 후, IAM 계정 URL로 접속하여 로그인 한다.

{{<image src="/posts/images/devops/Pasted image 20220505142542.png#center" width="350px">}}
