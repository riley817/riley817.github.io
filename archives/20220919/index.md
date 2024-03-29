# 

질문1) 회사는 Java 프레임워크로 작성된 새로운 온라인 거래 웹 애플리케이션이 있으며 Application Load Balancer 뒤의 EC2 인스턴스로 서비스 됩니다.
이러한 EC2 인스턴스에서 상태 확인을 수행하도록 로드 밸런서를 구성했습니다. 
이러한 EC2 인스턴스 중 하나가 상태 확인에 실패하면 어떻게 됩니까?

> `Application Load Balancer`가 상태 확인에 실패한 인스턴스로 트래픽 전송을 중지합니다.

- HealthCheck 정상 인스턴스 상태 : InService
- HealthCheck 비정상적인 인스턴스 상태 : OutOfService

질문2) 회사가 자체 관리 메세지 지향 미들웨어 시스템에서 Amazon SQS로 메세지 대기열을 마이그레이션하고 있습니다. 회사 개발팀은 SQS 사용 비용을 최소화하고자 합니다. 주어진 사용 사례에 대해 다음 중 어떤 옵션을 추천하겠습니까?

> SQS 롱 폴링을 사용하여 Amazon SQS 대기열에서 메세지를 검색합니다.


질문3) 당신은 AWS 글로벌 서비스를 구축할 수 있도록 클라우드 아키텍처를 구축하고 있다. 이 서비서는 업무 중단을 방지하기 위해 연중무휴 24x7로 제공되어야 하며 전체 AWS 리전의 운영 중단을 처리할 수 있을 만큼 충분히 탄력적이어야 합니다. 이 요구사항을 충족하기 위해 여러 AWS 리전에 AWS 리소스를 배포했습니다. Route 53을 사용하여 가능한 항상 모든 리소스를 사용할 수 있도록 설정해야 합니다. 리소스를 사용할 수 없게 되면 Route 53은 리소스가 비 정상임을 감지하고 쿼리에 응답할 때 리소스 포함을 중지해야 한다.

> 가중치 라우팅 정책을 사용하여 액티브-액티브 장애 조치를 구성

**Active-Active 장애조치**
- 리소스를 사용할 수 없을 때 Route53이 비정상 상태임을 판별하여 쿼리에 응답할 때 그 리소스를 포함하지 않음

**Active-Passive 장애조치**
- 쿼리에 응답할 때 Route53은 정상적인 1차 리소스만 포함. 모든 1차 리소스가 비정상이면 Route 53은 DNS 쿼리에 응답할 때 정상적인 2차 리소스만 포함

**Route 53 라우팅 정책**
- 단순 라우팅 정책 : 도메인에 대해 특정 기능을 수행하는 하나의 리소스만 있는 경우 
- 장애 조치 라우팅 정책 : Active-Passive 장애 조치를 구성하는 경우
- 지리 위치 라우팅 정책 : 사용자의 위치에 기반하여 트래픽 라우팅하는 경우
- 지리 근접 라우팅 정책 : 리소스의 위치를 기반으로 트래픽을 라우팅하고 필요에 따라 한 위치에서 다른 위치의 리소스로 트래픽을 보내려는 경우
- 지연 시간 라우팅 정책 : 여러 AWS 리전에 리소스가 있고 최상의 지연 시간을 제공하는 리전으로 트래픽을 라우팅
- 다중 응답 라우팅 정책 : Route 53이 DNS 쿼리에 무작위로 선택된 최대 8개의 정상 레코드로 응답하게 하려는 경우
- 가중치 기반 라우팅 정책 : 사용자가 지정한는 비율에 따라 여러 리소스로 트래픽을 라우팅하려는 경우 사용

질문5) 웹 애플리케이션은 몇 가지 Lambda 함수와 함께 Auto Scaling Group 내의 EC2 인스턴스 집합에서 호스팅 된다. 매주 애플리케이션 업데이트를 릴리즈 할 때마다 일부 리소스가 제대로 업데이트 되지 않는 불일치가 있다. 다운 타임을 최소화하면서 리소스를 함께 그룹화하고 새로운 코드 버전을 그룹간에 일관되게 배포 할 수 있는 방법이 필요

> CodeDeploy에서 배포 그룹을 사용하여 일관된 방식으로 코드 배포를 자동화 합니다.

질문6) 회사에서 이번에 개발한 애플리케이션의 데이터베이스에는 DynamoDB를 사용하고 있으며 인증에는 Cognito를 사용하기로 선택했다. 이 애플리케이션은 기밀 금융 거래가 포함되어있으므로 사용자 이름과 비밀번호에만 의존하지 않는 두 번째 인증 방법이 추가해야 한다.
AWS에서 어떻게 구현할 수 있는가?

> Cognito의 사용자 풀에 다단계 인증(MFA)을 추가하여 사용자의 신원을 보호한다.


질문9) 온라인 거래 플랫폼은 기본 Auto Scaling 종료 정책이 있고 인스턴스 보호가 구성되지 않은 온디맨드 EC2 인스턴스의 Auto Scaling 그룹에서 호스팅 된다. 이 시스템은 플랫폼에 고가용성 및 내결함성을 제공하기 위해 Application Load Balancer가 전면에 있는 한국 리전 (ap-northeast-2)의 3개의 가용 영역에 배포된다. ap-northeast-2a, ap-northeast-2b 및 ab-northeast-2c 가용 영역에는 각각 10, 8, 7개의 인스턴스가 실행된다. 들어오는 트래픽 수가 적어 스케일 인 작업이 트리거 되었다.
이 시나리오에서 어떤 인스턴스를 먼저 종료할지 결정하기 위해 Auto Scaling 그룹은 다음 중 어떤 작업을 수행하는가?

> - 다음 청구 시간에 가장 가까운 인스턴스를 선택한다
> - 가장 오래된 시작 구성이 있는 인스턴스를 선택한다.
> - 인스턴스 수가 가장 많은 가용 영역(ap-northeast-2a)를 선택한다.

질문15) 회사는 보고서를 Amazon S3 버킷에 저장하고 있다. IT 감사를 준수하기 위해 솔루션 아키텍트는 버킷에 추가된 모든 새 객체와 제거된 객체를 모두 추적해야 한다. 아키텍트는 이러한 이벤트에 대한 알림을 사후 처리를 위한 대기열과 운영팀에 알릴 Amazon SNS 주제에 게시하도록 Amazon S3를 구성해야 한다.
