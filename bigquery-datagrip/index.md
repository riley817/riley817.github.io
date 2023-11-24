# 구글 빅쿼리(BigQuery) 시작하기 및 DataGrip 연동


## 구글 빅쿼리 시작하기 및 DataGrip 연동하기

### 구글 클라우드 프로젝트 생성
---
#### 구글 계정 생성 
기본 90일 동안 $300 크레딧으로 무료 평가판을 사용할 수 있다. 무료 체험 기간이 지나면 무료 체험 중 생성한 리소스는 모두 중지 되며 유료 계정으로 업그레이드 하지 않으면 요금이 부과되지 않는다. 
{{<figure class="test" src="/posts/images/dbms/bigquery-datagrip/bigquery-datagrip-1.png#center">}}

### 새 프로젝트 버튼을 클릭하여 프로젝트 생성하기
{{<figure src="/posts/images/dbms/bigquery-datagrip/bigquery-datagrip-3.png#center">}}


### 데이터 세트 생성하기
---
[데이터 세트](https://cloud.google.com/bigquery/docs/datasets-intro?hl=ko)는 테이블과 뷰에 대한 액세스를 구성 및 제어하는 최상위 컨테이너이다. 테이블이나 뷰는 반드시 데이터 세트에 속해야 하므로 최소 한 개 이상의 데이터 세트를 구성해야 한다. 

{{<figure src="/posts/images/dbms/bigquery-datagrip/bigquery-datagrip-6.png#center" width="50%" caption="Google Cloud Platform 콘솔 메뉴에서 BigQuery 메뉴를 선택">}}
{{<figure src="/posts/images/dbms/bigquery-datagrip/bigquery-datagrip-7.png#center" width="50%" caption="[프로젝트 선택]-[데이터 세트 만들기] 클릭">}}
{{<figure src="/posts/images/dbms/bigquery-datagrip/bigquery-datagrip-8.png#center" width="70%" caption="비용은 리전마다 다르다. 어차피 테스트이므로 그냥 서울로 선택">}}
{{<figure src="/posts/images/dbms/bigquery-datagrip/bigquery-datagrip-9.png#center" width="50%" caption="프로젝트 하위에 데이터 세트가 생성된 것을 확인 할 수 있다.">}}

### 서비스 계정 생성하기
---
외부에서 GCP 서비스에 접근하고 BigQuery를 사용하기 위해서는 접근 계정과 사용할 수 있는 롤이 정의되어야 한다. **Identity and Access Management(IAM)** 기능을 통해 서비스 계정을 생성하고 롤을 정의한다.

#### BigQuery 용 서비스 계정 생성
{{<figure src="/posts/images/dbms/bigquery-datagrip/bigquery-datagrip-10.png#center" width="50%" caption="[IAM 및 관리자]-[서비스 계정] 메뉴를 선택한다.">}}
{{<figure src="/posts/images/dbms/bigquery-datagrip/bigquery-datagrip-11.png#center" caption="서비스 계정 만들기 버튼을 선택">}}
{{<figure src="/posts/images/dbms/bigquery-datagrip/bigquery-datagrip-12.png#center" width="70%" caption="서비스 계정 아이디를 입력">}}
{{<figure src="/posts/images/dbms/bigquery-datagrip/bigquery-datagrip-13.png#center" width="70%" caption="[역할]-[Basic]-[소유자] 선택">}}

#### 외부에서 접근할 키 생성
키를 만들면 자동으로 JSON 파일의 키가 다운로드 된다.
{{<figure src="/posts/images/dbms/bigquery-datagrip/bigquery-datagrip-14.png#center" caption="[작업]-[키관리] 드롭다운 메뉴 선택">}}
{{<figure src="/posts/images/dbms/bigquery-datagrip/bigquery-datagrip-15.png#center" width="70%" caption="새 키 만들기 클릭">}}
{{<figure src="/posts/images/dbms/bigquery-datagrip/bigquery-datagrip-16.png#center" width="70%" caption="JSON 선택 후 만들기 버튼 클릭">}}

### DataGrip 설정
---
`Project ID`, `Service account email` 등 항목을 입력해준다. `Key file`은 위에서 다운로드된 JSON 파일의 경로를 설정해준다. 만약 관련 드라이버가 잘 설치되지 않아 접속되지 않으면 [BigQuery 용 ODBC 및 JDBC 드라이버](https://cloud.google.com/bigquery/docs/reference/odbc-jdbc-drivers?insvid=165c7bd5951c5482--1536654613358&hl=ko) 별도로 설치 후 진행한다.
{{<figure src="/posts/images/dbms/bigquery-datagrip/bigquery-datagrip-17.png#center">}}




