# 7. AWS EC2 Instance Storage


## EBS Volume
- **EBS(Elastic Block Store) Volume**은 볼륨 인스턴스가 실행되는 동안 인스턴스에 연결할 수 있는 네트워크 드라이브이다.
- 인스턴스가 종료된 후에도 데이터를 유지할 수 있다.
- **한 번에 하나의 인스턴스에만 마운트 할 수 있다.** (CCP 레벨)
    - CCP 레벨 : 하나의 EBS는 하나의 EC2 인스턴스에만 마운트 가능
    - Associate Level: 일부 EBS 다중 연결
- 특정 가용 영역에바 바인딩 되어 있다.
- `네트워크 USB 스틱`과 유사하다.
- 프리티어는 한 달 30GB의 일반 용도 (SSD) 또는 마그네틱 유형으로 제공

### EBS Volume 특징
- **네트워크 드라이브이다.(즉, 물리적인 드라이브가 아님)**
    - 네트워크를 사용하여 인스턴스를 통신. 약간의 지연이 있을 수 있다.
    - EC2 인스턴스에 분리하여 다른 인스턴스에 빠르게 연결 가능
- **AZ에 묶여있다.**
    - `us-east-1a`의 EBS 볼륨을 `us-east-1b`에 연결할 수 없다.
    - 볼륨을 이동하려면 먼저 스냅샷을 생성해야 한다.
- **프로비저닝 된 용량(GB 및 IOPS 단위)**
    - 프로비저닝 된 모든 용량에 대해 청구
    - 시간이 지남에 따라 드라이브 용량을 늘릴 수 있다.

### Delete on Termination attribute
- EC2 인스턴스가 종료될 때 EBS 동작을 제어할 수 있다.
    - 기본적으로 루트 EBS 볼륨은 삭제된다. `Delete on Termination : checked(default)`
    - 기본적으로 연결된 다른 EBS 볼륨은 삭제되지 않는다.`Delete on Termination : unchecked`
- AWS 콘솔 및 CLI에서 설정 가능하다.
- **인스턴스 종료 시 루트 볼륨을 유지하고 싶으면 체크를 해제한다.**

## EBS 스냅샷
- 특정 시점에 EBS 볼륨의 백업(Snapshot)을 만들 수 있다.
- 스냅샷을 위해 볼륨을 분리할 필요는 없지만 권장
- AZ 또는 리전 간 스냅샷 복사 가능

### EBS 스냅샷 기능
|**스냅샷 기능**|**설명**|
|---|---|
|**EBS Snapshot Archive**|· 75% 더 저렴한 비용으로 `archive tier`로 스냅샷을 전송한다.<br/>· 아카이브를 복원하는데 24 ~ 72 시간 소요|
|**Recycle Bin for EBS Snapshot**|· 실수로 삭제한 후 복구할 수 있도록 삭제된 스냅샷을 보존하는 규칙을 설정<br/>· 보존 기간은 1일 ~ 1년

## AMI 
- **Amazon Machine Image**
- AMI는 EC2 인스턴스를 커스터마이징 한 것
    - 소프트웨어, 구성, 운영체제, 모니터링 등 모든 소프트웨어가 사전 패키지화되어 있어 부팅 및 구성 시간 단축
- AMI는 **특정 리전**만을 위해 구축되며 여러 리전간 복사 가능

    {{<admonition tip "다음 위치에서 EC2 인스턴스 시작">}} 
- **A Public AMI** : AWS 제공
- **Your own AMI** : 사용자가 직접 만들고 관리
- **AWS Marketplace AMI** : 다른 사용자가 AMI를 만듬 (잠재적으로 판매)
    {{</admonition>}}

### AMI 프로세스 (EC2 인스턴스)
- EC2 인스턴스를 시작하고 커스터마이징 한다.
- 인스턴스를 중단한다. (데이터 무결성을 위해)
- AMI 빌드 한다. - EBS 스냅샷도 생성
- 다른 인스턴스에서 AMI를 실행

## EC2 Instance Store
- EBS 볼륨은 좋지만 **제한적인** 성능을 갖는 네트워크 드라이브임
- **고성능의 하드웨어 디스크가 필요할 경우 EC2 Instance Store를 사용** 
- I/O 성능 향상
- EC2 Instance Store가 중지되면 스토리지가 손실 (ephemeral)
- buffer / cache / scratch data / temporary content 에 적합
- 하드웨어 장애 시 데이터 손실의 위험
- 백업 및 복제는 사용자의 책임

## EBS 볼륨 유형
- EBS 볼륨은 6가지 유형으로 제공
|**EBS 볼륨 유형**|**특징**|
|-------------------|---|
| **gp2/gp3 (SSD)**|다양한 워크로드에 대하 가격과 성능을 맞추는 범용 SSD 볼륨|
| **io1/io2 (SSD)**|mission-critical, 짧은 대기 시간 또는 높은 처리량 워크로드를 위한 최고성능 SSD 볼륨|
| **st1(HDD)**|자주 액세스하는 throughput-intensive 워크로드를 위해 설계된 저비용 HDD 볼륨|
| **sc1(HDD)**|액세스 빈도가 낮은 워크로드에 맞게 설계된 최저 비용 HDD 볼륨| 

- EBS 볼륨은 `Size | Throughput | IOPS(I/O Ops Per Sec)` 으로 구분할 수 있다.
- **오직 `gp2/gp3`,`io1/io2` 만 부팅 볼륨으로 사용 가능**

### General Purpose SSD

<table>
    <thead>
        <tr>
            <th></th>
            <th colspan="2" align="center" style="text-align: center;">General Purpose SSD</th>
        </tr>
    </thead>
    <tr>
        <td><b>Volume type</b></td>
        <td><b><code class="code">gp3</code></b></td>
        <td><b><code class="code">gp2</code></b></td>
    </tr>
    <tr>
        <td><b>Durability</b></td>
        <td colspan="2">99.8% - 99.9% durability (0.1% - 0.2% annual failure rate)</td>
    </tr>
    <tr>
        <td><b>Use cases</b></td>
        <td colspan="2">
            <div>
                <ul type="disc">
                    <li><p>대기시간이 짧은 대화형 애플리케이션</p></li>
                    <li><p>개발 또는 테스트 환경</p></li>
                    <li><p>가상 데스크톱</p></li>
                    <li><p>Medium-sized single-instance databases</p></li>
                    <li><p>Boot volumes</p></li>
                </ul>
            </div>
        </td>
    </tr>
    <tr>
        <td><b>Volume size</b></td>
        <td colspan="2">1 GiB - 16 TiB </td>
    </tr>
    <tr>
        <td><b>Max IOPS per volume</b> (16 KiB I/O) **</td>
        <td colspan="2">16,000</td>
    </tr>
    <tr>
        <td><b>Max throughput per volume</b> **</td>
        <td>1,000 MiB/s</td>
        <td>250 MiB/s *</td>
    </tr>
    <tr>
        <td><b>Amazon EBS Multi-attach</b></td>
        <td colspan="2">Not supported</td>
    </tr>
    <tr>
        <td><b>Boot volume</b></td>
        <td colspan="2">Supported</td>
    </tr>
</table>

### Provisioned IOPS(PIOPS) SSD
- 지속적인 IOPS 성능을 제공하는 비즈니스 애플리케이션
- 또는 16,000 이상의 IOPS가 필요한 애플리케이션
- **데이터베이스 워크로드에 적합** (스토리지 성능 및 일관성 민감한)
- Multi-attach EBS 지원
<table>
    <thead>
        <tr>
            <th></th>
            <th colspan="3" align="center" style="text-align: center;">Provisioned IOPS SSD</th>
        </tr>
    </thead>
    <tr>
        <td><b>Volume type</b></td>
        <td><code class="code">io2</code> Block Express *</td>
        <td><code class="code">io2</code></td>
        <td><code class="code">io1</code></td>
    </tr>
    <tr>
        <td><b>Durability</b></td>
        <td>99.999% durability (0.001% annual failure rate)</td>
        <td>99.999% durability (0.001% annual failure rate)</td>
        <td>99.8% - 99.9% durability (0.1% - 0.2% annual failure rate)</td>
    </tr>
    <tr>
        <td><b>Use cases</b></td>
        <td>
            <p>Workloads that require:</p>
            <div>
                <ul type="disc">
                    <li><p>Sub-millisecond latency</p></li>
                    <li><p>지속적인 IOPS 성능</p></li>
                    <li>
                        <p>More than 64,000 IOPS or 1,000 MiB/s of throughput</p>
                    </li>
                </ul>
            </div>
        </td>
        <td colspan="2">
            <div>
                <ul type="disc">
                    <li><p>Workloads that require sustained IOPS performance or more than 16,000 IOPS</p></li>
                    <li><p>I/O-intensive database workloads</p></li>
                </ul>
            </div>
        </td>
    </tr>
    <tr>
        <td><b>Volume size</b></td>
        <td>4 GiB - 64 TiB</td>
        <td colspan="2">4 GiB - 16 TiB </td>
    </tr>
    <tr>
        <td><b>Max IOPS per volume</b> (16 KiB I/O)</td>
        <td>256,000</td>
        <td colspan="2">64,000 †</td>
    </tr>
    <tr>
        <td><b>Max throughput per volume</b></td>
        <td>4,000 MiB/s</td>
        <td colspan="2">1,000 MiB/s †</td>
    </tr>
    <tr>
        <td><b>Amazon EBS Multi-attach</b></td>
        <td colspan="3"><b>Supported</b></td>
    </tr>
    <tr>
        <td><b>Boot volume</b></td>
        <td colspan="3">Supported</td>
    </tr>
</table>



### Hard Disk Drives(HDD)
- 부팅 볼륨으로 사용할 수 없음

<table>
    <thead>
        <tr>
            <th></th>
            <th>Throughput Optimized HDD</th>
            <th>Cold HDD</th>
        </tr>
    </thead>
    <tr>
        <td><b>Volume type</b></td>
        <td><code class="code">st1</code></td>
        <td><code class="code">sc1</code></td>
    </tr>
    <tr>
        <td><b>Durability</b></td>
        <td>99.8% - 99.9% durability (0.1% - 0.2% annual failure rate)</td>
        <td>99.8% - 99.9% durability (0.1% - 0.2% annual failure rate)</td>
    </tr>
    <tr>
        <td><b>Use cases</b></td>
        <td>
            <div>
                <ul type="disc">
                    <li><p>Big data</p></li>
                    <li><p>Data warehouses</p></li>
                    <li><p>Log processing</p></li>
                </ul>
            </div>
        </td>
        <td>
            <div>
                <ul type="disc">
                    <li><p>액세스 빈도가 낮은 데이터를 처리하기 위한 throughput-oriented 스토리지</p></li>
                    <li><p>가장 낮은 스토리지 비용이 중요한 시나리오</p></li>
                </ul>
            </div>
        </td>
    </tr>
    <tr>
        <td><b>Volume size</b></td>
        <td>125 GiB - 16 TiB</td>
        <td>125 GiB - 16 TiB </td>
    </tr>
    <tr>
        <td><b>Max IOPS per volume</b> (1 MiB I/O)</td>
        <td>500</td>
        <td>250</td>
    </tr>
    <tr>
        <td><b>Max throughput per volume</b></td>
        <td>500 MiB/s</td>
        <td>250 MiB/s</td>
    </tr>
    <tr>
        <td><b>Amazon EBS Multi-attach</b></td>
        <td>Not supported</td>
        <td>Not supported</td>
    </tr>
    <tr>
        <td><b>Boot volume</b></td>
        <td>Not supported</td>
        <td>Not supported</td>
    </tr>
</table>

## EBS Multi-Attach – io1/io2 family
<center><image src="/posts/images/aws/ebs-multi-attach.png" width="300px">
</center>

- 동일한 AZ의 여러 EC2 인스턴스에 동일한 EBS 볼륨 연결
- 각 인스턴스에는 볼륨에 대한 전체 읽기 및 쓰기 권한 있음
- **파일 시스템은 반드시  cluster-aware 사용해야 함(XFS, EX4는 사용할 수 없음)**

    {{<admonition tip "Use case">}}
- 클러스터링 된 리눅스 애플리케이션에서 애플리케이션 가용성 향상 (example: Teradata)
- 애플리케이션의 동시 쓰기 작업을 관리해야 하는 경우
    {{</admonition>}}

## EBS Encryption
{{<admonition success "암호화 된 EBS 볼륨을 생성하면 다음이 표시된다.">}}
- 저장된 데이터는 볼륨 내부에서 암호화 된다.
- 인스턴스와 볼륨 사이를 이동하는 이동중인 모든 데이터가 암호화 된다.
- 모든 스냅샷이 암호화 된다.
- 스냅샷에서 생성된 모든 볼륨이 암호화 된다.
{{</admonition>}}

- 암호화 및 복호화가 알아서 처리된다. (사용자는 할일이 없음)
- 암호화는 대기 시간에 미치는 영향을 최소화 한다.
- EBS 암호화는 KMS의 키를 활용한다. (AES-256)
- 암호화되지 않은 스냅샷은 복사하면 암호화가 가능
- 암호화된 불륨의 스냅샷은 암호화되어 있다.

### 암호화되지 않은 EBS 볼륨을 암호화하기
1. EBS 볼륨의 스냅샷을 생성한다.
2. EBS 스냅샷을 암호화한다. (using copy)
3. 생성한 스냅샷에서 새로운 EBS 볼륨을 만든다. (만든 볼륨은 암호화)
4. 암호화된 볼륨을 원래 인스턴스에 연결

## Amazon EFS – Elastic File System
- 여러 EC2에 마운트 할 수 있는 관리형 NFS(`network file system`)
- EFS는 다중 AZ의 EC2 인스턴스와 함께 작용한다.
- 고 가용성, 확장성, expensive(3x gp2), 사용당 지불
- 사용 사례 : 컨텐츠 관리, Web 서비스 제공, 데이터 쉐어링, 워드 프레스 등
- `NFSv4.1 protocol` 사용
- 보안 그룹을 통해 EFS 액세스를 컨트롤
- **Linux 기반 AMI 호환 (윈도우즈 아님)**
- KMS를 사용한 at-rest 암호화
- 표준 파일 API가 있는 **Linux POSIX 파일 시스템**
- 파일 시스템 자동 확장, 사용량별 지불, 용량 계획 없음

### EFS 성능 및 스토리지 Classes
| Performance & Storage Classes|                                           |
|-------------------------------------------------|-------------------------------------------|
| **EFS Scale**                                   | · 1,000대의 NFS 동시 클라이언트 : 10 GB+/s 처리량<br/>· 페타바이트 규모의 네트워크 파일 시스템으로 자동 확장 |
| **Performance mode (set at EFS creation time)** | · 범용 목적 (기본) : 지연 시간에 민감한 사용 사례 (웹서버, CMS 등)<br/>· MAX I/O : 지연 시간, 처리량, 병렬 처리(빅데이터, 미디어 프로세싱등)|
| **Throughput mode**                             | · Bursting(1TB=50MiB/s + 최대 100MiB/s 버스트)<br/> · 프로비저닝 : 스토리지 크기에 상관없이 처리량 설정(ex: 1 GiB/s for 1 TB 스토리지) |

### EFS - Storage Classes
#### Storage Tiers (라이프 사이클 관리 기능 - N일 후 파일 이동)
- **Standard** : 자주 액세스 하는 파일용
- **Infrequent access (EFS-IA)** : 파일 검색 비용, 저장 비용 절감 라이프 사이클 정책을 사용하여 EFS-IA 사용

#### 가용성 및 내구성
- Regional : Multi-AZ, 프로덕션 환경에 최적
- One Zone : 개발 환경에 적합한 단일 환경, 기본적으로 백업 지원 IA(EFS One Zone-IA)와의 호환성
- 90% 이상의 비용 절감

## EBS vs EFS 
|**EBS**|**EFS**|
|---|---|
|• 한 번에 하나의 인스턴스만 연결<br/>• AZ 수준에서 묶임<br/>• **gp2** : 디스크 크기가 증가하면 IO가 증가한다.<br/>• **io1** : 독립적으로 IO 증가 가능|• AZ 전체에 수백 개의 인스턴스 마운트 가능<br/>• EFS는 웹사이트의 파일을 공유할 때 사용할 수 있다.(워드 프레스)<br/>• 오직 POSIX만 지원<br/>• EFS는 EBS 보다 높은 가격대를 가지고 있다.<br/>• EFS-IA를 활용하여 비용 절감 가능|
|• AZ에서 EBS 볼륨을 마이그레이션하려면<br/>&nbsp;&nbsp;&nbsp;&nbsp;• 스냅샷을 생성한다.<br/>&nbsp;&nbsp;&nbsp;&nbsp;• 다른 가용영역으로 스냅샷을 복구한다.<br/>&nbsp;&nbsp;&nbsp;&nbsp;• EBS 백업은 IO를 사용하므로 응용프로그램이 많은 트래픽을 처리하는 동안 실행해서는 안된다.|   |
|• 루트 EBS는 EC2 인스턴스가 종료되면 인스턴스 볼륨이 기본적으로 종료된다. (비활성화 가능)||

