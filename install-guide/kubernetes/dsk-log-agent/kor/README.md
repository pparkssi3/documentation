# 쿠버네티스 환경에서 Log agent 헬름으로 설치하기

## DataSaker를 설치하셨나요?
현재 Kubernetes 환경에 `DataSaker`가 설치되어 있지 않다면 `DataSaker` 설치를 먼저 진행하여 주시기 바랍니다. [DataSaker 설치하기](../../README.md)

# Log agent 설치하기
## 1. Log agent 설정 값 등록
```shell
cat << EOF >> ~/datasaker/config.yaml

logAgent:
  enabled: true
  collect:
    - paths:
        - /var/log/containers/myapp/*.log
      exclude_paths: []
      keywords: []
      tag: dsk-api-prod
      service:
        name: api
        kind: agent
      source:
        type: app
        kind: etc
EOF
```

### Log agent 설정 값 
Log agent의 설정 값의 의미와 default값은 다음과 같습니다. 사용자마다 에이전트 설정에 대해 다른 요구사항이 있습니다. 따라서 에이전트 설정을 사용자 설정에 맞게 조정해야 합니다. 최적의 결과를 위해 에이전트 설정을 조정하세요.
"~/datasaker/config.yaml"에서 해당 값을 추가하거나 수정하세요.
```shell
logAgent:
  enabled: false                                # Log agent 활성화 설정
  tolerations: []                               # 배포할 워커 노드에 taint가 설정되어 있을 경우 taint를 추가합니다.
  imgPolicy: 'Always'                           # agent의 Image Policy를 설정합니다. [Always, IfNotPresent, Never]
  imgVersion: 'latest'                          # agent의 Image 태그를 설정합니다.
  logLevel: 'INFO'                              # agent에서 남기는 log level을 설정합니다. [debug > info > warn > error > panic > fatal]
  resources:                                    # agent의 resource를 설정합니다. 너무 작게할 경우 정상동작을 못할 수 있습니다.
    requests:
      cpu: 100m
      memory: 512Mi
    limits:
      cpu: 1
      memory: 2G
  collect: []
    - paths:
        - /var/log/containers/myapp/*.log      # 수집할 로그의 경로를 입력합니다.
      exclude_paths:                           # 수집 대상에서 제외할 경로를 입력합니다.
      keywords: []                             # 지정된 keywords가 포환된 log를 수집하도록 설정합니다.
      tag: sample postgres log                 # 수집된 log 데이터에 태그 정보를 추가합니다.
      service:                                 # 사용자가 구분하는 서비스의 정보를 입력합니다.
        name: my-postgres-service
        kind: tpcc 
      source:                                  # 수집 대상의 정보
        type: database                         # 수집 대상의 타입을 입력합니다. [database | app | syslog | etc]
        kind: postgres                         # 수집 대상의 DB종류를 입력합니다. DB가 아닐 경우 ETC로 입력합니다. [postgres | mysql | etc]
        address: 0.0.0.0:5555                  # 수집 대상의 주소를 입력합니다.
```

## 2. Log agent 동작
```shell
helm upgrade datasaker ~/datasaker/agent-helm -n datasaker \
  -f ~/datasaker/config.yaml
```