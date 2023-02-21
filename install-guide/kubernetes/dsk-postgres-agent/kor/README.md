# 쿠버네티스 환경에서 DataSaker Postgres agent 설치하기
Postgres agent는 DataSaker에서 postgres 정보를 수집하는 agent입니다.

## DataSaker를 설치하셨나요?
현재 Kubernetes 환경에 `DataSaker`가 설치되어 있지 않다면 `DataSaker` 설치를 먼저 진행하여 주시기 바랍니다. [DataSaker 설치하기](../../README.md)

# Postgres agent 설치하기
## 1. Postgres agent 설정값 등록
```shell
cat << EOF >> ~/datasaker/config.yaml

postgresAgents:
  list:
    - name: 'pg-1'
      tolerations: []
      imgPolicy: 'Always'
      imgVersion: 'latest'
      logLevel: 'INFO'
      postgresPlan: true
      targetAddr: '127.0.0.1'
      targetPort: '5432'
      database: "database"
      user: 'user'
      pass: "pass"
      enableExplain: true
      listenPort: 19187
      exporterArgs: []
      explain:
        scrape_interval: 5s
        scrape_timeout: 5s
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

### Postgres agent 설정 값
Postgres agent의 설정 값의 의미와 default값은 다음과 같습니다. 사용자마다 에이전트 설정에 대해 다른 요구사항이 있습니다. 따라서 에이전트 설정을 사용자 설정에 맞게 조정해야 합니다. 최적의 결과를 위해 에이전트 설정을 조정하세요.
"~/datasaker/config.yaml"에서 해당 값을 추가하거나 수정하세요.
```yaml
postgresAgents:
  list:
    - name: 'pg-1'                      # postgres를 구분할 수 있는 이름 (반듯이 사용자가 명시해야 합니다.)
      tolerations: []                   # 배포할 워커 노드에 taint가 설정되어 있을 경우 taint를 추가합니다.
      imgPolicy: 'Always'               # agent의 Image Policy를 설정합니다.
      imgVersion: 'latest'              # agent의 Image 태그를 설정합니다.
      logLevel: 'INFO'                  # agent에서 남기는 log level을 설정합니다. (debug > info > warn > error > panic > fatal)
      targetAddr: '127.0.0.1'           # postgres의 IP를 설정합니다.
      targetPort: '5432'                # postgres의 Port를 설정합니다.
      database: "database"              # 모니터링할 대상의 postgres의 데이터베이스를 설정합니다.
      user: 'user'                      # postgres 계정의 ID를 설정합니다.
      pass: "pass"                      # postgres 계정의 Password를 설정합니다.
      listenPort: 19187                 # agent에서 사용되는 exporter port를 설정합니다.
      exporterArgs: []                  # postgres exporter의 설정 arguments를 입력합니다.
      postgresPlan: true                # explain 수집 설정을 활성화 합니다.
      explain: 
        scrape_interval: 5s             # explain 수집 주기를 설정합니다.
        scrape_timeout: 5s              # explain 요청 시 타임아웃을 설정합니다.
        executor_number: 10             # explain worker 개수
        sender_number: 10               # DataSaker에 전송하는 worker의 개수 
      resources:                        # agent의 resource를 설정합니다. 너무 작게할 경우 정상동작을 못할 수 있습니다.
        requests:
          cpu: 100m
          memory: 512Mi
        limits:
          cpu: 1000m
          memory: 1000Mi
```

## 2. elasticsearch agent 동작
```shell
helm upgrade datasaker ~/datasaker/agent-helm -n datasaker \
  -f ~/datasaker/config.yaml
```
