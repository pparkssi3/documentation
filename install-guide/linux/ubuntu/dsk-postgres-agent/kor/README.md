# Ubuntu 환경에 DataSaker Postgres agent 설치하기 (Beta)
`postgres agent`는 데이터베이스의 상태를 실시간으로 수집합니다.\
이를 통해 데이터베이스의 성능 지표, 리소스 사용량 등 다양한 정보를 수집할 수 있습니다.\
수집된 데이터를 기반으로 데이터베이스의 성능 병목 현상을 파악하고, 대응할 수 있습니다.\
고객의 요구사항에 맞게 에이전트 설정을 조정하여 최적의 결과를 제공해 드립니다.

## postgres version
|version|support|
|---|---|
|postgres 15|X|
|postgres 14|O|
|postgres 13|X|
|postgres 12|X|
|postgres 11|X|
|postgres 10|X|
|postgres 9|X|
|postgres 8|X|

# DataSaker 선행 작업을 진행하였나요?
현재 Ubuntu 환경에서는 `DataSaker`의 선행 작업이 진행되지 않으셨다면 `DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. [DataSaker 선행 작업](${MANUAL_UBUNTU_KR})

# Postgres agent 설치하기

## 1. 패키지 설치
```bash
curl -fsSL -o installer.sh https://dsk-agent-s3.s3.ap-northeast-2.amazonaws.com/dsk-agent-s3/public/install.sh
chmod 700 installer.sh
sudo ./installer.sh dsk-postgres-agent
```

## 2. agent-config 설정
`/etc/datasaker/dsk-postgres-agent/agent-config.yml`에 내용을 기입합니다.
```yaml
agent:
  metadata:
    agent_name: my-dsk-postgres-agent
    cluster_id: my-cluster
  option:
    exporter_config:
      command: "/usr/bin/dsk-postgres-exporter"
      port: 19187
      args:
        - --extend.query-path=/etc/datasaker/dsk-postgres-agent/queries.yaml
        - --data-source-user=<monitoring account name>
        - --data-source-pass=<monitoring account pass>
        - --data-source-uri=<monitoring database uri>
    scrape_interval: 15s
    scrape_timeout: 5s
    scrape_configs:
      - job_name: dsk-postgres-agent
        url: localhost:19187
        filtering_configs:
          rule: drop
```

## 3. 패키지 실행
```shell
systemctl enable dsk-postgres-agent --now
```

## 4. 패키지 실행 상태 확인
```shell
systemctl status dsk-postgres-agent
```
또는
```shell
service dsk-postgres-agent
```

# Postgres agent 제거하기

## 1. 패키지 중단
```shell
systemctl stop dsk-postgres-agent
```

## 2. 패키지 제거
```shell
sudo apt remove dsk-postgres-agent
```
