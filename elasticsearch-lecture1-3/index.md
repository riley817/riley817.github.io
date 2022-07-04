# Elasticsearch 클러스터 분산구성 시나리오


## 2. Elasticsearch 클러스터 분산구성 시나리오

### 2.1 단일 노드에 3개의 샤드로 클러스터 구성하기
`blogs`라는 인덱스에 3개의 `primary` 샤드와 1개의 `replica` 샤드가 운용되도록 할당한다.

```bash
curl -X PUT "localhost:9200/blogs" -H 'Content-Type: application/json' -d
{
   "settings" : {
      "number_of_shards" : 3,
      "number_of_replicas" : 1
   }
}
```

{{< image src="/posts/images/elastic/cluster-scenario_01.png" caption="[그림1] 샤드의 개수는 3이고 레플리카 개수는 1개인 단일 노드"  >}}
- 클러스터는 정상적으로 작동되나 하드웨어의 오류가 발생할 경우 데이터 손실의 위험이 있다.
- 데이터 적재량이 많을 경우 싱글노드가 허용하는 볼륨을 모두 소진할 수도 있어 더이상 적재가 불가능 할 수도 있다.

### 2.2 두번째 노드를 시작하여 두번째 노드에 `Replica shard`가 복제되도록 설정한다.
- 클러스터에 동일한 설정의 노드를 한대 더 투입한다.
- 두 번째 노드가 클러스트에 추가되고, 두 번째 노드에는 각 샤드의 `Replica shard`가 생성된다.
{{< image src="/posts/images/elastic/cluster-scenario_02.png" caption="[그림2] 두 번째 노드에 replica shard 복제 설정"  >}}

### 2.3 세번째 노드를 시작하고 부하분산을 위하여 샤드를 재할당 한다.
- Node3 을 시작하게 되면 2개의 샤드가 Node3 으로 이동되었으며, 각 하드웨어의 리소스(CPU, RAM, I/O) 가 더 적은 수의 샤드에 공유되므로 각 샤드의 성능이 향상된다.
- 그러나 만약 한 대의 노드에서 Fail 이 발생할 경우 데이터의 안정성을 보장할 수 없다. 복제본 Replica Shard 의 개수를 추가한다. 기본 Replica Node 수를 2 개로 변경한다.

```bash
# 레플리카 개수를 2개로 변경
PUT /blogs/_settings
{
   "number_of_replicas" : 2
}
```

{{< image src="/posts/images/elastic/cluster-scenario_03.png" caption="[그림2] 두 번째 노드에 replica shard 복제 설정"  >}}

+ 어플리케이션의 수요가 증가함에 따라 노드를 확장 할 수 있다.
+ Primary Shard 3개, Replica Shard 가 3개 이므로 최대 6개의 노드로 확장이 가능하다.
+ 세번째 노드를 시작하여 노드가 분산되어 할당될 수 있도록 한다.



![image](/categories/images/elastic/cluster-scenario_04.png)

+ 위의 노드 구성을 통해 Node 2 개가 Fail 이 발생하여도 데이터의 손실은 막을 수 있다.

## 노드 한 대가 Fail 이 발생했을 경우
![image](/categories/images/elastic/cluster-scenario_05.png)

+ Master 노드였던 Node1 번에 Fail 이 발생한다면, 가장먼저 노드간 새로운 Master 노드를 선출한다.
+ 남아있는 나머지 노드들의 Replica Shard가 Primary Shard 로 승격되게 된다.

## 죽였던 Node1 번을 다시 Active 시킨다면, 나머지 노드들의 Primary Shard 를 재 복제하여 Replica Shard 로 만든다.
![image](/categories/images/elastic/cluster-scenario_06.png)

## 참고
+ [Elasticsearch Reference - distributed cluster](https://www.elastic.co/guide/en/elasticsearch/guide/2.x/distributed-cluster.html)
    
