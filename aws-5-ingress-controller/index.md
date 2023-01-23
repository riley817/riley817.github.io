# [AWS EKS] 5. 인그레스 컨트롤러 만들기


> ☁️  [Amazon EKS 웹 애플리케이션 구축하기](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/ko-KR/)
워크샵을 실습한 내용입니다.

## 인그레스 `Ingress`
클러스터 내의 서비스에 대한 외부 요청을 어떻게 처리할 것인지 네트워크 7계층 레벨에서 정의하는 쿠버네티스 오브젝트이다.

인그레스 오브젝트의 기본 기능은 다음과 같다.
- **외부 요청의 라우팅** : 특정 경로로 들어온 요청을 어떤 서비스로 전달할지 정의하는 라우터 규칙 설정
- **가상 호스트 기반의 요청 처리** : 같은 IP에 대해 다른 도메인 이름으로 요청했을 때 어떻게 처리할 것인지 정의
- **SSL/TLS 보안 연결 처리** : 요청을 라우팅 할 때, 보안 연결을 위한 인증서 적용

### 인그레스를 사용하는 이유
쿠버네티스 서비스 타입 중 NodePort 혹은 LoadBalancer 타입의 서비스를 사용해도 외부로 노출할 수 있지만, 인그레스 없이 서비스를 사용할 경우 SSL/TLS 보안 연결 등의 상세 옵션을 각각의 서비스와 디플로이먼트에 대해 일일이 설정을 해야 한다.

## 인그레스 컨트롤러
인그레스는 요청을 처리하는 규칙을 정의하는 선언적 오브젝트이며 이런  **인그레스 컨트롤러(Ingress Controller)** 라는 서버에 적용해야만 그 규칙을 사용할 수 있다. 따라서 쿠버네티스의 인그레스는 반드시 인그레스 컨트롤러와 함께 사용해야 한다.

`kube-controller-manager`의 일부로 실행되는 다른 컨트롤러와 달리 인그레스 컨트롤러는 클러스터와 함께 생성되지 않으므로, 직접 구현해야 한다.

{{< figure src="/posts/images/docker-kubernetes/20220508003505.png#center" caption="[출처] https://kubernetes.io/ko/docs/concepts/services-networking/ingress/">}}

## AWS Load Balancer 컨트롤러 만들기
쿠버네티스 프로젝트에서는 [AWS Load Balancer](https://github.com/kubernetes-sigs/aws-load-balancer-controller), [GCE](https://github.com/kubernetes/ingress-gce/blob/master/README.md#readme), [Nginx Ingress](https://github.com/kubernetes/ingress-nginx/blob/main/README.md#readme) 등의 인그레스 컨트롤러 프로젝트를 지원하고 있다. 워크샵 실습에는 **AWS Load Balancer**로 인그레스 컨트롤러를 생성했다.

> 사실 머선말인지는 잘 모르겠다... 조금 더 공부를 해야겠다능 🫠🫠🫠

AWS Load Balancer 컨트롤러는 쿠버네티스 ingress를 생성할 때 ALB(Application Load Balancer)와 NLB(Network Load Balancer)를 생성하고 관리한다. 

- **쿠버네티스 `Ingress`를 생성할 때 `Application Load Balancer`으로 프로비저닝된다.**
- **쿠버네티스 로드 밸런스 타입의 `Service`를 생성할 때, `Network Load Balancer`으로 프로비저닝된다.**

{{< figure src="/posts/images/docker-kubernetes/20220508223231.png#center" caption="[출처] https://aws.amazon.com/blogs/opensource/kubernetes-ingress-aws-alb-ingress-controller/">}}

1. Ingress 리소스가 kubernetes API에서 생성될 때 `alb-ingress-controller`는 변경 사항을 관찰한다.
2. `alb-ingress-controller`는 ingress 리소스에 추가된 어노테이션을 기반으로 AWS ALB를 생성한다.
3. 대상 그룹은 ingress 리소스에 지정된 각 back-end에 대해 생성된다.
4. ALB URL은 경로 또는 쿼리 매개변수를 사용하여 액세스한다.
5. Ingress 리소스에 구성된 규칙에 따라 요청은 특정 대상 그룹으로 리다이렉션되고 ClustIP 또는 NodePort를 사용하여 Pod 서비스에 도달한다.

### ALB(Application Load Balancer)
- OSI 모델 Layer7에서 부하분산을 지원한다.
- HTTP, HTTPS 트래픽을 로드밸런싱해서 내부 인스턴스에 전달한다.
- IP주소 + 포트번호 + 패킷내용을 보고 로드밸런서 스위칭이 일어난다.
- 여러 도메인, 호스트, 경로 기반의 라우팅이 가능하다.

### NLB(Network Load Balancer)
- OSI 모델 Layer4에서 부하분산을 지원한다.
- TCP/UDP 트래픽을 로드밸런싱해서 내부 인스턴스에 전달한다.
- 내부로 들어온 트래픽을 처리하고, 내부의 인스턴스로 트래픽을 전송한다.
- IP주소 + 포트번호로 로드밸런서 스위칭이 일어난다.

## IAM OIDC(OpenID Connect) Identity Provider 생성
> [OpenID Connect Tokens](https://kubernetes.io/docs/reference/access-authn-authz/authentication/#openid-connect-tokens)
> 나중에 알아보자...

AWS Load Balancer 컨트롤러가 워커 노드 위에서 동작하기 때문에 IAM permission을 통해 AWS ALB/NLB 리소스에 접근 가능한 OIDC를 먼저 생성해야 한다.

### 1. **IAM OIDC(OpenID Connect) Identity Provider** 생성

```bash
eksctl utils associate-iam-oidc-provider 
--region ap-northeast-2 \
--cluster eks-demo \
--approve
```

```bash
2022-05-08 22:46:25 [ℹ]  eksctl version 0.95.0
2022-05-08 22:46:25 [ℹ]  using region ap-northeast-2
2022-05-08 22:46:26 [ℹ]  will create IAM Open ID Connect provider for cluster "eks-demo" in "ap-northeast-2"
2022-05-08 22:46:27 [✔]  created IAM Open ID Connect provider for cluster "eks-demo" in "ap-northeast-2"
```

### 생성한 IAM OIDC Identity Provider 확인하기 (IAM 콘솔)
[IAM 자격 증명 공급자 콘솔(Identity Providers in IAM Console)](https://console.aws.amazon.com/iam/home#/providers) 에서 클러스터에 대한 OIDC 자격증명 공급자를 확인할 수 있다. 

{{< figure src="/posts/images/docker-kubernetes/20220509002417.png#center">}}

### 2. AWS Load Balancer Controller에 부여할 IAM Policy 생성
IAM 정책은 AWS Load Balancer Controller가 사용자를 대신하여 AWS API를 호출하도록 허용한다.

#### AWS 로드밸런서 컨트롤러의 IAM 정책 다운로드
```bash
curl -o /Users/riley/environment/manifests/alb-ingress-controller/iam_policy.json  https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/main/docs/install/iam_policy.json
```
#### AWSLoadBalancerControllerIAMPolicy라는 IAM Policy 생성
```bash
aws iam create-policy \
--policy-name AWSLoadBalancerControllerIAMPolicy \
--policy-document file:///Users/riley/environment/manifests/alb-ingress-controller/iam_policy.json
```

### 3. AWS Load Balancer Controller를 위한 Service Account 생성
AWS Load Balancer Controller에 쿠버네티스 서비스 계정에 대한 IAM 역할을 연결한다. -
- `--attach-policy-arn=arn:aws:iam::$ACCOUNT_ID:role/$IAM_ROLE_NAME`
- 자세한 내용은 문서를 참고 : [IAM 역할을 서비스 계정에 연결](https://docs.aws.amazon.com/ko_kr/eks/latest/userguide/specify-service-account-role.html)

```bash
eksctl create iamserviceaccount \
--cluster eks-demo \
--namespace kube-system \
--name aws-load-balancer-controller \
--attach-policy-arn arn:aws:iam::$ACCOUNT_ID:policy/AWSLoadBalancerControllerIAMPolicy \
--override-existing-serviceaccounts \
--approve
```

`AWSLoadBalancerControllerIAMPolicy`의 IAM Role을 갖는 EKS ServiceAccount 생성하고 Ingress와 Service에 ALB와 NLB를 각각 생성하고 관리할 수 있게 되었다.












