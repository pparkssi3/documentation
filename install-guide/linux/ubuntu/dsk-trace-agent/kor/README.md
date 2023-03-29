# Ubuntu 환경에 DataSaker Trace agent 설치하기 (Beta)
`Trace agent`는 opentelemetry와 Jaeger와 같은 오픈소스 분산 추적 시스템과 연동하여, 애플리케이션의 분산 추적 데이터를 수집합니다.
이를 통해 애플리케이션 내부의 다양한 서비스 간의 통신을 추적하고, 성능 병목 현상을 식별하여 최적화할 수 있습니다.
수집된 데이터는 빠르게 처리되어 실시간으로 모니터링 및 분석이 가능합니다.
고객의 요구사항에 맞게 "Trace Agent" 설정을 조정하여 최적의 결과를 제공해 드립니다.

# DataSaker 선행 작업을 진행하였나요?
현재 Ubuntu 환경에서는 `DataSaker`의 선행 작업이 진행되지 않으셨다면 `DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. [DataSaker 선행 작업](${MANUAL_UBUNTU_KR})

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
systemctl enable dsk-trace-agent --now
```

## 4. 패키지 실행 상태 확인

```shell
sudo systemctl status dsk-trace-agent
```

# Trace agent 제거하기

## 1. 패키지 중단

```shell
systemctl stop dsk-trace-agent
```

## 2. 패키지 제거

```shell
sudo apt remove dsk-trace-agent
```
