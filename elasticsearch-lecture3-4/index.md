# [Elasticsearch 검색 엔진 구축 강의] log4j2.properties


## log4j2.properties
Elasticsearch는 로깅을 위해 Log4j2를 사용한다. Log4j2는 `log4j2.properties` 파일을 사용하여 구성할 수 있다.

## 로그참조 파일
### ${sys:es.logs.base_path}
- 로그의 Base 디렉토리
- elasticsearch.yml 의 `path.log`에 설정된 경로

### ${sys:es.logs.cluster_name}
- 클러스트 이름을 나타낸다.
- elasticsearch.yml 의 `cluster.name`으로 설정할 수 있다.

### ${sys:es.logs.node_name}
- 노드의 이름을 나타냅니다.
- elasticsearch.yml 의 `node.name`으로 설정할 수 있다.

```bash
${sys:es.logs.base_path}${sys:file.separator}${sys:es.logs.cluster_name}.log
/var/log/elasticsearch/mycluster.log
```




