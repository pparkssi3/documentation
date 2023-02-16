# Ubuntu 환경 dsk-mysql-agent 설치 방법

## global-config 설정
`dsk-mysql-agent`는 의존성으로 `dsk-metric-sidecar`가 필요합니다.\
`apt`를 이용해 `dsk-mysql-agent`를 설치하는 경우 자동으로 `dsk-metric-sidecar`가 설치됩니다.\
또한 `dsk-metric-sidecar`에 의해 기본 `/etc/datasaker/global-config.yml`이 생성됩니다.

* dsk-metric-sidecar가 생성한 `global-config.yml`을 이용하는 경우
```bash
sudo sed -i "s!REPLACE_API_KEY!${VAR_GLOBAL_APIKEY}!g" /etc/datasaker/global-config.yml
```

* UI 상에서 생성된 코드를 이용하는 경우
```bash
echo "
global:
  api_key: ${VAR_GLOBAL_APIKEY}
  gates:
    metric_datagate:
      url: gate.kr.datasaker.io:31302
      remote_timeout: 5s
    manifest_datagate:
      url: gate.kr.datasaker.io:31301
      remote_timeout: 5s
    trace_datagate:
      url: gate.kr.datasaker.io:31300
      remote_timeout: 5s
    plan_datagate:
      url: gate.kr.datasaker.io:31303
      remote_timeout: 5s
    loggate:
      url: gate.kr.datasaker.io:31304
      remote_timeout: 5s
  agent_manager:
    url: api.kr.datasaker.io
    base_url: /dsk-agentmanager-api/agent
    send_interval: 1m
" | sudo tee /etc/datasaker/global-config.yml
```

## agent-config 설정
> `/etc/datasaker/dsk-mysql-agent/agent-config.yml`에 원하는 내용을 기입합니다.

```yaml
agent:
  metadata:
  # agent_name: 'dsk-mysql-agent'
  # cluster_id: 'saas-cluster'
  option:
    exporter_config:
      command: "/usr/bin/dsk-mysqld-exporter"
      port: 19104
      args:
        - --collect.info_schema.clientstats
        - --collect.info_schema.innodb_metrics
        - --collect.info_schema.innodb_tablespaces
        - --collect.info_schema.innodb_cmp
        - --collect.info_schema.innodb_cmpmem
        - --collect.info_schema.processlist
        - --collect.info_schema.query_response_time
        - --collect.info_schema.replica_host
        - --collect.info_schema.tables
        - --collect.info_schema.tables.databases=‘*’
        - --collect.info_schema.tablestats
        - --collect.info_schema.schemastats
        - --collect.info_schema.userstats
        - --collect.perf_schema.eventsstatements
        - --collect.perf_schema.eventsstatementssum
        - --collect.perf_schema.eventswaits
        - --collect.perf_schema.file_events
        - --collect.perf_schema.file_instances
        - --collect.perf_schema.file_instances.remove_prefix=false
        - --collect.perf_schema.indexiowaits
        - --collect.perf_schema.memory_events
        - --collect.perf_schema.memory_events.remove_prefix=false
        - --collect.perf_schema.tableiowaits
        - --collect.perf_schema.tablelocks
        - --collect.perf_schema.replication_group_members
        - --collect.perf_schema.replication_group_member_stats
        - --collect.perf_schema.replication_applier_status_by_worker
    scrape_configs:
      - job_name: 'dsk-mysql-agent'
        url: localhost:19104
        filtering_configs:
          rule: drop
```

## 1. 패키지 설치
> sudo 권한이 필요합니다.

```bash
> DSK_GLOBAL_API_KEY=${VAR_GLOBAL_APIKEY}
> curl -fsSL -o installer.sh https://nextcloud.exem-oss.org/s/qPwDBm8QRLf7PGA/download/dsk-mysql-agent-install.sh
> chmod 700 installer.sh
> sudo ./installer.sh

> dsk-mysql-agent status
```

## 2. 패키지 실행
```bash
$ sudo -E dsk-mysql-agent start
Agent is running
```

## 3. 패키지 실행 상태 확인

### Running
```bash
$ sudo dsk-mysql-agents status
Agent is running
```

### Not Running
```bash
$ sudo dsk-mysql-agents status
Agent is not running
```

## 4. 패키지 중단
```bash
sudo dsk-mysql-agent stop
```

## 5. 패키지 제거
```bash
sudo apt remove dsk-mysql-agent
```
