# Amazon RDS, Aurora & ElastiCache


## AWS RDS
RDS는 관계형 데이터 베이스 서비스 `Relational Database Service` 를 나타내며 SQL 쿼리 언어를 사용하는 데이터베이스를 위한 서비스이다.

{{<admonition success "RDS에서 제공하는 데이터베이스">}}
- Postgres
- MySQL
- MariaDB
- Oracle
- Microsoft SQL Server
- Aurora - AWS Proprietary database
{{</admonition>}}

### AWS RDS 사용시 이점
- RDS는 다음과 같은 서비스를 관리한다.
  - 프로비저닝 및 OS 패치 자동화
  - 지속적 백업과 특정 timestamp 기준 복구 `(Point Time Restore)`
  - 대시보드 모니터링
  - 읽기 성능 향상을 위한 읽기 전용 복제본
  - **재해복구 `DZ(Disaster Recovery)`를 위한 Multi AZ 셋업 가능**
  - 업그레이드를 위한 유지보수
  - 확장성 (vertical and horizontal)  
  - EBS 스토리지 백업 `(gp2 or io1)`
  > 그러나 인스턴스에 SSH를 따로 가질 수는 없다. 

## RDS Backup
RDS에서는 자동으로 백업이 실행된다.

### RDS 자동백업
- 매일 수행되는 데이터베이스 전체에 대한 백업(유지 관리 기간 중)
- RDS에 의해 트랜잭션 로그가 5분 마다 백업된다. → 원하는 시점으로 복원할 수 있는 기능 (가장 오래된 백업의 5분 전으로)     
- 보존기간은 7일 (최대 35일까지 늘릴 수 있다.)

### DB 스냅샷
- 사용자에 의해 수동으로 트리거 된다.
- 원하는 기간 동안 백업을 보존할 수 있다.

## RDS Storage Auto Scaling
- RDS DB 인스턴스의 스토리지를 동적으로 늘릴 수 있도록 지원한다.
- RDS가 사용 가능한 데이터베이스 스토리지가 부족함을 감지하면 자동으로 확장
- 데이터베이스의 스토리지를 수동 확장 방지
- **Maximum Storage Threshold** `최대 스토리지 임계값(최대 제한)` 을 설정해야 한다.
- **예측할 수 없는 워크로드 `unpredictable workloads` 가 있는 애플리케이션에 적합**
- 모든 RDS 데이터베이스 엔진을 지원 (MariaDB, MySQL, PostgreSQL, SQL Server, Oracle)

  {{<admonition type=info title="자동으로 스토리지가 변경되는 경우">}}
- 사용가능한 스토리지가 할당된 스토리지의 10% 미만임
- 최소 5분 이상 지속됨
- 마지막 수정 후 6시간이 지남
  {{</admonition>}}

## 읽기 확장성을 위한 RDS 읽기 복제본(Read Replicas)
<center>
  <image src="/posts/images/aws/rds-read-replicas.jpg" width="500px" caption="">
</center>

- 최대 5개 까지 가능
- AZ 내, AZ 교차, 리전 교차
- 읽기 복제본의 복제는 **ASYNC**이며 읽기 결과는 일관된다.
- 읽기 복제본은 자체 DB로 승격가능
- 애플리케이션에서는 읽기 복제본을 활용하기 위해서는 연결 문자열을 업데이트 해야 한다.

### RDS 읽기 복제본 사용 사례
<center>
  <image src="/posts/images/aws/rds-read-replicas-usecase.jpg" width="500px" caption="">
</center>

- 애플리케이션 분석을 실행하기 위한 애플리케이션을 실행하려는 경우
- 읽기 복제본을 생성 후 새로운 워크로드에서 실행하려는 경우
- 프로덕션의 응용프로그램은 영향받지 않음
- 읽기 복제본은 **SELECT 유형의 문에만 사용** (INSERT, UPDATE, DELETE는 사용불가능)

### 네트워크 비용
- AZ간 데이터가 이동할 때 네트워크 비용이 발생한다.
- **RDS 읽기 복제본를 사용하면 같은 리전이면 복제본의 수수료를 지불하지 않는다.**

## RDS Multi AZ`(Disaster Recovery)`
<center>
  <image src="/posts/images/aws/rds-multi-az-dr.png" width="500px" caption="">
</center>

- **동기식`SYNC`** 으로 복제
- 하나의 DNS 이름을 가지고 자동으로 `standby` 데이베이스에 복구
- 가용성 증가 
- 가용영역의 네트워크가 손실되는 경우 스토리지를 복구한다.
- 애플리케이션에 수동 개입이 없음
- Multi-AZ 복제본은 비용이 발생하지 않음
- 읽기 복제본은 재해 복구를 위해 다중 AZ로 설정된다.

### 단일 AZ에서 다중 AZ로
- zero downtime operation (DB 중지가 불필요하다)
- 데이터베이스의 `modify` 버튼만 클릭하면 된다.

  {{<admonition type=info title="내부적으로 발생하는 절차">}}
- 스냅샷이 생성
- 스냅샷에서 새로운 데이터베이스가 AZ에 복원
- 두 데이터베이스간 동기화 설정
  {{</admonition>}}

## RDS 보안 - Encryption
### At rest 암호화
- AWS KMS의 AES-256 암호화를 사용하여 마스터 및 읽기 전용 암호화 가능
- 암호화 실행 시간을 정의 해야 한다.
- **마스터 데이터베이스를 암호화하지 않으면 읽기 전용 데이터베이스도 암호화 할 수 없다**
- Oracle 및 SQL Server에서 TED(Transparent Data Encryption) 사용 가능하다. -> 데이터 암호화의 대안 제안

### In-flight 암호화
- SSL 인증서를 통한 데이터 전송중 암호화
- 데이터베이스를 연결 할 때 신뢰할 수 있는 인증서로 SSL 옵션을 제공하여 SSL을 제공할 수 있다.
{{<admonition type=success title="모든 클라이언트가 SSL 사용하게 하려면">}}
**PostgreSQL**<br/>
- AWS RDS Console (Parameter Groups)
- `rds.force_ssl=1`

**MySQL**<br/>
- 데이터베이스 내부에서 실행
- `GRANT USAGE ON *.* TO 'mysqluser'@'%' REQUIRE SSL;`
{{</admonition>}}

### 암호화 옵션

#### RDS 백업 암호화
- 암호화되지 않은 RDS 데이터베이스의 스냅샷은 암호화되지 않음.
- 암호화 된 RDS 데이터베이스만 스냅샷이 암호화 됨
- 스냅샷을 암호화 된 스냅샷으로 복사할 수 있음

#### 암호화되지 않은 RDS 데이터베이스 암호화
1. 암호화되지 않은 데이터베이스의 스냅샷을 생성한다.
2. 생성한 스냅샷을 복사하고 복사한 스냅샷에 대한 암호화를 활성화 한다.
3. 암호화 된 스냅샷에서 데이터베이스를 복원한다.
4. 응용프로그램에서 새 데이터베이스로 마이그레이션 후 이전 데이터베이스는 삭제한다.

## RDS 보안 : Network & IAM
### 네트워크 보안
- RDS는 일반적으로 public 서브넷이 아닌 private 서브넷에 구축된다.
- RDS 보안은 보안 그룹을 활용하여 작동하며, RDS와 통신할 수 있는 IP와 보안 그룹을 제어한다.

### 접근 제어 관리
- IAM 정책을 통해(RDS API를 통해) AWS RDS를 관리할 수 있는 사용자를 제어할 수 있다.
- 기존 사용자 및 암호화를 사용하여 데이터베이스에 로그인 할 수 있다.
- IAM 기반 인증을 사용하여 RDS MySQL 및 PostgreSQL에 로그인할 수 있다.

### RDS IAM Authentication
<center>
  <image src="/posts/images/aws/rds-auth-iam.png" width="300">
</center>

- IAM 데이터베이스 인증은 **MySQL 및 PostgreSQL**과 함께 작동한다.
- 비밀번호는 필요하지 않고 IAM & RDS API 호출을 통해 획득한 인증 토큰만 필요하다.
- 인증토큰은 15분의 lifetime을 갖는다.

#### 장점
- 네트워크의 in/out은 SSL을 통해 암호화된다.
- IAM에서 DB 대신에 사용자를 관리
- IAM 역할 및 EC2 인스턴스 프로파일을 활용하여 손쉽게 통합 가능하다.

## RDS 보안 요약

### Encryption at rest
1. DB 인스턴스를 처음 생성할 때만 수행
2. 암호화 되지 않은 DB -> 스냅샷 생성 -> 스냅샷을 암호화된 것으로 복사 -> 스냅샷에서 DB 생성

### 사용자가 해야할 일
- 데이터베이스 보안그룹의 ports/ip/security group 인바운드 규칙 확인한다.
- 데이터베이스 내 모든 사용자 생성 및 권한을 관리 (IAM을 통한)
- public 액세스가 없는 데이터베이스를 생성 
- 파라미터 그룹과 데이터베이스가 SSL 연결만 허용하도록 구성하여 암호화되는지 확인

### AWS 하는일
- SSH 액세스가 발생하지 않게 함
- 사용자가 수동으로 DB 패치를 하지 않아도 됨
- 사용자가 수동으로 OS 패치를 하지 않아도 됨
- 사용자가 기본 인스턴스를 확인하지 않아도 됨

## Amazon Aurora
- Aurora는 AWS의 독점 기술 (그러나 오픈소스는 아니다.)
- Postgres 그리고 MySQL은 모두 Aurora DB로 지원된다. 
    - 드라이버가 Postgres, mysql 데이터베이스인 것처럼 작동하는 것을 의미
- Aurora는 AWS 클라우드에 최적화되어 있으며, RDS의 MySQL 보다 5배, RDS의 Postgres 보다 3배 향상된 성능을 제공한다.
- 스토리지는 자동으로 10GB, 최대 128 TB 까지 증가
- MySQL을 5개의 복제본만 가질 수 있는 반면 Aurora는 15개의 복제본을 가질 수 있다. 
- 복제 프로세스가 더 빠르다 
- 즉각적인 장애 복구. native high availability (HA)
- RDS에 비해 비용이 20% 높지만 규모가 클 경우 더 효율적

### AWS Aurora의 고가용성과 읽기 확장성
{{<admonition success "6개의 데이터 복제본이 3개의 AZ에 걸쳐 저장">}}
- 쓰기의 경우 4개의 복제본만 있으면 된다.
- 읽기 시 3개의 복제본만 있으면 된다.
- peer-to-peer 복제를 통해 자가 복구한다.
- 수백개의 볼륨을 걸쳐 스토리지를 스트리핑
{{</admonition>}}
- 쓰기 가능 마스터 하나이다.
- 30초 이내에 자동으로 마스터 장애 복구 실행
- 마스터 + 최대 15개의 읽기 전용 제공
- **리전 간 읽기 전용 복제 지원**

### Aurora DB Cluster
<center>
  <image src="/posts/images/aws/db-cluster.jpg" width="1000px">
</center>

- **Writer Endpoint** : 항상 마스터를 가리킴
- **Reader Endpoint** : 오토 스케일링 된 읽기 전용 복제본 로드 밸런싱

## Aurora의 기능
- 장애복구 자동화 
- 백업과 복원
- 격리 및 보안
- 업계 규정 준수
- 푸시 버튼 스케일링
- 자동화 된 무중단 패치
- 향상된 모니터링
- 정기 유지보수
- `Backtrack` : 백업을 사용하지 않고 언제든지 데이터를 복원

## Aurora 보안
- RDS와 비슷한 수준의 보안 (같은 엔진을 사용)
- KMS 사용한 at rest 암호화
- 자동화된 백업, 스냅샷 그리고 암호화된 복제본
- SSL을 사용한 전송중 암호화 (MySQL, Postgres와 같은)
- **IAM 토큰을 사용한 인증이 가능 (RDS와 같은 방식)**
- 사용자는 보안그룹을 통해 인스턴스를 보호해야 한다.
- SSH에 접근할 수 없다.

## Aurora Replicas 

### Aurora Auto Scaling
<center>
  <image src="/posts/images/aws/aurora-replicas-auto-scaling.jpg" width="700px">
</center>

{{<admonition tip "Aurora auto scaling">}}
**Aurora 인스턴스 3개가 있다고 가정**
- Writer Endpoint : 1개
- Reader Endpoint : 2개

**많은 읽기 요청으로 CPU사용량 증가**
- Aurora 복제본이 추가되고 자동으로 Reader Endpoint가 새 복제본을 포함하도록 확장
- CPU 사용량을 낮추기 위해 분산된 방식으로 새로운 복제본이 트래픽 수신
{{</admonition>}}

### Aurora Custom Endpoints
- 사용자 지정 Endpoints로 Aurora 인스턴스의 하위 집합을 정의
- 예시) 특정 복제본에 대한 분석 쿼리를 실행할 때
- 일반적으로 사용자 정의 Endpoints를 정의한 후 Reader Endpoint는 정의하지 않는다.

<center>
  <image src="/posts/images/aws/aurora-custom-endpoints.jpg" caption="">
</center>

{{<admonition tip "Aurora custom endpoints">}}
**Aurora 두 종류 복제본이 있다고 가정**
- 복제본 db.r3.large : 2개 
- 복제본 db.r5.xlarge : 2개 - 용량이 더 큼

**특정 읽기전용 데이터베이스에 분석 쿼리 실행**
- 용량이 더 큰 인스턴스 서브셋을 사용자 endpoint로 정의
{{</admonition>}}

### Aurora Serverless
- 자동화 된 데이터베이스 인스턴스 및 실제 사용량을 기반으로 한 오토 스케일링 제공
- 간헐적이거나 예측할 수 없는 워크로드에 적합
- 용량 측정이 필요 없다
- 초당 비용 지불. 비용측면에서는 효과적

<center>
  <image src="/posts/images/aws/aurora-serverless.jpg" caption="">
</center>

{{<admonition tip "Aurora Serverless">}}
- Aurora에서 관리하는 Proxy Fleet과 통신
- 백엔드에서는 서버리스 방식으로 워크로드를 기반으로 한 Aurora 인스턴스 생성
{{</admonition>}}

### Multi-Master
- 쓰기 전용 마스터에 즉각적인 장애 조치가 필요한 경우.(Writer 노드에 고가용성이 필요한 경우)
- 모든 노드가 읽기와 쓰기 작업을 진행
- 쓰기 노드가 다운될경우 읽기 본제본을 새로운 마스터로 승격

### 글로벌 Aurora
#### Aurora 리전 간 읽기 복제
- 재해복구에 유용
- 간단하게 구현
#### Aurora Global Database(추천)
- 하나의 읽기/쓰기 기본 리전 `Primary Region`
- 5개까지 보조 리전 설정 가능 : 읽기 전용이며, 보조 리전당 랙은 1초 미만
- 리전당 최대 16개의 읽기 전용 복제본을 가질 수 있다.
- 대기 시간 단축에 도움
- RTO가 있으므로 복구 시간 목표는 1분 미만

### Aurora Machine Learning
- SQL을 통해 ML 기반의 예측을 애플리케이션에 추가
- Aurora와 AWS ML 서비스를 쉽고, 최적화, 안전하게 통합할 수 있다.
- 지원되는 서비스
 - Amazon SageMaker (use with any ML model)
 - Amazon Comprehend (for sentiment analysis)
- ML 경험이 없어도 된다.
- 사용사례 
    - 사기탐지, 광고 타게팅, 감정분석, 제품 추천

# Amazon ElastiCache
- 캐시는 높은 성능과 낮은 지연 시간을 가진 in-memory 데이터베이스를 의미한다.
- RDS 동일한 방식으로 관계형 데이터베이스를 관리
- Redis와 Memcached와 같은 캐시 기술 관리
- 읽기 집약적인 워크로드 부하를 줄일 수 있다.
- 애플리케이션에서 상태를 관리하지 않아도 된다.
- OS 운영, 패치, 최적화, 설치, 구성, 모니터링, 장애복구, 백업등의 기능을 AWS에서 주관한다.
- **ElasticCache를 사용하면 애플리케이션 코드를 향상시킬 수 있다.**

## ElastiCache Solution Architecture 
### DB Cache
- 애플리케이션은 ElasticCache에서 쿼리 할 수 없는 경우 RDS에서 가져와 ElasticCache에 저장한다.
- RDS에 부하 완화 지원
- 캐시에는 최신 데이터만 사용 할 수 있도록 무효화 전략 필요

### 사용자 세션 저장
- 사용자의 세션 데이터를 ElasticCache에 저장
- 애플리케이션은 ElasticCache에서 직접 사용자의 세션캐시를 검색 
- 다른 인스턴스로 접근시에도 사용자 재로그인이 필요 없음


##  Redis vs Memcached

### Redis
- 자동화 된 재해 복구 + 다중 AZ 
- 읽기 복제본을 통한 읽기 확장 및 고 가용성 
- AOP 지속성을 이용한 내구성
- 백업 및 복원 기능

### Memcached
- 데이터를 파티셔닝 한 다중 노드  (sharding)
- 가용성이 낮음
- 복제 X. 지속성 X 
- 백업 및 복원 기능 없음
- 다중 스레드 설계
- 분산 캐시


##  Cache Security
- IAM authentication 지원하지 않음
- ElasticCache는 AWS API 수준 보안에서만 사용

### Redis Auth
- 레디스 클러스터를 생성할 때 비밀번호나 토큰을 설정할 수 있다.
- 이는 security group에 대한 추가적인 수준의 보안
- SSL 전송 중 암호화 지원

### Memcached
- SASL 기반 인증 지원

## 데이터 로드 패턴
### Lazy Loading
- 모든 데이터를 읽은 후 캐시한 데이터를 로딩 할 수 있다.

### Write Through
- DB에 데이터를 쓸 때 캐시 데이터 추가 및 업데이트 요청

### Session Store
- 임시 세션 데이터를 캐시에 저장 (TTL 기능 사용)

### Redis Use Case
- 게이밍 리더보드 계산
- **Redis Sorted set**은 고유성과 순서 모두를 보장한다.
- 새 요소가 추가 될 때마다 실시간으로 순위 책정






