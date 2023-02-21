# Ubuntu 환경에 DataSaker Postgres agent 설치하기

## DataSaker 설정파일을 생성하셨나요?
현재 Ubuntu 환경에서는 `DataSaker` 에이전트를 설치하기 위해서는 기본 설정 파일이 생성되어 있어야 합니다. 만약 기본 설정 파일을 생성하지 않았다면 생성하여 주시기 바랍니다. [DataSaker 설정하기](../../README.md)

# Postgres agent 설치하기
## 1. agent-config 설정
`/etc/datasaker/dsk-postgres-agent/agent-config.yml`에 내용을 기입합니다.
```yaml
agent:
  metadata:
  # agent_name: my-dsk-postgres-agent
  # cluster_id: my-cluster
  option:
    exporter_config:
      command: "/usr/bin/dsk-postgres-exporter"
      port: 19187
      args:
        - --extend.query-path=/etc/datasaker/dsk-postgres-agent/queries.yaml
    scrape_interval: 15s
    scrape_timeout: 5s
    scrape_configs:
      - job_name: dsk-postgres-agent
        url: localhost:19187
        filtering_configs:
          rule: drop
```

## 2. 패키지 설치
```bash
> DSK_GLOBAL_API_KEY=${VAR_GLOBAL_APIKEY}
> curl -fsSL -o installer.sh https://nextcloud.exem-oss.org/s/M2FyX83ZcA6MZkW/download/dsk-postgres-agent-install.sh
> chmod 700 installer.sh
> sudo ./installer.sh

> dsk-postgres-agent status
```
## 3. 패키지 실행
```bash
$ sudo -E dsk-postgres-agent start
Agent is running
```

## 4. 패키지 실행 상태 확인
### Running
```bash
$ sudo dsk-postgres-agent status
Agent is running
```
### Not Running
```bash
$ sudo dsk-postgres-agent status
Agent is not running
```

# Postgres agent 제거하기
## 1. 패키지 중단
```bash
$ sudo dsk-postgres-agent stop
```

## 2. 패키지 제거
```bash
sudo apt remove dsk-postgres-agent
```
