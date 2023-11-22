# 데이터 분석을 위한 SQL 레시피 - 4장 시계열 기반으로 데이터 집계하기


> 🗄️ 데이터분석을 위한 SQL 레시피 책을 읽고 정리 / 요약 한 내용입니다.

{{< admonition type=success title="매출 데이터를 시계열로 집계 하기" open=true >}}
- 매출 데이터를 시계열로 집계하면 **규칙성**을 찾거나, 어떤 기간과 비교했을 때 **변화폭을 확인** 할 수 있다.
{{< /admonition >}}

## 9. 시계열 기반으로 데이터 집계하기

{{< admonition type=abstract title="매출 데이터 로그" open=false >}}
| dt | order\_id | user\_id | purchase\_amount |
| :--- | :--- | :--- | :--- |
| 2014-01-01 | 1 | rhwpvvitou | 13900 |
| 2014-01-01 | 2 | hqnwoamzic | 10616 |
| 2014-01-02 | 3 | tzlmqryunr | 21156 |
| 2014-01-02 | 4 | wkmqqwbyai | 14893 |
| 2014-01-03 | 5 | ciecbedwbq | 13054 |
| 2014-01-03 | 6 | svgnbqsagx | 24384 |
| 2014-01-03 | 7 | dfgqftdocu | 15591 |
| 2014-01-04 | 8 | sbgqlzkvyn | 3025 |
| 2014-01-04 | 9 | lbedmngbol | 24215 |
| 2014-01-04 | 10 | itlvssbsgx | 2059 |
| 2014-01-05 | 11 | jqcmmguhik | 4235 |
| 2014-01-05 | 12 | jgotcrfeyn | 28013 |
| 2014-01-05 | 13 | pgeojzoshx | 16008 |
| 2014-01-06 | 14 | msjberhxnx | 1980 |
| 2014-01-06 | 15 | tlhbolohte | 23494 |
| 2014-01-06 | 16 | gbchhkcotf | 3966 |
| 2014-01-07 | 17 | zfmbpvpzvu | 28159 |
| 2014-01-07 | 18 | yauwzpaxtx | 8715 |
| 2014-01-07 | 19 | uyqboqfgex | 10805 |
| 2014-01-08 | 20 | hiqdkrzcpq | 3462 |
| 2014-01-08 | 21 | zosbvlylpv | 13999 |
| 2014-01-08 | 22 | bwfbchzgnl | 2299 |
| 2014-01-09 | 23 | zzgauelgrt | 16475 |
| 2014-01-09 | 24 | qrzfcwecge | 6469 |
| 2014-01-10 | 25 | njbpsrvvcq | 16584 |
| 2014-01-10 | 26 | cyxfgumkst | 11339 |
{{< /admonition >}}


### 9.1 날짜별 매출 집계하기
{{< figure src="/posts/images/sql-recipe/chart-1.png#center">}}

- **매출을 집계하는 업무 : 가로축 날짜, 세로 축 금액**
- 날짜별로 매출을 집계하고 동시에 평균 구매액을 집계한다.
- 소셜 게임의 포인트 소비, 전자상거래 사이트에서의 쿠폰과 포인트 소비 추이등에도 적용
- 자료 청구, 신청 처럼 합계와 평균의 대상이 없는 경우 `COUNT` 함수로 추이를 확인한다.

```sql
SELECT
    dt
  , COUNT(*)                        AS purchase_count
  , SUM(purchase_amount)            AS total_amount
  , AVG(purchase_amount)            AS avg_amount
FROM purchase_log
GROUP BY dt
ORDER BY dt
;
```

{{< admonition type=example title="실행결과" open=false >}}
| dt | purchase\_count | total\_amount | avg\_amount |
| :--- | :--- | :--- | :--- |
| 2014-01-01 | 2 | 24516 | 12258 |
| 2014-01-02 | 2 | 36049 | 18024.5 |
| 2014-01-03 | 3 | 53029 | 17676.33 |
| 2014-01-04 | 3 | 29299 | 9766.33 |
| 2014-01-05 | 3 | 48256 | 16085.33 |
| 2014-01-06 | 3 | 29440 | 9813.33 |
| 2014-01-07 | 3 | 47679 | 15893 |
| 2014-01-08 | 3 | 19760 | 6586.67 |
| 2014-01-09 | 2 | 22944 | 11472 |
| 2014-01-10 | 2 | 27923 | 13961.5 |
{{< /admonition >}}

### 9.2 이동평균을 사용한 날짜별 추이 보기
{{< figure src="/posts/images/sql-recipe/chart-2.png#center">}}

- 날짜별 매출 리포트에서 매출이 주기적으로 높아지는 날(주말 등)이 있음 
- 해당 그래프로는 매출이 상승하는 경향이 있는지, 하락하는 경향이 있는지 판단하기 어려움
- 이러한 경우 7일 동안의 평균 매출을 사용하여 7일 이동 평균으로 표현할 수 있음

```sql
SELECT
    dt
  , SUM(purchase_amount) AS total_amount
  -- 최근 최대 7일 동안의 평균 계산하기
  , AVG(SUM(purchase_amount)) OVER(ORDER BY dt ROWS BETWEEN 6 PRECEDING AND CURRENT ROW ) as seven_day_avg
  -- 7일 데이터가 모두 있는 경우에만 계산
  , CASE WHEN 7 = COUNT(*) OVER(ORDER BY dt ROWS BETWEEN 6 PRECEDING AND CURRENT ROW) THEN AVG(SUM(purchase_amount)) OVER(ORDER BY dt ROWS BETWEEN 6 PRECEDING AND CURRENT ROW) END as seven_day_avg_strict
FROM purchase_log
GROUP BY dt
ORDER BY dt
;
```
- `seven_day_avg` : 과거 7일분의 데이터를 추출할 수 없는 경우는 해당일만을 가지고 평균을 구함
- `seven_day_avg_strict` : 과거 7일분의 데이터를 추출할 수 없는 경우는 NULL
- 이동평균만으로 리포트를 작성하면 날짜별로 변동을 파악하기 힘드므로 날짜별로 추이와 이동평균을 함께 표현하여 리포트를 만드는 것이 좋음

{{< admonition type=example title="실행결과" open=false >}}
| dt | total\_amount | seven\_day\_avg | seven\_day\_avg\_strict |
| :--- | :--- | :--- | :--- |
| 2014-01-01 | 24516 | 24516 | null |
| 2014-01-02 | 36049 | 30282.5 | null |
| 2014-01-03 | 53029 | 37864.67 | null |
| 2014-01-04 | 29299 | 35723.25 | null |
| 2014-01-05 | 48256 | 38229.8 | null |
| 2014-01-06 | 29440 | 36764.83 | null |
| 2014-01-07 | 47679 | 38324 | 38324 |
| 2014-01-08 | 19760 | 37644.57 | 37644.57 |
| 2014-01-09 | 22944 | 35772.43 | 35772.43 |
| 2014-01-10 | 27923 | 32185.86 | 32185.86 |
{{< /admonition >}}

### 9.3 당월 매출 누계 구하기
- 월별 목표를 설정할 때는 해당 월의 날짜별 매출 뿐만 아니라 해당 월에 어느정도 누적되었는지 동시에 확인할 수 있어야 함
- 날짜별로 매출을 집계하고 해당월의 누계를 구한다.

<br/>

**날짜별 매출과 당월 누계 매출을 집계하는 쿼리**
- 월별 누계 계산시 `OVER` 구에 `PARTITION BY substring(dt, 1, 7)`을 추가해 월별로 파티션을 생성

```sql
SELECT
  dt
  -- PostgreSQL, Hive, Redshift, SparkSQL의 경우 substring로 연-월 추출
  , substring(dt, 1, 7) AS year_month
  -- PostgreSQL, Hive, Bigquery, SparkSQL의 경우 substr 함수 사용
  , substr(dt, 1, 7) AS year_month

  , SUM(purchase_amount) AS total_amount

  -- PostgreSQL, Hive, Redshift, SparksQL
  , SUM(SUM(purchase_amount)) OVER(PARTITION BY substring(dt, 1, 7) ORDER BY dt ROWS UNBOUNDED PRECEDING) AS agg_amount

  -- BigQuery의 경우 substring을 substr로 수정
  , SUM(SUM(purchase_amount)) OVER(PARTITION BY substr(dt, 1, 7) ORDER BY dt ROWS UNBOUNDED PRECEDING) AS agg_amount
    
  FROM purchase_log
GROUP BY dt
ORDER BY dt
;
```
{{< admonition type=example title="실행결과" open=false >}}
| dt | year\_month | total\_amount | agg\_amount |
| :--- | :--- | :--- | :--- |
| 2014-01-01 | 2014-01 | 24516 | 24516 |
| 2014-01-02 | 2014-01 | 36049 | 60565 |
| 2014-01-03 | 2014-01 | 53029 | 113594 |
| 2014-01-04 | 2014-01 | 29299 | 142893 |
| 2014-01-05 | 2014-01 | 48256 | 191149 |
| 2014-01-06 | 2014-01 | 29440 | 220589 |
| 2014-01-07 | 2014-01 | 47679 | 268268 |
| 2014-01-08 | 2014-01 | 19760 | 288028 |
| 2014-01-09 | 2014-01 | 22944 | 310972 |
| 2014-01-10 | 2014-01 | 27923 | 338895 |
{{< /admonition >}}

**날짜별 매출을 일시 테이블로 만드는 쿼리**
- 반복해서 나오는 `SUM(purchase_amount)`과 `substring(dt, 1, 7)`을 `WITH` 구문으로 빼어 일시테이블(daily_purchase)로 처리
- 앞서 쿼리와 동일하지만 SELECT 구문 내부에 있는 컬럼의 의미를 쉽게 파악


```sql
WITH daily_purchase AS (
    SELECT
        dt
        -- BigQuery의 경우 substring -> substr  
      , substring(dt, 1, 4) as year
      , substring(dt, 6, 2) as month
      , substring(dt, 9, 2) as date
      , SUM(purchase_amount) as purchase_amount
      , COUNT(order_id) AS orders
    FROM purchase_log
    GROUP BY dt
) SELECT * FROM daily_purchase ORDER BY dt
;
```

{{< admonition type=example title="실행결과" open=false >}}
| dt | year | month | date | purchase\_amount | orders |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 2014-01-01 | 2014 | 01 | 01 | 24516 | 2 |
| 2014-01-02 | 2014 | 01 | 02 | 36049 | 2 |
| 2014-01-03 | 2014 | 01 | 03 | 53029 | 3 |
| 2014-01-04 | 2014 | 01 | 04 | 29299 | 3 |
| 2014-01-05 | 2014 | 01 | 05 | 48256 | 3 |
| 2014-01-06 | 2014 | 01 | 06 | 29440 | 3 |
| 2014-01-07 | 2014 | 01 | 07 | 47679 | 3 |
| 2014-01-08 | 2014 | 01 | 08 | 19760 | 3 |
| 2014-01-09 | 2014 | 01 | 09 | 22944 | 2 |
| 2014-01-10 | 2014 | 01 | 10 | 27923 | 2 |
{{< /admonition >}}

**daily_purchase 테이블에서 당월 누계 매출을 집계하는 쿼리**
```sql
WITH daily_purchase AS (
    SELECT
        dt
        -- BigQuery의 경우 substring -> substr  
      , substring(dt, 1, 4) as year
      , substring(dt, 6, 2) as month
      , substring(dt, 9, 2) as date
      , SUM(purchase_amount) as purchase_amount
      , COUNT(order_id) AS orders
    FROM purchase_log
    GROUP BY dt
) SELECT
    dt

  -- Redshift의 경우, concat 함수를 조합해서 사용하거나 || 연산자 사용  
  , CONCAT(year, '-', month) AS year_month
  , year || '-' || month AS year_month

  , purchase_amount
  , SUM(purchase_amount) OVER(PARTITION BY year, month ORDER BY dt ROWS UNBOUNDED PRECEDING) as agg_amount

FROM daily_purchase
ORDER BY dt;
```

{{< admonition type=example title="실행결과" open=false >}}
| dt | year\_month | purchase\_amount | agg\_amount |
| :--- | :--- | :--- | :--- |
| 2014-01-01 | 2014-01 | 24516 | 24516 |
| 2014-01-02 | 2014-01 | 36049 | 60565 |
| 2014-01-03 | 2014-01 | 53029 | 113594 |
| 2014-01-04 | 2014-01 | 29299 | 142893 |
| 2014-01-05 | 2014-01 | 48256 | 191149 |
| 2014-01-06 | 2014-01 | 29440 | 220589 |
| 2014-01-07 | 2014-01 | 47679 | 268268 |
| 2014-01-08 | 2014-01 | 19760 | 288028 |
| 2014-01-09 | 2014-01 | 22944 | 310972 |
| 2014-01-10 | 2014-01 | 27923 | 338895 |
{{< /admonition >}}

- 서비스 운용, 개발을 위한 SQL과 비교했을 때 빅데이터 분석 SQL은 성능이 조금 떨어지더라도 가독성과 재사용성을 중시
- 빅데이터 분석을 할 때는 SQL에 프로그램 개발 때 사용하는 _전처리_ 라는 사고 방식을 도입하는 경우도 많음

### 9.4 월별 매출의 작대비(작년 대비 비율) 구하기
{{< figure src="/posts/images/sql-recipe/chart-4.png#center">}}

- 월별 매출 추이를 추출하여 작년의 해당 월의 매출과 비교하기
- 월마다 `GROUP BY`를 적용하여 매출액을 계산하고 `CASE WHEN` 식을 사용하여 2014, 2015 각각 다른 컬럼으로 출력
- 2015년의 매출 / 2014년의 매출로 나누어 작대비를 계산한다.
```sql
WITH daily_purchase AS (
    SELECT
        dt
        -- BigQuery의 경우 substring -> substr  
      , substring(dt, 1, 4) as year
      , substring(dt, 6, 2) as month
      , substring(dt, 9, 2) as date
      , SUM(purchase_amount) as purchase_amount
      , COUNT(order_id) AS orders
    FROM purchase_log
    GROUP BY dt
) SELECT
    month
  , SUM(CASE year WHEN '2014' THEN purchase_amount END) as amount_2014
  , SUM(CASE year WHEN '2015' THEN purchase_amount END) as amount_2015
  , 100.0 * (SUM(CASE year WHEN '2015' THEN purchase_amount END) / SUM(CASE year WHEN '2014' THEN purchase_amount END)) as rate
FROM daily_purchase
GROUP BY month
ORDER BY month
;
```

{{< admonition type=example title="실행결과" open=false >}}
| month | amount\_2014 | amount\_2015 | rate |
| :--- | :--- | :--- | :--- |
| 01 | 13900 | 22111 | 159.07194244604318 |
| 02 | 28469 | 11965 | 42.02817099300994 |
| 03 | 18899 | 20215 | 106.9633313931954 |
| 04 | 12394 | 11792 | 95.14281103759885 |
| 05 | 2282 | 18087 | 792.5942156003505 |
| 06 | 10180 | 18859 | 185.25540275049116 |
| 07 | 4027 | 14919 | 370.4742984852247 |
| 08 | 6243 | 12906 | 206.7275348390197 |
| 09 | 3832 | 5696 | 148.64300626304802 |
| 10 | 6716 | 13398 | 199.49374627754617 |
| 11 | 16444 | 6213 | 37.782777912916565 |
| 12 | 29199 | 26024 | 89.12633994314874 |
{{< /admonition >}}

- 트렌드로 매출이 늘어난 경우라도 전년대비 떨어졌다면 성장이 둔화됐다고 판단할 수 있다.

### 9.5 Z 차트로 업적의 추이 확인하기
- 서비스, 상품, 콘텐츠 중에는 계절에 따라 추이가 변동하는 경우가 존재
- Z차트 : 월차매출, 매출누계, 이동년계라는 3개의 지표로 구성

#### Z 차트 작성

**월차매출**

매출 합계를 월별로 집계
{{< figure src="/posts/images/sql-recipe/z-chart-1.png#center">}}



**매출누계**

해당 월의 매출이 이전월까지의 매출 누계를 합한 값
{{< figure src="/posts/images/sql-recipe/z-chart-2.png#center">}}

**이동년계**

해당 월의 매출에 과거 11개월의 매출을 합한 값
{{< figure src="/posts/images/sql-recipe/z-chart-3.png#center">}}

#### Z 차트를 분석할 때의 정리

**매출누계에서 주목할 점**

- **직선** : 월차 매출이 일정할 경우 
- **가로축에서 오른쪽으로 갈수록 기울기가 급해지는 곡선** : 최근 매출이 상승
- **가로축에서 갈수록 기울기가 완만해지는 곡선** : 최근 매출이 감소

**이동년계에서 주목할 점**

- **직선** : 작년과 올해 매출이 일정
- **오른쪽 위로 올라 감** : 매출이 오르는 경향이 있음
- **오른쪽 아래로 내려 감** : 매출이 감소하는 경향이 있음

#### 다양한 형태의 Z 차트

#### Z 차트 작성하기 위한 지표 집계하기
- 구매 로그를 기반으로 월별 매출을 집계
- 각 월의 매출에 대해 누계매출, 이동년계를 계산

```sql
WITH daily_purchase AS (
    SELECT
        dt
      , substring(dt, 1, 4) as year
      , substring(dt, 6, 2) as month
      , substring(dt, 9, 2) as date
      , SUM(purchase_amount) as purchase_amount
      , COUNT(order_id) AS orders
    FROM purchase_log
    GROUP BY dt
), 
monthly_amount AS (
    SELECT year, month, SUM(purchase_amount) as amount
    FROM daily_purchase
    GROUP BY year, month
), 
alc_index AS (
    SELECT
        year
      , month
      , amount
      -- 2015년의 누계 매출 집계하기
      , SUM(CASE WHEN year = '2015' THEN amount END) OVER (ORDER BY year, month ROWS UNBOUNDED PRECEDING) as agg_amount
      -- 당월부터 11개월 이전까지의 매출 합계(이동년계) 집계하기
      , SUM(amount) OVER (ORDER BY year, month ROWS BETWEEN 11 PRECEDING AND CURRENT ROW )  as year_avg_amount
    FROM monthly_amount 
    ORDER BY year, month
)
SELECT
    concat(year, '-', month) as year_month

   -- Redshift의 경우 concat 함수를 조합하거나, || 연산자 사용
   , concat(concat(year, '-'), month) AS year_month
 
   , amount
   , agg_amount
   , year_avg_amount

FROM calc_index
WHERE year = '2015' -- 2015년 데이터만 압축하기
ORDER BY year_month
;
```
{{< admonition type=example title="실행결과" open=false >}}
| year\_month | amount | agg\_amount | year\_avg\_amount |
| :--- | :--- | :--- | :--- |
| 2015-01 | 22111 | 22111 | 160796 |
| 2015-02 | 11965 | 34076 | 144292 |
| 2015-03 | 20215 | 54291 | 145608 |
| 2015-04 | 11792 | 66083 | 145006 |
| 2015-05 | 18087 | 84170 | 160811 |
| 2015-06 | 18859 | 103029 | 169490 |
| 2015-07 | 14919 | 117948 | 180382 |
| 2015-08 | 12906 | 130854 | 187045 |
| 2015-09 | 5696 | 136550 | 188909 |
| 2015-10 | 13398 | 149948 | 195591 |
| 2015-11 | 6213 | 156161 | 185360 |
| 2015-12 | 26024 | 182185 | 182185 |
{{< /admonition >}}

- **누계매출** : `SUM` 내부 함수에서 `CASE` 식을 사용하여 2015년 매출만 압축 후 `SUM` 윈도 함수를 사용하여 누계 계산
- **이동년계** : 당월을 포함한 과거 11개월의 매출 합계를 구함 `ROWS BETWEEN 11 PRECEDING AND CURRENT ROW` 를 지정하여 현재행에서 11행 이전까지 데이터 합계를 포함
- 매출이 없는 달의 경우 CASE 식을 사용하여 집계 대상을 압축

### 9.6 매출을 파악할 때 중요 포인트
- 매출 집계만으로는 매출의 상승/하락에 관한 본질적인 이유를 알 수 없음
- 매출 결과의 원인이라 할 수있는 구매 횟수, 구매 단가 등 주변 데이터를 포함해서 리포트를 생성해야 함

**매출과 관련된 지표를 집계하는 쿼리**

```sql
WITH
daily_purchase AS (
  SELECT
      dt
    , substring(dt, 1, 4) as year
    , substring(dt, 6, 2) as month
    , substring(dt, 9, 2) as date
    , SUM(purchase_amount) as purchase_amount
    , COUNT(order_id) AS orders
  FROM purchase_log
  GROUP BY dt
)
, monthly_purchase AS (
  SELECT
    year
    , month
    , SUM(orders) AS orders
    , AVG(purchase_amount) AS avg_amount
    , SUM(purchase_amount) AS monthly
  FROM daily_purchase
  GROUP BY year, month
)
SELECT
  concat(year, '-', month) AS year_month
  -- Redshift의 경우 concat 함수를 조합하거나, || 연산자 사용
  -- year || '-' || month AS year_month

  , orders
  , avg_amount
  , monthly
  , SUM(monthly) OVER(PARTITION BY year ORDER BY month ROWS UNBOUNDED PRECEDING) AS agg_amount

  -- 12개월 전의 매출 구하기
  , LAG(monthly, 12) OVER(ORDER BY year, month) as last_year
  -- sparkSQL의 경우 다음과 같이 사용 
  --, LAG(monthly, 12) OVER(ORDER BY year, month ROWS BETWEEN 12 PRECEDING AND 12 PRECEDING) AS last_year

  , 100.0 * monthly / LAG(monthly, 12) OVER(ORDER BY year, month)  as rate
  -- sparkSQL의 경우 다음과 같이 사용
  --, 100.0 * monthly / LAG(monthly, 12) OVER(ORDER BY year, month ROWS BETWEEN 12 PRECEDING AND 12 PRECEDING) as rate

FROM monthly_purchase
ORDER BY year, month
;
```
{{< admonition type=example title="실행결과" open=false >}}
| year\_month | orders | avg\_amount | monthly | agg\_amount | last\_year | rate |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 2014-01 | 1 | 13900 | 13900 | 13900 | null | null |
| 2014-02 | 1 | 28469 | 28469 | 42369 | null | null |
| 2014-03 | 1 | 18899 | 18899 | 61268 | null | null |
| 2014-04 | 1 | 12394 | 12394 | 73662 | null | null |
| 2014-05 | 1 | 2282 | 2282 | 75944 | null | null |
| 2014-06 | 1 | 10180 | 10180 | 86124 | null | null |
| 2014-07 | 1 | 4027 | 4027 | 90151 | null | null |
| 2014-08 | 1 | 6243 | 6243 | 96394 | null | null |
| 2014-09 | 1 | 3832 | 3832 | 100226 | null | null |
| 2014-10 | 1 | 6716 | 6716 | 106942 | null | null |
| 2014-11 | 1 | 16444 | 16444 | 123386 | null | null |
| 2014-12 | 1 | 29199 | 29199 | 152585 | null | null |
| 2015-01 | 1 | 22111 | 22111 | 22111 | 13900 | 159.0719424460431655 |
| 2015-02 | 1 | 11965 | 11965 | 34076 | 28469 | 42.0281709930099406 |
| 2015-03 | 1 | 20215 | 20215 | 54291 | 18899 | 106.9633313931954072 |
| 2015-04 | 1 | 11792 | 11792 | 66083 | 12394 | 95.1428110375988381 |
| 2015-05 | 1 | 18087 | 18087 | 84170 | 2282 | 792.5942156003505697 |
| 2015-06 | 1 | 18859 | 18859 | 103029 | 10180 | 185.2554027504911591 |
| 2015-07 | 1 | 14919 | 14919 | 117948 | 4027 | 370.4742984852247331 |
| 2015-08 | 1 | 12906 | 12906 | 130854 | 6243 | 206.7275348390197021 |
| 2015-09 | 1 | 5696 | 5696 | 136550 | 3832 | 148.6430062630480167 |
| 2015-10 | 1 | 13398 | 13398 | 149948 | 6716 | 199.4937462775461584 |
| 2015-11 | 1 | 6213 | 6213 | 156161 | 16444 | 37.7827779129165653 |
| 2015-12 | 1 | 26024 | 26024 | 182185 | 29199 | 89.126339943148738 |
{{< /admonition >}}

- 작년 같은 달 매출을 구할 때 `LAG` 함수를 사용하여 12개월 전의 매출을 추출
- BigQuery 등은 데이터를 읽어 들일 때 과금이 발생하므로 불필요한 데이터를 읽어 들임을 줄여야함


