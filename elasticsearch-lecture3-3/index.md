# [Elasticsearch 검색 엔진 구축 강의] 3.3 jvm.options


## jvm.options
* jvm.options 의 설정을 통해 `JVM(Java Virtual Machine)` 의 옵션을 변경할 수 있다.
* 설정 파일은 `config/jvm.options`(tar or zip 배포판) 또는 `/etc/elasticsearch/jvm.options`(rpm 패키지 설치) 에서 위치하고 있다.

## JVM Heap Size Configuration
Elasticsearch 는 Java 기반이기 때문에 Heap 메모리를 어떻게 설정하느냐에 따라 성능에 큰 영향을 미치게 된다. 
기본적으로 Elasticsearch 의 최소, 최대 Heap 사이즈는 **2GB**로 설정되어 있다. 
실제 운영환경에 따라서 Elasticsearch 에서 충분한 Heap 을 사용할 수 있도록 Heap Size 를 구성하는 것이 중요하다.


### Elasticsearch 와 Heapsize
아래는 `jvm.options`에서 최소 및 최대 힙 사이즈 크기를 설정하는 예시이다.
{{< highlight shell>}}
# set the minimun heap size to 2GB
-Xms2g
# set the maximum heap size to 2GB
-Xmx2g 
{{< /highlight>}}

### 설정 권고 사항
##### 최소 힙 크기(Xms) 와 최대 힙 크기(Xmx) 를 동일하게 설정하는 것이 좋다.
힙 크기가 크면 클 수록 많은 데이터를 힙 영역에서 사용할 수 있으나 GC (Garbage Collection) 발생에 대한 JVM 의 성능 저하 및 정지에 대한 이슈를 고려해야 한다. (Stop the world)
##### 최대 힙 크기(Xmx) 를 실제 물리메모리(RAM) 에 50%를 넘지 않게 하는 것이 좋다.
##### 힙 크기는 32G 를 넘지 않는 것이 좋다.
JVM 은 데이더(Object) 에 접근하기 위한 메모리상의 주소를 OOP(Ordinary Object Pointer) 라는 구조체로 저장한다. 
OOP 는 아키텍쳐에 따라 32bit 와 62bit 크기의 주소값으로 참조하게 되며 32bit 환경에서 2^32 (= 4GB) 의 주소값을 나타낼 수 있고 64 bit 환경에서는 2^64(=18EB) 까지의 주소값을 표시할 수 있다. 
64bit 환경에서는 메모리의 참조 영역이 넓기 때문에 성능이 떨어질 수 밖에 없다. 
이러한 문제 때문에 JVM 은 32bit 기반의 OOP를 이용하며 Heap 영역이 4GB 를 넘어가는 경우에 메모리 주소의 offset 을 가리키는 `Compressed OOP`를 활용하게 된다. 
Compressed OOP 의 경우 메모리 주소의 Offset 을 가리키게 되며 이 오프셋은 8의 n 배수로 계산되어 기존 OOP 보다 8 배 더 많은 32GB 참조가 가능해진다.


| JVM option | Description |
| ----------| -----------|
| -XX:+UseConcMarkSweepGC | 기본적으로 CMS GC 옵션을 사용.|
| -XX:CMSInitiatingOccupancyFraction | old generation 힙 공간의 사용량을 지정하여 CMS GC 주기를 설정할 수 있다. <br/> ex) -XX:CMSInitiatingOccupancyFraction=75 이면 old generation 75% 인 경우 CMS 주기를 시작하라는 의미.  |
| -XX:+UseCMSInitiatingOccupancyOnly | GC 통계를 따르지 않고 CMSInitiatingOccupancyFraction 을 기준으로 GC 주기를 시작 |
| -XX : HeapDumpOnOutOfMemoryError | OOM(Out of Memory) 등으로 더이상 힙 영역을 할당할 수 없는 경우 Heap Dump 를 생성하는 옵션  |
| -XX:HeapDumpPath | Heap Dump를 저장할 경로를 지정 <br/> ex) -XX:HeapDumpPath=/var/lib/elasticsearch  |
| -XX:ErrorFile=filename | 심각한 오류 로그(JVM Fatal Error logs를 받을 수 있는 경로를 지정 <br/> -XX:ErrorFile=/var/log/elasticsearch/hs_err_pid%p.log|





