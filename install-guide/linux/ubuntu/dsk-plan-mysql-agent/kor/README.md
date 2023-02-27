# Ubuntu 환경에 DataSaker MySQL agent 설치하기
'Mysql agent'는 데이터베이스의 상태 및 슬로우 쿼리를 실시간으로 수집합니다.
이를 통해 데이터베이스의 성능 지표, 리소스 사용량, 슬로우 쿼리 등 다양한 정보를 수집할 수 있습니다.
수집된 데이터를 기반으로 데이터베이스의 성능 병목 현상을 파악하고, 대응할 수 있습니다.
또한, 슬로우 쿼리를 탐지하여 인덱스 생성, 쿼리 최적화 등의 방법으로 데이터베이스 성능을 개선할 수 있습니다.
고객의 요구사항에 맞게 에이전트 설정을 조정하여 최적의 결과를 제공해 드립니다.

# DataSaker 설정파일을 생성하셨나요?
현재 Ubuntu 환경에서는 `DataSaker` 에이전트를 설치하기 위해서는 기본 설정 파일이 생성되어 있어야 합니다. 만약 기본 설정 파일을 생성하지 않았다면 생성하여 주시기 바랍니다. [DataSaker 설정하기](https://github.com/datasaker/documentation/tree/main/install-guide/linux/ubuntu)

# MySQL agent 설치하기
## 1. agent-config 설정
> `/etc/datasaker/dsk-plan-mysql-agent/agent-config.yml`에 원하는 내용을 기입한다.\
> `cluster_id`의 경우 `dsk-plan-postgres13-agent init <cluster_id>`를 통해 초기화 가능합니다.

```bash
$ sudo dsk-plan-mysql-agent init ${VAR_CLUSTER_NAME}
$ cat /etc/datasaker/dsk-plan-mysql-agent/agent-config.yml

agent:
  metadata:
    agent_name: "dsk-plan-mysql-agent"        # replace you want
    cluster_id: REPLACE_CLUSTER_ID            # replace you want
  data_source_name:
    user:                                     # <user_name>
    password:                                 # <user_password>
    address:                                  # <database_address>
    port:                                     # <database_port>
    DBName:                                   # <database_name>
  explain:
    scrape_interval: 30s                      # <activity_session_scrape_time>
    scrape_timeout: 5s                        # <activity_session_scrape_query_timeout>
    slow_query_standard: 5s                   # <slow_query_standard> 
    executor_number: 10                       # <explain executor number>
    sender_number: 10                         # <explain sender number>
    activity_query_buffer: 50                 # <activity query buffer>
    plan_sender_buffer: 50                    # <explain result buffer>
```

## 2. 패키지 설치
```bash
> DSK_GLOBAL_APIKEY=${VAR_GLOBAL_APIKEY}

> curl -fsSL -o installer.sh https://dsk-agent-s3.s3.ap-northeast-2.amazonaws.com/dsk-agent-s3/public/install.sh
> chmod 700 installer.sh
> sudo ./installer.sh dsk-plan-mysql-agent

> dsk-plan-mysql-agent --help
```

## 3. 패키지 실행
```bash
$ sudo dsk-plan-mysql-agent start
  Agent is running
```

## 4. 패키지 실행 상태 확인
### Running
```bash
$ sudo dsk-plan-mysql-agent status
  Agent is running
```
### Not Running
```bash
$ sudo dsk-plan-mysql-agent status
  Agent is not running
```

# Log agent 제거하기
## 1. 패키지 중단
```bash
$ sudo dsk-plan-mysql-agent stop
```

## 2. 패키지 제거
```bash
$ sudo apt remove dsk-plan-mysql-agent
``` 