# Ubuntu 환경 plan-mysql-agent 설치 방법

## 2. global-config 설정
> `/etc/datasaker/global-config.yml`이 존재하지 않는 경우, 아래 작성된 `global-config.yml`을 작성합니다.

**global-config.yml**
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
" > /etc/datasaker/global-config.yml
```

## 3. agent-config 설정
> `/etc/datasaker/dsk-plan-mysql-agent/agent-config.yml`에 원하는 내용을 기입한다.\
> `cluster_id`의 경우 `dsk-plan-postgres13-agent init <cluster_id>`를 통해 초기화 가능합니다.

```yaml
agent:
  metadata:
    agent_name: "dsk-plan-mysql-agent" # replace you want
    cluster_id: REPLACE_CLUSTER_ID # replace you want
  data_source_name:
    user: # <user_name>
    password: # <user_password>
    address: # <database_address>
    port: # <database_port>
    DBName: # <database_name>
  explain:
    scrape_interval: 30s # <activity_session_scrape_time>
    scrape_timeout: 5s # <activity_session_scrape_query_timeout>
    slow_query_standard: 5s # <slow_query_standard> 
    executor_number: 10 # <explain executor number>
    sender_number: 10 # <explain sender number>
    activity_query_buffer: 50 # <activity query buffer>
    plan_sender_buffer: 50 # <explain result buffer>
```

## 1. 패키지 설치

> sudo 권한이 필요합니다.

```bash
> DSK_GLOBAL_APIKEY=${VAR_GLOBAL_APIKEY}

> curl -fsSL -o installer.sh https://nextcloud.exem-oss.org/s/gCTi3jGR484G4ki/download/dsk-plan-mysql-agent-install.sh
> chmod 700 installer.sh
> sudo ./installer.sh

> dsk-plan-mysql-agent --help
```


## 2. 패키지 초기화
> sudo 권한이 필요합니다.

```bash
$ sudo dsk-plan-mysql-agent init ${VAR_CLUSTER_NAME}
$ cat /etc/datasaker/dsk-plan-mysql-agent/agent-config.yml
agent:
  metadata:
    agent_name: "dsk-plan-mysql-agent" # replace you want
    cluster_id: ${VAR_CLUSTER_NAME} # replace you want
  data_source_name:
    user: # <user_name>
    password: # <user_password>
    address: # <database_address>
    port: # <database_port>
    DBName: # <database_name>
  explain:
    scrape_interval: 30s # <activity_session_scrape_time>
    scrape_timeout: 5s # <activity_session_scrape_query_timeout>
    slow_query_standard: 5s # <slow_query_standard> 
    executor_number: 10 # <explain executor number>
    sender_number: 10 # <explain sender number>
    activity_query_buffer: 50 # <activity query buffer>
    plan_sender_buffer: 50 # <explain result buffer>
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
## 5. 패키지 중단
```bash
$ sudo dsk-plan-mysql-agent stop
```

## 6. 패키지 제거
```bash
$ sudo apt remove dsk-plan-mysql-agent
``` 