# Amazon Linux 2023 환경에 Datasaker의 Base agent 설치하기 (Beta)
`Base agent`는 서버에서 발생하는 다양한 정보를 실시간으로 수집합니다.
예를 들어, 메모리, CPU 사용량 등 서버의 성능 지표, 네트워크 트래픽, Container 정보 등 다양한 정보를 수집할 수 있습니다.
이를 통해 고객은 서버의 상태를 실시간으로 모니터링할 수 있으며, 이를 기반으로 서버의 성능을 최적화하고 안정성을 높일 수 있습니다.
고객의 요구사항에 맞게 에이전트 설정을 조정하여 최적의 결과를 제공해 드립니다.

# Datasaker 선행 작업을 진행하였나요?
현재 Amazon Linux 2023  환경에서 `DataSaker`의 선행 작업이 진행되지 않으셨다면 `DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. [DataSaker 선행 작업](${MANUAL_AMAZON_LINUX_KR})

# Base agent 설치하기
## 1. 패키지 설치
`DataSaker`의 `Base agent`를 설치하기 위해서는 sudo 권한이 필요합니다.
```shell
yum install dsk-node-agent
```

## 2. Base agent 설정
```shell
vi /etc/datasaker/dsk-node-agent/agent-config.yml
```
필요에 따라 다음 내용을 수정합니다.
```yaml
# Base agent 설정 파일
agent:
  agent_name: "your_agent_name_what_you_want" # 에이전트 이름 (별칭) default=dsk-node-agent
  cluster_id: "test-cluster_id"               # 관제 대상이 되는 환경이 어떤 클러스터로 묶여있는지에 대한 설정 default=unknown
```

## 3. 패키지 실행
```shell
systemctl enable dsk-node-agent --now
```

## 4. 패키지 실행 상태 확인
```shell
systemctl status dsk-node-agent
```

# Base agent 제거하기
## 1. 패키지 중단
```shell
systemctl stop dsk-node-agent
```

## 2. 패키지 제거
```shell
yum remove dsk-node-agent
```
