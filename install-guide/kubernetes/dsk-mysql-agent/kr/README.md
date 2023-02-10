# 쿠버네티스 환경에서 MySQL 에이전트 helm으로 설치하기

## 기본 에이전트를 설치하셨나요?

**기본 에이전트**가 설치되어 있지 않으면, 먼저 **기본 에이전트**를 설치해야됩니다. (링크)

## ~/datasaker/agent-helm/values.yaml 수정

'list:' 뒤에 대괄호를 제거합니다.  
'-name' 부터 주석을 제거 하여 mysql의 정보를 입력합니다.

```shell
vim ~/datasaker/agent-helm/values.yaml
```

mysqlAgent 항목을 수정합니다.

```yaml
mysqlAgent:
  tolerations: []
  list: #[] <- 대괄호 제거 혹은 주석처리
    - name: 'mysql-1'
      targetIp: '127.0.0.1'
      targetPort: '3306'
      user: 'user'
      pass: "pass"
      imgPolicy: 'Always'
      imgVersion: 'develop'
      logLevel: 'DEBUG'
      extraArgs:
        - --collect.heartbeat
        - --collect.heartbeat.utc
      resources:
        requests:
          cpu: 100m
          memory: 512Mi
        limits:
          cpu: 1000m
          memory: 1000Mi
```

### 에이전트 배포 전략

에이전트 배포 설정은 '~/datasaker/agent-helm/values.yaml' 에 `toleration` 설정을 추가한다. toleration 에 대한 자세한 설명은 [쿠버네티스 공식 문서 링크](https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration/)를 참고한다.

## 헬름 업그래이드 수행

```shell
helm upgrade datasaker ~/datasaker/agent-helm -n datasaker \
  -f ~/datasaker/config.yaml
```

## extraArgs value

**데이터세이커 MySQL 에이전트**의 인자 목록입니다.

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