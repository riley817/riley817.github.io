# 2.1 Elasticsearch 기본 동작


## Index 생성하기
인덱스를 생성하는 방법
+ Index Settings 를 정의한다.
+ Index Mappings 를 정의한다. 
+ 사용자 정의된 도큐먼트를 인덱싱한다. 

### Index Settings
#### Static Index Settings
- `index.number_of_shards` : 인덱스가 가져야 하는 Primary 샤드의 개수 설정. (기본값 5)

#### Dynamic Index Settings
- `index.number_of_replicas` : 각 기본 샤드의 복제본 (Replica 샤드 개수 설정). 기본값은 1
- `index.refresh_interval` : 검색 commit point 를 만드는 refresh interval 설정 (새로 고침 작업수행 빈도). -1 비활성화. 기본값 1s
- `index.routing.allocation.enable` : 인덱스의 샤드들의 라우팅 허용 설정

#### Other Settings
* Analysis, Mapping, Slowlog ... 

### 인덱스 Settings 로 인덱싱 하기
#### PUT Method 를 사용
```bash
curl -X PUT "localhost:9200/twitter" -H 'Content-Type: application/json' -d
{
    "settings" : {
        "index" : {
            "number_of_shards" : 3, 
            "number_of_replicas" : 2 
        }
    }
}
```
#### CLI 사용
```curl
curl -X PUT -H "Content-Type:application/json" -d '{"settings" : {"index" : {"number_of_shards" : 3,"number_of_replicas" : 1}}}' http://localhost:9200/twitter2
```

## Index 삭제하기
인덱스 삭제시에는 조심해서 삭제해야 한다. 주로 nginx 와 같은 웹서버를 앞단에 두어 특정 IP 에서만 DELETE Method 를 요청할 수 있도록 설정하거나 혹은 Index 의 Read only 설정을 사용하여 아예 삭제 할 수 없도록 설정하는 것이 좋다.

## 참고
+ [Elasticsearch Reference - index modules](https://www.elastic.co/guide/en/elasticsearch/reference/current/index-modules.html#index-modules-settings)

