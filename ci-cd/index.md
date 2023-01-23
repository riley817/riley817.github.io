# [CI/CD 서버 구축하기] 1. 개념



## 요약
CI/CD는 애플리케이션 개발 단계를 자동화하여 애플리케이션을 보다 짧은 주기로 고객에게 제공하는 방법이다. 아래 세 가지 단계로 구분 할 수 있다.
- **지속적인 통합 `Continuous Integration`**
- **지속적인 서비스 제공 `Continuous Delivery`** 
- **지속적인 배포 `Continuous Deployment`**

{{<image src="/posts/images/devops/cicd.png#center" caption="[출처] https://www.redhat.com/ko/topics/devops/what-is-ci-cd">}}
> 💡  "CD"는 지속적인 서비스 제공(Continuous Delivery) 및/또는 지속적인 배포(Continuous Deployment)를 의미하며 이 두 용어는 상호 교환적으로 사용할 수 있다.

## 개념
### CI
다수의 개발자가 작성 및 수정한 코드가 지속적으로 통합/테스트(Continuous Integration) 되는 것을 의미한다. CI 작업 순서는 도구, 프로그래밍 언어, 프로젝트 등 기타 여러 요인에 따라 많이 다르지만 일반적으로는 다음과 같다.
- 코드를 레포지토리로 푸시한다.
- 정적 분석 
- 배포 전 테스트
- 테스트 환경에 패키징 및 배포
- 배포 후 테스트

{{< image src="/posts/images/devops/ci-workflow.png#center" caption="[출처] www.pepgotesting.com" >}}


### CD
CD는 지속적 제공 **Continuous Delivery**과 지속적 배포 **Continuous Deployment**를 두 가지를 모두 의미한다.

- **지속적 제공 Continuous Delivery** : CI 수행 후 프로세스에 유효한 코드를 레포지토리에 올리는 것을 자동화 한다. 프로덕션 레벨로 배포가 가능한 상태까지 준비해되는 것이 목표. (코드의 변경 사항 병합, 테스트 자동화, 코드 릴리스 자동화 포함)

- **지속적 배포 Continuous Deployment** : CI/CD 마지막 단계. 프로덕션 준비가 완료된 빌드를 코드 리포지토리에 자동으로 릴리즈 한다.

## CI/CD 도구들
CI/CD를 위한 도구들은 굉장히 많다. 본인 팀에 맞는 형태의 도구를 고르면 된다.

> 🐹  
> - 나는 계속 스타트업에서 일했기 때문에 무료로 구성할 수 있는 Jenkins 와 GitLab을 가장 많이 사용했다. docker를 통해 쉽게 구성이 가능하다.
> - Bamboo의 경우 Jira의 이슈 시스템과 연동하여 사용할 수 있어 프로젝트 관리 측면에서 매우 유용하다. (대신 프로젝트 인원이 많은 경우 가격이 사악...)
> - GitHub Action/Travis CI 는 따로 CI 서버를 구성하지 않아도 되기 때문에 내 블로그는 GitHub Action을 통해 빌드 및 배포하고 있다.


{{< figure src="/posts/images/devops/top-5-cicd-tool.png#center" caption="[출처] https://www.katalon.com/resources-center/blog/ci-cd-tools/" >}}


## 참고링크
- [RedHat 토픽 - CI/CD(지속적 통합/지속적 제공): 개념, 방법, 장점, 구현 과정](https://www.redhat.com/ko/topics/devops/what-is-ci-cd)
- [Continuous Integration: A "Typical" Process](https://developers.redhat.com/blog/2017/09/06/continuous-integration-a-typical-process?extIdCarryOver=true&sc_cid=701f2000001OH7EAAW)

