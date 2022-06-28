# Oracle Scheduler DBMS_SCHEDULER 개념 및 사용법


# Oracle Scheduler - DBMS_SCHEDULER
- 오라클에서 정해진 시간에 반복된 작업을 수행하기 위해 `DBMS_JOB` 패키지를 활용하였지만 Oracle 10g 부터는 좀 더 확장된 기능을 가진 `DBMS_SCHEDULER` 패키지를 제공하고 있다.
- DBMS_JOB과의 가장 큰 차이점은 DBMS_JOB에서는 불가능하던 외부스크립트 (프로시저나, 함수 이외에도 OS에서 생성된 각종 유틸, 프로그램 까지) 실행이 가능하다.

### Class 생성
- 클래스를 지정하지 않게 되면 `DEFAULT_JOB_CLASS` 에 포함되는데 이 경우 기본 로깅 정책을 따른다.
- 기본로깅 정책은 `DBMS_SCHEDULER.LOGGING_RUNS` 이며 로깅레벨을 별도로 설정하거나 다른 스케쥴잡과 그룹화하여 관리하고 싶을 경우 Class를 생성한다.

```sql
/**
 * CLASS 생성 예제
 * 로그정책 : DBMS_SCHEDULER.LOGGING_FAILED_RUNS
 * 30일만 로그 보관
 * */
BEGIN
    DBMS_SCHEDULER.CREATE_JOB_CLASS (
        job_class_name => 'EXAMPLE_JOB_CLASS',
        resource_consumer_group => 'DEFAULT_CONSUMER_GROUP',
        logging_level => DBMS_SCHEDULER.LOGGING_FAILED_RUNS,
        log_history => 30,
        comments => 'JOB CLASS 설명 입니다.'
    );
END;
```


### Scheduler Job 생성하고 활성화 하기
* 로그정책 : DBMS_SCHEDULER.LOGGING_FAILED_RUNS. 30일만 로그를 보관한다.
* 실행 주기는 30초 마다로 설정

```sql
-- Job 생성
BEGIN 
	DBMS_SCHEDULER.CREATE_JOB(
		JOB_NAME => 'test_job',
	    JOB_TYPE => 'STORED_PROCEDURE',
	    JOB_ACTION => 'test_procedure',
	    START_DATE => SYSDATE,
	    REPEAT_INTERVAL => 'FREQ=SECONDLY;INTERVAL=30',
	    COMMENTS => '테스트 잡'
	);
END;

-- Job 활성화
BEGIN
    DBMS_SCHEDULER.ENABLE('JBT_SEARCH_TRUCK_FLTER');
END;
```

### Oracle Scheduler Job에 로그 레벨 세팅하기
| Logging Level | Description |
|---|---|
|DBMS_SCHEDULER.LOGGING_OFF |No logging is performed.|
|DBMS_SCHEDULER.LOGGING_FAILED_RUNS |A log entry is made only if the job fails.|
|DBMS_SCHEDULER.LOGGING_RUNS|A log entry is made each time the job is run.|
|DBMS_SCHEDULER.LOGGING_FULL|A log entry is made every time the job runs and for every operation performed on a job, including create, enable/disable, update (with SET_ATTRIBUTE), stop, and drop.|

```sql
BEGIN
  DBMS_SCHEDULER.set_attribute (
    name      => 'test_log_job',
    attribute => 'logging_level',
    value     => DBMS_SCHEDULER.logging_off);
END;

-- 스케쥴 잡 로그 테이블
SELECT * FROM USER_SCHEDULER_JOB_LOG;
```

### 기타 설정 메서드
```sql
-- Job 활성화
BEGIN
	DBMS_SCHEDULER.ENABLE('JBT_SEARCH_TRUCK_FLTER');
END;

-- Job 비활성화
BEGIN
	DBMS_SCHEDULER.DISABLE('JBT_SEARCH_TRUCK_FLTER');
END;

-- Job 지우기
BEGIN DBMS_SCHEDULER.DROP_JOB (job_name => 'JBT_SEARCH_TRUCK_FLTER'); END;
```
