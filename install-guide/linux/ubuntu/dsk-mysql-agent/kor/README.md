# Ubuntu 환경에 DataSaker Mysql agent 설치하기
'Mysql agent'는 데이터베이스의 상태 및 슬로우 쿼리를 실시간으로 수집합니다.
이를 통해 데이터베이스의 성능 지표, 리소스 사용량, 슬로우 쿼리 등 다양한 정보를 수집할 수 있습니다.
수집된 데이터를 기반으로 데이터베이스의 성능 병목 현상을 파악하고, 대응할 수 있습니다.
또한, 슬로우 쿼리를 탐지하여 인덱스 생성, 쿼리 최적화 등의 방법으로 데이터베이스 성능을 개선할 수 있습니다.
고객의 요구사항에 맞게 에이전트 설정을 조정하여 최적의 결과를 제공해 드립니다.

# DataSaker 설정파일을 생성하셨나요?
현재 Ubuntu 환경에서는 `DataSaker` 에이전트를 설치하기 위해서는 기본 설정 파일이 생성되어 있어야 합니다. 만약 기본 설정 파일을 생성하지 않았다면 생성하여 주시기 바랍니다. [DataSaker 설정하기](https://github.com/datasaker/documentation/tree/main/install-guide/linux/ubuntu)

# Mysql agent 설치하기
## 1. agent-config 설정
`/etc/datasaker/dsk-mysql-agent/agent-config.yml`에 내용을 기입합니다.
```yaml
agent:
  metadata:
    agent_name: 'dsk-mysql-agent'
    cluster_id: 'saas-cluster'
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
        url: localhost:19104                                              # 
        filtering_configs:
          rule: drop
```

## 2. 패키지 설치
```bash
curl -fsSL -o installer.sh https://dsk-agent-s3.s3.ap-northeast-2.amazonaws.com/dsk-agent-s3/public/install.sh
chmod 700 installer.sh
sudo ./installer.sh dsk-mysql-agent
```

## 3. 패키지 실행
```bash
$ sudo -E dsk-mysql-agent start
Agent is running
```

## 4. 패키지 실행 상태 확인
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

---
# Mysql agent 제거하기
## 1. 패키지 중단
```bash
sudo dsk-mysql-agent stop
```

## 2. 패키지 제거
```bash
sudo apt remove dsk-mysql-agent
```
