# [Elasticsearch 검색 엔진 구축 강의] 그 외 설정


## limits.conf
Elasticsearch 는 많은 파일을 쓰거나 핸들링하게 된다. 열 수 있는 File `descriptor`가 부족하면 데이터의 손실로 이어질 수 있으며 실행할 수 있는 최대 File Descriptor 를 65,536 개 이상으로 설정하도록 한다.

**vi /etc/security/limits.conf**
```bash
elasticsearch soft nofile 65536
elasticsearch hard nofile 65536
```

## Thread 수
Elasticsearch 는 여러 유형의 작업에 대해 많은 Thread Pool을 사용하게 된다. Elasticsearch 사용자가 만들 수 있는 Thread 가 4096 개 이상인지 확인한다.
```bash
elasticsearch soft noproc 4096
elasticsearch hard noproc 4096
```

## Sysconfig file
RPM 또는 Debian 패키지를 사용하여 시스템 설정 및 환경 변수를 지정할 수 있다.
```bash
/etc/sysconfig/elasticsearch
```

## Virtual Memory
Elasticsearch 는 기본적으로 Index 를 저장하기 위해 `FileSystem`을 사용하기 위한 `mmmap`을 사용한다. 
`mmap`의 개수는 기본 운영체제의 제한이 너무 낮아 메모리 부족 예외를 발생시키기도 한다.

```bash
sudo vi /etc/sysctl.conf

# /etc/sysctl.conf
vm.max_map_count=22144
```

```bash
sudo sysstl -p
```

## Swap disabling
일반적으로 JVM 옵션에서 메모리 사용이 제어되므로 Swap을 활성시킬 필요는 없다. 

```bash
# 일시적으로 swap을 비활성화
sudo swapoff -a
```

## swappiness configuration
`m.swappiness`가 1로 설정되어 있는지 확인한다. 이렇게 하면 swap되는 경우가 줄어들고 일반적인 상황에서는 swapping으로 이어지지 않는다.


