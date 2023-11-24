# 데이터 분석을 위한 SQL 레시피 - 5장 사용자를 파악하기 위한 데이터 추출


> 🗄️ 데이터분석을 위한 SQL 레시피 책을 읽고 정리 / 요약 한 내용입니다.

{{< admonition type=success title="시계열에 따른 사용자 전체의 상태 변화 찾기" open=true >}}
- 사용자는 서비스 사용 시작일부터 시간이 지나면서 '충성도 높은 사용자', '사용을 중지', '휴면' 등으로 상태가 변화
  - **사용자가 계속해서 사용(리피트)**
  - **사용자가 사용을 중단(탈퇴/휴면)**
- 서비스 제공자는
  - 사용자가 어느 정도에서 계속 사용하고 있는지 파악하고 목표와의 괴리를 어떻게 좁힐 것 인지 방안을 검토
  - 휴면 사용자를 어떻게 다시 사용하게 만들지도 생각
- 완전 탈퇴의 경우 복귀는 어려우나 휴면 사용자는 메일 매거진/CM/광고 등을 활용하여 다시 사용하게 유도
{{< /admonition >}}

## 샘플 데이터
- **SNS 사용자 마스터 테이블** 
- **SNS 액션 로그 테이블**
{{< admonition type=abstract title="사용자 마스터 테이블과 액션 로그 테이블" open=false >}}
```sql
DROP TABLE IF EXISTS mst_users;
CREATE TABLE mst_users(
    user_id         varchar(255)
  , sex             varchar(255)
  , birth_date      varchar(255)
  , register_date   varchar(255)
  , register_device varchar(255)
  , withdraw_date   varchar(255)
);

INSERT INTO mst_users
VALUES
    ('U001', 'M', '1977-06-17', '2016-10-01', 'pc' , NULL        )
  , ('U002', 'F', '1953-06-12', '2016-10-01', 'sp' , '2016-10-10')
  , ('U003', 'M', '1965-01-06', '2016-10-01', 'pc' , NULL        )
  , ('U004', 'F', '1954-05-21', '2016-10-05', 'pc' , NULL        )
  , ('U005', 'M', '1987-11-23', '2016-10-05', 'sp' , NULL        )
  , ('U006', 'F', '1950-01-21', '2016-10-10', 'pc' , '2016-10-10')
  , ('U007', 'F', '1950-07-18', '2016-10-10', 'app', NULL        )
  , ('U008', 'F', '2006-12-09', '2016-10-10', 'sp' , NULL        )
  , ('U009', 'M', '2004-10-23', '2016-10-15', 'pc' , NULL        )
  , ('U010', 'F', '1987-03-18', '2016-10-16', 'pc' , NULL        )
  , ('U011', 'F', '1993-10-21', '2016-10-18', 'pc' , NULL        )
  , ('U012', 'M', '1993-12-22', '2016-10-18', 'app', NULL        )
  , ('U013', 'M', '1988-02-09', '2016-10-20', 'app', NULL        )
  , ('U014', 'F', '1994-04-07', '2016-10-25', 'sp' , NULL        )
  , ('U015', 'F', '1994-03-01', '2016-11-01', 'app', NULL        )
  , ('U016', 'F', '1991-09-02', '2016-11-01', 'pc' , NULL        )
  , ('U017', 'F', '1972-05-21', '2016-11-01', 'app', NULL        )
  , ('U018', 'M', '2009-10-12', '2016-11-01', 'app', NULL        )
  , ('U019', 'M', '1957-05-18', '2016-11-01', 'pc' , NULL        )
  , ('U020', 'F', '1954-04-17', '2016-11-03', 'app', NULL        )
  , ('U021', 'M', '2002-08-14', '2016-11-03', 'sp' , NULL        )
  , ('U022', 'M', '1979-12-09', '2016-11-03', 'app', NULL        )
  , ('U023', 'M', '1992-01-12', '2016-11-04', 'sp' , NULL        )
  , ('U024', 'F', '1962-10-16', '2016-11-05', 'app', NULL        )
  , ('U025', 'F', '1958-06-26', '2016-11-05', 'app', NULL        )
  , ('U026', 'M', '1969-02-21', '2016-11-10', 'sp' , NULL        )
  , ('U027', 'F', '2001-07-10', '2016-11-10', 'pc' , NULL        )
  , ('U028', 'M', '1976-05-26', '2016-11-15', 'app', NULL        )
  , ('U029', 'M', '1964-04-06', '2016-11-28', 'pc' , NULL        )
  , ('U030', 'M', '1959-10-07', '2016-11-28', 'sp' , NULL        )
;

```
{{< /admonition >}}

## 12.1 등록 수의 추이와 경향보기
---
- **사용자 등록이 필요한 서비스에서 등록 수는 중요한 지표**
- 등록자가 감소 경향이면 서비스 활성화 하기 어려움
- 등로가가 증가 경향이면 서비스에서 이탈할지 아닐지 분석하여 서비스 활성화와 연결

### 날짜별 등록 수의 추이
- 사용자 수를 집계할 때는 사용자를 유일하게 식별 가능한 ID로 중복을 제거하여 집계
```sql
SELECT
    register_date
   , COUNT(DISTINCT user_id) AS register_count
FROM sample_data.mst_users
  GROUP BY register_date
  ORDER BY register_date
;
```

### 월별 등록 수 추이
- 월별로 등록 수와 전월비(전월대비)를 집계하기
- 월별로 집계하므로 날짜 자료형에서 연과월 데이터만 추출 : `year_month`
- 전월 등록 수와 비율을 집계시에는 `LAG` 윈도우 함수 사용

#### 매달 등록 수와 전월비를 계산하는 쿼리
```sql
WITH mst_users_with_year_month AS (
    SELECT
        *,
        -- PostgreSQL, Hive, Redshift, SparkSQL : substring
        -- ,substring(register_date, 1, 7) AS year_month
        -- BigQuery, PostgreSQL, Hive, Redshift, SparkSQL : substr
        substr (register_date, 1, 7) AS year_month
    FROM sample_data.mst_users
)
SELECT
    year_month
  , COUNT(DISTINCT user_id) AS register_count

  -- SparkSQL 의 경우
  -- , LAG(COUNT(DISTINCT user_id)) OVER(ORDER BY year_month ROWS BETWEEN 1 PRECEDING AND 1 PRECEDING) AS last_month_count
  , LAG(COUNT(DISTINCT user_id)) OVER(ORDER BY year_month)  as last_month_count

  -- SparkSQL 의 경우
  --, 1.0 * COUNT(DISTINCT user_id) / LAG(COUNT(DISTINCT user_id)) OVER(ORDER BY year_month ROWS BETWEEN 1 PRECEDING AND 1 PRECEDING) as month_over_month_ratio
  , 1.0 * COUNT(DISTINCT user_id) / LAG(COUNT(DISTINCT user_id)) OVER(ORDER BY year_month) AS month_over_month_ratio
FROM mst_users_with_year_month GROUP BY year_month
```

#### 매달 등록 수와 등록 디바이스별 추이
- 레코드 내 저장된 정보를 사용해 여러 속성의 내역을 집계하기
- 등록 디바이스 `mst_users`의 `device` 컬럼을 사용하여 등록수 내역을 집계해보기
- 등록과 동시에 추가되는 정보가 있다면 다양한 컬럼으로 대체하여 집계가 가능

```sql
WITH mst_users_with_year_month AS (
    SELECT
        *,
        -- PostgreSQL, Hive, Redshift, SparkSQL : substring
        -- ,substring(register_date, 1, 7) AS year_month
        -- BigQuery, PostgreSQL, Hive, Redshift, SparkSQL : substr
        substr (register_date, 1, 7) AS year_month
    FROM sample_data.mst_users
)
SELECT
    year_month
  , COUNT(DISTINCT user_id) AS register_count
  , COUNT(DISTINCT CASE WHEN register_device = 'pc' THEN user_id END) AS register_pc
  , COUNT(DISTINCT CASE WHEN register_device = 'sp' THEN user_id END) AS register_sp
  , COUNT(DISTINCT CASE WHEN register_device = 'app' THEN user_id END) AS register_app
FROM mst_users_with_year_month 
GROUP BY year_month

```

#### 💡 Tip
{{< admonition type=tip title="사용자 마스터 테이블과 액션 로그 테이블" open=true >}}
- 등록한 디바이스에 따라 사용자 행동이 달라질 수 있음
- 추가로 멀티 디바이스 사용자가 존재하며 이러한 사용자 존재도 파악해두면 도움이 됨.
{{< /admonition >}}

## 12.2 지속률과 정착률 산출하기
---
등록 시점을 기준으로 일정한 기간 동안 사용자가 지속해서 사용하고 있는지 조사할 때 **지속률**과 **정착률**을 사용하면 경향을 쉽게 파악 가능

### 지속률과 정착률의 정의

#### 지속률
**등록일을 기준으로 이후 지정일 동안 사용자가 서비스를 얼마나 이용했는지 나타내는 지표**

1. 6월12일에 등록한 사용자가 다음날에도 서비스를 사용했다면 1일 사용자 2일후에도 사용하고 있다면 2일 지속자
2. 매일 서비스를 사용하지 않아도 판정 날짜에 사용했다면 지속자로 취급
3. `지속률 = <사용자 수> / <등록 수>`
{{<figure src="/posts/images/sql-recipe/continuation-rate.png#center">}}

#### 정착률
**등록일을 기준으로 이후 지정한 7일 동안 사용자가 서비스를 사용했는지 나타내는 지표**
- 정착은 지속과 다르게 7일이라는 기간 내 한번이라도 서비스를 사용하면 정착자로 다룬다.
- 7일 정착률은 **등록 후** 1일 부터 7일까지의 정착률을 기준으로 산출
  - ex) 등록일이 10/3일 경우 14일 정착률 판정기간은 : 10/11 ~ 10/17
- **7일 동안 서비스를 사용한 날짜가 하루든 3일이든 1로 취급**
- `정착률 = <사용자 수> / <등록 수>`

#### 지속률과 정착률 사용 구분하기
|**지표**|**용도**|
|:-:|:-|
|**지속률**   |사용자가 매일 사용했으면 하는 서비스<br/> 예시) 뉴스 사이트, 소셜 게임, SNS 등 |
|**정착률**   |사용자에게 어떤 목적이 생겼을 때 사용했으면 하는 서비스<br/>예시) EC 사이트, 리뷰 사이트, Q&A 사이트, 사진 투고 사이트 등|

### 지속률과 관계있는 리포트

#### 날짜별 n일 지속률 추이
- 등록일을 기준으로 `다음날(1일) 지속률`을 집계한다.
- **지정한 날짜에 등록한 사용자 중에서 다음날에도 서비스를 사용한 사람의 비율**
- 단순하게 사용자 수를 세고 나누어도 되나 
  - **지정한 날짜 다음에 사용한 사용자에게 1, 아닌경우 0으로 플래그를 붙이고 `AVG` 함수를 적용해 평균을 구하는 방법이 더 간단**
- 다음날 지속률을 집계하려면 다음날 로그 데이터가 모두 쌓여있어야 함
  - 로그 집계 기간 중 가장 최신날짜를 추출하고 이러한 최신 일자를 넘는 기간의 지속률은 NULL로 출력하도록 하여 해결

##### 로그 최근 일자와 사용자별 등록일의 다음날을 계산하는 쿼리
```sql
WITH action_log_with_mst_users AS (
    SELECT
        u.user_id
      , u.register_date

      -- 액션 날짜와 로그 전체의 최신 날짜를 날짜 자료형으로 변환하기
      -- , CAST(a.stamp AS date) AS action_date
      -- , MAX(CAST(a.stamp AS date)) OVER() AS latest_date
      -- BigQuery : 한번 타임스탬프 자료형으로 변환하고 날짜 자료형으로 변환
      , date(timestamp(a.stamp))                AS action_date
      , MAX(date(timestamp(a.stamp))) OVER ()   AS latest_date

      -- 등록일 다음날 계산하기
      -- PostgreSQL : , CAST(u.register_date::date + '1 day'::interval AS date) AS next_day_1
      -- Redshift   : , dateadd(day, 1, u.register_date::date) AS next_day_1
      -- BigQuery
      , date_add(CAST(u.register_date AS date), interval 1 day) AS next_day_1
      -- Redshift   : , dateadd(day, 1, u.register_date::date) AS next_day_1
      -- Hive, SparkSQL : , date_add(CAST(u.register_date AS date) , 1) AS next_day_1
    FROM sample_data.mst_users AS u
    LEFT OUTER JOIN sample_data.action_log AS a ON u.user_id = a.user_id
)
SELECT * FROM action_log_with_mst_users
ORDER BY register_date;
```

##### 사용자의 액션 플래그를 계산하는 쿼리
```sql
WITH action_log_with_mst_users AS (
    SELECT
        u.user_id
      , u.register_date
      , date(timestamp(a.stamp))                AS action_date
      , MAX(date(timestamp(a.stamp))) OVER ()   AS latest_date
      , date_add(CAST(u.register_date AS date), interval 1 day) AS next_day_1
    FROM sample_data.mst_users AS u
    LEFT OUTER JOIN sample_data.action_log AS a ON u.user_id = a.user_id
)
, user_action_flag AS (
    SELECT
        user_id
      , register_date
      -- 4. 등록일 다음날에 액션을 했는지 안 했는지를 플래그로 나타내기
      , SIGN(
        -- 3. 사용자 별로 등록일 다음날에 한 액션의 합 구하기
            SUM(
                    -- 1. 등록일 다음날이 로그의 최신 날짜 이전인지 확인
                    CASE WHEN next_day_1 <= latest_date THEN
                        -- 2. 등록일 다음날의 날짜에 액션을 했다면 1, 아니면 0
                        CASE WHEN next_day_1 = action_date THEN 1 ELSE 0 END
                    END
            )
        ) AS next_1_day_action
    FROM action_log_with_mst_users
    GROUP BY user_id, register_date
)
SELECT * FROM user_action_flag
ORDER BY register_date
;
```
- 등록일이 로그 최신날짜 이후 인 사용자는 다음날의 액션 플래그는 NULL

##### 다음날 지속률을 계산하는 쿼리
- 사용자 액션 플래그를 0, 1로 표현했다면 그래프 값에 100.0을 곱해 `AVG`로 평균을 구해 퍼센트 단위로 나타낸다.
```sql
WITH action_log_with_mst_users AS (
    SELECT
        u.user_id
      , u.register_date
      , date(timestamp(a.stamp))                AS action_date
      , MAX(date(timestamp(a.stamp))) OVER ()   AS latest_date
      , date_add(CAST(u.register_date AS date), interval 1 day) AS next_day_1
    FROM sample_data.mst_users AS u
    LEFT OUTER JOIN sample_data.action_log AS a ON u.user_id = a.user_id
)
, user_action_flag AS (
    SELECT
        user_id
      , register_date
      -- 4. 등록일 다음날에 액션을 했는지 안 했는지를 플래그로 나타내기
      , SIGN(
        -- 3. 사용자 별로 등록일 다음날에 한 액션의 합 구하기
            SUM(
                    -- 1. 등록일 다음날이 로그의 최신 날짜 이전인지 확인
                    CASE WHEN next_day_1 <= latest_date THEN
                        -- 2. 등록일 다음날의 날짜에 액션을 했다면 1, 아니면 0
                        CASE WHEN next_day_1 = action_date THEN 1 ELSE 0 END
                    END
            )
        ) AS next_1_day_action
    FROM action_log_with_mst_users
    GROUP BY user_id, register_date
)
SELECT 
    register_date
  , AVG(100.0 * next_1_day_action) AS repeat_rate_1_day
FROM user_action_flag
ORDER BY register_date;
```

##### 지속률 지표를 관리하는 마스터 테이블을 작성하는 쿼리
- n번째 이후의 지속률은 일시 테이블을 사용해 각각의 지표를 세로기반으로 표현할 수 있게 한다
```sql
WITH repeat_interval AS (
    -- PostgreSQL : VALUES 구문으로 일시 테이블 생성 가능
    -- Hive, Redshift, BigQuery, SparkSQL : UNION ALL 등올 대
    SELECT '01 day repeat' AS index_name, 1 AS interval_date
    UNION ALL SELECT '02 day repeat' AS index_name, 2 AS interval_date
    UNION ALL SELECT '03 day repeat' AS index_name, 3 AS interval_date
    UNION ALL SELECT '04 day repeat' AS index_name, 4 AS interval_date
    UNION ALL SELECT '05 day repeat' AS index_name, 5 AS interval_date
    UNION ALL SELECT '06 day repeat' AS index_name, 6 AS interval_date
    UNION ALL SELECT '07 day repeat' AS index_name, 7 AS interval_date
) SELECT * FROM repeat_interval ORDER BY index_name;
```

##### ✅ 지속률을 세로 기반으로 집계하는 쿼리
- 출력 결과를 보면 n일 지속률을 계산하기 위해 필요한 판정 기간의 로그가 존재하지 않는 경우 지표가 NULL로 출력되는 것을 알 수 있다.
```sql
WITH repeat_interval AS (
    -- PostgreSQL : VALUES 구문으로 일시 테이블 생성 가능
    -- Hive, Redshift, BigQuery, SparkSQL : UNION ALL 등올 대
    SELECT '01 day repeat' AS index_name, 1 AS interval_date
    UNION ALL SELECT '02 day repeat' AS index_name, 2 AS interval_date
    UNION ALL SELECT '03 day repeat' AS index_name, 3 AS interval_date
    UNION ALL SELECT '04 day repeat' AS index_name, 4 AS interval_date
    UNION ALL SELECT '05 day repeat' AS index_name, 5 AS interval_date
    UNION ALL SELECT '06 day repeat' AS index_name, 6 AS interval_date
    UNION ALL SELECT '07 day repeat' AS index_name, 7 AS interval_date
)
, action_log_with_index_date AS (
    SELECT
        u.user_id
      , u.register_date
      -- 액션 날짜와 로그 전체 최신 날짜를 날짜 형식으로 변환
      -- , CAST(a.stamp AS date)                AS action_date
      -- , MAX(CAST(a.stamp AS date)) OVER()    AS latest_date
      -- BigQuery : 한 번 타임스탬프 형식으로 변환 후 날짜 자료형으로 변환하기
      , date(timestamp(a.stamp))                AS action_date
      , MAX(date(timestamp(a.stamp))) OVER ()   AS latest_date

      -- 등록일로부터 n일 후 날짜 계산하기
      , r.index_name
      -- BigQuery
      , date_add(CAST(u.register_date AS date), interval r.interval_date day) as index_date
      -- Hive, SparkSQL : , date_add(CAST(u.register_date AS date), r.interval_date) as index_date
    FROM sample_data.mst_users AS u
    LEFT OUTER JOIN sample_data.action_log AS a ON u.user_id = a.user_id
    CROSS JOIN repeat_interval AS r
)
, user_action_flag AS (

    SELECT
        user_id
      , register_date
      , index_name
        -- 4. 등록일로부터 n일 후에 액션을 했는지 플래그로 나타내기
      , SIGN(
            -- 3. 사용자 별로 n일 후 한 액션의 합 구하기
            SUM(
                    -- 1. 등록일로부터 n일 후가 로그의 최신날짜 이전확인
                    CASE WHEN index_date <= latest_date THEN
                        -- 2. 등록일로부터 n일 후의 날짜에 액션을 했다면 1 아니면 0 지정
                        CASE WHEN index_date = action_date THEN 1 ELSE 0 END
                    END
            )
        ) AS index_date_action
    FROM action_log_with_index_date
    GROUP BY user_id, register_date, index_name, index_date
)
SELECT
    register_date
  , index_name
  , AVG(100.0 * index_date_action) AS repeat_rate
FROM user_action_flag
GROUP BY register_date, index_name
ORDER BY register_date, index_name
;
```

### 정착률 관련 리포트

#### 매일의 n일 정착률 추이
- 7일 정착률이 극단적으로 낮은 경우에는 정착률이 아니라 `다음날 지속률 ~ 7일 지속률`을 확인해서 문제를 검토해야한다.

##### 정착률 지표를 관리하는 마스터 테이블을 작성하는 쿼리

```sql
WITH repeat_interval AS (
    SELECT '01 day repeat' AS index_name, 1 AS interval_begin_date, 7 AS interval_end_date
    UNION ALL SELECT '02 day repeat' AS index_name, 8 AS interval_begin_date, 14 AS interval_end_date
    UNION ALL SELECT '03 day repeat' AS index_name, 15 AS interval_begin_date, 21 AS interval_end_date
    UNION ALL SELECT '04 day repeat' AS index_name, 22 AS interval_begin_date, 28 AS interval_end_date
)
SELECT * FROM repeat_interval;
```

#####  ✅ 정착률을 계산하는 쿼리
```sql
WITH repeat_interval AS (
    SELECT '01 day repeat' AS index_name, 1 AS interval_begin_date, 7 AS interval_end_date
    UNION ALL SELECT '02 day repeat' AS index_name, 8 AS interval_begin_date, 14 AS interval_end_date
    UNION ALL SELECT '03 day repeat' AS index_name, 15 AS interval_begin_date, 21 AS interval_end_date
    UNION ALL SELECT '04 day repeat' AS index_name, 22 AS interval_begin_date, 28 AS interval_end_date
)
, action_log_with_index_date AS (
    SELECT
        u.user_id
        , u.register_date

        -- 액션 날짜와 로그 전체 최신 날짜를 날짜 형식으로 변환
        -- , CAST(a.stamp AS date)                AS action_date
        -- , MAX(CAST(a.stamp AS date)) OVER()    AS latest_date
        -- BigQuery : 한 번 타임스탬프 형식으로 변환 후 날짜 자료형으로 변환하기
        , date(timestamp(a.stamp))                AS action_date
        , MAX(date(timestamp(a.stamp))) OVER ()   AS latest_date
        , r.index_name

        -- 지표의 대상 기간 시작일과 종료일 계산기
        -- PostgreSQL
        -- , CAST(u.register_date::date + '1 day'::interval * r.interval_begin_date AS date) AS index_begin_date
        -- , CAST(u.register_date::date + '1 day'::interval * r.interval_end_date AS date) AS index_end_date

        -- Redshift
        -- , dateadd(day, r.interval_begin_date, u.register_date::date) AS index_begin_date
        -- , dateadd(day, r.interval_end_date, u.register_date::date) AS index_end_date

        -- BigQuery
        , date_add(CAST(u.register_date AS date), interval r.interval_begin_date day) AS index_begin_date
        , date_add(CAST(u.register_date AS date), interval r.interval_end_date day) AS index_end_date

        -- Redshift
        -- , dateadd(CAST(u.register_date AS date), r.interval_begin_date) AS index_begin_date
        -- , dateadd(CAST(u.register_date AS date), r.interval_end_date) AS index_end_date

        -- Hive, SparkSQL
        -- , date_add(CAST(u.register_date AS date) , r.interval_begin_date) AS index_begin_date
        -- , date_add(CAST(u.register_date AS date) , r.interval_end_date) AS index_end_date
    FROM sample_data.mst_users AS u
    LEFT OUTER JOIN sample_data.action_log AS a ON u.user_id = a.user_id
    CROSS JOIN repeat_interval AS r
)
, user_action_flag AS (
    SELECT
        user_id
      , register_date
      , index_name
        -- 4. 지표의 대상 기간에 액션을 했는지 플래그로 나타내기
      , SIGN(
            -- 3. 사용자 별로 대상 기간에 한 액션의 합 구하기
            SUM(
                    -- 1. 대상 기간의 종료일이 로그의 최신날짜 이전
                    CASE WHEN index_end_date <= latest_date THEN
                        -- 2. 지표의 대상 기간에 액션을 했다면 1 아니면 0 지정
                        CASE WHEN action_date BETWEEN index_begin_date AND index_end_date THEN 1 ELSE 0 END
                    END
            )
        ) AS index_date_action
    FROM action_log_with_index_date
    GROUP BY user_id, register_date, index_name, index_begin_date, index_end_date
)
SELECT
    register_date
  , index_name
  , AVG(100.0 * index_date_action) AS index_rate
FROM user_action_flag
GROUP BY register_date, index_name
ORDER BY register_date, index_name
;
```

#### n일 지속률과 n일 정착률 추이
- `n일 지속률`과 `n일 정착률`을 따로 집계할 경우
  - 등록 후 며칠간 사용자가 안정적으로 서비스를 사용하는지, 며칠 후 서비스를 그만두는 사용자가 많아지는 지 등을 알 수 있다.
- 지속률과 정착률이 극단적으로 떨어지는 시점이 있으면
  - 적절한 대책을 통해 지속률과 정착률이 다시 안정적으로 돌아올 때까지 사용자을 붙잡아 둘 수 있음

##### 지속률 지표를 관리하는 마스터 테이블을 정착률 형식으로 수정한 쿼리
```sql
WITH repeat_interval AS (
    SELECT '01 day repeat' AS index_name, 1 AS interval_begin_date, 1 AS interval_end_date
    UNION ALL SELECT '02 day repeat' AS index_name, 2 AS interval_begin_date, 2 AS interval_end_date
    UNION ALL SELECT '03 day repeat' AS index_name, 3 AS interval_begin_date, 3 AS interval_end_date
    UNION ALL SELECT '04 day repeat' AS index_name, 4 AS interval_begin_date, 4 AS interval_end_date
    UNION ALL SELECT '05 day repeat' AS index_name, 5 AS interval_begin_date, 5 AS interval_end_date
    UNION ALL SELECT '06 day repeat' AS index_name, 6 AS interval_begin_date, 6 AS interval_end_date
    UNION ALL SELECT '07 day repeat' AS index_name, 7 AS interval_begin_date, 7 AS interval_end_date
    UNION ALL SELECT '07 day repeat' AS index_name, 1 AS interval_begin_date, 7 AS interval_end_date
    UNION ALL SELECT '07 day repeat' AS index_name, 8 AS interval_begin_date, 14 AS interval_end_date
    UNION ALL SELECT '14 day repeat' AS index_name, 15 AS interval_begin_date, 21 AS interval_end_date
    UNION ALL SELECT '21 day repeat' AS index_name, 22 AS interval_begin_date, 28 AS interval_end_date
)SELECT * FROM repeat_interval
ORDER BY index_name
```  
##### ✅ n일 지속률들을 집계하는 쿼리
```sql
WITH repeat_interval AS (
    SELECT '01 day repeat' AS index_name, 1 AS interval_begin_date, 1 AS interval_end_date
    UNION ALL SELECT '02 day repeat' AS index_name, 2 AS interval_begin_date, 2 AS interval_end_date
    UNION ALL SELECT '03 day repeat' AS index_name, 3 AS interval_begin_date, 3 AS interval_end_date
    UNION ALL SELECT '04 day repeat' AS index_name, 4 AS interval_begin_date, 4 AS interval_end_date
    UNION ALL SELECT '05 day repeat' AS index_name, 5 AS interval_begin_date, 5 AS interval_end_date
    UNION ALL SELECT '06 day repeat' AS index_name, 6 AS interval_begin_date, 6 AS interval_end_date
    UNION ALL SELECT '07 day repeat' AS index_name, 7 AS interval_begin_date, 7 AS interval_end_date
    UNION ALL SELECT '07 day repeat' AS index_name, 1 AS interval_begin_date, 7 AS interval_end_date
    UNION ALL SELECT '07 day repeat' AS index_name, 8 AS interval_begin_date, 14 AS interval_end_date
    UNION ALL SELECT '14 day repeat' AS index_name, 15 AS interval_begin_date, 21 AS interval_end_date
    UNION ALL SELECT '21 day repeat' AS index_name, 22 AS interval_begin_date, 28 AS interval_end_date
)
, action_log_with_index_date AS (
    SELECT
        u.user_id
        , u.register_date
        , date(timestamp(a.stamp))                AS action_date
        , MAX(date(timestamp(a.stamp))) OVER ()   AS latest_date
        , r.index_name
        , date_add(CAST(u.register_date AS date), interval r.interval_begin_date day) AS index_begin_date
        , date_add(CAST(u.register_date AS date), interval r.interval_end_date day) AS index_end_date
    FROM sample_data.mst_users AS u
    LEFT OUTER JOIN sample_data.action_log AS a ON u.user_id = a.user_id
    CROSS JOIN repeat_interval AS r
)
, user_action_flag AS (
    SELECT
        user_id
      , register_date
      , index_name
        -- 4. 지표의 대상 기간에 액션을 했는지 플래그로 나타내기
      , SIGN(
            -- 3. 사용자 별로 대상 기간에 한 액션의 합 구하기
            SUM(
                    -- 1. 대상 기간의 종료일이 로그의 최신날짜 이전
                    CASE WHEN index_end_date <= latest_date THEN
                        -- 2. 지표의 대상 기간에 액션을 했다면 1 아니면 0 지정
                        CASE WHEN action_date BETWEEN index_begin_date AND index_end_date THEN 1 ELSE 0 END
                    END
            )
        ) AS index_date_action
    FROM action_log_with_index_date
    GROUP BY user_id, register_date, index_name, index_begin_date, index_end_date
)
SELECT
    index_name
  , AVG(100.0 * index_date_action) AS repeat_rate
FROM user_action_flag
GROUP BY index_name
ORDER BY index_name
;
```

##### 💡 Tip
{{< admonition type=tip title="Tip" open=true >}}
- 지속률과 정착률은 모두 등록일 기준으로 n일 이후 행동을 집계하는 것
- 등록일로 부터 n일이 경과하지 않으면 집계가 불가능
- 장기간 집계보다 **1일 지속률, 7일 지속률, 7일 정착률 처럼 단기간에 보고 대책을 세울 수 있는 지표를 활용**
- 정착률은 7일 동안 기간을 집계하므로 실제 며칠 사용했는지는 알 수 없다

{{< /admonition >}}

## 12.3 지속과 정착에 영향을 주는 액션 집계하기
- 지속률과 정착률 추이를 계산하여 사용자의 상황을 이해하는 것도 중요하지만, **무엇때문에 추이가 발생하는지 모르면 대책을 제대로 세울 수없음**
- 지표와 리포트를 만들 때 OO율을 올리자 처럼 새로운 목표와 과제가 있어야 함
- 그 목표를 위해 무엇이 사용자에게 영향을 주는지에 대해 구할 수 있어야 한다.

### 1일 지속률을 개선하기
- 1일 지속률을 개선하려면 등록한 당일 사용자들이 무엇을 했는지 파악
- 14일 정착률을 개선하고 싶다면 7일 정착률의 판정 기간동안 사용자가 어떤 행동을 했는지 조사
- 사용자의 1일 지속률이 높고, 비사용자의 1일 지속률이 낮은 액션이 1일 지속률에 더 영향을 줌
- 사용에 영향을 많이 주는 액션이 낮다면
  - 사용자들이 해당 액션을 할 수 있게 설명을 추가
  - 이벤트를 통한 사용 촉진
  - 사이트의 디자인가 동선 검토

#### 모든 사용자와 액션의 조합을 도출하는 쿼리
- 액션에 대한 사용자와 비사용자의 다음날 지속률을 함께 계산
  - 모든 액션의 조합을 만든 뒤 사용자 액션 실행 여부를 0과 1로 나타내는 테이블을 만든다.
  - `CROSS JOIN`을 통해 모든 사용자와 액션을 조합하는 임시 테이블을 생성
```sql
WITH repeat_interval AS (
    SELECT '01 day repeat' AS index_name, 1 AS interval_begin_date, 1 AS interval_end_date
)
, action_log_with_index_date AS (
    SELECT
        u.user_id
        , u.register_date
        , date(timestamp(a.stamp))                AS action_date
        , MAX(date(timestamp(a.stamp))) OVER ()   AS latest_date
        , r.index_name
        , date_add(CAST(u.register_date AS date), interval r.interval_begin_date day) AS index_begin_date
        , date_add(CAST(u.register_date AS date), interval r.interval_end_date day) AS index_end_date
    FROM sample_data.mst_users AS u
    LEFT OUTER JOIN sample_data.action_log AS a ON u.user_id = a.user_id
    CROSS JOIN repeat_interval AS r
)
, user_action_flag AS (
    SELECT
        user_id
      , register_date
      , index_name
        -- 4. 지표의 대상 기간에 액션을 했는지 플래그로 나타내기
      , SIGN(
            -- 3. 사용자 별로 대상 기간에 한 액션의 합 구하기
            SUM(
                    -- 1. 대상 기간의 종료일이 로그의 최신날짜 이전
                    CASE WHEN index_end_date <= latest_date THEN
                        -- 2. 지표의 대상 기간에 액션을 했다면 1 아니면 0 지정
                        CASE WHEN action_date BETWEEN index_begin_date AND index_end_date THEN 1 ELSE 0 END
                    END
            )
        ) AS index_date_action
    FROM action_log_with_index_date
    GROUP BY user_id, register_date, index_name, index_begin_date, index_end_date
)
, mst_actions AS (
    SELECT 'view' AS action
    UNION ALL SELECT 'comment' AS action
    UNION ALL SELECT 'follow' AS action
)
, mst_user_actions AS (
    SELECT
        u.user_id
      , u.register_date
      , a.action
    FROM sample_data.mst_users AS u
    CROSS JOIN mst_actions AS a
)
, register_action_flag AS (
    SELECT DISTINCT
        m.user_id
      , m.register_date
      , m.action
      , CASE WHEN a.action IS NOT NULL THEN 1 ELSE 0 END AS do_action
      , index_name
      , index_date_action
    FROM mst_user_actions AS m
    LEFT JOIN sample_data.action_log AS a
        ON m.user_id = a.user_id
        -- BigQuery
        AND CAST(m.register_date AS date) = date(timestamp(a.stamp))
        -- AND CAST(m.register_date AS date) = CAST(a.stamp AS date)
        AND m.action = a.action
    LEFT JOIN user_action_flag AS f ON m.user_id = f.user_id
        WHERE f.index_date_action IS NOT NULL
)
SELECT *
FROM register_action_flag ORDER BY user_id, index_name, action
;
```

#### 사용자의 액션로그를 0, 1 플래그로 표현하는 쿼리
- 사용자의 액션 로그를 0,1이라는 플래그로 표현 
```sql
WITH repeat_interval AS (
    SELECT '01 day repeat' AS index_name, 1 AS interval_begin_date, 1 AS interval_end_date
)
, action_log_with_index_date AS (
    SELECT
        u.user_id
        , u.register_date
        , date(timestamp(a.stamp))                AS action_date
        , MAX(date(timestamp(a.stamp))) OVER ()   AS latest_date
        , r.index_name
        , date_add(CAST(u.register_date AS date), interval r.interval_begin_date day) AS index_begin_date
        , date_add(CAST(u.register_date AS date), interval r.interval_end_date day) AS index_end_date
    FROM sample_data.mst_users AS u
    LEFT OUTER JOIN sample_data.action_log AS a ON u.user_id = a.user_id
    CROSS JOIN repeat_interval AS r
)
, user_action_flag AS (
    SELECT
        user_id
      , register_date
      , index_name
        -- 4. 지표의 대상 기간에 액션을 했는지 플래그로 나타내기
      , SIGN(
            -- 3. 사용자 별로 대상 기간에 한 액션의 합 구하기
            SUM(
                    -- 1. 대상 기간의 종료일이 로그의 최신날짜 이전
                    CASE WHEN index_end_date <= latest_date THEN
                        -- 2. 지표의 대상 기간에 액션을 했다면 1 아니면 0 지정
                        CASE WHEN action_date BETWEEN index_begin_date AND index_end_date THEN 1 ELSE 0 END
                    END
            )
        ) AS index_date_action
    FROM action_log_with_index_date
    GROUP BY user_id, register_date, index_name, index_begin_date, index_end_date
)
, mst_actions AS (
    SELECT 'view' AS action
    UNION ALL SELECT 'comment' AS action
    UNION ALL SELECT 'follow' AS action
)
, mst_user_actions AS (
    SELECT
        u.user_id
      , u.register_date
      , a.action
    FROM sample_data.mst_users AS u
    CROSS JOIN mst_actions AS a
)
, register_action_flag AS (
    SELECT DISTINCT
        m.user_id
      , m.register_date
      , m.action
      , CASE WHEN a.action IS NOT NULL THEN 1 ELSE 0 END AS do_action
      , index_name
      , index_date_action
    FROM mst_user_actions AS m
    LEFT JOIN sample_data.action_log AS a
        ON m.user_id = a.user_id
        -- BigQuery
        AND CAST(m.register_date AS date) = date(timestamp(a.stamp))
        -- AND CAST(m.register_date AS date) = CAST(a.stamp AS date)
        AND m.action = a.action
    LEFT JOIN user_action_flag AS f ON m.user_id = f.user_id
        WHERE f.index_date_action IS NOT NULL
)
SELECT *
FROM register_action_flag ORDER BY user_id, index_name, action
;
```

#### ✅ 액션에 따른 지속률과 정착률을 집계하는 쿼리
- 사용자의 액션 로그를 0,1로 표현 후 조건에 따라 비율을 `AVG` 함수로 계산
- 등록일에 해당 액션을 사용한 사용자는 `do_action=1` 하지 않은 경우 `do_action=0` 
- 사용자와 비사용자를 기반으로 `index_date_action`의 평균을 계산하면 1일 지속률을 구할 수 있음
```sql
WITH repeat_interval AS (
    SELECT '01 day repeat' AS index_name, 1 AS interval_begin_date, 1 AS interval_end_date
)
, action_log_with_index_date AS (
    SELECT
        u.user_id
        , u.register_date
        , date(timestamp(a.stamp))                AS action_date
        , MAX(date(timestamp(a.stamp))) OVER ()   AS latest_date
        , r.index_name
        , date_add(CAST(u.register_date AS date), interval r.interval_begin_date day) AS index_begin_date
        , date_add(CAST(u.register_date AS date), interval r.interval_end_date day) AS index_end_date
    FROM sample_data.mst_users AS u
    LEFT OUTER JOIN sample_data.action_log AS a ON u.user_id = a.user_id
    CROSS JOIN repeat_interval AS r
)
, user_action_flag AS (
    SELECT
        user_id
      , register_date
      , index_name
        -- 4. 지표의 대상 기간에 액션을 했는지 플래그로 나타내기
      , SIGN(
            -- 3. 사용자 별로 대상 기간에 한 액션의 합 구하기
            SUM(
                    -- 1. 대상 기간의 종료일이 로그의 최신날짜 이전
                    CASE WHEN index_end_date <= latest_date THEN
                        -- 2. 지표의 대상 기간에 액션을 했다면 1 아니면 0 지정
                        CASE WHEN action_date BETWEEN index_begin_date AND index_end_date THEN 1 ELSE 0 END
                    END
            )
        ) AS index_date_action
    FROM action_log_with_index_date
    GROUP BY user_id, register_date, index_name, index_begin_date, index_end_date
)
, mst_actions AS (
    SELECT 'view' AS action
    UNION ALL SELECT 'comment' AS action
    UNION ALL SELECT 'follow' AS action
)
, mst_user_actions AS (
    SELECT
        u.user_id
      , u.register_date
      , a.action
    FROM sample_data.mst_users AS u
    CROSS JOIN mst_actions AS a
)
, register_action_flag AS (
    SELECT DISTINCT
        m.user_id
      , m.register_date
      , m.action
      , CASE WHEN a.action IS NOT NULL THEN 1 ELSE 0 END AS do_action
      , index_name
      , index_date_action
    FROM mst_user_actions AS m
    LEFT JOIN sample_data.action_log AS a
        ON m.user_id = a.user_id
        -- BigQuery
        AND CAST(m.register_date AS date) = date(timestamp(a.stamp))
        -- AND CAST(m.register_date AS date) = CAST(a.stamp AS date)
        AND m.action = a.action
    LEFT JOIN user_action_flag AS f ON m.user_id = f.user_id
        WHERE f.index_date_action IS NOT NULL
)
SELECT
    action
  , COUNT(1) AS users
  , AVG(100.0 * do_action) AS usage_rate
  , index_name
  , AVG(CASE do_action WHEN 1 THEN 100.0 * index_date_action END) AS idx_rate
  , AVG(CASE do_action WHEN 0 THEN 100.0 * index_date_action END) AS no_action_idx_rate

FROM register_action_flag
GROUP BY index_name, action
ORDER BY index_name, action
;
```  

#### 💡 Tip
{{< admonition type=tip title="Tip" open=true >}}
- 해당 액션을 실행하는 진입 장벽이 높으면 지속률과 정착률에 영향이 적더라도 액션을 실행하는 진입장벽이 낮은 액션을 기반으로 대책을 세우는게 좋음
  - 예시) 동영상 업로드 보다 이미지 업로드 촉진하기 등
{{< /admonition >}}

## 12.4 액션 수에 따른 정착률 계산하기
- SNS 사례 중 '등록 후 1주일 이내 10명을 팔로우하면 해당 사용자는 서비스를 계속해서 사용한다'
  - '알 수도 있는 사람', 'OO 님을 함께 알고 있습니까?' 등의 기능을 제공해 인기 사용자를 팔로우하는 튜토리얼을 만들어 서비스 활성화를 유도
- 등록 후 7일 동안(7일정착률) 실행한 액션 수에 따라 14일 정착률이 어떻게 변화하는지 집계

### 7일 정착률 기간에 발생한 액션수에 따라 14일 정착률을 집계

#### 액션의 계급 마스터와 사용자 액션 플래그의 조합을 산출하는 쿼리
```sql
WITH repeat_interval AS (
    SELECT '14 day repeat' AS index_name, 8 AS interval_begin_date, 14 AS interval_end_date
)
, action_log_with_index_date AS (
    SELECT
        u.user_id
        , u.register_date
        , date(timestamp(a.stamp))                AS action_date
        , MAX(date(timestamp(a.stamp))) OVER ()   AS latest_date
        , r.index_name
        , date_add(CAST(u.register_date AS date), interval r.interval_begin_date day) AS index_begin_date
        , date_add(CAST(u.register_date AS date), interval r.interval_end_date day) AS index_end_date
    FROM sample_data.mst_users AS u
    LEFT OUTER JOIN sample_data.action_log AS a ON u.user_id = a.user_id
    CROSS JOIN repeat_interval AS r
)
, mst_actions AS (
    SELECT 'view' AS action
    UNION ALL SELECT 'comment' AS action
    UNION ALL SELECT 'follow' AS action
),
user_action_flag AS (
    -- 사용자가 액션을 했으면 1, 안했으면 0
    SELECT
        user_id
      , SIGN(SUM(CASE WHEN action = 'view' THEN 1 ELSE 0 END))  AS has_purchase
      , SIGN(SUM(CASE WHEN action = 'comment' THEN 1 ELSE 0 END))    AS has_review
      , SIGN(SUM(CASE WHEN action = 'follow' THEN 1 ELSE 0 END))  AS has_favorite
    FROM sample_data.action_log
    GROUP BY user_id
),
mst_action_bucket AS (
    -- 액션 단계 마스터
    -- PostgreSQL의 경우 VALUES 구문으로 일시 테이블 생성 가능
    -- Hive, Redshift, BigQuery, SparkSQL 의 경우 SELECT와 UNION ALL 구문으로 대체 가능
    SELECT 'comment' AS action, 0 AS min_count, 0 AS max_count
    UNION ALL SELECT 'comment' AS action, 1 AS min_count, 5 AS max_count
    UNION ALL SELECT 'comment' AS action, 6 AS min_count, 10 AS max_count
    UNION ALL SELECT 'comment' AS action, 11 AS min_count, 9999 AS max_count -- 간단하게 9999로 설정
    UNION ALL SELECT 'follow' AS action, 0 AS min_count, 0 AS max_count
    UNION ALL SELECT 'follow' AS action, 1 AS min_count, 5 AS max_count
    UNION ALL SELECT 'follow' AS action, 6 AS min_count, 10 AS max_count
    UNION ALL SELECT 'follow' AS action, 11 AS min_count, 9999 AS max_count
)
, mst_user_action_bucket AS (
    SELECT u.user_id, u.register_date, a.action, a.min_count, a.max_count
    FROM sample_data.mst_users u
    CROSS JOIN mst_action_bucket AS a
)
SELECT * FROM mst_user_action_bucket ORDER BY user_id, action, min_count
;
```

#### 등록 후 7일 동안의 액션 수를 집계하는 쿼리
- 7일 동안의 로그를 `LEFT JOIN` 후 등록 후 7일 동안 액션 수를 집계
```sql
WITH repeat_interval AS (
    SELECT '14 day repeat' AS index_name, 8 AS interval_begin_date, 14 AS interval_end_date
)
, action_log_with_index_date AS (
    SELECT
        u.user_id
        , u.register_date
        , date(timestamp(a.stamp))                AS action_date
        , MAX(date(timestamp(a.stamp))) OVER ()   AS latest_date
        , r.index_name
        , date_add(CAST(u.register_date AS date), interval r.interval_begin_date day) AS index_begin_date
        , date_add(CAST(u.register_date AS date), interval r.interval_end_date day) AS index_end_date
    FROM sample_data.mst_users AS u
    LEFT OUTER JOIN sample_data.action_log AS a ON u.user_id = a.user_id
    CROSS JOIN repeat_interval AS r
)
, user_action_flag AS (
    SELECT
        user_id
      , register_date
      , index_name
        -- 4. 지표의 대상 기간에 액션을 했는지 플래그로 나타내기
      , SIGN(
            -- 3. 사용자 별로 대상 기간에 한 액션의 합 구하기
            SUM(
                    -- 1. 대상 기간의 종료일이 로그의 최신날짜 이전
                    CASE WHEN index_end_date <= latest_date THEN
                        -- 2. 지표의 대상 기간에 액션을 했다면 1 아니면 0 지정
                        CASE WHEN action_date BETWEEN index_begin_date AND index_end_date THEN 1 ELSE 0 END
                    END
            )
        ) AS index_date_action
    FROM action_log_with_index_date
    GROUP BY user_id, register_date, index_name, index_begin_date, index_end_date
)
,mst_action_bucket AS (
    -- 액션 단계 마스터
    SELECT 'comment' AS action, 0 AS min_count, 0 AS max_count
    UNION ALL SELECT 'comment' AS action, 1 AS min_count, 5 AS max_count
    UNION ALL SELECT 'comment' AS action, 6 AS min_count, 10 AS max_count
    UNION ALL SELECT 'comment' AS action, 11 AS min_count, 9999 AS max_count -- 간단하게 9999로 설정
    UNION ALL SELECT 'follow' AS action, 0 AS min_count, 0 AS max_count
    UNION ALL SELECT 'follow' AS action, 1 AS min_count, 5 AS max_count
    UNION ALL SELECT 'follow' AS action, 6 AS min_count, 10 AS max_count
    UNION ALL SELECT 'follow' AS action, 11 AS min_count, 9999 AS max_count
)
, mst_user_action_bucket AS (
    SELECT u.user_id, u.register_date, a.action, a.min_count, a.max_count
    FROM sample_data.mst_users u
    CROSS JOIN mst_action_bucket AS a
),
register_action_flag AS (
    -- 등록일에서 7일 후까지의 액션 수를 세고, 액션 단계와 14일 정착 달성 플래그 계산하기
    SELECT
        m.user_id
      , m.action
      , m.min_count
      , m.max_count
      , COUNT(a.action) AS action_count
      , CASE
          WHEN COUNT(a.action) BETWEEN m.min_count AND m.max_count THEN 1
          ELSE 0
        END AS achieve
      , index_name
      , index_date_action
    FROM mst_user_action_bucket AS m
    LEFT JOIN sample_data.action_log AS a ON m.user_id = a.user_id

    -- 등록 당일부터 ~ 7일 후 액션 로그 결합
    -- PostgreSQL, RedShift
    -- AND CAST(a.stamp AS date) BETWEEN CAST(m.register_date AS date) AND CAST(m.register_date AS date) + interval '7 days'
    -- BigQuery
        AND date(timestamp(a.stamp)) BETWEEN CAST(m.register_date AS date) AND date_add(CAST(m.register_date AS date), interval 7 day)
    -- SparkSQL
    -- AND date(a.stamp as date) BETWEEN CAST(m.register_date AS date) AND date_add(CAST(m.register_date AS date), interval 7 day)
    -- Hive : JOIN 구문에 BETWEEN을 사용할 수 없으므로 WHERE 구문을 사용해야함
        AND m.action = a.action
    LEFT JOIN user_action_flag AS f ON m.user_id = f.user_id
        WHERE f.index_date_action IS NOT NULL
    GROUP BY m.user_id, m.action, m.min_count, m.max_count, f.index_name, f.index_date_action
)
SELECT * FROM register_action_flag ORDER BY user_id, action, min_count
;
```

#### ✅ 등록 후 7일 동안의 액션 횟수별로 14일 정착률을 집계하는 쿼리
```sql
WITH repeat_interval AS (
    SELECT '14 day repeat' AS index_name, 8 AS interval_begin_date, 14 AS interval_end_date
)
, action_log_with_index_date AS (
    SELECT
        u.user_id
        , u.register_date
        , date(timestamp(a.stamp))                AS action_date
        , MAX(date(timestamp(a.stamp))) OVER ()   AS latest_date
        , r.index_name
        , date_add(CAST(u.register_date AS date), interval r.interval_begin_date day) AS index_begin_date
        , date_add(CAST(u.register_date AS date), interval r.interval_end_date day) AS index_end_date
    FROM sample_data.mst_users AS u
    LEFT OUTER JOIN sample_data.action_log AS a ON u.user_id = a.user_id
    CROSS JOIN repeat_interval AS r
)
, user_action_flag AS (
    SELECT
        user_id
      , register_date
      , index_name
        -- 4. 지표의 대상 기간에 액션을 했는지 플래그로 나타내기
      , SIGN(
            -- 3. 사용자 별로 대상 기간에 한 액션의 합 구하기
            SUM(
                    -- 1. 대상 기간의 종료일이 로그의 최신날짜 이전
                    CASE WHEN index_end_date <= latest_date THEN
                        -- 2. 지표의 대상 기간에 액션을 했다면 1 아니면 0 지정
                        CASE WHEN action_date BETWEEN index_begin_date AND index_end_date THEN 1 ELSE 0 END
                    END
            )
        ) AS index_date_action
    FROM action_log_with_index_date
    GROUP BY user_id, register_date, index_name, index_begin_date, index_end_date
)
,mst_action_bucket AS (
    -- 액션 단계 마스터
    SELECT 'comment' AS action, 0 AS min_count, 0 AS max_count
    UNION ALL SELECT 'comment' AS action, 1 AS min_count, 5 AS max_count
    UNION ALL SELECT 'comment' AS action, 6 AS min_count, 10 AS max_count
    UNION ALL SELECT 'comment' AS action, 11 AS min_count, 9999 AS max_count -- 간단하게 9999로 설정
    UNION ALL SELECT 'follow' AS action, 0 AS min_count, 0 AS max_count
    UNION ALL SELECT 'follow' AS action, 1 AS min_count, 5 AS max_count
    UNION ALL SELECT 'follow' AS action, 6 AS min_count, 10 AS max_count
    UNION ALL SELECT 'follow' AS action, 11 AS min_count, 9999 AS max_count
)
, mst_user_action_bucket AS (
    SELECT u.user_id, u.register_date, a.action, a.min_count, a.max_count
    FROM sample_data.mst_users u
    CROSS JOIN mst_action_bucket AS a
),
register_action_flag AS (
    -- 등록일에서 7일 후까지의 액션 수를 세고, 액션 단계와 14일 정착 달성 플래그 계산하기
    SELECT
        m.user_id
      , m.action
      , m.min_count
      , m.max_count
      , COUNT(a.action) AS action_count
      , CASE
          WHEN COUNT(a.action) BETWEEN m.min_count AND m.max_count THEN 1
          ELSE 0
        END AS achieve
      , index_name
      , index_date_action
    FROM mst_user_action_bucket AS m
    LEFT JOIN sample_data.action_log AS a ON m.user_id = a.user_id

    -- 등록 당일부터 ~ 7일 후 액션 로그 결합
    -- PostgreSQL, RedShift
    -- AND CAST(a.stamp AS date) BETWEEN CAST(m.register_date AS date) AND CAST(m.register_date AS date) + interval '7 days'
    -- BigQuery
        AND date(timestamp(a.stamp)) BETWEEN CAST(m.register_date AS date) AND date_add(CAST(m.register_date AS date), interval 7 day)
    -- SparkSQL
    -- AND date(a.stamp as date) BETWEEN CAST(m.register_date AS date) AND date_add(CAST(m.register_date AS date), interval 7 day)
    -- Hive : JOIN 구문에 BETWEEN을 사용할 수 없으므로 WHERE 구문을 사용해야함
        AND m.action = a.action
    LEFT JOIN user_action_flag AS f ON m.user_id = f.user_id
        WHERE f.index_date_action IS NOT NULL
    GROUP BY m.user_id, m.action, m.min_count, m.max_count, f.index_name, f.index_date_action
)
SELECT
    action
  -- PostgreSQL, Redshift
  -- min_count || ' ~ ' || max_count AS count_range
  -- BigQuery
  , CONCAT(CAST(min_count AS string), ' ~ ', CAST(max_count AS string)) AS count_range
  , SUM(CASE achieve WHEN 1 THEN 1 ELSE 0 END) AS achieve
  , index_name
  , AVG(CASE achieve WHEN 1 THEN 100.0 * index_date_action END) AS achieve_index_rate
FROM register_action_flag
GROUP BY index_name, action, min_count, max_count
ORDER BY index_name, action, min_count
;
```


