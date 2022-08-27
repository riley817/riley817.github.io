# AWS EC2 Instance Storage


## EBS Volume
- **EBS(Elastic Block Store) Volume**은 볼륨 인스턴스가 실행되는 동안 인스턴스에 연결할 수 있는 네트워크 드라이브이다.
- 인스턴스가 종료된 후에도 데이터를 유지할 수 있다.
- **한 번에 하나의 인스턴스에만 마운트 할 수 있다.** (CCP 레벨)
    - CCP 레벨 : 하나의 EBS는 하나의 EC2 인스턴스에만 마운트 가능
    - Associate Level: 일부 EBS 다중 연결
- `네트워크 USB 스틱`
- 프리티어는 한 달 30GB의 일반 용도 (SSD) 또는 마그네틱 유형으로 제공

### EBS Volume 특징
- **네트워크 드라이브이다.(즉, 물리적인 드라이브가 아님)**
    - 네트워크를 사용하여 인스턴스를 통신. 약간의 지연이 있을 수 있다.
    - EC2 인스턴스에 분리하여 다른 인스턴스에 빠르게 연결 가능
- **AZ에 잠겨있다**
    - `us-east-1a`의 EBS 볼륨을 `us-east-1b`에 연결할 수 없다.
    - 볼륨을 이동하려면 먼저 스냅샷을 생성해야 한다.
- **프로비저닝 된 용량(GB 및 IOPS 단위)**
    - 프로비저닝 된 모든 용량에 대해 청구
    - 시간이 지남에 따라 드라이브 용량을 늘릴 수 있다.

### Delete on Termination attribute
- EC2 인스턴스가 종료될 때 EBS 동작을 제어할 수 있다.
    - 기본적으로 루트 EBS 볼륨은 삭제된다. `Delete on Termination : checked(default)`
    - 기본적으로 다른 연결된 EBS 볼륨은 삭제되지 않는다.`Delete on Termination : unchecked`
- AWS 콘솔 및 CLI에서 설정 가능하다.
- **인스턴스 종료 시 루트 볼륨을 유지하고 싶으면 체크를 해제한다.**

### EBS 스냅샷
- 특정 시점에 EBS 볼륨의 백업(Snapshot)을 만들 수 있다.
- 스냅샷을 위해 볼륨을 분리할 필요는 없지만 권장
- AZ 또는 리전 간 스냅샷 복사 가능

#### EBS 스냅샷 기능
##### EBS 스냅샷 Archive
- 75% 더 저렴한 `archive tier`로 스냅샷을 이동
- 아카이브를 복원하는데 24시간 ~ 72시간 이내 소요됨

##### Recycle Bin for EBS Snapshots
- 실수로 삭제한 후 복구할 수 있도록 삭제된 스냅샷을 보존하는 규칙을 설정
- 보존 기간 지정(1 day ~ 1 year)

## AMI 
- **Amazon Machine Image**
- AMI는 EC2 인스턴스를 커스터마이징 한 것
    - 소프트웨어, 구성, 운영체제, 모니터링 등 모든 소프트웨어가 사전 패키지화되어 있어 부팅 및 구성 시간 단축
- AMI **특정 리전**을 위해 구축 (리전간 복사 가능)

{{<admonition type=tip >}} 
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
|   EBS volume type   |   |
|-------------------|---|
| **gp2/gp3 (SSD)**   |다양한 워크로드에 대하 가격과 성능을 맞추는 범용 SSD 볼륨|
| **io1/io2 (SSD)**   |mission-critical, 짧은 대기 시간 또는 높은 처리량 워크로드를 위한 최고성능 SSD 볼륨|
| **st1(HDD)**        |자주 액세스하는 throughput-intensive 워크로드를 위해 설계된 저비용 HDD 볼륨|
| **sc1(HDD)**        |액세스 빈도가 낮은 워크로드에 맞게 설계된 최저 비용 HDD 볼륨| 

- EBS 볼륨은 `Size | Throughput | IOPS(I/O Ops Per Sec)` 으로 구분
- `gp2/gp3` 및 `io1/io2`만 부팅 볼륨으로 사용 가능

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
                    <li><p>Sustained IOPS performance</p></li>
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
        <td colspan="3">Supported</td>
    </tr>
    <tr>
        <td><b>Boot volume</b></td>
        <td colspan="3">Supported</td>
    </tr>
</table>



|              volume type             |   |
|------------------------------------|---|
| **io1/io2 (4GiB~16TiB)**             |· **Max PIOPS:** Nitro EC2 인스턴스의 경우 64,000 개, 기타 인스턴스 32,000 개<br/>· 스토리지 크기와 관계없이 PIOPS 증가 가능<br/>· io2 내구성 및 GiB당 IOPS 향상(io1과 동일한 가격)     |
| **io2 Block Express (4Gib ~ 64TiB)** |· Sub-millisecond latency<br/>· Max PIOPS: 256,000, IOPS: GiB 비율 1,000:1 |

### Hard Disk Drives(HDD)
- 부팅 볼륨으로 사용할 수 없음
- 125 GiB ~ 16 TiB

| volume type                        |                                           |
|------------------------------------|-------------------------------------------|
| **Throughput Optimized HDD (st1)** |· Big Data, Data Warehouses, Log Processing<br/>· **Max throughput 500 MiB/s - max IOPS 500** |
| **Cold HDD (sc1)**                 |· 자주 액세스하지 않는 데이터<br/>· 최저 비용이 중요한 경우<br/>· **Max throughput 250 MiB/s – max IOPS 250** |

