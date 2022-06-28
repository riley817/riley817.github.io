# [AWS EKS] 3. EKS Cluster 생성하기


> _☁️  **Amazon EKS 웹 애플리케이션 구축하기** 워크샵을 실습한 내용입니다._\
> ☁️  AWS Workshop 링크 : https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/ko-KR/ 

**Amazon EKS 클러스터는 다양한 방식으로 배포할 수 있다.**
- [AWS 콘솔 창](https://console.aws.amazon.com/eks/home#/)으로 배포
- [AWS CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html) 혹은 [AWS CDK](https://docs.aws.amazon.com/cdk/api/v1/) 와 같은 IaC(Infrastructure as Code) 도구를 사용해 배포
- EKS의 공식 CLI인 [eksctl](https://eksctl.io/) 로 배포
- Terraform, Pulumi, Rancher 등으로 배포

## eksctl로 Cluster 생성하기
아무 옵션없이 `eksctl create cluster` 실행하면 default parameter로 클러스터가 배포된다. 그러나 `yaml` 파일로 작성한 구성 파일을 작성하여 배포하면 구성파일에 명시한 오브젝트들의 바라는 상태(desired state)를 쉽게 파악하고 관리할 수 있는 이점이 있다.

### 1. Cluster 구성파일 작성

#### ~/environment/eks-demo-cluster.yaml
```yaml
apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig

metadata:
  name: eks-demo # 생성할 EKS 클러스터명
  region: ap-northeast-2 # 클러스터를 생성할 리전
  version: "1.21"

vpc:
  cidr: "192.168.0.0/16" # 클러스터에서 사용할 VPC의 CIDR

managedNodeGroups:
  - name: node-group # 클러스터의 노드 그룹명
    instanceType: m5.large # 클러스터 워커 노드의 인스턴스 타입
    desiredCapacity: 3 # 클러스터 워커 노드의 갯수
    volumeSize: 10  # 클러스터 워커 노드의 EBS 용량 (단위: GiB)
    iam:
      withAddonPolicies:
        imageBuilder: true # Amazon ECR에 대한 권한 추가
        # albIngress: true  # albIngress에 대한 권한 추가
        cloudWatch: true # cloudWatch에 대한 권한 추가
        autoScaler: true # auto scaling에 대한 권한 추가

cloudWatch:
  clusterLogging:
    enableTypes: ["*"]
```

### 2. 명령어를 통해 클러스터를 배포한다.
- [eksctl 클러스터 생성 관리 설정 참고](https://eksctl.io/usage/creating-and-managing-clusters/)
```bash
eksctl create cluster -f eks-demo-cluster.yaml
```
클러스터가 완전히 배포되는데까지는 약 15~20분이 소요된다. [AWS CloudFormation 콘솔창](https://ap-northeast-2.console.aws.amazon.com/cloudformation/home)에서도 진행사항을 파악할 수 있다.

{{< figure src="/categories/images/devops/20220507221120.png#center">}}

{{< figure src="/categories/images/devops/20220506233659.png#center">}}

#### 생성한 클러스터 정보 확인
```bash
$ AWS_PROFILE=riley-admin eksctl get clusters 

2022-05-07 22:52:40 [ℹ]  eksctl version 0.95.0
2022-05-07 22:52:40 [ℹ]  using region ap-northeast-2
NAME		REGION		EKSCTL CREATED
eks-demo	ap-northeast-2	True
```

### 3. kubectl 인증 정보 설정

#### 컨텍스트 리스트 출력하기
```bash
$ kubectl config get-contexts

CURRENT   NAME               CLUSTER                                                         AUTHINFO                                                        NAMESPACE
*         eks-demo-cluster   eks-demo.ap-northeast-2.eksctl.io                               Administrator@eks-demo.ap-northeast-2.eksctl.io                 
```
#### kubectl 인증정보 alias 지정
```bash
$ kubectx eks-demo-cluster=Administrator@eks-demo.ap-northeast-2.eksctl.io

Context "Administrator@eks-demo.ap-northeast-2.eksctl.io" renamed to "eks-demo-cluster".
```
#### 컨텍스트 스위칭
```bash
# switch context
kubectl config use-context eks-demo-cluster

# kubectx로 switch context
kubectx eks-demo-cluster
```

#### 배포된 노드 확인
```bash
$ kubectx eks-demo-cluster
Switched to context "eks-demo-cluster".

$ kubectl get nodes 
NAME                                                STATUS   ROLES    AGE   VERSION
ip-192-168-22-34.ap-northeast-2.compute.internal    Ready    <none>   29m   v1.21.5-eks-9017834
ip-192-168-58-247.ap-northeast-2.compute.internal   Ready    <none>   29m   v1.21.5-eks-9017834
ip-192-168-81-71.ap-northeast-2.compute.internal    Ready    <none>   29m   v1.21.5-eks-9017834
```
