# 데이터 분석을 위한 SQL 레시피 - 5장 사용자를 파악하기 위한 데이터 추출 (1)


> 🗄️ 데이터분석을 위한 SQL 레시피 책을 읽고 정리 / 요약 한 내용입니다.

## 11. 사용자 전체의 특징과 경향 찾기
- **서비스를 제공하는 것은 사용자에게 가치를 제공하는 것**

- 서비스 제공자 측에서 관련 정보로 알고 싶은 것
  - 사용자의 속성(나이, 성별, 주소지)
  - 사용자의 행동(구매한 상품, 사용한 기능, 사용한 빈도)

### 샘플 데이터
- **사용자 마스터 테이블** 
  - 가입시 사용자 성별, 가입 날짜, 사용 장치 등
  - 탈퇴시는 날짜만 기록
- **액션 로그 테이블**
  - 페이지 열람, 관심 상품 등록, 카트 추가, 구매, 리뷰 등 각 액션에 이름을 명시
  - `user_id`가 NULL 인 경우 비로그인 회원
  - 액션 로그 테이블을 따로 만들고 액션 내용을 적어두면 별도의 JOIN, UNION 없이 데이터를 다룰 수 있음

{{< admonition type=abstract title="사용자 마스터 테이블과 액션 로그 테이블" open=false >}}
```sql
-- 사용자 마스터 정보
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
;

-- 액션 로그
DROP TABLE IF EXISTS action_log;
CREATE TABLE action_log(
    session  varchar(255)
  , user_id  varchar(255)
  , action   varchar(255)
  , category varchar(255)
  , products varchar(255)
  , amount   integer
  , stamp    varchar(255)
);

INSERT INTO action_log
VALUES
    ('989004ea', 'U001', 'purchase', 'drama' , 'D001,D002', 2000, '2016-11-03 18:10:00')
  , ('989004ea', 'U001', 'view'    , NULL    , NULL       , NULL, '2016-11-03 18:00:00')
  , ('989004ea', 'U001', 'favorite', 'drama' , 'D001'     , NULL, '2016-11-03 18:00:00')
  , ('989004ea', 'U001', 'review'  , 'drama' , 'D001'     , NULL, '2016-11-03 18:00:00')
  , ('989004ea', 'U001', 'add_cart', 'drama' , 'D001'     , NULL, '2016-11-03 18:00:00')
  , ('989004ea', 'U001', 'add_cart', 'drama' , 'D001'     , NULL, '2016-11-03 18:00:00')
  , ('989004ea', 'U001', 'add_cart', 'drama' , 'D001'     , NULL, '2016-11-03 18:00:00')
  , ('989004ea', 'U001', 'add_cart', 'drama' , 'D001'     , NULL, '2016-11-03 18:00:00')
  , ('989004ea', 'U001', 'add_cart', 'drama' , 'D001'     , NULL, '2016-11-03 18:00:00')
  , ('989004ea', 'U001', 'add_cart', 'drama' , 'D002'     , NULL, '2016-11-03 18:01:00')
  , ('989004ea', 'U001', 'add_cart', 'drama' , 'D001,D002', NULL, '2016-11-03 18:02:00')
  , ('989004ea', 'U001', 'purchase', 'drama' , 'D001,D002', 2000, '2016-11-03 18:10:00')
  , ('47db0370', 'U002', 'add_cart', 'drama' , 'D001'     , NULL, '2016-11-03 19:00:00')
  , ('47db0370', 'U002', 'purchase', 'drama' , 'D001'     , 1000, '2016-11-03 20:00:00')
  , ('47db0370', 'U002', 'add_cart', 'drama' , 'D002'     , NULL, '2016-11-03 20:30:00')
  , ('87b5725f', 'U001', 'add_cart', 'action', 'A004'     , NULL, '2016-11-04 12:00:00')
  , ('87b5725f', 'U001', 'add_cart', 'action', 'A005'     , NULL, '2016-11-04 12:00:00')
  , ('87b5725f', 'U001', 'add_cart', 'action', 'A006'     , NULL, '2016-11-04 12:00:00')
  , ('9afaf87c', 'U002', 'purchase', 'drama' , 'D002'     , 1000, '2016-11-04 13:00:00')
  , ('9afaf87c', 'U001', 'purchase', 'action', 'A005,A006', 1000, '2016-11-04 15:00:00')
;
```
{{< /admonition >}}

### 11.1 사용자의 액션 수 집계하기
---
#### 액션과 관련된 지표 집계하기
- 사용자들이 특정 기간동안 얼마나 사용하는지 집계한다.
- 사용자가 평균적으로 액션을 몇번 이나 했는지도 집계 (1명당 액션 수)
- `UU(Unique Users)` : 중복 없이 집계된 사용자 수
- `특정 액션 UU / 전체 액션 UU = 사용율(usage_rate)` 

**액션 수와 비율을 계산하는 쿼리**
- 전체 UU를 구하고 그 값을 `CROSS JOIN` 하여 액션 로그를 결합

```sql
WITH state as (
    -- 로그 전체의 유니크 사용자 수 구하기
    SELECT COUNT(DISTINCT session) AS total_uu
    FROM sample_data.action_log
)
SELECT
    l.action
  , COUNT(DISTINCT l.session) AS action_uu      -- 액션 UU
  , COUNT(1)                  AS action_count   -- 액션 수
  , s.total_uu                                  -- 전체 UU

  -- 사용률 : 액션 UU / 전체 UU
  , 100.0 * COUNT(DISTINCT l.session) / s.total_uu AS usage_rate

  -- 1인당 액션 수 : 액션 수 / 액션 UU
  , 1.0 * COUNT(1) / COUNT(DISTINCT l.session) AS count_per_user

FROM sample_data.action_log AS l
CROSS JOIN state AS s -- 로그 전체의 유니크 사용자 수를 모든 레코드에 결합하기
GROUP BY l.action, s.total_uu
;
```

#### 로그인 사용자와 비로그인 사용자를 구분해서 집계하기
- 회원, 비회원을 나누어 집계하면 충성도가 높은 사용자와 낮은 사용자가 어떤 경향을 보이는지 파악
- 로그인, 비로그인, 회원, 비회원을 판별하기 위해서는 **로그 데이터에 `session` 정보**가 있어야 한다.

**로그인 상태를 판별하는 쿼리**
- `user_id` 여부에 따라 `login_status`를 `login`과 `guest`로 구분
- `login_status` 기반으로 액션 수, UU 집계
```sql
WITH action_log_with_status AS (
    SELECT
        session
      , user_id
      , action
        -- user_id 가 NULL 또는 빈 문자가 아닌 경우 login 이라 판정하기
      , CASE WHEN COALESCE(user_id, '') <> '' THEN 'login' ELSE 'guest' END AS login_status
    FROM sample_data.action_log
)
SELECT * FROM action_log_with_status
;
```

**로그인 상태에 따라 액션 수 등을 따로 집계하는 쿼리(ROLLUP 활용)**
- `login_status`가 비로그인`guest`/로그인`logint` 상관없이 전체`all`로 집계 
- `ROLLUP` 구문을 사용 : RedShift, BigQuery는 `UNION ALL` 사용

```sql
WITH action_log_with_status AS (
    SELECT
        session
      , user_id
      , action
        -- user_id 가 NULL 또는 빈 문자가 아닌 경우 login 이라 판정하기
      , CASE WHEN COALESCE(user_id, '') <> '' THEN 'login' ELSE 'guest' END AS login_status
    FROM sample_data.action_log
)
SELECT
    COALESCE(action, 'all') AS action
  , login_status
  , COALESCE(login_status, 'all') AS login_status
  , COUNT(DISTINCT session) AS action_uu
  , COUNT(1) AS action_count
FROM action_log_with_status
-- PostgreSQL, SparkSQL
GROUP BY ROLLUP(action, login_status)
-- Hive 의 경우
-- action, login_status WITH ROLLUP
;
```

#### 회원과 비회원을 구분해서 집계하기
- 로그인 상태가 아니더라도 이전에 한번이라도 로그인 했다면 회원으로 계산하기
- 로그 데이터에 회원상태를 추가해야 한다.
- `action_log_with_status` 가상 테이블에 회원상태를 추가함

**회원 상태를 판별하는 쿼리**
- `session`에 한 번이라도 로그인했다면 `MAX(user_id)` 로 추출 할 수 있음
- 로그인 하기 이전의 상태를 비회원으로 다루고 싶다면 내부에 `ORDER BY stamp`를 지정


```sql
WITH action_log_with_status AS (
    SELECT
        session
      , user_id
      , action
      -- 로그를 타임스탬프 순서로 나열하고, 한번이라도 로그인한 사용자일 경우 이후의 모든 로그 상태를 member로 설정
      , CASE WHEN COALESCE(MAX(user_id) OVER(PARTITION BY session ORDER BY stamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW), '') <> '' THEN 'member'
        ELSE 'none'
        END AS member_status
      , stamp
    FROM sample_data.action_log
)
SELECT * FROM action_log_with_status
;
```

{{< admonition type=tip title="tip" open=true >}}
- 로그인한 사용자를 빈 문자열로 지정한 경우 `COUNT(DISTINCT user_id)` 의 결과에 포함된다.
- 사용자 수를 정화하게 추출하려면 사용자 ID를 NULL로 지정
- 빈 값을 NULL, 빈문자열 혹은 특수한 값으로 나타낼지는 쿼리 최적화에 따라 다름 
{{< /admonition >}}

### 11.2 연령별 구분 집계하기
---
- 사용자의 속성을 정의하고 집계하면 다양한 리포트를 만들 수 있음
- 시청률 분석에 많이 사용되는 연령별 집계하기

**연령별 구분 목록**
|연령별 구분|설명|
|:---:|:---:|
|C  |4~12세 남성/여성  |
|T |13~19세 남성/여성  |
|M1  |20~34 남성  |
|M2  |35~49 남성  |
|M3  |50세 이상 남성  |
|F1  |20~34 여성  |
|F2  |35~49 여성  |
|F3  |50세 이상 여성  |

**사용자의 생일을 계산하는 쿼리**
- `mst_users_with_int_birth_date` : 생일과 특정 날짜를 정수로 표현한 결과 임시 테이블
- `mst_users_with_age` : 생일 정보를 부여한 사용자 마스터
```sql
WITH mst_users_with_int_birth_date AS (
    SELECT
        *
      -- 특정 날짜(2017년 1월 1일)의 정수 표현
      , 20170101 AS int_specific_date

      -- 문자열로 구성된 생년월일을 정수 표현으로 변환하기
      -- PostgreSQL, Redshift 의 경우 다음과 같이 작성
      --, CAST(replace(substring(birth_date, 1, 10), '-', '') AS int64) AS int_birth_date

      -- BigQuery 의 경우
      , CAST(replace(substr(birth_date, 1, 10), '-', '') AS int64) AS int_birth_date
      -- Hive, SparkSQL 의 경우
      --, CAST(regexp_replace(substring(birth_date, 1, 10), '-', '') AS int) AS int_birth_Date
    FROM sample_data.mst_users
),
mst_users_with_age AS (
    SELECT
        *
      , floor((int_specific_date - int_birth_date) / 10000) AS age
    FROM mst_users_with_int_birth_date
)
SELECT
    user_id, sex, birth_date, age
FROM mst_users_with_age
;
```

**성별과 연령으로 연령별 구분하는 계산 쿼리**
- `cagetory` 컬럼에서 연령별 구분을 계산 
- 20세 이상일 경우 접두사 `M`, `F`를 계산하고 `CONCAT` 함수로 결합

```sql
WITH mst_users_with_int_birth_date AS (
    SELECT
        *
      , 20170101 AS int_specific_date
      , CAST(replace(substr(birth_date, 1, 10), '-', '') AS int64) AS int_birth_date
    FROM sample_data.mst_users
),
mst_users_with_age AS (
    SELECT
        *
      , floor((int_specific_date - int_birth_date) / 10000) AS age
    FROM mst_users_with_int_birth_date
),
mst_users_with_category AS (
    SELECT
        user_id
      , sex
      , age
      , CONCAT(CASE WHEN 20 <= age THEN sex ELSE '' END
              ,CASE
                 WHEN age BETWEEN 4 AND 12 THEN 'C'
                 WHEN age BETWEEN 13 AND 19 THEN 'T'
                 WHEN age BETWEEN 20 AND 34 THEN '1'
                 WHEN age BETWEEN 35 AND 49 THEN '2'
                 WHEN age >= 50 THEN '3'
               END) AS category
    FROM mst_users_with_age
)
SELECT * FROM mst_users_with_category
;
```

**연령별 구분의 사람 수를 계산하는 쿼리**
```sql
WITH mst_users_with_int_birth_date AS (
    SELECT
        *
      , 20170101 AS int_specific_date
      , CAST(replace(substr(birth_date, 1, 10), '-', '') AS int64) AS int_birth_date
    FROM sample_data.mst_users
),
mst_users_with_age AS (
    SELECT
        *
      , floor((int_specific_date - int_birth_date) / 10000) AS age
    FROM mst_users_with_int_birth_date
),
mst_users_with_category AS (
    SELECT
        user_id
      , sex
      , age
      , CONCAT(CASE WHEN 20 <= age THEN sex ELSE '' END
              ,CASE
                 WHEN age BETWEEN 4 AND 12 THEN 'C'
                 WHEN age BETWEEN 13 AND 19 THEN 'T'
                 WHEN age BETWEEN 20 AND 34 THEN '1'
                 WHEN age BETWEEN 35 AND 49 THEN '2'
                 WHEN age >= 50 THEN '3'
               END) AS category
    FROM mst_users_with_age
)
SELECT category, COUNT(*) AS user_count
FROM mst_users_with_category
GROUP BY category
;
```

{{< admonition type=tip title="tip" open=true >}}
- 연령을 단순하게 계산하면 특징을 파악하기 어렵고 데모그래픽`Demographic` 등에 활용하기 어려움
- 서비스에 따라 `M1`, `F1`으로 정의하는 것이 적절하지 않을 수 있음 : 새로운 기준을 정의 
{{< /admonition >}}


### 11.3 연령별 구분의 특징 추출하기
---
- 연령별 구분을 사용하여 각각 구매한 상품의 카테고리를 집계하기
- 카테고리 내부에서 연령별 분포 확인 가능
**연령별 구분과 카테고리를 집계하는 쿼리**
```sql
WITH mst_users_with_int_birth_date AS (
    SELECT
        *
      , 20170101 AS int_specific_date
      , CAST(replace(substr(birth_date, 1, 10), '-', '') AS int64) AS int_birth_date
    FROM sample_data.mst_users
),
mst_users_with_age AS (
    SELECT
        *
      , floor((int_specific_date - int_birth_date) / 10000) AS age
    FROM mst_users_with_int_birth_date
),
mst_users_with_category AS (
    SELECT
        user_id
      , sex
      , age
      , CONCAT(CASE WHEN 20 <= age THEN sex ELSE '' END
              ,CASE
                 WHEN age BETWEEN 4 AND 12 THEN 'C'
                 WHEN age BETWEEN 13 AND 19 THEN 'T'
                 WHEN age BETWEEN 20 AND 34 THEN '1'
                 WHEN age BETWEEN 35 AND 49 THEN '2'
                 WHEN age >= 50 THEN '3'
               END) AS category
    FROM mst_users_with_age
)
SELECT
    p.category AS product_category
  , u.category AS user_category
  , COUNT(*)   AS purchase_count
FROM sample_data.action_log AS p
JOIN mst_users_with_category AS u ON p.user_id = u.user_id
WHERE p.action = 'purchase'
GROUP BY p.category, u.category
ORDER BY p.category, u.category
;

```

### 11.4 사용자의 방문 빈도 집계하기
---
- 서비스를 한 주 동안 며칠 사용하는 사용자가 몇 명인지를 집계
- **일주일 동안 사용자 사용 일수와 구성비**를 집계

**한 주에 며칠 사용되었는지 집계**
- `action_log` 테이블에 사용자별 ID로 `DISTINCT`를 적용하여 사용일수 집계

```sql
WITH action_log_with_dt AS (
    SELECT
        *,

        -- 타임스탬프에서 날짜 추출하기
        -- PostgreSQL, Hive, Redshift, SparkSQL : substring 사용
        -- substring(stamp, 1, 10) AS dt
        -- PostgreSQL, Hive, BigQuery, SparkSQL : substr 사용
        substr(stamp, 1, 10) AS dt
    FROM sample_data.action_log
)
, action_day_count_per_user AS (
    SELECT
        user_id
      , COUNT(DISTINCT dt) AS action_day_count
    FROM action_log_with_dt
    -- 2016년 11월 1일부터 11월 7일까지의 한 주 동안을 대상으로 선정
    WHERE dt BETWEEN '2016-10-11' AND '2016-11-07'
    GROUP BY user_id
)
SELECT
    action_day_count
  , COUNT(DISTINCT user_id) AS user_count
FROM action_day_count_per_user
GROUP BY action_day_count
ORDER BY action_day_count
;

```

**구성비와 구성비누계를 계산하는 쿼리**
```sql
WITH action_log_with_dt AS (
    SELECT
        *,

        -- 타임스탬프에서 날짜 추출하기
        -- PostgreSQL, Hive, Redshift, SparkSQL : substring 사용
        -- substring(stamp, 1, 10) AS dt
        -- PostgreSQL, Hive, BigQuery, SparkSQL : substr 사용
        substr(stamp, 1, 10) AS dt
    FROM sample_data.action_log
)
, action_day_count_per_user AS (
    SELECT
        user_id
      , COUNT(DISTINCT dt) AS action_day_count
    FROM action_log_with_dt
    -- 2016년 11월 1일부터 11월 7일까지의 한 주 동안을 대상으로 선정
    WHERE dt BETWEEN '2016-10-11' AND '2016-11-07'
    GROUP BY user_id
)
SELECT
    action_day_count
  , COUNT(DISTINCT user_id) AS user_count

  -- 구성비
  , 100.0 * COUNT(DISTINCT user_id) / SUM(COUNT(DISTINCT user_id)) OVER () as composition_ratio

  -- 구성비누계
  , 100.0 * SUM(COUNT(DISTINCT user_id)) OVER(ORDER BY action_day_count ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW )
          / SUM(COUNT(DISTINCT user_id)) OVER() AS camulative_ratio

FROM action_day_count_per_user
GROUP BY action_day_count
ORDER BY action_day_count
;
```

### 11.5 벤 다이어그램으로 사용자 액션 집계하기
---
- 서비스 내의 여러 기능을 사용 현황을 조사하여 예상대로 사용하는지 확인해야 함
- `has_purchase`, `has_review`, `has_ravorite` : 등록액션한 경우 1, 아닌경우 0
- 데이터 툴에 따라 곧바로 벤 다이어그램을 만들어 주는 기능 존재



**사용자들의 액션 플래그를 집계하는 쿼리**
```sql
WITH user_action_flag AS (
    SELECT
        user_id
      , SIGN(SUM(CASE WHEN action = 'purchase' THEN 1 ELSE 0 END))  AS has_purchase
      , SIGN(SUM(CASE WHEN action = 'review' THEN 1 ELSE 0 END))    AS has_review
      , SIGN(SUM(CASE WHEN action = 'favorite' THEN 1 ELSE 0 END))  AS has_favorite
    FROM action_log
    GROUP BY user_id
), active_venn_diagram AS (
    SELECT
        has_purchase
      , has_review
      , has_favorite
      , COUNT(1) AS users
    FROM user_action_flag
    GROUP BY CUBE(has_purchase, has_review, has_favorite)
)
SELECT * FROM active_venn_diagram
ORDER BY has_purchase, has_review, has_favorite
;
```

**모든 액션 조합에 대한 사용자 수 계산하기**
- 벤 다이어그램을 그리기 위해서는 `구매 액션만 한 사용자 수`, `구매와 리뷰 액션을 한 사용자 수` 처럼 하나의 액션 두 갱의 액션을 몇 명인지 계산필요
- PostgreSQL : `CUBE` 구문을 사용하면 쉽계 계산

```sql
-- PostgreSQL
WITH user_action_flag AS (
    SELECT
        user_id
      , SIGN(SUM(CASE WHEN action = 'purchase' THEN 1 ELSE 0 END))  AS has_purchase
      , SIGN(SUM(CASE WHEN action = 'review' THEN 1 ELSE 0 END))    AS has_review
      , SIGN(SUM(CASE WHEN action = 'favorite' THEN 1 ELSE 0 END))  AS has_favorite
    FROM action_log
    GROUP BY user_id
), active_venn_diagram AS (
    SELECT
        has_purchase
      , has_review
      , has_favorite
      , COUNT(1) AS users
    FROM user_action_flag
    GROUP BY CUBE(has_purchase, has_review, has_favorite)
)
SELECT * FROM active_venn_diagram
ORDER BY has_purchase, has_review, has_favorite
;
```

**CUBE 구문을 사용하지 않고 표준 SQL 구문으로 작성한 쿼리**
- `UNION ALL`을 많이 사용하므로 성능이 좋지 않다.
```sql
WITH user_action_flag AS (
    -- 사용자가 액션을 했으면 1, 안했으면 0
    SELECT
        user_id
      , SIGN(SUM(CASE WHEN action = 'purchase' THEN 1 ELSE 0 END))  AS has_purchase
      , SIGN(SUM(CASE WHEN action = 'review' THEN 1 ELSE 0 END))    AS has_review
      , SIGN(SUM(CASE WHEN action = 'favorite' THEN 1 ELSE 0 END))  AS has_favorite
    FROM sample_data.action_log
    GROUP BY user_id
),
action_venn_diagram AS (
    -- 모든 액션 조합을 개별적으로 구하고 UNION ALL로 결합

    -- 3개의 액션을 모두 한 경우 집계
    SELECT has_purchase, has_review, has_favorite, COUNT(*) AS users
    FROM user_action_flag GROUP BY has_purchase, has_review, has_favorite

    -- 3개의 액션 중 2개의 액션을 한 경우 집계
    UNION ALL
    SELECT NULL AS has_purchase, has_review, has_favorite, COUNT(*) AS users
    FROM user_action_flag GROUP BY has_review, has_favorite
    UNION ALL
    SELECT has_purchase, NULL AS has_review, has_favorite, COUNT(*) AS users
    FROM user_action_flag GROUP BY has_purchase, has_favorite
    UNION ALL
    SELECT has_purchase, has_review, NULL AS has_favorite, COUNT(*) AS users
    FROM user_action_flag GROUP BY has_purchase, has_review

    -- 3개의 액션 중 1개의 액션을 한 경우 집계
    UNION ALL
    SELECT NULL AS has_purchase, NULL AS has_review, has_favorite, COUNT(*) AS users
    FROM user_action_flag GROUP BY has_favorite
    UNION ALL
    SELECT NULL AS has_purchase, has_review, NULL AS has_favorite, COUNT(*) AS users
    FROM user_action_flag GROUP BY has_review
    UNION ALL
    SELECT has_purchase, NULL AS has_review, NULL AS has_favorite, COUNT(*) AS users
    FROM user_action_flag GROUP BY has_purchase

    -- 액션과 관계 없이 모든 사용자 집계
    UNION ALL
    SELECT NULL AS has_purchase, NULL AS has_review, NULL AS has_favorite, COUNT(*) AS users
    FROM user_action_flag
)
SELECT * FROM action_venn_diagram
ORDER BY has_purchase, has_review, has_favorite
;
```

**유사적으로 NULL을 포함한 레코드를 추가해서 CUBE 구문과 같은 결과를 얻는 쿼리**
```sql
WITH user_action_flag AS (
    -- 사용자가 액션을 했으면 1, 안했으면 0
    SELECT
        user_id
      , SIGN(SUM(CASE WHEN action = 'purchase' THEN 1 ELSE 0 END))  AS has_purchase
      , SIGN(SUM(CASE WHEN action = 'review' THEN 1 ELSE 0 END))    AS has_review
      , SIGN(SUM(CASE WHEN action = 'favorite' THEN 1 ELSE 0 END))  AS has_favorite
    FROM sample_data.action_log
    GROUP BY user_id
)
, action_venn_diagram AS (
    SELECT
        mod_has_purchase
      , mod_has_review
      , mod_has_favorite
      , COUNT(1) AS users
    FROM user_action_flag
    -- 각 컬럼에 NULL을 포함하는 레코드를 유사적으로 추가하기
    -- BigQuery의 경우 CROSS JOIN unnest 함수 사용
    CROSS JOIN unnest(array[has_purchase, NULL]) AS mod_has_purchase
    CROSS JOIN unnest(array[has_review, NULL]) AS mod_has_review
    CROSS JOIN unnest(array[has_favorite, NULL]) AS mod_has_favorite

    -- Hive, SparkSQL의 경우 : LATERAL VIEW와 explode 함수 사용
    -- LATERAL VIEW explode(array(has_purchase, NULL)) e1 AS mode_has_purchase
    -- LATERAL VIEW explode(array(has_review, NULL)) e2 AS mod_has_review
    -- LATERAL VIEW explode(array(has_favorite, NULL)) e3 AS mod_has_favorite
    GROUP BY mod_has_purchase, mod_has_review, mod_has_favorite
) SELECT * FROM action_venn_diagram ORDER BY mod_has_purchase, mod_has_review, mod_has_favorite
;
```

**벤 다이어그램을 만들기 위해 데이터를 가공하는 쿼리**
```sql
WITH user_action_flag AS (
    -- 사용자가 액션을 했으면 1, 안했으면 0
    SELECT
        user_id
      , SIGN(SUM(CASE WHEN action = 'purchase' THEN 1 ELSE 0 END))  AS has_purchase
      , SIGN(SUM(CASE WHEN action = 'review' THEN 1 ELSE 0 END))    AS has_review
      , SIGN(SUM(CASE WHEN action = 'favorite' THEN 1 ELSE 0 END))  AS has_favorite
    FROM sample_data.action_log
    GROUP BY user_id
)
, action_venn_diagram AS (
        SELECT
        mod_has_purchase
      , mod_has_review
      , mod_has_favorite
      , COUNT(1) AS users
    FROM user_action_flag
    CROSS JOIN unnest(array[has_purchase, NULL]) AS mod_has_purchase
    CROSS JOIN unnest(array[has_review, NULL]) AS mod_has_review
    CROSS JOIN unnest(array[has_favorite, NULL]) AS mod_has_favorite
    GROUP BY mod_has_purchase, mod_has_review, mod_has_favorite
)
SELECT
    -- 플래그 0, 1을 가공하기
    CASE mod_has_purchase WHEN 1 THEN 'purchase' WHEN 0 THEN 'not purchase' ELSE 'any' END AS has_purchase
  , CASE mod_has_review WHEN 1 THEN 'review' WHEN 0 THEN 'not review' ELSE 'any' END AS has_review
  , CASE mod_has_favorite WHEN 1 THEN 'favorite' WHEN 0 THEN 'not favorite' ELSE 'any' END AS has_favorite
  , users

  -- 전체 사용자 수를 기반으로 비율 구하기
  , 100.0 * users / NULLIF (SUM(CASE WHEN mod_has_purchase IS NULL AND mod_has_review IS NULL AND mod_has_favorite IS NULL THEN users ELSE 0 END) OVER(), 0) AS ratio
FROM action_venn_diagram
ORDER BY has_purchase, has_review, has_favorite
;
```

{{< admonition type=tip title="SNS 적용 방법" open=true >}}
- 글을 작성하지 않고 다른 사람의 글만 확인하는 사용자
- 글을 많이 작성하는 사용자
- 글을 거의 작성하지 않지만 댓글은 많이 작성하는 사용자
- 글과 댓글 모두 적극적으로 작성하는 사용자
{{< /admonition >}}

### 11.6 Decile 분석을 사용해 사용자를 10단계 그룹으로 나누기
- 사용자 특징을 분석 시 성별과 연령 등의 데이터가 있다면 이러한 속성에 따른 특징을 확인가능
- 데모그래픽한 데이터가 존재하지 않을 경우 사용자 액션으로 속성을 정의해 보는 것도 좋음
- 데이터를 10단계로 분할해서 중요도를 파악하는 `Decile` 분석 방법을 사용

#### Decile 분석 과정
사용자의 구매 금액에 따라 순위를 구분하고 중요도를 파악하는 리포트를 생성하기

1. 사용자를 구매 금액이 많은 순으로 정렬
2. 정렬된 사용자 상위부터 10%씩 Decile 10까지의 그룹을 할당한다.
3. 각 그룹의 구매 금액 합계를 집계한다.
4. 전체 구매 금액에 대해 각 `Decile`의 구매 금액 비율(구성비)를 계산한다.
5. 상위에서 누적으로 어느 정도의 비율을 차지하는지 구성비누계를 집계한다.

**구매액이 많은 순서로 사용자 그룹을 10등분 하는 쿼리**
- 정렬된 사용자 상위에서 10%씩 Decile 1 ~ Decile 10 까지 그룹을 할당
- 같은 수로 데이터 그룹을 만들 때는 `NTILE` 윈도 함수를 사용

```sql
WITH user_purchase_amount AS (
    SELECT
        user_id
      , SUM(amount) AS purchase_amount
    FROM sample_data.action_log
    WHERE action = 'purchase'
    GROUP BY user_id
),
user_with_decile AS (
    SELECT
        user_id
      , purchase_amount
      , ntile(10) OVER (ORDER BY purchase_amount DESC) AS decile
    FROM user_purchase_amount
)
SELECT * FROM user_with_decile
;
```

**10등분한 Decile들을 집계하는 쿼리**
```sql
WITH user_purchase_amount AS (
    SELECT
        user_id
      , SUM(amount) AS purchase_amount
    FROM sample_data.action_log
    WHERE action = 'purchase'
    GROUP BY user_id
),
user_with_decile AS (
    SELECT
        user_id
      , purchase_amount
      , ntile(10) OVER (ORDER BY purchase_amount DESC) AS decile
    FROM user_purchase_amount
),
decile_with_purchase_amount AS (
    SELECT
        decile
      , SUM(purchase_amount) AS amount
      , AVG(purchase_amount) AS avg_amount
      , SUM(SUM(purchase_amount)) OVER (ORDER BY decile) AS cumulative_amount
      , SUM(SUM(purchase_amount)) OVER () AS total_amount
    FROM user_with_decile GROUP BY decile
)
SELECT
*
FROM decile_with_purchase_amount;
```

**구매액이 많은 Decile 순서로 구성비와 구성비누계를 계산하는 쿼리**
```sql
WITH user_purchase_amount AS (
    SELECT
        user_id
      , SUM(amount) AS purchase_amount
    FROM sample_data.action_log
    WHERE action = 'purchase'
    GROUP BY user_id
),
user_with_decile AS (
    SELECT
        user_id
      , purchase_amount
      , ntile(10) OVER (ORDER BY purchase_amount DESC) AS decile
    FROM user_purchase_amount
),
decile_with_purchase_amount AS (
    SELECT
        decile
      , SUM(purchase_amount) AS amount
      , AVG(purchase_amount) AS avg_amount
      , SUM(SUM(purchase_amount)) OVER (ORDER BY decile) AS cumulative_amount
      , SUM(SUM(purchase_amount)) OVER () AS total_amount
    FROM user_with_decile GROUP BY decile
)
SELECT
    decile
  , amount
  , avg_amount
  , 100.0 * amount / total_amount AS total_ratio
  , 100.0 * cumulative_amount / total_amount AS cumulative_ratio
FROM decile_with_purchase_amount;

```

{{< admonition type=tip title="Decile 분석을 시행하면" open=true >}}
- `Decile`의 특징을 다른 분석 방법으로 세분화하여 조사하면 사용자의 속성을 자세하게 파악 가능
- 예시로 `Decile 7 ~ 10` 은 정착되지 않은 고객을 나타내면
  - 메일 매거진 등으로 리텐션을 높이는 등의 대책을 강구해볼 수 있음
  - 이미 매거진을 보내는 상황이면 메일 매거진을 보낼 때 추가적인 데이터를 수집하여 해당 decile에 해당하는 사람들의 속성과 관련 데이터를 수집하고 활용
{{< /admonition >}}

### 11.7 RFM 분석으로 사용자를 세 가지  그룹으로 나누기
- **Decile 분석의 문제점**
  - 검색 기간에 따라 문제가 발생
  - 검색기간이 너무 긴 경우 - 과거의 우수고객이어도 현재는 다른서비스를 사용하는 휴면 고객이 포함될 가능성 있음
  - 검색기간이 너무 짧은 경우 - 정기적으로 구매하는 안정 고객이 포함되지 않고 해당 기간동안 일시적으로 많이 구매한 사용자가 우수 고객 취급

#### RFM 분석의 세가지 지표 집계하기
- RFM 분석의 세가지 지표
  - **Recency(최근 구매일)** : 최근 무언가를 구매한 사용자를 우량 고객으로 취급
  - **Frequency(구매 횟수)** : 사용자가 구매한 횟수를 세고 많을 수록 우량 고객으로 취급
  - **Mondtary(구매 금액 합계)** : 사용자의 구매 금액 합계를 집계하고 금액이 높을 수록 우량 고객으로 취급
- Recency, Frequency, Monetary 3 개의 앞글자를 따서 RFM 분석이라고 함
- Decile 분석에서는 한 번 구매로 비싼 물건을 구매한 사용자와 정기적으로 저렴한 물건을 여러번 구매한 사용자가 같은 그룹으로 판별

```sql
WITH purchase_log AS (
    SELECT
        user_id
      , amount

      -- PostgreSQL, Hive, BigQuery, SparkSQL 의 경우 substring 사용
      -- , substring(stamp, 1, 10) AS dt

      -- PostgreSQL, Hive, BigQuery, SparkSQL 의 경우 substr 사용
      , substr(stamp, 1, 10) AS dt
    FROM sample_data.action_log
    WHERE action = 'purchase'
)
, user_rfm AS (
    SELECT
        user_id
      , MAX(dt) AS recent_date

      -- BigQuery : date_diff 함수 사용
      , date_diff(CURRENT_DATE, date(timestamp(MAX(dt))), day) AS recency
      -- PostgreSQL, Redshift : 날짜 형식끼리 빼기 가능
      --, CURRENT_DATE - MAX(dt::date) AS recency
      -- Hive, SparkSQL : datediff 함수 사용
      --, datediff(CURRENT_DATE(), to_date(MAX(dt))) AS recency

      , COUNT(dt) AS frequency
      , SUM(amount) AS monetary
    FROM purchase_log
    GROUP BY user_id
) 
SELECT * FROM user_rfm
;
```
#### RFM 랭크정의하기
- RFM 분석에서는 3개의 지표를 각각 5개의 그룹으로 나누는 것이 일반적
  - `125 = 5 * 5 * 5` 개의 그룹으로 사용자를 나누어 파악

**RFM 랭크 정의 테이블**
|랭크  |R:최근 구매일  |F:누계 구매 횟수  |M:누개 구매 금액  |
|---|---|---|---|
|5  |14일 이내  |20회 이상  |300만원 이상  |
|4  |28일 이내  |10회 이상  |100만원 이상  |
|3  |60일 이내  |5회 이상  |30만원 이상  |
|2  |90일 이내  |2회 이상  |5만원 이상  |
|1 |91일 이내  |1회  |5만원 미만  |

**사용자들의 RFM 랭크를 계산하는 쿼리**
```sql
WITH purchase_log AS (
    SELECT
        user_id
      , amount
      , substr(stamp, 1, 10) AS dt
    FROM sample_data.action_log
    WHERE action = 'purchase'
)
, user_rfm AS (
    SELECT
        user_id
      , MAX(dt) AS recent_date
      , date_diff(CURRENT_DATE, date(timestamp(MAX(dt))), day) AS recency
      , COUNT(dt) AS frequency
      , SUM(amount) AS monetary
    FROM purchase_log
    GROUP BY user_id
), user_rmf_rank AS (
    SELECT
        user_id
      , recent_date
      , recency
      , frequency
      , monetary
      , CASE
          WHEN recency < 14 THEN 5
          WHEN recency < 28 THEN 4
          WHEN recency < 60 THEN 3
          WHEN recency < 90 THEN 2
          ELSE 1
        END AS r
      , CASE
          WHEN 20 <= frequency THEN 5
          WHEN 10 <= frequency THEN 4
          WHEN 5 <= frequency THEN 3
          WHEN 2 <= frequency THEN 2
          ELSE 1
        END AS f
      , CASE
          WHEN 300000 <= monetary THEN 5
          WHEN 100000 <= monetary THEN 4
          WHEN 30000 <= monetary THEN 3
          WHEN 5000 <= monetary THEN 2
          ELSE 1
        END AS m
    FROM user_rfm
)
SELECT * FROM user_rmf_rank;

```

**각 그룹에 사람 수를 확인하는 쿼리**
```sql
WITH purchase_log AS (
    SELECT
        user_id
      , amount
      , substr(stamp, 1, 10) AS dt
    FROM sample_data.action_log
    WHERE action = 'purchase'
)
, user_rfm AS (
    SELECT
        user_id
      , MAX(dt) AS recent_date
      , date_diff(CURRENT_DATE, date(timestamp(MAX(dt))), day) AS recency
      , COUNT(dt) AS frequency
      , SUM(amount) AS monetary
    FROM purchase_log
    GROUP BY user_id
), user_rmf_rank AS (
    SELECT
        user_id
      , recent_date
      , recency
      , frequency
      , monetary
      , CASE
          WHEN recency < 14 THEN 5
          WHEN recency < 28 THEN 4
          WHEN recency < 60 THEN 3
          WHEN recency < 90 THEN 2
          ELSE 1
        END AS r
      , CASE
          WHEN 20 <= frequency THEN 5
          WHEN 10 <= frequency THEN 4
          WHEN 5 <= frequency THEN 3
          WHEN 2 <= frequency THEN 2
          ELSE 1
        END AS f
      , CASE
          WHEN 300000 <= monetary THEN 5
          WHEN 100000 <= monetary THEN 4
          WHEN 30000 <= monetary THEN 3
          WHEN 5000 <= monetary THEN 2
          ELSE 1
        END AS m
    FROM user_rfm
),
mst_rfm_index AS (
    -- 1 ~ 5 까지 숫자를 가지는 테이블 만들기
    -- PostgreSQL의 경우 generate_series 등으로도 대체 가능
    SELECT 1 AS rfm_index
    UNION ALL SELECT 2 AS rfm_index
    UNION ALL SELECT 3 AS rfm_index
    UNION ALL SELECT 4 AS rfm_index
    UNION ALL SELECT 5 AS rfm_index
)
, rfm_flag AS (
    SELECT
        m.rfm_index
      , CASE WHEN m.rfm_index = r.r THEN 1 ELSE 0 END AS r_flag
    FROM mst_rfm_index AS m
    CROSS JOIN user_rmf_rank AS r
)
SELECT
    rfm_index
  , SUM(r_flag) AS r
  , SUM(r_flag) AS f
  , SUM(r_flag) AS m
FROM rfm_flag GROUP BY rfm_index ORDER BY rfm_index DESC
;
```
- 극단적으로 적은 사용자 수의 그룹이 발생한다면, RFM 랭크 정의를 수정해야 함
- 그룹이 그분되면 각각 FRM 랭크별로 어떠한 대체

#### 사용자를 1차원으로 구분하기
- **RFM 분석을 3차원으로 표현하면 125개의 그룹이 발생하므로 관리가 매우 어려움**
- `R+F+M` 각 랭크 합계를 기반으로 13개 그룹으로 나누어 관리

**통합 랭크별 사용자 수**
|**통합 랭크**|**R**|**F**|**M**|**사용자 수**|
|:-----:|:-----:|:-----:|:-----:|:-----:|
|15|5|5|5|9
|14|5|5|4|13
||5|4|5|15
||4|5|5|15
|13|5|5|3|16
|&nbsp;| | | | | 
|…|…|…|…|…
|3|1|1|1|842

**통합 랭크를 계산하는 쿼리**
```sql
WITH purchase_log AS (
    SELECT
        user_id
      , amount
      , substr(stamp, 1, 10) AS dt
    FROM sample_data.action_log
    WHERE action = 'purchase'
)
, user_rfm AS (
    SELECT
        user_id
      , MAX(dt) AS recent_date
      , date_diff(CURRENT_DATE, date(timestamp(MAX(dt))), day) AS recency
      , COUNT(dt) AS frequency
      , SUM(amount) AS monetary
    FROM purchase_log
    GROUP BY user_id
), user_rmf_rank AS (
    SELECT
        user_id
      , recent_date
      , recency
      , frequency
      , monetary
      , CASE
          WHEN recency < 14 THEN 5
          WHEN recency < 28 THEN 4
          WHEN recency < 60 THEN 3
          WHEN recency < 90 THEN 2
          ELSE 1
        END AS r
      , CASE
          WHEN 20 <= frequency THEN 5
          WHEN 10 <= frequency THEN 4
          WHEN 5 <= frequency THEN 3
          WHEN 2 <= frequency THEN 2
          ELSE 1
        END AS f
      , CASE
          WHEN 300000 <= monetary THEN 5
          WHEN 100000 <= monetary THEN 4
          WHEN 30000 <= monetary THEN 3
          WHEN 5000 <= monetary THEN 2
          ELSE 1
        END AS m
    FROM user_rfm
)
SELECT
    r + f + m AS total_rank
  , r, f, m
  , COUNT(user_id) as count
FROM user_rmf_rank
GROUP BY r, f, m
ORDER BY total_rank DESC, r DESC, f DESC, m DESC;
```

**종합 랭크별로 사용자 수를 집계하는 쿼리**
```sql

WITH purchase_log AS (
    SELECT
        user_id
      , amount
      , substr(stamp, 1, 10) AS dt
    FROM sample_data.action_log
    WHERE action = 'purchase'
)
, user_rfm AS (
    SELECT
        user_id
      , MAX(dt) AS recent_date
      , date_diff(CURRENT_DATE, date(timestamp(MAX(dt))), day) AS recency
      , COUNT(dt) AS frequency
      , SUM(amount) AS monetary
    FROM purchase_log
    GROUP BY user_id
), user_rmf_rank AS (
    SELECT
        user_id
      , recent_date
      , recency
      , frequency
      , monetary
      , CASE
          WHEN recency < 14 THEN 5
          WHEN recency < 28 THEN 4
          WHEN recency < 60 THEN 3
          WHEN recency < 90 THEN 2
          ELSE 1
        END AS r
      , CASE
          WHEN 20 <= frequency THEN 5
          WHEN 10 <= frequency THEN 4
          WHEN 5 <= frequency THEN 3
          WHEN 2 <= frequency THEN 2
          ELSE 1
        END AS f
      , CASE
          WHEN 300000 <= monetary THEN 5
          WHEN 100000 <= monetary THEN 4
          WHEN 30000 <= monetary THEN 3
          WHEN 5000 <= monetary THEN 2
          ELSE 1
        END AS m
    FROM user_rfm
)
SELECT
    r + f + m AS total_rank
  , COUNT(user_id) as count
FROM user_rmf_rank
-- PostgreSQL, Redshift, BigQuery : SELECT 구문에서 정의한 별칭을 GROUP BY 문에 지정 가능
GROUP BY total_rank
-- PostgreSQL, Hive, Redshift, SparkSQL : SELECT 구문에서 별칭을 지정하기 전 식을 GROUP BY 문에 지정 가능
-- GROUP BY r + f + m
ORDER BY total_rank DESC;
```

#### 2차원으로 사용자 인식하기
- RFM을 지표 2개를 사용해서 사용자 층을 정의한다.
- 2차원으로 사용자 층을 구성하면 
  - 각 사용자 층에 어떤 마케팅 대책을 실시 할지
  - 상위 사용자 층으로 어떻게 구성할지 생각 해 볼 수 있다.

**R과 F를 사용해 2차원으로 사용자 인식**

{{<figure src="/posts/images/sql-recipe/rfm-2.png#center">}}

**R과 F를 사용해 2차원 사용자 층의 사용자 수를 집계하는 쿼리**
```sql
WITH purchase_log AS (
    SELECT
        user_id
      , amount
      , substr(stamp, 1, 10) AS dt
    FROM sample_data.action_log
    WHERE action = 'purchase'
)
, user_rfm AS (
    SELECT
        user_id
      , MAX(dt) AS recent_date
      , date_diff(CURRENT_DATE, date(timestamp(MAX(dt))), day) AS recency
      , COUNT(dt) AS frequency
      , SUM(amount) AS monetary
    FROM purchase_log
    GROUP BY user_id
), user_rmf_rank AS (
    SELECT
        user_id
      , recent_date
      , recency
      , frequency
      , monetary
      , CASE
          WHEN recency < 14 THEN 5
          WHEN recency < 28 THEN 4
          WHEN recency < 60 THEN 3
          WHEN recency < 90 THEN 2
          ELSE 1
        END AS r
      , CASE
          WHEN 20 <= frequency THEN 5
          WHEN 10 <= frequency THEN 4
          WHEN 5 <= frequency THEN 3
          WHEN 2 <= frequency THEN 2
          ELSE 1
        END AS f
      , CASE
          WHEN 300000 <= monetary THEN 5
          WHEN 100000 <= monetary THEN 4
          WHEN 30000 <= monetary THEN 3
          WHEN 5000 <= monetary THEN 2
          ELSE 1
        END AS m
    FROM user_rfm
)
SELECT
    -- BigQuery : Concat 함수의 매개변수를 string 으로 변환해야 함
    CONCAT('r_', CAST(r AS string))  AS r_rank
    --, CONCAT('r_', r AS string)  AS r_rank
    , COUNT(CASE WHEN f = 5 THEN 1 END) AS f_5
    , COUNT(CASE WHEN f = 4 THEN 1 END) AS f_4
    , COUNT(CASE WHEN f = 3 THEN 1 END) AS f_3
    , COUNT(CASE WHEN f = 2 THEN 1 END) AS f_2
    , COUNT(CASE WHEN f = 1 THEN 1 END) AS f_1
FROM user_rmf_rank GROUP BY r
ORDER BY r_rank DESC
;
```

**어떤 대책을 실시할지 생각해보기**
{{<figure src="/posts/images/sql-recipe/rfm-3.png#center">}}
