# 쿠버네티스 환경에서 DataSaker Trace agent 설치하기
Trace agent는 `DataSaker`에서 trace 정보를 수집하는 agent입니다.

## DataSaker를 설치하셨나요?
현재 Kubernetes 환경에 `DataSaker`가 설치되어 있지 않다면 `DataSaker` 설치를 먼저 진행하여 주시기 바랍니다. [DataSaker 설치하기](../../README.md)

# Trace agent 설치하기
## 1. Trace agent 설정 값 등록
```shell
cat << EOF >> ~/datasaker/config.yaml

traceAgent:
  enabled: true
EOF
```

### Trace agent 설정 값
Trace agent의 설정 값의 의미와 default값은 다음과 같습니다. 사용자마다 에이전트 설정에 대해 다른 요구사항이 있습니다. 따라서 에이전트 설정을 사용자 설정에 맞게 조정해야 합니다. 최적의 결과를 위해 에이전트 설정을 조정하세요
"~/datasaker/config.yaml"에서 해당 값을 추가하거나 수정하세요.
```yaml
traceAgent:
  enabled: false                    # Trace agent를 활성화를 설정합니다.
  tolerations: []                   # 워커 노드에 taint가 설정되어 있을 경우 taint를 추가합니다.
  imgPolicy: Always                 # agent의 Image Policy를 설정합니다. (Always, IfNotPresent, Never)
  imgVersion: 'latest'              # agent의 Image Version를 설정합니다.
  logLevel: 'INFO'                  # agent의 log level을 설정합니다. (debug > info > warn > error > panic > fatal)
  tags: 'key1=value1,key2=value2'   # 사용자 설정으로 데이터에 추가할 Tag를 입력하세요. 
  resource:                         # agent의 resource를 설정합니다. 너무 작게할 경우 정상동작을 못할 수 있습니다.
    requests:
      cpu: 100m
      memory: 512Mi
    limits:
      cpu: 1000m
      memory: 1000Mi
```

## 2. Trace agent 동작
```shell
helm upgrade datasaker ~/datasaker/agent-helm -n datasaker \
  -f ~/datasaker/config.yaml
```

# Trace agent 포트 정보
|Port|Protocol|Describe|
|---|---|---|
|6831|UDP|thrift-compact|
|6832|UDP|thrift-binary|
|14250|TCP|jaeger-grpc|
|14268|TCP|jaeger-http|
|4317|TCP|otlp-grpc|
|4318|TCP|otlp-http|
