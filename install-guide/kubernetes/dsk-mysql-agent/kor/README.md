# 쿠버네티스 환경에서 DataSaker MySQL agents 설치하기
'MySQL agents'는 `mysql agent`와 `plan-mysql-agent`로 구성되어 있습니다.\
이를 통해 데이터베이스의 성능 지표, 리소스 사용량, 슬로우 쿼리 등 다양한 정보를 수집할 수 있습니다.\
고객의 요구사항에 따라 `plan-mysql-agent`는 `on/off` 형태로 사용 할 수 있습니다.

## mysql agent
`mysql agent`는 데이터베이스의 상태를 실시간으로 수집합니다.\
이를 통해 데이터베이스의 성능 지표, 리소스 사용량 등 다양한 정보를 수집할 수 있습니다.\
수집된 데이터를 기반으로 데이터베이스의 성능 병목 현상을 파악하고, 대응할 수 있습니다.\
고객의 요구사항에 맞게 에이전트 설정을 조정하여 최적의 결과를 제공해 드립니다.

## plan-mysql agent
`plan-mysql-agent`는 데이터베이스의 `active session`을 실시간으로 수집합니다.\
이를 통해 데이터베이스의 슬로우 쿼리에 대한 정보를 수집할 수 있습니다.\
슬로우 쿼리를 탐지하여 인덱스 생성, 쿼리 최적화 등의 방법으로 데이터베이스 성능을 개선할 수 있습니다.\
고객의 요구사항에 맞게 에이전트 설정을 조정하여 최적의 결과를 제공해 드립니다.

# DataSaker를 설치하셨나요?
현재 Kubernetes 환경에 `DataSaker`가 설치되어 있지 않다면 `DataSaker` 설치를 먼저 진행하여 주시기 바랍니다. [DataSaker 설치하기](https://github.com/datasaker/documentation/tree/main/install-guide/kubernetes)

# Mysql agent 설치하기
## 1. Mysql agent 설정값 등록
```shell
cat << EOF >> ~/datasaker/config.yaml

mysqlAgents:
  list:
    - name: 'my-mysql'
      tolerations: []
      imgPolicy: 'Always'
      imgVersion: 'latest'
      logLevel: 'INFO'
      mysqlPlan: true
      targetAddr: '127.0.0.1'
      targetPort: '3306'
      database: "database"
      user: 'user'
      pass: "pass"
      listenPort: 19104
      exporterArgs: []
      extraArgs: []
      explain:
        scrape_interval: 5s
        scrape_timeout: 5s
        slow_query_standard: 5s
        executor_number: 10
        sender_number: 10
        activity_query_buffer: 50
        plan_sender_buffer: 50
      resources:
        requests:
          cpu: 100m
          memory: 512Mi
        limits:
          cpu: 1000m
          memory: 1000Mi
EOF
```

### Mysql agent 설정 값 
Mysql agent의 설정 값의 의미와 default값은 다음과 같습니다. 사용자마다 에이전트 설정에 대해 다른 요구사항이 있습니다. 따라서 에이전트 설정을 사용자 설정에 맞게 조정해야 합니다. 최적의 결과를 위해 에이전트 설정을 조정하세요.
"~/datasaker/config.yaml"에서 해당 값을 추가하거나 수정하세요.
```yaml
mysqlAgents:
  list:
    - name: 'my-mysql'                      # mysql를 구분할 수 있는 이름 (반듯이 사용자가 명시해야 합니다.)
      tolerations: []                       # 배포할 워커 노드에 taint가 설정되어 있을 경우 taint를 추가합니다.
      imgPolicy: 'Always'                   # agent의 Image Policy를 설정합니다. [Always, IfNotPresent, Never]
      imgVersion: 'latest'                  # agent의 Image 태그를 설정합니다.
      logLevel: 'INFO'                      # agent에서 남기는 log level을 설정합니다. [debug > info > warn > error > panic > fatal]
      mysqlPlan: true                       # plan 데이터 수집 설정을 활성화 합니다.
      targetAddr: '127.0.0.1'               # mysql의 IP를 설정합니다.
      targetPort: '3306'                    # mysql의 Port를 설정합니다.
      database: "database"                  # 모니터링할 대상의 mysql의의 데이터베이스를 설정합니다.
      user: 'user'                          # mysql 계정의 ID를 설정합니다.
      pass: "pass"                          # mysql 계정의 Password를 설정합니다.
      listenPort: 19104                     # agent에서 사용되는 exporter port를 설정합니다.
      exporterArgs: []                      # mysql exporter의 arguments
      extraArgs: [] 
      explain:
        scrape_interval: 5s                 # explain 수집 주기를 설정합니다.
        scrape_timeout: 5s                  # explain 요청 시 타임아웃을 설정합니다.
        slow_query_standard: 5s             # slow query 기준을 설정합니다.
        executor_number: 10                 # explain worker 개수
        sender_number: 10                   # DataSaker에 전송하는 worker의 개수 
        slow_query_standard: 5s             # slow query의 기준이 되는 시간
      resources:                            # agent의 resource를 설정합니다. 너무 작게할 경우 정상동작을 못할 수 있습니다.
        requests:
          cpu: 100m
          memory: 512Mi
        limits:
          cpu: 1000m
          memory: 1000Mi
```

## 2. Mysql agent 동작
```shell
helm upgrade datasaker ~/datasaker/agent-helm -n datasaker \
  -f ~/datasaker/config.yaml
```

## exporterArgs value
`DataSaker MySQL 에이전트`의 인자 목록입니다.

| Argument                                                  | MySQL Version | Description                                                                           |
|-----------------------------------------------------------|---------------|---------------------------------------------------------------------------------------|
| collect.info_schema.clientstats                           | 5.5           | If running with userstat=1, set to true to collect client statistics.                 |
| collect.info_schema.innodb_metrics                        | 5.6           | Collect metrics from information_schema.innodb_metrics.                               |
| collect.info_schema.innodb_tablespaces                    | 5.7           | Collect metrics from information_schema.innodb_sys_tablespaces.                       |
| collect.info_schema.innodb_cmp                            | 5.5           | Collect InnoDB compressed tables metrics from information_schema.innodb_cmp.          |
| collect.info_schema.innodb_cmpmem                         | 5.5           | Collect InnoDB buffer pool compression metrics from information_schema.innodb_cmpmem. |
| collect.info_schema.processlist                           | 5.1           | Collect thread state counts from information_schema.processlist.                      |
| collect.info_schema.processlist.min_time                  | 5.1           | Minimum time a thread must be in each state to be counted. (default: 0)               |
| collect.info_schema.query_response_time                   | 5.5           | Collect query response time distribution if query_response_time_stats is ON.          |
| collect.info_schema.replica_host                          | 5.6           | Collect metrics from information_schema.replica_host_status.                          |
| collect.info_schema.tables                                | 5.1           | Collect metrics from information_schema.tables.                                       |
| collect.info_schema.tables.databases                      | 5.1           | The list of databases to collect table stats for, or '*' for all.                     |
| collect.info_schema.tablestats                            | 5.1           | If running with userstat=1, set to true to collect table statistics.                  |
| collect.info_schema.schemastats                           | 5.1           | If running with userstat=1, set to true to collect schema statistics                  |
| collect.info_schema.userstats                             | 5.1           | If running with userstat=1, set to true to collect user statistics.                   |
| collect.perf_schema.eventsstatements                      | 5.6           | Collect metrics from performance_schema.events_statements_summary_by_digest.          |
| collect.perf_schema.eventsstatements.digest_text_limit    | 5.6           | Maximum length of the normalized statement text. (default: 120)                       |
| collect.perf_schema.eventsstatements.limit                | 5.6           | Limit the number of events statements digests by response time. (default: 250)        |
| collect.perf_schema.eventsstatements.timelimit            | 5.6           | Limit how old the 'last_seen' events statements can be, in seconds. (default: 86400)  |
| collect.perf_schema.eventsstatementssum                   | 5.7           | Collect metrics from performance_schema.events_statements_summary_by_digest summed.   |
| collect.perf_schema.eventswaits                           | 5.5           | Collect metrics from performance_schema.events_waits_summary_global_by_event_name.    |
| collect.perf_schema.file_events                           | 5.6           | Collect metrics from performance_schema.file_summary_by_event_name.                   |
| collect.perf_schema.file_instances                        | 5.5           | Collect metrics from performance_schema.file_summary_by_instance.                     |
| collect.perf_schema.file_instances.remove_prefix          | 5.5           | Remove path prefix in performance_schema.file_summary_by_instance.                    |
| collect.perf_schema.indexiowaits                          | 5.6           | Collect metrics from performance_schema.table_io_waits_summary_by_index_usage.        |
| collect.perf_schema.memory_events                         | 5.7           | Collect metrics from performance_schema.memory_summary_global_by_event_name.          |
| collect.perf_schema.memory_events.remove_prefix           | 5.7           | Remove instrument prefix in performance_schema.memory_summary_global_by_event_name.   |
| collect.perf_schema.tableiowaits                          | 5.6           | Collect metrics from performance_schema.table_io_waits_summary_by_table.              |
| collect.perf_schema.tablelocks                            | 5.6           | Collect metrics from performance_schema.table_lock_waits_summary_by_table.            |
| collect.perf_schema.replication_group_members             | 5.7           | Collect metrics from performance_schema.replication_group_members.                    |
| collect.perf_schema.replication_group_member_stats        | 5.7           | Collect metrics from performance_schema.replication_group_member_stats.               |
| collect.perf_schema.replication_applier_status_by_worker  | 5.7           | Collect metrics from performance_schema.replication_applier_status_by_worker.         |

## optional collector flags

| Argument                        | MySQL Version | Description                                                                                         |
|---------------------------------|---------------|-----------------------------------------------------------------------------------------------------|
| collect.auto_increment.columns  | 5.1           | Collect auto_increment columns and max values from information_schema.                              |
| collect.binlog_size             | 5.1           | Collect the current size of all registered binlog files                                             |
| collect.engine_innodb_status    | 5.1           | Collect from SHOW ENGINE INNODB STATUS.                                                             |
| collect.engine_tokudb_status    | 5.6           | Collect from SHOW ENGINE TOKUDB STATUS.                                                             |
| collect.global_status           | 5.1           | Collect from SHOW GLOBAL STATUS (Enabled by default)                                                |
| collect.global_variables        | 5.1           | Collect from SHOW GLOBAL VARIABLES (Enabled by default)                                             |
| collect.mysql.user              | 5.5           | Collect data from mysql.user table                                                                  |
| collect.slave_status            | 5.1           | Collect from SHOW SLAVE STATUS (Enabled by default)                                                 |
| collect.slave_hosts             | 5.1           | Collect from SHOW SLAVE HOSTS                                                                       |
| collect.heartbeat               | 5.1           | Collect from heartbeat.                                                                             |
| collect.heartbeat.database      | 5.1           | Database from where to collect heartbeat data. (default: heartbeat)                                 |
| collect.heartbeat.table         | 5.1           | Table from where to collect heartbeat data. (default: heartbeat)                                    |
| collect.heartbeat.utc           | 5.1           | Use UTC for timestamps of the current server (pt-heartbeat is called with --utc). (default: false)  |