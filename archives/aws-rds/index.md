# 

# 9. **RDS, Aurora & ElastiCache**

---

## AWS RDS

RDS는 관계형 데이터 베이스 서비스 `Relational Database Service` 를 나타내며 SQL 쿼리 언어를 사용하는 데이터베이스를 위한 서비스이다.

### RDS에서 제공하는 데이터베이스

- Postgres
- MySQL
- MariaDB
- Oracle
- Microsoft SQL Server
- Aurora - AWS Proprietary database

### EC2에 DB를 운용하지 않고 RDS 사용했을 때 이점

AWS RDS 데이터서비스 뿐만 아니라 여러 기타 서비스를 포함하고 있다. 

- 프로비저닝 및 OS 패치 자동화
- 지속적 백업과 특정 timestamp 기준 복구 `Point Time Restore`
- 대시보드 모니터링
- 읽기 성능 향상을 위한 읽기 전용 레플리카
- 재해복구 `DZ(Disaster Recovery)` 를 위한 Multi AZ 셋업 가능
- 업그레이드를 위한 유지보수
- 확장성 (vertical and horizontal)
- EBS 스토리지 백업 (`gp2 or io1`)

> ****[⚠️](https://emojipedia.org/warning/)** 그러나 인스턴스에 SSH를 따로 가질 수는 없다.
> 

## RDS Backup

RDS에서는 자동으로 백업이 실행된다.

### RDS 자동백업

- 매일 수행되는 데이터베이스 전체에 대한 백업
