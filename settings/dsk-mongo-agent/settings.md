# Arguments

| Argument                          | Description                                                                                                                            |
|-----------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| --mongodb.collstats-colls         | List of comma separared databases.collections to get $collStats                                                                        |
| --mongodb.indexstats-colls        | List of comma separared databases.collections to get $indexStats                                                                       |
| --[no-]mongodb.direct-connect     | Whether or not a direct connect should be made. Direct connections are not valid if multiple hosts are specified or an SRV URI is used |
| --[no-]mongodb.global-conn-pool   | Use global connection pool instead of creating new pool for each http request                                                          |
| --mongodb.uri                     | MongoDB connection URI ($MONGODB_URI)                                                                                                  |
| --web.listen-address              | Address to listen on for web interface and telemetry                                                                                   |
| --web.telemetry-path              | Metrics expose path                                                                                                                    |
| --web.config                      | Path to the file having Prometheus TLS config for basic auth                                                                           |
| --log.level                       | Only log messages with the given severity or above. Valid levels: [debug, info, warn, error, fatal]                                    |
| --collector.diagnosticdata        | Enable collecting metrics from getDiagnosticData                                                                                       |
| --collector.replicasetstatus      | Enable collecting metrics from replSetGetStatus                                                                                        |
| --collector.dbstats               | Enable collecting metrics from dbStats                                                                                                 |
| --collector.topmetrics            | Enable collecting metrics from top admin command                                                                                       |
| --collector.indexstats            | Enable collecting metrics from $indexStats                                                                                             |
| --collector.collstats             | Enable collecting metrics from $collStats                                                                                              |
| --collect-all                     | Enable all collectors. Same as specifying all --collector.<name>                                                                       |
| --collector.collstats-limit=0     | Disable collstats, dbstats, topmetrics and indexstats collector if there are more than <n> collections. 0=No limit                     |
| --metrics.overridedescendingindex | Enable descending index name override to replace -1 with _DESC                                                                         |
