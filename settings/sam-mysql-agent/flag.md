Ref: https://github.com/prometheus/mysqld_exporter#readme

# Running

## Single exporter mode

Running using ~/.my.cnf:

    ./mysqld_exporter <flags>

## Multi-target support

This exporter supports the multi-target pattern. This allows running a single instance of this exporter for multiple MySQL targets.

To use the multi-target functionality, send an http request to the endpoint /probe?target=foo:5432 where target is set to the DSN of the MySQL instance to scrape metrics from.

To avoid putting sensitive information like username and password in the URL, you can have multiple configurations in `config.my-cnf` file and match it by adding `&auth_module=<section>` to the request.

Sample config file for multiple configurations

        [client]
        user = foo
        password = foo123
        [client.servers]
        user = bar
        password = bar123

On the prometheus side you can set a scrape config as follows

        - job_name: mysql # To get metrics about the mysql exporter’s targets
          params:
            # Not required. Will match value to child in config file. Default value is `client`.
            auth_module: client.servers
          static_configs:
            - targets:
              # All mysql hostnames to monitor.
              - server1:3306
              - server2:3306
          relabel_configs:
            - source_labels: [__address__]
              target_label: __param_target
            - source_labels: [__param_target]
              target_label: instance
            - target_label: __address__
              # The mysqld_exporter host:port
              replacement: localhost:9104

## Flag format

Example format for flags for version > 0.10.0:

    --collect.auto_increment.columns
    --no-collect.auto_increment.columns

Example format for flags for version <= 0.10.0:

    -collect.auto_increment.columns
    -collect.auto_increment.columns=[true|false]

### Collector Flags

| Name                                                     | MySQL Version | Description                                                                                            |
|----------------------------------------------------------|---------------|--------------------------------------------------------------------------------------------------------|
| collect.auto_increment.columns                           | 5.1           | Collect auto_increment columns and max values from information_schema.                                 |
| collect.binlog_size                                      | 5.1           | Collect the current size of all registered binlog files                                                |
| collect.engine_innodb_status                             | 5.1           | Collect from SHOW ENGINE INNODB STATUS.                                                                |
| collect.engine_tokudb_status                             | 5.6           | Collect from SHOW ENGINE TOKUDB STATUS.                                                                |
| collect.global_status                                    | 5.1           | Collect from SHOW GLOBAL STATUS (Enabled by default)                                                   |
| collect.global_variables                                 | 5.1           | Collect from SHOW GLOBAL VARIABLES (Enabled by default)                                                |
| collect.info_schema.clientstats                          | 5.5           | If running with userstat=1, set to true to collect client statistics.                                  |
| collect.info_schema.innodb_metrics                       | 5.6           | Collect metrics from information_schema.innodb_metrics.                                                |
| collect.info_schema.innodb_tablespaces                   | 5.7           | Collect metrics from information_schema.innodb_sys_tablespaces.                                        |
| collect.info_schema.innodb_cmp                           | 5.5           | Collect InnoDB compressed tables metrics from information_schema.innodb_cmp.                           |
| collect.info_schema.innodb_cmpmem                        | 5.5           | Collect InnoDB buffer pool compression metrics from information_schema.innodb_cmpmem.                  |
| collect.info_schema.processlist                          | 5.1           | Collect thread state counts from information_schema.processlist.                                       |
| collect.info_schema.processlist.min_time                 | 5.1           | Minimum time a thread must be in each state to be counted. (default: 0)                                |
| collect.info_schema.query_response_time                  | 5.5           | Collect query response time distribution if query_response_time_stats is ON.                           |
| collect.info_schema.replica_host                         | 5.6           | Collect metrics from information_schema.replica_host_status.                                           |
| collect.info_schema.tables                               | 5.1           | Collect metrics from information_schema.tables.                                                        |
| collect.info_schema.tables.databases                     | 5.1           | The list of databases to collect table stats for, or '`*`' for all.                                    |
| collect.info_schema.tablestats                           | 5.1           | If running with userstat=1, set to true to collect table statistics.                                   |
| collect.info_schema.schemastats                          | 5.1           | If running with userstat=1, set to true to collect schema statistics                                   |
| collect.info_schema.userstats                            | 5.1           | If running with userstat=1, set to true to collect user statistics.                                    |
| collect.mysql.user                                       | 5.5           | Collect data from mysql.user table                                                                     |
| collect.perf_schema.eventsstatements                     | 5.6           | Collect metrics from performance_schema.events_statements_summary_by_digest.                           |
| collect.perf_schema.eventsstatements.digest_text_limit   | 5.6           | Maximum length of the normalized statement text. (default: 120)                                        |
| collect.perf_schema.eventsstatements.limit               | 5.6           | Limit the number of events statements digests by response time. (default: 250)                         |
| collect.perf_schema.eventsstatements.timelimit           | 5.6           | Limit how old the 'last_seen' events statements can be, in seconds. (default: 86400)                   |
| collect.perf_schema.eventsstatementssum                  | 5.7           | Collect metrics from performance_schema.events_statements_summary_by_digest summed.                    |
| collect.perf_schema.eventswaits                          | 5.5           | Collect metrics from performance_schema.events_waits_summary_global_by_event_name.                     |
| collect.perf_schema.file_events                          | 5.6           | Collect metrics from performance_schema.file_summary_by_event_name.                                    |
| collect.perf_schema.file_instances                       | 5.5           | Collect metrics from performance_schema.file_summary_by_instance.                                      |
| collect.perf_schema.file_instances.remove_prefix         | 5.5           | Remove path prefix in performance_schema.file_summary_by_instance.                                     |
| collect.perf_schema.indexiowaits                         | 5.6           | Collect metrics from performance_schema.table_io_waits_summary_by_index_usage.                         |
| collect.perf_schema.memory_events                        | 5.7           | Collect metrics from performance_schema.memory_summary_global_by_event_name.                           |
| collect.perf_schema.memory_events.remove_prefix          | 5.7           | Remove instrument prefix in performance_schema.memory_summary_global_by_event_name.                    |
| collect.perf_schema.tableiowaits                         | 5.6           | Collect metrics from performance_schema.table_io_waits_summary_by_table.                               |
| collect.perf_schema.tablelocks                           | 5.6           | Collect metrics from performance_schema.table_lock_waits_summary_by_table.                             |
| collect.perf_schema.replication_group_members            | 5.7           | Collect metrics from performance_schema.replication_group_members.                                     |
| collect.perf_schema.replication_group_member_stats       | 5.7           | Collect metrics from performance_schema.replication_group_member_stats.                                |
| collect.perf_schema.replication_applier_status_by_worker | 5.7           | Collect metrics from performance_schema.replication_applier_status_by_worker.                          |
| collect.slave_status                                     | 5.1           | Collect from SHOW SLAVE STATUS (Enabled by default)                                                    |
| collect.slave_hosts                                      | 5.1           | Collect from SHOW SLAVE HOSTS                                                                          |
| collect.heartbeat                                        | 5.1           | Collect from heartbeat.                                                                                |
| collect.heartbeat.database                               | 5.1           | Database from where to collect heartbeat data. (default: heartbeat)                                    |
| collect.heartbeat.table                                  | 5.1           | Table from where to collect heartbeat data. (default: heartbeat)                                       |
| collect.heartbeat.utc                                    | 5.1           | Use UTC for timestamps of the current server (`pt-heartbeat` is called with `--utc`). (default: false) |

### General Flags

| Name                       | Description                                                                                            |
|----------------------------|--------------------------------------------------------------------------------------------------------|
| mysqld.address             | Hostname and port used for connecting to MySQL server, format: `host:port`. (default: `locahost:3306`) |
| mysqld.username            | Username to be used for connecting to MySQL Server                                                     |
| config.my-cnf              | Path to .my.cnf file to read MySQL credentials from. (default: `~/.my.cnf`)                            |
| log.level                  | Logging verbosity (default: info)                                                                      |
| exporter.lock_wait_timeout | Set a lock_wait_timeout (in seconds) on the connection to avoid long metadata locking. (default: 2)    |
| exporter.log_slow_filter   | Add a log_slow_filter to avoid slow query logging of scrapes.  NOTE: Not supported by Oracle MySQL.    |
| tls.insecure-skip-verify   | Ignore tls verification errors.                                                                        |
| web.config.file            | Path to a web configuration file                                                                       |
| web.listen-address         | Address to listen on for web interface and telemetry.                                                  |
| web.telemetry-path         | Path under which to expose metrics.                                                                    |
| version                    | Print the version information.                                                                         |

### Environment Variables

| Name                     | Description                                        |
|--------------------------|----------------------------------------------------|
| MYSQLD_EXPORTER_PASSWORD | Password to be used for connecting to MySQL Server |