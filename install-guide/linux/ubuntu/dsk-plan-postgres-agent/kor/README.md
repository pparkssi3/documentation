# Ubuntu 환경에 DataSaker Plan Postgres agent 설치하기
`plan-postgres-agent`는 데이터베이스의 `active session`을 실시간으로 수집합니다.\
이를 통해 데이터베이스의 슬로우 쿼리에 대한 정보를 수집할 수 있습니다.\
슬로우 쿼리를 탐지하여 인덱스 생성, 쿼리 최적화 등의 방법으로 데이터베이스 성능을 개선할 수 있습니다.\
고객의 요구사항에 맞게 에이전트 설정을 조정하여 최적의 결과를 제공해 드립니다.

# DataSaker 설정파일을 생성하셨나요?
현재 Ubuntu 환경에서는 `DataSaker` 에이전트를 설치하기 위해서는 기본 설정 파일이 생성되어 있어야 합니다. 만약 기본 설정 파일을 생성하지 않았다면 생성하여 주시기 바랍니다. [DataSaker 설정하기](https://github.com/datasaker/documentation/tree/main/install-guide/linux/ubuntu)

# Plan Postgres Agent 설치하기

## 1. 패키지 설치
```shell
curl -fsSL -o installer.sh https://dsk-agent-s3.s3.ap-northeast-2.amazonaws.com/dsk-agent-s3/public/install.sh
chmod 700 installer.sh
sudo ./installer.sh dsk-plan-postgres-agent
```

## 2. agent-config 설정
```bash
sudo dsk-plan-postgres-agent init ${VAR_CLUSTER_NAME}
cat /etc/datasaker/dsk-plan-postgres-agent/agent-config.yml
agent:
  metadata:
    agent_name: "dsk-plan-postgres-agent"     # replace you want
    cluster_id: ${VAR_CLUSTER_NAME}           # replace you want
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

## 3. 패키지 실행
```shell
systemctl enable dsk-plan-postgres-agent --now
```

## 4. 패키지 실행 상태 확인
```shell
systemctl status dsk-plan-postgrea-agent
```
또는
```shell
serivce dsk-plan-postgres-agent
```

# Plan Postgres Agent 제거하기
## 1. 패키지 중단
```shell
systemctl stop dsk-plan-postgres-agent
```

## 2. 패키지 제거
```shell
sudo apt remove dsk-plan-postgres-agent
```
