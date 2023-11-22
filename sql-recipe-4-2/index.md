# 데이터 분석을 위한 SQL 레시피 - 4장 시계열 기반으로 데이터 집계하기


> 🗄️ 데이터분석을 위한 SQL 레시피 책을 읽고 정리 / 요약 한 내용입니다.

## 10. 다면적인 축을 사용해 데이터 집약하기
- 매출의 시계열뿐만 아니라 상품의 카테고리, 가격 등을 조합하여 데이터의 특징을 추출해 리포팅

### 샘플 데이터
{{< admonition type=abstract title="매출 데이터 로그" open=false >}}
```sql
DROP TABLE IF EXISTS purchase_detail_log;
CREATE TABLE purchase_detail_log(
    dt           varchar(255)
  , order_id     integer
  , user_id      varchar(255)
  , item_id      varchar(255)
  , price        integer
  , category     varchar(255)
  , sub_category varchar(255)
);

INSERT INTO purchase_detail_log
VALUES
    ('2017-01-18', 48291, 'usr33395', 'lad533', 37300,  'ladys_fashion', 'bag')
  , ('2017-01-18', 48291, 'usr33395', 'lad329', 97300,  'ladys_fashion', 'jacket')
  , ('2017-01-18', 48291, 'usr33395', 'lad102', 114600, 'ladys_fashion', 'jacket')
  , ('2017-01-18', 48291, 'usr33395', 'lad886', 33300,  'ladys_fashion', 'bag')
  , ('2017-01-18', 48292, 'usr52832', 'dvd871', 32800,  'dvd'          , 'documentary')
  , ('2017-01-18', 48292, 'usr52832', 'gam167', 26000,  'game'         , 'accessories')
  , ('2017-01-18', 48292, 'usr52832', 'lad289', 57300,  'ladys_fashion', 'bag')
  , ('2017-01-18', 48293, 'usr28891', 'out977', 28600,  'outdoor'      , 'camp')
  , ('2017-01-18', 48293, 'usr28891', 'boo256', 22500,  'book'         , 'business')
  , ('2017-01-18', 48293, 'usr28891', 'lad125', 61500,  'ladys_fashion', 'jacket')
  , ('2017-01-18', 48294, 'usr33604', 'mem233', 116300, 'mens_fashion' , 'jacket')
  , ('2017-01-18', 48294, 'usr33604', 'cd477' , 25800,  'cd'           , 'classic')
  , ('2017-01-18', 48294, 'usr33604', 'boo468', 31000,  'book'         , 'business')
  , ('2017-01-18', 48294, 'usr33604', 'foo402', 48700,  'food'         , 'meats')
  , ('2017-01-18', 48295, 'usr38013', 'foo134', 32000,  'food'         , 'fish')
  , ('2017-01-18', 48295, 'usr38013', 'lad147', 96100,  'ladys_fashion', 'jacket')
 ;
```
{{< /admonition >}}

### 10.1 카테고리별 매출과 소계 계산하기
- 리포트는 전체적인 수치 개요를 전하면서 해당 내역을 다양한 관점에서 설명해야 한다.
- **드릴다운** 이 필요
  - 가장 요약된 레벨부터 가장 상세한 레벨의 차원의 계층까지 계층에 따라 분석에 필요한 요약 수준을 바꿀 수 있는 기능
  - 매출 합계를 제시 후 웹사이트, 모바일 사이트로 구분 하거나, 상품 카테고리별로 어떻게 구성되는지, 회원/비회원 페이지 뷰 비율에 따라 어떻게 다른 지 등

```sql

WITH sub_category_amount AS (
    -- 소 카테고리 매출 집계하기
    SELECT
        category        AS category
      , sub_category    AS sub_category
      , sum(price)      AS amount
    FROM sample_data.purchase_detail_log
    GROUP BY category, sub_category
),
category_amount as (
    -- 대 카테고리 매출 집계하기
    SELECT
        category        AS category
      , 'all'           AS sub_category
      , sum(price)      AS amount
    FROM sample_data.purchase_detail_log
    GROUP BY category
),
total_amount as (
    -- 전체 매출 집계하기
    SELECT
        'all' as category
      , 'all' as sub_category
      , sum(price) as amount
    FROM sample_data.purchase_detail_log
)
SELECT category, sub_category, amount FROM sub_category_amount
UNION ALL SELECT category, sub_category, amount FROM category_amount
UNION ALL SELECT category, sub_category, amount FROM total_amount
;
```

{{< admonition type=example title="실행결과" open=false >}}
| category | sub\_category | amount |
| :--- | :--- | :--- |
| all | all | 861100 |
| book | all | 53500 |
| outdoor | all | 28600 |
| ladys\_fashion | all | 497400 |
| food | all | 80700 |
| mens\_fashion | all | 116300 |
| cd | all | 25800 |
| game | all | 26000 |
| dvd | all | 32800 |
| book | business | 53500 |
| outdoor | camp | 28600 |
| ladys\_fashion | jacket | 369500 |
| ladys\_fashion | bag | 127900 |
| food | meats | 48700 |
| mens\_fashion | jacket | 116300 |
| cd | classic | 25800 |
| food | fish | 32000 |
| game | accessories | 26000 |
| dvd | documentary | 32800 |
{{< /admonition >}}

- `UNION ALL`을 사용해 테이블을 결합하는 방법은 테이블을 여러 번 불러오고 데이터를 결합하는 비용이 발생 : **성능이 좋지 않음**
- PostgreSQL, Hive, SparkSQL의 경우 `SQL99`에 도입된 **ROLLUP** 을 권장

**ROLLUP을 사용하여 카테고리 별 매출과 소계를 동시에 구하는 쿼리**
```sql
SELECT
    COALESCE(category, 'all')       AS category
  , COALESCE(sub_category, 'all')   AS sub_category
  , SUM(price)                      AS amount
FROM purchase_detail_log
GROUP BY ROLLUP (category, sub_category)
 -- HIVE 에서는 다음과 같이 사용
 -- GROUP BY category, sub_category WITH ROLLUP
;
```

- 대부분의 리포트 툴은 소계를 계산해주는 기능이 존재 -> 따라서 최소 단위로 집계 후 따로 소계 계산이 가능

### 10.2 ABC 분석으로 잘 팔리는 상품 판별하기
- **ABC 분석** : 매출 중요도에 따라 상품을 나누고 그게 맞게 전략을 만들 때 사용. 재고 관리 등에서 사용하는 분석법

#### ABC 분석을 위한 데이터 작성 방법
1. 매출이 높은 순서대로 데이터를 정렬한다.
2. 매출 합계를 집계한다.
3. 매출 합계를 기반으로 데이터가 차지하는 비율을 계산하고 구성비를 구한다.
4. 계산한 카테고리의 구성비를 기반으로 구성비누계를 구한다.
  - 카테고리의 매출과 해당 시점까지의 누계를 따로 계산 후 총 매출로 나누면 구성비누계를 구할 . 수있음

**매출 구성비누계와 ABC 등급을 계산하는 쿼리**
- 2015년 12월 한 달 동안의 구매로그를 기준으로 상품 
  - 카테고리 별 매출 계산
  - 전체 매출에 대한 항목별 매출 구성비, 구성비누계 계산
  - 구성비누계를 기준으로 상위 `0~70%`, `70~90%`, `90~100%`의 등급을 나눔

```sql
WITH monthly_sales AS (
    SELECT
        category
      , SUM(price) AS amount -- 항목별 매출 계산하기
    FROM purchase_detail_log
    WHERE dt BETWEEN '2015-12-01' AND '2015-12-31' -- 대상 1개월 동안의 로그를 조건으로 걸기
    GROUP BY category
),
sales_composition_ratio AS (
    SELECT
        category
      , amount

      -- 구성비 : 100.0 * 항목별 매출 / 전체 매출
      , 100.0 * amount / SUM(amount) OVER() AS composition_ratio

      -- 구성비누계 : 100.0 * 항목별 누계 매출 / 전체 매출
      , 100.0 * SUM(amount) OVER (ORDER BY amount DESC ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW )
            / SUM(amount) OVER () AS cumulative_ratio
    FROM monthly_sales
)
SELECT
    *
  -- 구성비누계 범위에 따라 순위 붙이기
  , CASE
      WHEN cumulative_ratio BETWEEN 0 AND 70 THEN 'A'
      WHEN cumulative_ratio BETWEEN 70 AND 90 THEN 'B'
      WHEN cumulative_ratio BETWEEN 90 AND 100 THEN 'C'
    END as abc_rank
FROM sales_composition_ratio
ORDER BY amount DESC
;
```

{{< admonition type=example title="실행결과" open=false >}}
| category | amount | composition\_ratio | cumulative\_ratio | abc\_rank |
| :--- | :--- | :--- | :--- | :--- |
| ladys\_fashion | 497400 | 57.7633259783997213 | 57.7633259783997213 | A |
| mens\_fashion | 116300 | 13.5059807223319011 | 71.2693067007316223 | B |
| food | 80700 | 9.3717338288235977 | 80.6410405295552201 | B |
| book | 53500 | 6.2129833933341075 | 86.8540239228893276 | B |
| dvd | 32800 | 3.8090814075020323 | 90.6631053303913599 | C |
| outdoor | 28600 | 3.3213331784926257 | 93.9844385088839856 | C |
| game | 26000 | 3.0193937986296597 | 97.0038323075136453 | C |
| cd | 25800 | 2.9961676924863547 | 100 | C |
{{< /admonition >}}


### 10.3 팬 차트로 상품의 매출 증가율 확인하기
- **팬 차트** : 기준 시점을 100% 로 두고 이후 숫자 변동을 확인할 수 있게 해주는 그래프
- 변화가 백분율로 포시되어 작은 변화도 쉽게 인지

```sql
WITH daily_category_amount AS (
    SELECT
        dt
      , category
       -- PostgreSQL, Hive, Redshift, SparkSQL은 다음과 같이 구성
       -- BigQuery의 경우 substring을 sdubstr로 수정
      , substring(dt, 1, 4) AS year
      , substring(dt, 6, 2) AS month
      , substring(dt, 9, 2) AS date
      , SUM(price)          AS amount
    FROM purchase_detail_log
    GROUP BY dt, category
),
monthly_category_amount AS (
    SELECT
        concat(year, '-', month) AS year_month
      -- Redshift의 경우 concat 함수를 조합해서 사용하거나 || 연산자 사용
      -- year || '-' || month AS year_month
      , category
      , SUM(amount)              AS amount
    FROM daily_category_amount
    GROUP BY year, month, category
)
SELECT
    year_month
  , category
  , amount
  -- 기준이 되는 매출을 가장 가장 첫 월의 매출로 잡음
  , FIRST_VALUE(amount) OVER (PARTITION BY category ORDER BY year_month, category ROWS UNBOUNDED PRECEDING ) as base_amount
  -- 기준 매출에 비율을 구함
  , 100.0 * amount / FIRST_VALUE(amount) OVER (PARTITION BY category ORDER BY year_month, category ROWS UNBOUNDED PRECEDING ) as rate
FROM monthly_category_amount
ORDER BY year_month, category;

```

- 팬 차트에서 어떤 시점에 매출을 기준으로 잡느냐에 따라 성장/쇠퇴 경향 판단이 크게 달라질 수 있음
- 계절 변동이 제일 적은 평균적인 달을 기준으로 선택
- 근거를 가지고 기준점을 채택해야 함

### 10.4 히스토그램으로 구매가격대 집계하기
- 히스토그램 **가로 축에 단계(데이터의 범위)**, **세로 축에 도수(데이터의 개수)**를 나타내는 그래프
- 데이터가 어떻게 분산되어 있는지를 한눈에 파악 가능
- **최빈값** : 데이터의 산에서 가장 높은 부분
- 데이터의 분포에 따라 최빈값은 평균값과 비슷하지 않을 수 있음

#### 히스토그램 만드는 법
히스토그램을 만들기 위해 도수 분포표를 만들어야 함
1. 최댓값, 최솟값, 범위(최댓값-최솟값) 을 구한다
2. 범위를 기반으로 몇 개의 계급으로 나눌지 결정. 각 계급의 상한/하한을 구한다.
3. 계급에 들어가는 데이터 개수(도수)를 구하고 이를 표로 정리한다.

|가격대 하한(이상)|가격대 상한(미만)|도수|
|---	|---	|---	|
|0  	| 5000 	|52  	|
|5000  	|10,000  	|156  	|
|10,000 	|15,000   	|728  	|
|15,000 	|20,000   	|884  	|
|20,000 	|25,000  	|572  	|
|25,000 	|30,000   	|260  	|
|30,000 	|35,000  	|52  	|

- 히스토그램은 연속된 데이터의 분포를 파악하기 위해 사용하므로 각각의 막대 사이에 공백을 넣지 않음

#### 임의의 계층 수로 히스토그램 만들기
- 구매된 상품의 가격대를 대상으로 히스토그램을 만들기
- 매출 상품의 최대, 최소 가격을 구하고 범위를 10등 분하는 히스토그램을 생성한다.

**최댓값, 최솟값, 범위를 구하는 쿼리**

```sql
WITH state AS (
    SELECT
        MAX(price) AS max_price
      , MIN(price) AS min_price
      , MAX(price) - MIN(price) AS range_price
      , 10 AS bucket_num
    FROM purchase_detail_log
)
SELECT * FROM state
;
```
{{< admonition type=example title="실행결과" open=false >}}
| max\_price | min\_price | range\_price | bucket\_num |
| :--- | :--- | :--- | :--- |
| 116300 | 22500 | 93800 | 10 |
{{< /admonition >}}

- 최소 금액 ~ 최대 금액의 범위를 계층으로 분할하려면 **정규화 금액(매출금액 - 최솟금액)** 을 계산
- 첫 계층 범위는 `금액 범위 / 계급 수` 로 나누어 구함
- `PostrgreSQL`의 경우 `width_bucket` 함수로 한번에 구할 수 있음

**데이터의 계층을 구하는 쿼리**
```sql
WITH state AS (
    SELECT
        MAX(price) AS max_price
      , MIN(price) AS min_price
      , MAX(price) - MIN(price) AS range_price
      , 10 AS bucket_num
    FROM purchase_detail_log
),
purchage_log_with_bucket AS (
    SELECT
        price
      , min_price

      -- 정규화 금액 : 대상금액에서 최소 금액을 뺀 것
      , price - min_price AS diff 

      -- 계층 범위 : 금액 범위를 계층 수로 나눈 것
      , 1.0 * range_price / bucket_num AS bucket_range

      -- 계층 판정 : FLOOR(정규화 금액 / 계층 범위)
      -- index가 1부터 시작이므로 +1
      , FLOOR(1.0 * (price - min_price) / (1.0 * range_price / bucket_num)) + 1 as bucket

      -- PostgreSQL의 경우 width_bucket 함수 사용 가능
      , width_bucket(price, min_price, max_price, bucket_num) AS bucket2
    FROM purchase_detail_log, state
)
SELECT * FROM purchage_log_with_bucket
;
```

{{< admonition type=example title="실행결과" open=false >}}
| price | min\_price | diff | bucket\_range | bucket | bucket2 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 22500 | 22500 | 0 | 9380 | 1 | 1 |
| 25800 | 22500 | 3300 | 9380 | 1 | 1 |
| 26000 | 22500 | 3500 | 9380 | 1 | 1 |
| 28600 | 22500 | 6100 | 9380 | 1 | 1 |
| 31000 | 22500 | 8500 | 9380 | 1 | 1 |
| 32000 | 22500 | 9500 | 9380 | 2 | 2 |
| 32800 | 22500 | 10300 | 9380 | 2 | 2 |
| 33300 | 22500 | 10800 | 9380 | 2 | 2 |
| 37300 | 22500 | 14800 | 9380 | 2 | 2 |
| 48700 | 22500 | 26200 | 9380 | 3 | 3 |
| 57300 | 22500 | 34800 | 9380 | 4 | 4 |
| 61500 | 22500 | 39000 | 9380 | 5 | 5 |
| 96100 | 22500 | 73600 | 9380 | 8 | 8 |
| 97300 | 22500 | 74800 | 9380 | 8 | 8 |
| 114600 | 22500 | 92100 | 9380 | 10 | 10 |
| 116300 | 22500 | 93800 | 9380 | 11 | 11 |
{{< /admonition >}}

- 위의 쿼리는 계급 판정 시 `계급 하한 이상 ~ 계급 상한 미만`을 사용하므로 10의 범위가 35,000으로 잡히게 됨
  - 35,000 의 경우 11로 처리됨
  - `금액의 최댓값 + 1` 로 모든 레코드가 계급 상한 미만이 되게 만들어 주어야 한다.

**계급 상한을 조정한 쿼리**
```sql
WITH state AS (
    SELECT
       -- 금액의 최대 값 + 1
        MAX(price) + 1 AS max_price
      , MIN(price) AS min_price
      -- 금액의 범위 + 1
      , MAX(price) + 1 - MIN(price) AS range_price
      -- 계층 수
      , 10 AS bucket_num
    FROM purchase_detail_log
),
purchage_log_with_bucket AS (
    SELECT
        price
      , min_price

      -- 정규화 금액 : 대상금액에서 최소 금액을 뺀 것
      , price - min_price AS diff 

      -- 계층 범위 : 금액 범위를 계층 수로 나눈 것
      , 1.0 * range_price / bucket_num AS bucket_range

      -- 계층 판정 : FLOOR(정규화 금액 / 계층 범위)
      -- index가 1부터 시작이므로 +1
      , FLOOR(1.0 * (price - min_price) / (1.0 * range_price / bucket_num)) + 1 as bucket

      -- PostgreSQL의 경우 width_bucket 함수 사용 가능
      , width_bucket(price, min_price, max_price, bucket_num) AS bucket2
    FROM purchase_detail_log, state
)
SELECT * FROM purchage_log_with_bucket

```

{{< admonition type=example title="실행결과" open=false >}}
| price | min\_price | diff | bucket\_range | bucket | bucket2 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 22500 | 22500 | 0 | 9380.1 | 1 | 1 |
| 25800 | 22500 | 3300 | 9380.1 | 1 | 1 |
| 26000 | 22500 | 3500 | 9380.1 | 1 | 1 |
| 28600 | 22500 | 6100 | 9380.1 | 1 | 1 |
| 31000 | 22500 | 8500 | 9380.1 | 1 | 1 |
| 32000 | 22500 | 9500 | 9380.1 | 2 | 2 |
| 32800 | 22500 | 10300 | 9380.1 | 2 | 2 |
| 33300 | 22500 | 10800 | 9380.1 | 2 | 2 |
| 37300 | 22500 | 14800 | 9380.1 | 2 | 2 |
| 48700 | 22500 | 26200 | 9380.1 | 3 | 3 |
| 57300 | 22500 | 34800 | 9380.1 | 4 | 4 |
| 61500 | 22500 | 39000 | 9380.1 | 5 | 5 |
| 96100 | 22500 | 73600 | 9380.1 | 8 | 8 |
| 97300 | 22500 | 74800 | 9380.1 | 8 | 8 |
| 114600 | 22500 | 92100 | 9380.1 | 10 | 10 |
| 116300 | 22500 | 93800 | 9380.1 | 10 | 10 |
{{< /admonition >}}

**히스토그램을 구하는 쿼리**

```sql
WITH state AS (
    SELECT
        MAX(price) + 1              AS max_price
      , MIN(price)                  AS min_price
      , MAX(price) + 1 - MIN(price) AS range_price
      , 10                           AS bucket_num
    FROM purchase_detail_log
),
purchage_log_with_bucket AS (
    SELECT
        price
      , min_price
      , price - min_price AS diff
      , 1.0 * range_price / bucket_num AS bucket_range
      , FLOOR(1.0 * (price - min_price) / (1.0 * range_price / bucket_num)) + 1 as bucket
      , width_bucket(price, min_price, max_price, bucket_num) AS bucket2
    FROM purchase_detail_log, state
)
SELECT
    bucket
    -- 계층의 하한과 상한 계산하기
  , min_price + bucket_range * (bucket - 1)     AS lower_limit
  , min_price + bucket_range * bucket           AS upper_limit

  -- 도수 세기
  , COUNT(price)                                AS num_purchase
  -- 합계 금액 계산
  , SUM(price)                                  AS total_amount
FROM purchage_log_with_bucket
GROUP BY bucket, min_price, bucket_range
ORDER BY bucket
;
```
{{< admonition type=example title="실행결과" open=false >}}
| bucket | lower\_limit | upper\_limit | num\_purchase | total\_amount |
| :--- | :--- | :--- | :--- | :--- |
| 1 | 22500 | 31880.1 | 5 | 133900 |
| 2 | 31880.1 | 41260.2 | 4 | 135400 |
| 3 | 41260.2 | 50640.3 | 1 | 48700 |
| 4 | 50640.3 | 60020.4 | 1 | 57300 |
| 5 | 60020.4 | 69400.5 | 1 | 61500 |
| 8 | 88160.7 | 97540.8 | 2 | 193400 |
| 10 | 106920.9 | 116301 | 2 | 230900 |
{{< /admonition >}}

#### 임의의 계층 너비로 히스토그램 작성하기
- 가격의 상한/하한 기준으로 최적의 범위를 구할 수 있음
- 소수점으로 계층을 구분한 리포트는 직감적이지 않음
  - 리포트를 받아보는 쪽에서도 쉽게 이해하고 납득할 수 있게 계층을 구분 해야함
- 0 ~ 50,000 원의 범위를 10개의 계층으로 구분하는 쿼리

**히스토그램의 상한과 하한을 수동으로 조정한 쿼리**
```sql
WITH state AS (
    SELECT
        50000   AS max_price -- 금액의 최댓값
      , 0       AS min_price -- 금액의 최솟값
      , 50000   AS range_price -- 금액의 범위
      , 10      AS bucket_num -- 계층 수
    FROM purchase_detail_log
),
purchage_log_with_bucket AS (
    SELECT
        price
      , min_price
      , price - min_price AS diff
      , 1.0 * range_price / bucket_num AS bucket_range
      , FLOOR(1.0 * (price - min_price) / (1.0 * range_price / bucket_num)) + 1 as bucket
      , width_bucket(price, min_price, max_price, bucket_num) AS bucket2
    FROM purchase_detail_log, state
)
SELECT
    bucket
  , min_price + bucket_range * (bucket - 1) AS lower_limit
  , min_price + bucket_range * bucket       AS upper_limit
  , COUNT(price)                            AS num_purchase
  , SUM(price)                              AS total_amount
FROM purchage_log_with_bucket
GROUP BY bucket, min_price, bucket_range
ORDER BY bucket;
```

