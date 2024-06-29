# [Elasticsearch 검색 엔진 구축 강의] elasticsearch.yml


## elasticsearch.yml
`elasticsearch.yml` 은 데이터 파일 위치, 로그파일 위치 등 클러스터의 핵심적인 설정을 할 수 있는 구성 파일이다. 파일 포맷은 **YAML** 로 되어있다.
```yaml
# path.data : 데이터파일의 위치를 설정 
# path.logs : ES 의 로그 파일이 저장될 위치를 설정
path: 
 data: /var/lib/elasticsearch
 logs: /var/log/elasticsearch

# 클러스터를 고유하게 식별할 수 있는 이름 설정
cluster:
  name: es-cluster

# 노드를 고유하게 식별할 수 있는 이름설정
# 보통 호스트명 기준으로 설정하는 것이 운영에 용이    
node:
  name: es-master01
```

### path.data
`path.data` 는 Index 의 데이터를 저장할 경로를 지정할 수 있으며 경로는 하나 혹은 여러개로도 지정이 가능하다.

```yaml
# ex) single path
path: 
    data: /data1

# ex ) multi path
path:
    data: /data1, /data2
```

### path.logs
`path.logs` 는 Elasticsearch의 로그를 저장할 경로를 지정 할 수 있으며 어플리케이션 운영 로그와 Elasticsearch Deprecated 로그, Indexing, Searching Slow 로그 등이 저장된다.

## Discovery
discovery 모듈은 클러스터 내의 노드를 발견하고 마스터 노드를 찾아내는 방식을 의미한다. Elasticsearch 는 P2P 기반의 시스템이며 노드간 통신은 ping 을 기반으로 동작한다.
이 discovery 모듈에는 `Zen discovery`와 클라우드 플랫폼에 따라 `EC2 discovery`, `GCE(Google Computer Engine) discovery` 등을 지원한다. 

### discovery.zen.ping.unicast.hosts
동일한 클러스터 명을 전제로 설정된 호스트들 가운데 Master를 선출할 수 있다. 이 설정에는 다른 노드 들에게 마스터 노드의 목록을 제공할 수 있다.
```yaml
# ex) master node host 
discovery.zen.ping.unicast.hosts:  [ "1.1.1.1:9300", "1.1.1.2:9300", "2.2.2.1:9300", ]
```

### discovery.zen.minimum_master_nodes
적합한 마스터 노드의 최소 갯수를 설정 할 수 있다. 
이 설정을 통하여 네트워크 장애등으로 인한 동일 클러스트 내에 마스터들이 최소 갯수만큼 내려가면 데이터 무결성을 위해 클러스터가 중지처리 된다. ( Split Brain 피하기 위함.)

{{<admonition type=success title="마스터 노드의 최소 갯수">}}
**master_eligible_nodes / 2 + 1**
{{</admonition>}}

## Network 설정
### network.host
- 노드가 응답 할 수 있는 IP 또는 호스트를 설정한다.
- 기본적으로 `Elasticsearch`는 루프백 주소에만 바인드 한다. (ex 127.0.0.1 ) 이는 단일 노드에서는 문제 없으나 여러 노드가 운용되는 클러스트 구성을 위해서는 비 루프백 주소를 바인드 해 주어야 한다.

### network.bind_host
notework.host 설정에서 외부의 데이터 호출을 받는 부분만 분리 할 수 있다.
```yaml
# ex) network.bind_host
network.bind_host: 0.0.0.0 
```

### network.publish_host
클러스트 내 다른 노드들과 통신 하는 부분만 분리 할 수 있다.
```yaml
# ex) network.bind_host
 network.publish_host: 10.190.5.5
```

### http.port
HTTP 프로토콜을 통해 Elasticsearch 의 API 를 전달할 때 사용할 포트를 지정할 수 있다.
```yaml
# ex) http.port
http.port: 9200
```

### transport.tcp.port
클러스터 내에 노드들이 서로 통신할 때 사용할 포트를 설정한다. 노드들은 서로의 용량과 샤드들의 상태를 알아야 하기 때문에 TCP 통신을 한다.

```yaml
# ex) transport.tcp.port
transport.tcp.port: 9300
```

## Node Role 설정
- Node Role에는 `master-eligible`, `data`, `ingest node`, `coordinate node` 가 있다.
- elasticsearch.yml 설정을 통하여 각 노드의 역할을 부여할 수 있다.

### Master-eligible Node
마스터 노드로서의 역할을 할 수 있는 노드를 의미

```yaml
# ex) master node role
node.master: true
node.data: false
node.ingest: false 
```

### Data Node
데이터를 저장되는 역할을 할 수 있는 노드를 의미

```yaml
# ex) data node role
master.node: false
node.data: true
node.ingest: false 
```

### Ingest Node
문서가 인덱싱 되기 전에 파이프라인을 통해 사전처리를 할 수 있는 역할이 부여된 노드. 기본값은 true이며 보통 client node 를 세팅할 때 사용한다.

```yaml
# ex) ingest node role
master.node: false
node.data: false
node.ingest: true 
```

### Coordinate Node
클라이언트의 요청을 받고 라우팅 및 분산처리만 할 수 있는 역할이 부여된 노드.

```yaml
# ex) Coordinate node role
master.node: false
node.data: false
node.ingest: false 
```


## 그외 설정
### http.cors.enabled: true
웹 브라우저나 Elasticsearch 에서 접근할 수 있도록 설정. Head / HQ 플러그인 사용시 설정할 수 있다.

### http.cors.allow-origin: "*"
웹 브라우저로 접근할 수 있는 IP ACL 설정

## 참고

- [discovery.zen.minimum_master_nodes](https://www.elastic.co/guide/en/elasticsearch/reference/6.4/discovery-settings.html#minimum_master_nodes) 
- [node-ingest-node](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-node.html#node-ingest-node)
