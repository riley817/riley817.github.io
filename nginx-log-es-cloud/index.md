# [Elastic Cloud] nginx 로그를 Elasticsearch Cloud로 수집하기


## 개요
파일비트를 사용하여 웹 서버(nginx) 로그 파일을 수집하고 Elasticsearch Cloud에 전송한다.
Cloud에서 생성한 Kibana 사이트에 접속하면 여러 플랫폼에서 데이터를 수집할 수 있는 예시 파일을 참고 할 수 있다.

{{< figure src="/posts/images/045200.png#center" >}}

### 파일비트 설치
파일비트는 수집할 로그가 쌓이는 서버에 설치했다.

```bash
cd /usr/local/src
sudo curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.11.1-linux-x86_64.tar.gz
tar xzvf filebeat-7.11.1-linux-x86_64.tar.gz
cd filebeat-7.11.1-linux-x86_64
```

### Elastic Cloud 인증정보 설정
Elastic Cloud 접속을 위하여 `filebeat.yml`의 관련항목의 주석을 해제하고 인증정보를 설정한다. 그 외 필요한 설정 정보는 아래 링크를 참조한다.
* [Configuration filebeat.yml](https://www.elastic.co/guide/en/beats/filebeat/current/configuring-howto-filebeat.html#configuring-howto-filebeat)

#### filebeat.yml
```yaml
# =============================== Elastic Cloud ================================
# These settings simplify using Filebeat with the Elastic Cloud (https://cloud.elastic.co/).
# The cloud.id setting overwrites the `output.elasticsearch.hosts` and
# `setup.kibana.host` options.
# You can find the `cloud.id` in the Elastic Cloud web UI.
cloud.id: "<CLOUD_ID>"
# The cloud.auth setting overwrites the `output.elasticsearch.username` and
# `output.elasticsearch.password` settings. The format is `<user>:<pass>`.
cloud.auth: "<CLOUD_USER>:<CLOUD_PASS>"
```
* **cloud.id** : 연결하려는 elastic cloud 아이디를 복사하여 넣는다. cloud id는 메인화면에서 확인 가능하다.
* **cloud.auth** : 항목에는 **"user:pass"** 형태로 basic authentication 형태로 작성한다.

### filebeat input 설정
* 엘라스틱서치로 전송할 input 파일 정보를 설정한다.
* 특별한 설정을 하지 않고 filebeat의 nginx 모듈을 활성화 한 경우 nginx의 기본 로그 폴더가 자동으로 설정된다.
    - `/var/log/nginx/access.log*`, `/var/log/nginx/error.log*` 등
* 특정 로그 디렉토리를 설정하려면 직접 input 설정에 경로를 지정해야 한다.

#### filebeat.yml
```yaml
# ============================== Filebeat inputs ===============================
filebeat.inputs:
# Each - is an input. Most options can be set at the input level, so
# you can use different inputs for various configurations.
# Below are the input specific configurations.
- type: log
  # Change to true to enable this input configuration.
  enabled: true
  # Paths that should be crawled and fetched. Glob based paths.
  paths:
    - /home/riley/logs/*.log
    - /home/riley/logs/tomcat/*.log
    #- c:\programdata\elasticsearch\logs\*
  # Exclude lines. A list of regular expressions to match. It drops the lines that are
  # matching any regular expression from the list.
  #exclude_lines: ['^DBG']
  # Include lines. A list of regular expressions to match. It exports the lines that are
  # matching any regular expression from the list.
  #include_lines: ['^ERR', '^WARN']
  # Exclude files. A list of regular expressions to match. Filebeat drops the files that
  # are matching any regular expression from the list. By default, no files are dropped.
  #exclude_files: ['.gz$']
  # Optional additional fields. These fields can be freely picked
  # to add additional information to the crawled log files for filtering
  #fields:
  #  level: debug
  #  review: 1
  ### Multiline options
  # Multiline can be used for log messages spanning multiple lines. This is common
  # for Java Stack Traces or C-Line Continuation
  # The regexp Pattern that has to be matched. The example pattern matches all lines starting with [
  #multiline.pattern: ^\[
  # Defines if the pattern set under pattern should be negated or not. Default is false.
  #multiline.negate: false
  # Match can be set to "after" or "before". It is used to define if lines should be append to a pattern
  # that was (not) matched before or after or as long as a pattern is not matched based on negate.
  # Note: After is the equivalent to previous and before is the equivalent to to next in Logstash
  #multiline.match: after
```
* **filebeat.inputs.enabled** : true 로 설정
* **filebeat.inputs.path** : 사용자 지정 nginx 로그 경로 형식을 작성한다.

### Output 설정
#### filebeat.yml
```yaml
# ================================== Outputs ===================================
# Configure what output to use when sending the data collected by the beat.
# ---------------------------- Elasticsearch Output ----------------------------
output.elasticsearch:
  # Array of hosts to connect to.
  hosts: ["<ENDPOINT>"]
  # Protocol - either `http` (default) or `https`.
  protocol: "https"
```

* 파일은 Elasticsearch Cloud로 전송할 것이기 때문에 Elasticsearch 정보를 설정한다.
* **host** : Elasticsearch cloud의 endpoint
* **protocol** : endpoint가 ssl이 적용되어있는 경우 https로 설정한다.

### filebeat 설정 적용 및 실행
#### filebeat nginx 모듈 활성화

```bash
sudo ./filebeat modules enable nginx
```

{{< figure src="/posts/images/061016.png#center" >}}

#### filebeat 실행

```bash
sudo ./filebeat -e
```

#### 403 ERROR 일 경우
{{< figure src="/posts/images/61327.png#center" >}}

### Kibana 실행
* Cloud에 있는 Kibana를 실행하게 되면 파일비트로 전송된 nginx Log를 확인 할 수 있다. 
* `filebeat-*` 형식으로 인덱스 패턴이 자동으로 등록된다.
