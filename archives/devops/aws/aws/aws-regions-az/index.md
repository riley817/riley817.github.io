# AWS Overview


# [Global Infrastructure](https://aws.amazon.com/ko/about-aws/global-infrastructure/)

{{< admonition type=tip title="Note" open=true >}}
- AWS Regions 
- AWS Availability Zones
- AWS Data Centers
- AWS Edge Locations / Points of Presence
{{< /admonition >}} 

## AWS Regions
- 데이터 센터의 집합 `cluster of data centers`
- 대부분의 AWS 서비스들은 특정 리전에 국한`region-scoped`되어 있다.
- 각 `Region`은 최소 두 개 이상의 개별 가용 영역`Availability Zones`로 구성

### AWS Region을 선택하는 방법
- **Compliance with data governance and legal requirements** : 데이터의 법률 준수 및 요구사항
- **Proximity to customers:** : 고객과의 접근성. 대기시간 단축
- **Available services within a Region** : 특정 리전에서만 가능한 AWS 서비인지 확인
- **Pricing** : 비용은 지역에 따라 가격이 다르다.

## AWS Availability Zones
- 각각의 리전은 많은 가용영역으로 구성 (보통은 3개 최소 2개 최대 6개로 구성)
    - ap-southeast-2a
    - ap-southeast-2b
    - ap-southeast-2c
- 다른 가용영역(AZ)의 장애로부터 격리되어 있다.
- 고대역폭, 초저지연 네트워킹으로 서로 연결되어 리전을 형성. 

## AWS Points of Presence (Edge Locations)
- `Amazon CloudFront`에서는 현재 410개 넘는 상호 접속위치(POP)(엣지 로케이션 400개 이상, 리전별 중간 티어 캐시 13개)로 구성된 글로벌 네트워크를 사용
- 최종 사용자에게 짧은 지연 시간과 높은 처리량의 컨텐츠를 제공할 수 있다.

{{< image src="/posts/images/aws/Cloudfront-Map_9.24_2x.2eeac6e52bf404816c6d0aac3edbeb7b6b87fdaa.png" caption="[출처] https://aws.amazon.com/ko/cloudfront/features/">}}


