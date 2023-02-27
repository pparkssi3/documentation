# 쿠버네티스 환경에서 Log agent 헬름으로 설치하기
`Log agent`는 다양한 환경에서 시스템 또는 애플리케이션에서 생성되는 로그 데이터를 거의 실시간으로 수집하고, 처리 및 전송하는 에이전트입니다.
`Log agent`를 통해 여러 대의 서버에서 생성되는 로그 데이터를 중앙 집중적으로 관리하고 분석하기 위해 사용할 수 있습니다.
그에 따라 사용자는 시스템 또는 애플리케이션에서 발생하는 문제를 빠르게 감지하고 대응할 수 있습니다.
또한, 로그 데이터를 분석하여 보안, 성능, 비즈니스 분석 등 다양한 목적으로 활용할 수 있습니다.
고객의 요구사항에 맞게 에이전트 설정을 조정하여 최적의 결과를 제공해 드립니다.

# DataSaker를 설치하셨나요?
현재 Kubernetes 환경에 `DataSaker`가 설치되어 있지 않다면 `DataSaker` 설치를 먼저 진행하여 주시기 바랍니다. [DataSaker 설치하기](https://github.com/datasaker/documentation/tree/main/install-guide/kubernetes)

# Log agent 설치하기

기본적으로, `Log agent`는 쿠버네티스 환경에서 데몬셋(daemonset)으로 배포됩니다.
따라서, 모든 노드에 `Log agent`가 설치됩니다. 만약, 특정 노드에만 `Log agent`를 설치하고 싶다면, 해당 노드에 affinity 또는 nodeSelector를 추가적으로 설정해주십시오. 

## 1. Log agent 설정 값 등록
```shell
cat << EOF >> ~/datasaker/config.yaml

logAgent:
  enabled: true
  collect:
    - paths:
        - /var/log/containers/*.log
      exclude_paths: []
      keywords: []
      tag: kubernetes-app-log
      service:
        name: datasaker
        kind: agent
      source:
        type: app
        kind: etc
EOF
```

### Log agent 설정 값 
`Log agent`의 설정 값의 의미와 default값은 다음과 같습니다. 사용자마다 에이전트 설정에 대해 다른 요구사항이 있습니다. 따라서 에이전트 설정을 사용자 설정에 맞게 조정해야 합니다. 최적의 결과를 위해 에이전트 설정을 조정하세요.
"~/datasaker/config.yaml"에서 해당 값을 추가하거나 수정하세요.
```yaml
logAgent:
  enabled: false                                # Log agent 활성화 설정 [true | false ]
  tolerations: []                               # 배포할 워커 노드에 taint가 설정되어 있을 경우 toleration 설정을 추가합니다.
  imgPolicy: 'Always'                           # agent의 Image Policy를 설정합니다. [Always | IfNotPresent | Never]
  imgVersion: 'latest'                          # agent의 Image 태그를 설정합니다.
  logLevel: 'INFO'                              # agent에서 남기는 log level을 설정합니다. [debug > info > warn > error > panic > fatal]
  resources:                                    # agent의 resource를 설정합니다. 너무 작게할 경우 정상동작을 못할 수 있습니다.
    requests:
      cpu: 100m
      memory: 512Mi
    limits:
      cpu: 1
      memory: 2G
  collect:
    - paths:                                   # 수집할 로그의 경로를 입력합니다.
        - /var/log/containers/postgres.log
      exclude_paths: []                        # 수집 대상에서 제외할 경로를 입력합니다.
      keywords: []                             # 지정된 키워드가 포환된 로그만 수집하도록 설정합니다.
      tag: datasaker                           # 수집할 로그 데이터에 태그 정보를 추가합니다.
      service:                                 # 수집할 로그를 분류하기 위해 수집 대상의 서비스의 정보를 입력합니다.
        name: user-custom-service-name         # 서비스 이름 정보 (기본 설정값: default) 
        kind: user-custom-service-kind         # 서비스 종류 정보
      source:                                  # 수집 대상의 정보
        type: database                         # 수집 대상의 타입을 입력합니다. [database | app | syslog | etc] (기본 설정값: etc)
        kind: postgres                         # 수집 대상의 application 개발 언어 및 database 종류를 입력합니다. [postgres | mysql | java | etc] (기본 설정값: etc)
        address: 0.0.0.0:5432                  # 수집 대상 database 주소를 입력합니다.
```

## 2. Log agent 동작
```shell
helm upgrade datasaker ~/datasaker/agent-helm -n datasaker \
  -f ~/datasaker/config.yaml
```

# Log agent 사용 방법

## 1. 반드시 하나 이상의 로그 수집 경로(`path`)를 입력하십시오.

로그 수집 경로를 작성하지 않을 경우, `Log agent`가 정상적으로 동작하지 않을 수 있습니다.
```yaml
collect:
  - paths:                                   # 수집할 로그의 경로를 입력합니다.
      - /var/log/containers/postgres.log
```

## 2. 키워드(`keywords`) 설정에 유의하십시오.

`keywords` 설정에 등록한 문자열이 포함된 로그만 수집합니다. 하나 이상의 `keywords`를 설정할 경우 한 개의 `keywords`라도 포함되어 있으면 로그를 수집합니다.
```yaml
keywords: []                             # 지정된 키워드가 포환된 로그만 수집하도록 설정합니다.
```

## 3. 로그 수집 대상의 `type`이 `database`인 경우 `address` 설정을 권장합니다.

해당 설정을 통해 관련 database agent 에서 로그 정보를 맵핑하여 보여드립니다. `address` 설정을 하지 않으면 해당 기능을 사용하지 못할 수 있습니다.
```yaml
      source:
        type: database
        kind: postgres
        address: 0.0.0.0:5432
```

[//]: # (### 4. 권장 로그 설정 - 각 source kind 별 설정 방법)
