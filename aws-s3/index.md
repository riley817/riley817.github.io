# Amazon S3


## Amazon S3
- `Amazon S3`은 AWS의 주요한 서비스 중 하나이다.
- **무한 확장`infinitely scaling`** 가능한 storage
- 많은 웹사이트에서 `Amazon S3`을 backbone으로 사용한다.
- AWS의 다수의 서비스에서도 `Amazon S3` 통합하여 사용할 수 있다.

## Amazon S3 Overview
### Buckets
- `Amazon S3`에서는 **object(files)** 을  **buckets(directories)** 에 저장할 수 있다.
- 버켓은 반드시 **전역적으로 유일`globally unique name`** 해야 한다.    
- 버켓은 `region level` 정의된다. (S3는 전역서비스지만 버켓은 리전리소스)
- Naming convention
    - 대문자 불가
    - `_` 불가
    - 길이는 3~63 자리 
    - IP 주소가 아닐 것
    - 반드시 소문자로 또는 숫자로 시작

### Objects
- `Object`는 파일이며 키를 가짐 
- **key는 전체경로`full path`를 나타낸다** 
    - s3://my-bucket/`my_file.txt` : `my_file.txt`이 키 값을 나타낸다.
    - s3://my-bucket/`my_folder1/another_folder/my_file.txt` : `my_folder1/another_folder/my_file.txt`전체 경로가 키 값을 나타낸다.
- 키는 **prefix + object name**으로 구성되어있음.
    - s3://my-bucket/my_folder1/another_folder/my_file.txt
    - **prefix** : `my_folder1/another_folder/`
    - **object name** : `my_file.txt`
- 버켓에는 디렉토리 개념이 없다. (UI만 그럴뿐)
- 슬래시("/")가 포함된 긴 이름만 키로 사용가능하다.
- Object의 값은 컨텐츠의 내용을 나타낸다.
    - 최대 Object 크기는 5TB(5000GB)
    - **5GB 이상 업로드하는 경우 `Multi-part 업로드`를 사용해야 한다.**
- **Metadata** : 키/값 쌍으로 정의된 리스트. 시스템 또는 사용자 메타데이터로 사용
- **Tags** : Unicode 키/값 쌍. 최대 10개까지 정의. 보안, lifecycle 정보를 지정시 유용
- **Version ID** : versioning을 활성화 한 경우 Object의 버전별로 ID로 관리

### Versioning
- `Amazon S3`은 파일에 버전을 설정할 수 있다.
- 버켓 수준에서 활성화 가능
- 같은 키로 object를 overwrite 했을 때 버전이 증가된다.
- 버켓에는 버전을 지정하는 것을 권장한다.

#### 버켓에 버전을 지정해야 하는 이유
- 의도하지 않은 삭제로부터 보호. **버전 복원 가능**
- 이전 버전으로 쉽게 롤백이 가능하다. 

{{<admonition type=note >}}
- versioning을 활성화하기 전 저장되어 버전이 지정되지 않은 파일의 경우 버전은 "null"이다.
- 버전 관리를 일시 중단해도 이전 버전은 삭제되지 않음.
{{</admonition>}}

## S3 Object의 암호화
### S3 Object 암호화 방식
- `Amazon S3`에 업로드 된 object는 AWS 서버내에 존재하므로 객체로 접근이 불가능하게 보호해야 한다.
- object의 암호화를 통해 보안을 강화할 수 있다.

| 방법 | 설명 |
|--|----------|
|**SSE-S3**|AWS에서 objects에서 사용하는 키를 처리 및 관리|
|**SSE-KMS**|AWS Key Management Service를 통해 암호화 키를 관리|
|**SSE-C**|사용자가 자신의 암호화 키를 관리|
|**Client Side Encryption**|클라이언트에서 암호화 처리|

#### SSE-S3
- `Amazon S3`가 암호화 시 사용할 키를 처리 및 관리
- Object는 서버 측에서 암호화 된다.
- 유형은 **AES-256** 알고리즘을 사용
- `SSE-S3` 암호화를 사용하려면 아래 헤더를 반드시 설정해야 한다.
    - **“x-amz-server-side-encryption": "AES256"**
- 암호화 키는 S3가 소유하고 S3가 관리한다.

##### SSE-S3 암호화 과정
1. Object HTTP/S로 헤더값을 지정하여 업로드
2. 헤더를 통해 Amazon S3가 관리하는 데이터키를 통해 object를 encryption하여 버켓에 저장

#### SSE-KMS
- `AWS Key Management Service(KMS)`가 암호화 시 사용할 키를 처리 및 관리한다.
- KMS에서 사용자 제어 및 감시를 추적한다.
- Object는 서버 측에서 암호화 된다.
- `SSE-KMS` 암호화를 사용하려면 아래 헤더를 반드시 설정해야 한다.
    - **“x-amz-server-side-encryption": ”aws:kms"**

##### SSE-KMS 암호화 과정
1. Object HTTP/S로 헤더값을 지정하여 업로드
2. 헤더를 통해 Amazon S3에서는 KMS의 사용자의 마스터키를 사용
3. KMS에서 가져오느 사용자의 마스터키로 object를 encryption하여 버켓에 저장

#### SSE-C 
- AWS 밖에서 고객이 관리하는 키를 사용하여 서버측에서 데이터를 암호화
- 고객이 제공하는 암호화 키를 Amazon S3에서는 저장하지 않음
- **HTTPS**를 반드시 사용
- 암호화 키는 매 요청시 마다 HTTP 헤더에 반드시 추가하여 전송


##### SSE-C 암호화 과정
1. Object는 반드시 HTTPS 만 가능하며 헤더에는 고객이 관리하는 암호화 키를 포함
2. 사용자가 제공한 암호화 키로 Object를 암호화 하여 버켓에 저장
3. 버켓에서 obejct를 가져올 때에는 저장당시 암호화 했던 키를 제공

#### Client Side Encryption
- Amazon S3 Encryption Client와 같은 라이브러리를 사용하여 클라이언트 측에서 암호화.
- S3로 전송하기 전에 클라이언트가 직접 데이터를 암호화해야 한다.
- 데이터를 검색할 때 클라이언트가 직접 데이터를 복호화해야 한다.
- 고객은 모든 키와 암호화 싸이클은 직접 관리 해야한다.

## 전송 암호화 `Encryption in transit` (SSL/TLS)

#### Amazon S3 endpoint
- **HTTP endpoint** : 암호화되지 않음
- **HTTPS endpoint** : encryption in flight

- 원하는 endpoint를 자유롭게 선택할 수 있지만 HTTPS를 사용하는 것을 권장
- 대부분의 클라이언트는 `HTTPS` endpoint를 기본으로 사용한다.
- `SSE-C` 방식에서는 반드시 HTTPS가 필수사항
- 전송계층 상의 암호화를 **SSL/TLS** 라고 한다. 



