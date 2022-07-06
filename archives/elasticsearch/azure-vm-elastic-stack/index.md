# Azure VM에 Elastic Stack 설정

## VM 세팅

### 리소스 그룹 생성
- `--location` 으로 리젼을 선택할 수 있다.
```bash
# 리젼 코드 조회하기
az account list-locations -o table

# elastic Resource Group 생성
az group create --name elastic --location koreasouth
```
![azure-vm-1](/categories/images/elastic/resource-group.png)

### VM 생성하기
- `elastic` 그룹에 `VM` 을 생성한다.
```bash
yoon@Azure:~$ az vm create \
> --resource-group elastic \
> --name rileyVM \
> --image UbuntuLTS \
> --admin-username riley \
> --generate-ssh-keys
```

![azure-vm-2](/categories/images/elastic/azure-vm2.png)

* [빠른 시작: Azure CLI를 사용하여 Linux VM 만들기 - Azure Virtual Machines](https://docs.microsoft.com/ko-kr/azure/virtual-machines/linux/quick-create-cli)

### SSH 접속 세팅
- VM 생성 시 `--generate-ssh-ke` 의 옵션을 주어 SSH Key를 생성하였다.

#### SSH Private Key 다운로드방법

- 클라우드 쉘을 실행한다.
- 쉘 명령어 창에 파일 다운로드를 선택한다.
![azure-vm-3](/categories/images/elastic/azure-vm3.png)
- `/.ssh/id_rsa`  PK 가 저장된 위치를 입력 후 다운로드 버튼 클릭
![azure-vm-4](/categories/images/elastic/azure-vm4.png)
- 접속 시 private Key를 사용하여 접속하도록 설정한다.

  * Moba X Term
    ![azure-vm-5](/categories/images/elastic/azure-vm5.png)
  * ~/.ssh/config 설정
    ```bash
    Host riley-azure
      HostName {IP 주소}
      User {유저명}
      Port {SSH 포트}
      PreferredAuthentications publickey
      IdentityFile ~/.ssh/id_rsa_azure
    ```

## VM에 Elasticsearch 와 Kibana 를 설치하기
* Azure 레퍼런스에 잘 정리되어 있다. 👻
* [Azure의 개발 가상 머신에서 ElasticSearch 배포 - Azure Virtual Machines](https://docs.microsoft.com/ko-kr/azure/virtual-machines/linux/tutorial-elasticsearch)

## Elasticsearch, Kibana 보안설정하기
Elasticsearch와 Kibana를 외부에서도 접근하고 싶다면 보안 설정을 해주어야 한다.

### Elasticsearch와 Kibana host 설정
- 외부 IP로 접근이 가능하도록 host를 `0.0.0.0` 로 설정한 후 각각 서비스를 restart 한다.

### elasticsearch.yml 설정
- /etc/elasticsearch/elasticsearch.yml
- `node.name`에 노드 이름을 지어준다.
- `network.host: 0.0.0.0` 으로 설정해야 외부 IP에서 방화벽이 열려있는 경우 접근 할 수 있다.
- `cluster.initial_master_nodes: ["node-1"]`
    - 엘라스틱서치를 여러개 돌릴경우 각각의 노드이름을 지정해주어야 한다.

```yaml
# ======================== Elasticsearch Configuration =========================
#
# NOTE: Elasticsearch comes with reasonable defaults for most settings.
#       Before you set out to tweak and tune the configuration, make sure you
#       understand what are you trying to accomplish and the consequences.
#
# The primary way of configuring a node is via this file. This template lists
# the most important settings you may want to configure for a production cluster.
#
# Please consult the documentation for further information on configuration options:
# https://www.elastic.co/guide/en/elasticsearch/reference/index.html
#
# ---------------------------------- Cluster -----------------------------------
#
# Use a descriptive name for your cluster:
#
#cluster.name: my-application
#
# ------------------------------------ Node ------------------------------------
#
# Use a descriptive name for the node:
#
node.name: node-1
#
# Add custom attributes to the node:
#
#node.attr.rack: r1
#
# ----------------------------------- Paths ------------------------------------
#
# Path to directory where to store the data (separate multiple locations by comma):
#
path.data: /var/lib/elasticsearch
#
# Path to log files:
#
path.logs: /var/log/elasticsearch
#
# ----------------------------------- Memory -----------------------------------
#
# Lock the memory on startup:
#
#bootstrap.memory_lock: true
#
# Make sure that the heap size is set to about half the memory available
# on the system and that the owner of the process is allowed to use this
# limit.
#
# Elasticsearch performs poorly when the system is swapping the memory.
#
# ---------------------------------- Network -----------------------------------
#
# Set the bind address to a specific IP (IPv4 or IPv6):
#
network.host: 0.0.0.0
#
# Set a custom port for HTTP:
#
#http.port: 9200
#
# For more information, consult the network module documentation.
#
# --------------------------------- Discovery ----------------------------------
#
# Pass an initial list of hosts to perform discovery when this node is started:
# The default list of hosts is ["127.0.0.1", "[::1]"]
#
discovery.seed_hosts: ["127.0.0.1"]
#
# Bootstrap the cluster using an initial set of master-eligible nodes:
#
cluster.initial_master_nodes: ["node-1"]
#
# For more information, consult the discovery and cluster formation module documentation.
#
# ---------------------------------- Gateway -----------------------------------
#
# Block initial recovery after a full cluster restart until N nodes are started:
#
#gateway.recover_after_nodes: 3
#
# For more information, consult the gateway module documentation.
#
# ---------------------------------- Various -----------------------------------
#
# Require explicit names when deleting indices:
#
#action.destructive_requires_name: true
```

### kibana.yml 설정
- /etc/kibana/kibana.yml
- `server.host`에 `0.0.0.0`으로 설정해준다.

```yaml
# Kibana is served by a back end server. This setting specifies the port to use.
#server.port: 5601

# Specifies the address to which the Kibana server will bind. IP addresses and host names are both valid values.
# The default is 'localhost', which usually means remote machines will not be able to connect.
# To allow connections from remote users, set this parameter to a non-loopback address.
server.host: "0.0.0.0"
```

### Azure 인바운드 설정하기
- 홈 > 가상 머신 > {가상머신 이름} > 설정 > 네트워킹
  ![azure-vm-7](/categories/images/elastic/azure-vm7.png)
- 특정 IP에서만 접근 가능하도록 IP 추가 지정할 포트가 여러개인 경우 , 로 여러개를 나열하면 된다.
  ![azure-vm-8](/categories/images/elastic/azure-vm8.png)



