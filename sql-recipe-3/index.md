# 데이터 분석을 위한 SQL 레시피 - 3장 데이터 가공을 위한 SQL


> 🗄️ 데이터분석을 위한 SQL 레시피 책을 읽고 정리 / 요약 한 내용입니다.

{{< admonition type=success title="데이터를 가공해야 하는 이유" open=true >}}
1. 다룰 데이터가 데이터 분석 용도로 상정되지 않은 경우
2. 연산할 때 비교 가능한 상태를 만들고 오류를 회피하기 위한 경우
{{< /admonition >}}

## 5. 하나의 값 조작하기

### 5.1 코드 값을 레이블로 변경하기
디폴트 값을 지정할 경우 ELSE 구문을 사용한다.
```sql
SELECT
     user_id
   , CASE
       WHEN register_device = 1 THEN '데스크톱'
       WHEN register_device = 2 THEN '스마트폰'
       WHEN register_device = 3 THEN '애플리케이션'
       ELSE ''
     END as device_name
FROM mst_users;
```

### 5.2 URL 요소 추출하기
{{< admonition type=abstract title="접근 로그 테이블" open=false >}}
| stamp | referrer | url |
| :--- | :--- | :--- |
| 2016-08-26 12:02:00 | http://www.other.com/path1/index.php?k1=v1&k2=v2#Ref1 | http://www.example.com/video/detail?id=001 |
| 2016-08-26 12:02:01 | http://www.other.net/path1/index.php?k1=v1&k2=v2#Ref1 | http://www.example.com/video#ref |
| 2016-08-26 12:02:01 | https://www.other.com/ | http://www.example.com/book/detail?id=002 |
{{< /admonition >}}

#### 레퍼러로 어떤 웹 페이지를 거쳐 넘어왔는지 판별하기
```sql
SELECT
  stamp
  -- PostgreSQLdml ruddn substring 함수와 정규 표현식 사용
  , substring(referrer from 'https?://([^/]*)') AS referrer_host
  
  -- Redshift의 경우, 정규 표현식에 그룹을 사용할 수 없으므로,
  -- regexp_substr 함수와 regexp_replace 함수를 조합
  , regexp_replace(regexp_substr(referrer, 'https?://[^/]*'), 'https?://', '') AS referrer_host

  -- Hive, SparkSQL의 경우, parse_url 함수로 호스트이름 추출
  , parse_url(referrer, 'HOST') AS referrer_host

  -- BigQuery의 경우, host 함수 사용
  , host(referrer) AS referrer_host

FROM access_log
```

{{< admonition type=example title="실행결과" open=false >}}
| stamp | referrer\_host |
| :--- | :--- |
| 2016-08-26 12:02:00 | www.other.com |
| 2016-08-26 12:02:01 | www.other.net |
| 2016-08-26 12:02:01 | www.other.com |
{{< /admonition >}}

#### URL에서 경로와 요청 매개변수 값 추출하기

```sql
SELECT
  stamp
  , url

  -- PostgreSQL의 경우 substring 함수와 정규 표현식 사용
  , substring(url from '//[^/]+([^?#]+)') AS path
  , substring(url from 'id=([^&]*)') AS id

  -- Redshift의 경우 regexp_substr 함수와 regexp_replace 함수를 조합하여 사용
  , regexp_replace(regexp_substr(url, '//[^/]+[^?#]+'), '//[^/]+', '') AS path
  , regexp_replace(regexp_substr(url, 'id=[^&]*'), 'id=', '') AS id

  -- BigQuery의 경우 정규 표현식과 regexp_extract 함수 사용
  , regexp_extract(url, '//[^/]+([^&#]+)') AS path
  , regexp_extract(url, 'id=([^&]*)') AS id

  -- Hive, SparkSQL의 경우 parse_url 함수로 url 경로 / 쿼리 매개변수 추출
  , parse_url(url, 'PATH') AS path
  , parse_url(url, 'QUERY', 'id') As id

FROM access_log;
```

{{< admonition type=example title="실행결과" open=false >}}
| stamp | url | path | id |
| :--- | :--- | :--- | :--- |
| 2016-08-26 12:02:00 | http://www.example.com/video/detail?id=001 | /video/detail | 001 |
| 2016-08-26 12:02:01 | http://www.example.com/video#ref | /video | null |
| 2016-08-26 12:02:01 | http://www.example.com/book/detail?id=002 | /book/detail | 002 |
{{< /admonition >}}


### 5.3 문자열을 배열로 분해하기
- 빅데이터 분석시 가장 많이 사용되는 자료형은 문자열
- 문자열 자료형은 범용적이므로 세부적으로 분해해서 사용하는 경우가 많음
```sql
SELECT
  stamp
  , url

  -- PostgreSQL의 경우, split_part로 n번째 요소 추출
  , split_part(substring(url from '//[^/]+([^?#]+)'), '/', 2) AS path1
  , split_part(substring(url from '//[^/]+([^?#]+)'), '/', 3) AS path2

  -- Redshift도 split_part로 n번째 요소 추출
  , split_part(regexp_replace(regexp_substr(url, '//[^/]+[^?#]+'), '//[^/]+', ''), '/', 2) AS path1
  , split_part(regexp_replace(regexp_substr(url, '//[^/]+[^?#]+'), '//[^/]+', ''), '/', 3) AS path2

  -- BigQuery의 경우 split 함수를 사용하여 배열로 자름(별도 인덱스 지정 필요)
  , split(regexp_extract(url, '//[^/]+([^&#]+)'), '/')[SAFE_ORDINAL(2)] AS path1
  , split(regexp_extract(url, '//[^/]+([^&#]+)'), '/')[SAFE_ORDINAL(3)] AS path2

  -- Hive, SparkSQL도 split 함수를 사용하여 배열로 자름
  , split(parse_url(url, 'PATH') , '/')[1] AS path1
  , split(parse_url(url, 'PATH') , '/')[2] AS path2

FROM access_log
```

{{< admonition type=example title="실행결과" open=false >}}
| stamp | url | path1 | path2 |
| :--- | :--- | :--- | :--- |
| 2016-08-26 12:02:00 | http://www.example.com/video/detail?id=001 | video | detail |
| 2016-08-26 12:02:01 | http://www.example.com/video#ref | video |  |
| 2016-08-26 12:02:01 | http://www.example.com/book/detail?id=002 | book | detail |
{{< /admonition >}}

- `Redshift`는 배열자료형을 지원하지 않음
- `BigQuery`는 배열의 인덱스를 0부터 시작하려면 `OFFSET`, 1부터 시작하려면 `ORDINAL`을 지정
  - 배열 길이 이상의 인덱스에 접근하면 오류를 리턴하며 NULL을 리턴하고 싶은 경우 `SAFE_OFFSET` 또는 `SAFE_ORDINAL`을 지정

### 5.4 날짜와 타임스탬프 다루기
- 날짜와 시간 정보는 로그 데이터에서 빠지지 않는 정보
- **타임존을 고려해야 하고 미들웨어 차이를 주의**

#### 현재 날짜와 타임스탬프 추출하기
- `PostgreSQL`을 제외한 미들웨어는 타임존 없는 타임스템프 리턴
- `BigQuery`는 UTC로 리턴됨
```sql

SELECT
  -- PostgreSQL, Hive, BigQuery의 경우 : CURRENT_DATE, CURRENT_TIMESTAMP 상수 사용
    CURRENT_DATE AS dt
  , CURRENT_TIMESTAMP AS stamp

  -- Hive, BigQuery, SparkSQL : CURRENT_DATE(), CURRENT_TIMESTAMP() 함수 사용
    CURRENT_DATE() AS dt
  , CURRENT_TIMESTAMP() AS stamp

  -- Redshift : 현재 날짜는 CURRENT_DATE, 현재 타임 스탬프는 GETDATE() 사용
    CURRENT_DATE AS dt
  , GETDATE() AS stamp

  -- PostgreSQL, CURRENT_TIMESTAMP, timezone이 적용된 타임스탬프
  -- 타임존을 적용하고 싶지 않을 때, LOCALTIMESTAMP 사용
  , LOCALTIMESTAMP AS stamp
```

{{< admonition type=example title="실행결과" open=false >}}
| dt | stamp |
| :--- | :--- |
| 2023-11-21 | 2023-11-21 11:27:01.452351 +00:00 |
{{< /admonition >}}

#### 지정한 값의 날짜/시각 데이터 추출
- 문자열로 지정한 날짜와 시각은 CAST 함수를 사용하는 방법이 범용적
```sql
-- 타임스탬프 자료형의 데이터에서 연, 월, 일 등을 추출하는 쿼리
SELECT
  -- PostgreSQL, Hive, Redshift, Bigquery, SparkSQL : `CAST(value AS type)` 사용
    CAST('2016-01-30' AS date) AS dt
  , CAST('2016-01-30 12:00:00' AS timestamp) AS stamp

  -- Hive, Bigquery : `type(value)` 사용
    date('2016-01-30') AS dt
  , timestamp('2016-01-30 12:00:00') AS stamp

  -- PostgreSQL, Hive, Redshift, BigQuery, SparkSQL : `type value` 사용
  -- 단, value는 상수이므로, 컬럼 이름 지정 불가능
    date '2016-01-30' AS dt
  , timestamp '2016-01-30 12:00:00' AS stamp

  -- PostgreSQL, Redshift : `value::type` 사용
    '2016-01-30'::date AS dt
  , '2016-01-30 12:00:00'::timestamp AS stamp
```

{{< admonition type=example title="실행결과" open=false >}}
| dt | stamp |
| :--- | :--- |
| 2016-01-30 | 2016-01-30 12:00:00.000000 |
{{< /admonition >}}

#### 날짜/시각에서 특정 필드 추가하기
```sql
SELECT
  stamp
  -- PostgreSQL, Redshift, BigQuery : EXTRACT 함수 사용
  , EXTRACT(YEAR  FROM stamp) AS year
  , EXTRACT(MONTH FROM stamp) AS month
  , EXTRACT(DAY   FROM stamp) AS day
  , EXTRACT(HOUR  FROM stamp) AS hour

  -- Hive, SparkSQL
  , YEAR(stamp)   AS year
  , MONTH(stamp)  AS month
  , DAY(stamp)    AS day
  , HOUR(stamp)   AS hour
FROM
  (SELECT CAST('2020-01-16 22:22:00' AS timestamp) AS stamp) AS t
```
{{< admonition type=example title="실행결과" open=false >}}
| stamp | year | month | day | hour |
| :--- | :--- | :--- | :--- | :--- |
| 2016-01-30 12:00:00.000000 | 2016 | 1 | 30 | 12 |
{{< /admonition >}}

```sql
SELECT
  stamp

  -- PostgreSQL, Hive, Redshift, SparkSQL : substring 함수 사용
  , substring(stamp, 1, 4)  AS year
  , substring(stamp, 6, 2)  AS month
  , substring(stamp, 9, 2)  AS day
  , substring(stamp, 12, 2) AS hour
  -- 연, 월을 함께 추출
  , substring(stamp, 1, 7)  AS year_month

  --- PostgreSQL, Hive, BigQuery, SparkSQL : substr 함수 사용
  , substr(stamp, 1, 4)   AS year
  , substr(stamp, 6, 2)   AS month
  , substr(stamp, 9, 2)   AS day
  , substr(stamp, 12, 2)  AS hour
  , substr(stamp, 1, 7)   AS year_month
FROM
  -- PostgreSQL, Redshift의 경우 문자열 자료형(text)
  (SELECT CAST('2020-01-16 22:26:00' AS text) AS stamp) AS t

  -- Hive, BigQuery, SparkSQL의 경우 문자열 자료형(string)
  (SELECT CAST('2020-01-16 22:26:00' AS string) AS stamp) AS t
```
{{< admonition type=example title="실행결과" open=false >}}
| stamp | year | month | day | hour | year\_month |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 2016-01-30 12:00:00 | 2016 | 01 | 30 | 12 | 2016-01 |
{{< /admonition >}}

### 5.5 결손 값을 디폴트 값으로 대치하기
- 문자열과 숫자를 다룰때는 NULL 값에 주의
- **처리 대상 데이터가 원하는 형태가 아닐 경우 반드시 데이터를 가공**

{{< admonition type=abstract title="쿠폰 사용 여부가 함께 있는 구매로그 테이블" open=false >}}
| purchase\_id | amount | coupon |
| :--- | :--- | :--- |
| 10001 | 3280 | null |
| 10002 | 4650 | 500 |
| 10003 | 3870 | null |
{{< /admonition >}}

```sql
SELECT
  purchase_id
  , amount
  , coupon
  , amount - coupon AS discount_amount1
  , amount - COALESCE(coupon, 0) AS discount_amount2
FROM
  purchase_log_with_coupon

```
{{< admonition type=example title="실행결과" open=false >}}
| purchase\_id | amount | coupon | discount\_amount | discount\_amount2 |
| :--- | :--- | :--- | :--- | :--- |
| 10001 | 3280 | null | null | 3280 |
| 10002 | 4650 | 500 | 4150 | 4150 |
| 10003 | 3870 | null | null | 3870 |
{{< /admonition >}}

## 6. 여러 개의 값에 대한 조작

{{< admonition type=success title="새로운 지표 정의하기" open=true >}}
- 여러 지표를 기반으로 새로운 지표를 정의 할 수 있음
  - `페이지 뷰` / `방문자 수` = `사용자 한 명이 페이지를 몇 번이나 방문 했는가?`
- 방문한 사용자 중 특정 행동(클릭 또는 구매 등)을 한 사용자의 비율
  - **CTR(Click Through Rate)** : 클릭율
  - **CVR(Conversion Rate)** : 전환율 
{{< /admonition >}}

### 6.1 문자열 연결하기

{{< admonition type=abstract title="사용자 주소 정보 테이블" open=false >}}
| user\_id | pref\_name | city\_name |
| :--- | :--- | :--- |
| U001 | 서울특별시 | 강서구 |
| U002 | 경기도수원시 | 장안구 |
| U003 | 제주특별자치도 | 서귀포시 |
{{< /admonition >}}

**문자열을 연결하는 쿼리**
```sql
SELECT
  user_id

  -- PostgreSQL, Hive, Redshift, BigQuery, SparkSQL : 모두 CONCAT 함수 사용 가능
  -- 다만 redshift의 경우는 매개변수를 2개밖에 못받는다
  , CONCAT(pref_name, city_name) AS pref_city
  
  -- PostgreSQL, Redshift의 경우 || 연산자 사용 가능
  , pref_name || city_name AS pref_City
FROM
  mst_user_location
```

{{< admonition type=example title="실행결과" open=false >}}
| user\_id | perf\_city | pref\_city2 |
| :--- | :--- | :--- |
| U001 | 서울특별시강서구 | 서울특별시강서구 |
| U002 | 경기도수원시장안구 | 경기도수원시장안구 |
| U003 | 제주특별자치도서귀포시 | 제주특별자치도서귀포시 |
{{< /admonition >}}

### 6.2 여러 개의 값 비교하기
{{< admonition type=abstract title="4분기 매출 테이블" open=false >}}
| year | q1 | q2 | q3 | q4 |
| :--- | :--- | :--- | :--- | :--- |
| 2015 | 82000 | 83000 | 78000 | 83000 |
| 2016 | 85000 | 85000 | 80000 | 81000 |
| 2017 | 92000 | 81000 | null | null |
{{< /admonition >}}

#### 분기별 매출 증감 판정하기
- `SIGN` : 매개변수 양수이면 1, 0이면 0, 음수 -1을 리턴하는 함수

```sql
SELECT
    year
  , q1
  , q2
  
  -- q1과 q2의 매출변화 평가
  , CASE
      WHEN q1 < q2 THEN '+'
      WHEN q1 = q2 THEN ' '
      ELSE '-'
    END AS judge_q1_q2
  
  -- q1, q2의 매출액 차이 계싼
  , q2 - q1 AS diff_q2_q1
  
  -- q1과 q2의 매출 변화를 1, 0, -1로 표현
  , SIGN(q2 - q1) AS sign_q2_q1
FROM quarterly_Sales
ORDER BY year
```
{{< admonition type=example title="실행결과" open=false >}}
| year | q1 | q2 | judge\_q1\_q2 | diff\_q2\_q1 | sign\_q1\_q2 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 2015 | 82000 | 83000 | + | 1000 | 1 |
| 2016 | 85000 | 85000 |   | 0 | 0 |
| 2017 | 92000 | 81000 | - | -11000 | -1 |
{{< /admonition >}}

#### 연간 최대/최소 4분기 매출 찾기
- 여러 컬럼에서 최댓값, 최솟값 찾기 : greatest, least
- greatest, least 함수 둘다 SQL 표준에는 포함되지 않지만 대부분의 SQL 쿼리 엔진에서 구현

```sql
SELECT
    year
  -- q1 ~ q4의 최대 매출 구하기
  , greatest(q1, q2, q3, q4) AS greatest_sales
  
  -- q1 ~ q4의 최소 매출 구하기
  , least(q1, q2, q3, q4) AS least_sales
FROM quarterly_sales
ORDER BY year
```
{{< admonition type=example title="실행결과" open=false >}}
| year | greatest\_sales | least\_sales |
| :--- | :--- | :--- |
| 2015 | 83000 | 78000 |
| 2016 | 85000 | 80000 |
| 2017 | 92000 | 81000 |
{{< /admonition >}}

#### 연간 평균 4분기 매출 계산하기

**단순한 연산으로 평균 4분기 매출을 구하는 쿼리**
- NULL 값에 대해 `COALESCE` 함수를 사용해 적절한 값으로 변환
```sql
SELECT
    year
  , (q1 + q2 + q3 + q4) / 4 AS average
FROM quarterly_sales
ORDER BY year
```
{{< admonition type=example title="실행결과" open=false >}}
| year | average |
| :--- | :--- |
| 2015 | 81500 |
| 2016 | 82750 |
| 2017 | null |
{{< /admonition >}}

**COALESCE를 사용해 NULL을 0으로 변환하고 평균값을 구하는 쿼리**
- q3, q4를 매출을 0으로 변환하면 q1, q2 의 매출 합계를 4로 나누게 되어 평균값이 낮아짐
```sql
SELECT year
  , (COALESCE(q1, 0) + COALESCE(q2,0) + COALESCE(q3,0) + COALESCE(q4,0)) / 4 as average
FROM quarterly_sales
ORDER BY year
```
{{< admonition type=example title="실행결과" open=false >}}
| year | average |
| :--- | :--- |
| 2015 | 81500 |
| 2016 | 82750 |
| 2017 | 43250 |
{{< /admonition >}}

**NULL이 아닌 컬럼만 사용해서 평균값을 구한는 쿼리**
- 2017년의 q1, q2의 매출로만 평균을 구하려면 NULL 이 아닌 컬럼의 수를 세서 나눠야함 
- COALESCE, SIGN 함수를 사용하여 NULL이 아닌 컬럼의 수를 셀 수 있다.
```sql
SELECT year
  , (COALESCE(q1, 0) + COALESCE(q2,0) + COALESCE(q3,0) + COALESCE(q4,0)) / (SIGN(COALESCE(q1, 0)) + SIGN(COALESCE(q2,0)) + SIGN(COALESCE(q3,0)) + SIGN(COALESCE(q4,0))) as average
FROM quarterly_sales
ORDER BY year
```

{{< admonition type=example title="실행결과" open=false >}}
| year | average3 |
| :--- | :--- |
| 2015 | 81500 |
| 2016 | 82750 |
| 2017 | 86500 |
{{< /admonition >}}


### 6.3 2개의 값 비율 계산하기

{{< admonition type=abstract title="광고 통계 정보 테이블" open=false >}}
| dt | ad\_id | impressions | clicks |
| :--- | :--- | :--- | :--- |
| 2017-04-01 | 001 | 100000 | 3000 |
| 2017-04-01 | 002 | 120000 | 1200 |
| 2017-04-01 | 003 | 500000 | 10000 |
| 2017-04-02 | 001 | 0 | 0 |
| 2017-04-02 | 002 | 130000 | 1400 |
| 2017-04-02 | 003 | 620000 | 15000 |
{{< /admonition >}}

#### 정수 자료형의 데이터 나누기
- 데이터에서 각 광고의 **CTR (클릭수 / 노출 수)을 계산**
- 실수를 상수로 앞에 두고 계산하면, 암묵적으로 자료형 변환 
  - 나는 해당 결과가 실수가 안나옴

```sql

SELECT
    dt
  , ad_id

  -- Hive, Redshift, Bigquery, SparkSQL
  -- 정수를 나눌때, 자동으로 실수형 변환
  , clicks / impressions AS ctr
  
  -- PostgreSQL, 정수 나눌경우, 소수점이 잘리므로, 명시적으로 자료형 변환
  , CAST(clicks AS double precision) / impressions AS ctr
  
  -- 실수를 상수로 앞에 두고 계산하면, 암묵적으로 자료형 변환
  , 100.0 * clicks / impressions AS ctr_as_percent
FROM advertising_stats
WHERE dt = '2017-04-01'
ORDER BY dt, ad_id
```

{{< admonition type=example title="실행결과" open=false >}}
| year | average3 |
| :--- | :--- |
| 2015 | 81500 |
| 2016 | 82750 |
| 2017 | 86500 |
{{< /admonition >}}

#### 0 으로 나누는 것 피하기
- NULL 전파를 사용하면 0으로 나누는 것을 피할 수 있다.
- NULL 전파란 연산 결과 모두가 NULL이 되는 SQL 성질
  - `NULLIF(impressions, 0)` : impressions 값이 0이면 NULL로 처리
```sql
SELECT
    dt
  , ad_id
  
  -- CASE 식으로 분모가 0일 경우를 분기, 0으로 나누지 않도록 함
  , CASE
       WHEN impressions > 0 THEN 100.0 * clicks / impressions
    END AS ctr_as_percent_by_case
  
  -- 분모가 0이라면 NULL로 변환하여, 0으로 나누지 않도록 함
  -- PostgreSQL, Redshift, BigQuery, SparkSQL의 경우 NULLIF 함수 사용
  , 100.0 * clicks / NULLIF(impressions, 0) AS ctr_as_percent_by_null

  -- Hive의 경우 NULLIF 대신 CASE식 사용하기
  , 100*0 * clicks / CASE WHEN impressions = 0 THEN NULL ELSE impressions END
FROM advertising_stats
ORDER_BY dt, ad_id;
```

### 6.4 두 값의 거리 계산하기
- 데이터 분석 분야에서는 물리적 공간 길이가 아닌 거리라는 개념이 많이 등장
  - 시험을 보았을 때 평균에서 어느정도 떨어져 있는가?
  - 작년 매출과 올해 매출에 어느 정도의 차이가 있는가
  - 어떤 사용자가 있을 때, 해당 사용자와 구매 경향이 비슷한 사용자를 뽑을 때도 사용

#### 숫자 데이터의 절대값, 제곱 평균 제곱근(RMS) 계산하기
{{< admonition type=abstract title="일차원 위치 정보 테이블" open=false >}}
| x1 | x2 |
| :--- | :--- |
| 5 | 10 |
| 10 | 5 |
| -2 | 4 |
| 3 | 3 |
| 0 | 1 |
{{< /admonition >}}

- x1, x2의 거리를 절댓값을 사용
- 평균 제곱근 : 두 값의 차이를 제곱한 뒤 제곱근을 적용
- **ABS** : 절대값 계산
- **POWER** : 제곱 계산
- **SQRT** : 제곱근 계산

```sql
SELECT
    abs(x1- x2) as abs
  , sqrt(power(x1 - x2, 2)) as rms
FROM location_1d;
```
{{< admonition type=example title="실행결과" open=false >}}
| abs | rms |
| :--- | :--- |
| 5 | 5 |
| 5 | 5 |
| 6 | 6 |
| 0 | 0 |
| 1 | 1 |
{{< /admonition >}}

#### xy 평면 위에 있는 두 점의 유클리드 거리 계산하기
{{< admonition type=abstract title="이차원 위치 정보 테이블" open=false >}}
| x1 | y1 | x2 | y2 |
| :--- | :--- | :--- | :--- |
| 0 | 0 | 2 | 2 |
| 3 | 5 | 1 | 2 |
| 5 | 3 | 2 | 1 |
{{< /admonition >}}

- xy 평면위에 존재하는 두 점 (x1, y1), (x2, y2) 사이의 유클리드 거리 계산
- **PostgreSQL에는 POINT** 자료형으로 좌표를 다루는 자료구조가 존재
  - POINT 자료형으로 변환한뒤 거리 연산자 `<->` 을 이용

```sql
SELECT
    sqrt(power(x1 - x2, 2) + power(y1 - y2)) AS dist
  -- PostgreSQL, point 자료형과 거리 연산자 (<->) 사용
  , point(x1, y1) <-> point(x2, y2) AS dist
FROM location_2d;
```
{{< admonition type=example title="실행결과" open=false >}}
| dist |
| :--- |
| 2.8284271247461903 |
| 3.605551275463989 |
| 3.605551275463989 |
{{< /admonition >}}

### 6.5 날짜/시간 계산하기
{{< admonition type=abstract title="등록 시간과 생일을 포함하는 사용자 마스터" open=false >}}
| user\_id | register\_stamp | birth\_date |
| :--- | :--- | :--- |
| U001 | 2016-02-28 10:00:00 | 2000-02-29 |
| U002 | 2016-02-29 10:00:00 | 2000-02-29 |
| U003 | 2016-03-01 10:00:00 | 2000-02-29 |
{{< /admonition >}}

**미래 또는 과거의 날짜/시간을 계산하는 쿼리**
```sql
SELECT
    user_id
  -- PostgreSQL, interval 자료형의 데이터에 사칙 연산 적용
  , register_stamp::timestamp                           AS register_stamp
  , register_stamp::timestamp + '1 hour'::interval      AS after_1_hour
  , register_stamp::timestamp - '30 minutes'::interval  AS berfore_30_minutes

  , register_stamp::date                                AS register_date
  , (register_stamp::date + '1 day'::interval)::date    AS after_1_day
  , (register_stamp::date - '1 month'::interval)::date  AS before_1_month

  -- Redshift, dateadd 함수 사용
  , register_stamp::timestamp                           AS register_stamp
  , dateadd(hour, 1 ,register_stamp::timestamp)         AS after_1_hour
  , dateadd(monute, -30, register_stamp::timestamp)     AS before_30_minutes

  , register_stamp::date                                AS register_date
  , dateadd(day, 1, register_stamp::date)               AS after_1_day
  , dateadd(month, -1, register_stamp:date)             AS before_1_month

  -- BigQuery, timestamp_add/sub, date_add/sub 함수 사용
  , timestamp(register_stamp)                                     AS register_stamp
  , timestamp_add(timestamp(register_stamp), interval 1 hour)     AS after_1_hour
  , timestamp_add(timestamp(register_stamp), interval 30 minute)  AS before_30_minutes

  -- 타임스탬프 문자열 기반으로 직접 날짜 계산을 할 수 없으므로
  -- 타임 스탬프 자료형 -> 날짜/시간 자료형 변환 뒤 계산
  , date(timestamp(register_stamp))                             AS register_date
  , date_add(date(timestamp(register_stamp)), interval 1 day)   AS after_1_day
  , date_sub(date(timestamp(register_stamp)), interval 1 month) AS before_1_month

  -- Hive, SparkSQL, 날짜/시각 계산 함수 제공 x
  -- unixtime으로 변환 후, 초단위로 계산 적용뒤 다시 타임스탬프로 변환
  , CAST(register_stamp AS timestamp)                       AS register_stamp
  , from_unixtime(unix_timestamp(register_stamp) + 60 * 60) AS after_1_hour
  , from_unixtime(unix_timestamp(register_stamp) - 30 * 60) AS before_30_minutes

  --- 타임스탬프 문자열을 날짜 변환시, to_date 함수 사용
  -- 단, hive 2.1.0 이전 버전의 경우, 문자열 자료형 리턴
  , to_date(register_stamp) AS register_date

  -- day/month 계산 시, date_add / date_months 함수 사용
  -- 단, year 계산 함수는 제공되지 않음
  , date_add(to_date(regsiter_stamp), 1)    AS after_1_day
  , add_months(to_date(register_stamp), -1) AS before_1_month
FROM mst_users_with_dates
```

#### 날짜 데이터의 차이 계산하기
- 날짜/시간 데이터의 계산은 미들웨어에 따라 표현에 차이가 큼
- 실무에서는 날짜/시간 데이터는 수치 또는 문자열 등으로 변환해 다루는 것이 편한 경우가 많음
```sql
SELECT
  user_id

  -- PostgreSQL, Redshift, 날짜 자료형 끼리 연산 가능
  , CURRENT_DATE                        AS today
  , register_stamp::date                AS register_date
  , CURRENT_DATE - register_stamp::date AS diff_days

  -- BigQuery의 경우 date_diff 함수 사용
  , CURRENT_DATE                                                  AS today
  , date(timestamp(register_stamp))                               AS register_date
  , date_diff(CURRENT_DATE, date(timestamp(register_stamp)), day) AS diff_Days

  -- Hive, SparkSQL의 경우 datediff 함수 사용
  , CURRENT_DATE()                                    AS today
  , to_date(register_stamp)                           AS register_date
  , datediff(CURRENT_DATE(), to_date(register_stamp)) AS diff_days
FROM mst_users_with_dates
```

#### 사용자의 생년월일로 나이 계산하기
- 나이는 윤년 등으로 단순히 365로 나누어 계산 할 수 없음
- PostgreSQL의 경우 `age` 함수로 나이 계산 가능 (Redshift도 age 함수가 존재하나 공식지원은 아님)

**age 함수를 사용해 나이를 계산하는 쿼리**
```sql
SELECT
  user_id

  -- PostgreSQL, age 함수와 EXTRACT 함수를 이용하여 나이 집계
  , CURRENT_DATE          AS today
  , regsiter_stamp::date  AS register_date
  , birth_date::date      AS birth_date
  , EXTRACT(YEAR FROM age(birth_date::date)) AS current_age
  , EXTRACT(YEAR FROM age(register_stamp::date, birth_date::date)) AS register_age
FROM mst_users_with_dates;
```
{{< admonition type=example title="실행결과" open=false >}}
| user\_id | today | register\_date | birth\_date | current\_age | register\_age |
| :--- | :--- | :--- | :--- | :--- | :--- |
| U001 | 2023-11-21 | 2016-02-28 | 2000-02-29 | 23 | 15 |
| U002 | 2023-11-21 | 2016-02-29 | 2000-02-29 | 23 | 16 |
| U003 | 2023-11-21 | 2016-03-01 | 2000-02-29 | 23 | 16 |
{{< /admonition >}}

**연 부분 차이를 계산하는 쿼리**
```sql
SELECT
  user_id

  -- Redshift, datediff 함수로 year을 지정하더라도, 연 부분 차이는 계산 불가
  , CURRENT_DATE          AS today
  , register_stamp::date  AS register_date
  , birth_date::date      AS birth_date
  , datediff(year, birth_date::date, CURRENT_DATE)
  , datediff(year, birth_date::date, register_stamp::date)

  -- BigQuery, date_diff 함수로 year 지정시에도, 연 부분 차이 계산 불가
  , CURRENT_DATE                    AS today
  , date(timestamp(register_stamp)) AS register_date
  , date(timestamp(birth_date))     AS birth_date
  , date_diff(CURRENT_DATE, date(timestamp(birth_date)), year)                    AS current_age
  , date_diff(date(timestamp(register_stamp)), date(timestamp(birth_date)), year) AS register_age
FROM mst_users_with_dates;
```
**등록 시점과 현재 시점의 나이를 문자열로 계산하는 쿼리**
```sql

SELECT
  user_id
  , substring(register_stamp, 1, 10) AS register_date
  , birth_date

  -- 등록 시점의 나이 계산
  , floor(
    ( CAST(replace(substring(register_stamp, 1, 10), '-'. '') AS integer)
      - CAST(replace(birth_date, '-', '') AS integer)
      ) / 10000
  ) AS register_age

  -- 현재 시점의 나이 계산
  , floor (
    ( CAST(replace(CAST(CURRENT_DATE as text), '-', '') AS integer)
      - CAST(replace(birth_datey, '-', '') AS integer)
    ) / 10000
  ) AS current_age

  -- BigQuery, text -> string, integer -> int64
  ( CAST(replace(CAST(CURRENT_DATE AS string), '-', '') AS int64)
    - CAST(replace(birth_date, '-', '') AS int64)
  ) / 10000

  -- Hive, SparkSQL, replace -> regexp_replace, text -> string
  -- integer -> int
  -- SparkSQL, CURRENT_DATE -> CURRENT_DATE()
  ( CAST(regexp_replace(CAST(CURRENT_DATE() AS string), '-', '') AS int)
    - CAST(regexp_replace(birth_date, '-', '') AS int)
  ) / 10000
FROM mst_users_with_dates
;
```
{{< admonition type=example title="실행결과" open=false >}}
| user\_id | register\_date | birth\_date | register\_age | register\_age | t |
| :--- | :--- | :--- | :--- | :--- | :--- |
| U001 | 2016-02-28 | 2000-02-29 | 15 | 23 | 20231121 |
| U002 | 2016-02-29 | 2000-02-29 | 16 | 23 | 20231121 |
| U003 | 2016-03-01 | 2000-02-29 | 16 | 23 | 20231121 |
{{< /admonition >}}

### 6.6 IP 주소 다루기

#### IP 주소 자료형 활용하기
- PostgreSQL에는 IP 주소를 다루기 위한 inet 자료형이 존재
- inet 자료형을 사용하여 IP 주소를 쉽게 비교 가능

```sql
SELECT
    CAST('127.0.0.1' as inet) < CAST('127.0.0.2' as inet) as lt
  , CAST('127.0.0.1' as inet) > CAST('192.168.0.1' as inet) as gt
;
```

{{< admonition type=example title="실행결과" open=false >}}
| lt | gt |
| :--- | :--- |
| true | false |
{{< /admonition >}}

**inet 자료형을 사용해 IP 주소 범위를 다루는 쿼리**
```sql
SELECT CAST('127.0.0.1' as inet) << CAST('127.0.0.0/8' as inet) as is_contained;
```
{{< admonition type=example title="실행결과" open=false >}}
| is\_contained |
| :--- |
| true |
{{< /admonition >}}

#### 정수 또는 문자열로 IP 주소 다루기
- IP 주소 전용 자료형이 제공되지 않는 미들웨어에 사용

**IP 주소를 정수 자료형으로 변환하기**
- IP 주소를 정수 자료형으로 변경하면 대소 비교가 가능
- 텍스트 자료형으로 정의된 IP 주소에 4개의 10진수 부분을 정수 자료형으로 추출
```sql
SELECT
  ip

  -- PostgreSQL, Redshift의 경우 splift_part로 문자열 분해
  , CAST(split_part(ip, '.', 1) AS integer) AS ip_part_1
  , CAST(split_part(ip, '.', 2) AS integer) AS ip_part_2
  , CAST(split_part(ip, '.', 3) AS integer) AS ip_part_3
  , CAST(split_part(ip, '.', 4) AS integer) AS ip_part_4

  -- BigQuer, split 함수로 배열 분해, n번째 요소 추출
  , CAST(split(ip, '.')[SAFE_ORDINAL(1)] AS int64) AS ip_part_1
  , CAST(split(ip, '.')[SAFE_ORDINAL(2)] AS int64) AS ip_part_2
  , CAST(split(ip, '.')[SAFE_ORDINAL(3)] AS int64) AS ip_part_3
  , CAST(split(ip, '.')[SAFE_ORDINAL(4)] AS int64) AS ip_part_4

  -- Hive, SparkSQL, split 함수로 배열 분해, n번째 요소 추출
  -- 이때 '.'가 특수문자이므로, \로 escaping
  , CAST(split(ip, '\\.')[0] AS int) AS ip_part_1
  , CAST(split(ip, '\\.')[1] AS int) AS ip_part_2
  , CAST(split(ip, '\\.')[2] AS int) AS ip_part_3
  , CAST(split(ip, '\\.')[3] AS int) AS ip_part_4
FROM
  (SELECT '192.168.0.1' AS ip) AS t
  
  -- PostgreSQL의 경우 명시적 자료형 변환
  (SELECT CAST('192.168.0.1' AS text) AS ip) AS t
;
```

{{< admonition type=example title="실행결과" open=false >}}
| ip | ip\_part\_1 | ip\_part\_2 | ip\_part\_3 | ip\_part\_4 |
| :--- | :--- | :--- | :--- | :--- |
| 192.168.0.1 | 192 | 168 | 0 | 1 |
{{< /admonition >}}


**IP 주소를 정수 자료형 표기로 변환하는 쿼리**
- 4개의 10진수 부분을 2^24, 2^16, 2^8, 2^0 만큼 곱하고 더하여 정수 자료형으로 표기
- IP 주소가 정수 자료형으로 변환되므로 대소 비교 또는 범위 판정 가능
```sql
SELECT
  ip
  -- PostgreSQL, Redshift의 경우 splift_part로 문자열 분해
  , CAST(split_part(ip, '.', 1) AS integer) * 2^24
    + CAST(split_part(ip, '.', 2) AS integer) * 2^16
    + CAST(split_part(ip, '.', 3) AS integer) * 2^8
    + CAST(split_part(ip, '.', 4) AS integer) * 2^0
  AS ip_integer

  -- BigQuer, split 함수로 배열 분해, n번째 요소 추출
  , CAST(split(ip, '.')[SAFE_ORDINAL(1)] AS int64) * pow(2, 24)
    + CAST(split(ip, '.')[SAFE_ORDINAL(2)] AS int64) * pow(2, 16)
    + CAST(split(ip, '.')[SAFE_ORDINAL(3)] AS int64) * pow(2, 8)
    + CAST(split(ip, '.')[SAFE_ORDINAL(4)] AS int64) * pow(2, 0)
  AS ip_integer

  -- Hive, SparkSQL, split 함수로 배열 분해, nq번째 요소 추출
  -- 이때 '.'가 특수문자이므로, \로 escaping
  , CAST(split(ip, '\\.')[0] AS int) * pow(2, 24)
    + CAST(split(ip, '\\.')[1] AS int) * pow(2, 16)
    + CAST(split(ip, '\\.')[2] AS int) * pow(2, 8)
    + CAST(split(ip, '\\.')[3] AS int) * pow(2, 0)
  AS ip_integer
FROM
  (SELECT '192.168.0.1' AS ip) AS t
  
  -- PostgreSQL의 경우 명시적 자료형 변환
  (SELECT CAST('192.168.0.1' AS text) AS ip) AS t
;
```
{{< admonition type=example title="실행결과" open=false >}}
| ip | ip\_integer |
| :--- | :--- |
| 192.168.0.1 | 3232235521 |
{{< /admonition >}}

**IP 주소를 0으로 메우기**
- 각 10진수 부분을 3자리 숫자가 되게 앞 부분을 0으로 채워 문자열로 변환
```sql
SELECT
  ip

  -- PostgreSQL, Redshift, lpad 함수로 0 메우기
  , lpad(split_part(ip, '.', 1), 3, '0')
    || lpad(split_part(ip, '.', 2), 3, '0')
    || lpad(split_part(ip, '.', 3), 3, '0')
    || lpad(split_part(ip, '.', 4), 3, '0')
  AS ip_padding

  -- BigQuery, split 함수로 배열 분해, n번째 요소 추출
  , CONCAT(
    lpad(split(ip, '.')[SAFE_ORDINAL(1)], 3, '0')
    , lpad(split(ip, '.')[SAFE_ORDINAL(2)], 3, '0')
    , lpad(split(ip, '.')[SAFE_ORDINAL(3)], 3, '0')
    , lpad(split(ip, '.')[SAFE_ORDINAL(4)], 3, '0')
  ) AS ip_padding

  -- Hive, SparkSQL, split 함수로 배열 분해, n번째 요소 추출
  -- .이 특수문자 이므로 \로 escaping
  , CONCAT(
    lpad(split(ip, '\\.')[0], 3, '0')
    , lpad(split(ip, '\\.')[1], 3, '0')
    , lpad(split(ip, '\\.')[2], 3, '0')
    , lpad(split(ip, '\\.')[3], 3, '0')
  ) AS ip_padding
FROM
  (SELECT '192.168.0.1' AS ip) AS t

  -- PostgreSQL의 경우 명시적 자료형 변환
  (SELECT CAST('192.168.0.1' AS text) AS ip) AS t
;
```

{{< admonition type=example title="실행결과" open=false >}}
| ip | ip_padding |
| :--- | :--- |
| 192.168.0.1 | 192168000001 |
{{< /admonition >}}

- **lpad** : 지정한 문자 수가 되게 문자열 왼쪽을 메우기

