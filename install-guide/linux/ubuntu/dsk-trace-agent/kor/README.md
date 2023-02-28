# Ubuntu 환경에 DataSaker Trace agent 설치하기
`Trace agent`는 opentelemetry와 Jaeger와 같은 오픈소스 분산 추적 시스템과 연동하여, 애플리케이션의 분산 추적 데이터를 수집합니다.
이를 통해 애플리케이션 내부의 다양한 서비스 간의 통신을 추적하고, 성능 병목 현상을 식별하여 최적화할 수 있습니다.
수집된 데이터는 빠르게 처리되어 실시간으로 모니터링 및 분석이 가능합니다.
고객의 요구사항에 맞게 "Trace Agent" 설정을 조정하여 최적의 결과를 제공해 드립니다.

# DataSaker 설정파일을 생성하셨나요?
현재 Ubuntu 환경에서는 `DataSaker` 에이전트를 설치하기 위해서는 기본 설정 파일이 생성되어 있어야 합니다. 만약 기본 설정 파일을 생성하지 않았다면 생성하여 주시기 바랍니다. [DataSaker 설정하기](https://github.com/datasaker/documentation/tree/main/install-guide/linux/ubuntu)

# Trace agent 설치하기
## 1. 패키지 설치
<!-- 
example API Key : VAR_GLOBAL_APIKEY=1234567890abcdef1234567890abcdef
 -->
``` shell
curl -fsSL -o installer.sh https://dsk-agent-s3.s3.ap-northeast-2.amazonaws.com/dsk-agent-s3/public/install.sh
chmod 700 installer.sh
sudo ./installer.sh dsk-trace-agent
```

## 2. Trace agent 설정
``` shell
vi /etc/datasaker/dsk-trace-agent/agent-config.yaml
```

필요에 따라 다음 내용을 수정합니다.

``` yaml
# Trace agent 설정 파일
agent:
  agent_name: "your_agent_name_what_you_want" # default=trace-agent
  cluster_id: "test-cluster-id"               # default=unknown_cluster
```

## 3. 패키지 실행
```shell
sudo /usr/bin/dsk-trace-agent start
```

## 4. 패키지 실행 상태 확인
```shell
$ sudo dsk-trace-agents status
Agent is running
Exporter is running
```

# Trace agent 제거하기
## 1. 패키지 중단
```shell
sudo dsk-trace-agent stop
```

## 2. 패키지 제거
```shell
sudo apt-get remove dsk-trace-agent
```
