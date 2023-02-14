# 쿠버네티스 환경에서 데이터세이커 base agent 설치하기
**base agent**는 **node agent**와 **container agent**로 구성되어 있습니다. **node agent**는 agent가 배포된 노드의 상태 값 및 자원 사용량에 대한 정보를 수집하며, **container agent**는 agent가 배포된 노드의 컨테이너 정보를 수집합니다.

## 데이터세이커를 설치하셨나요?
현재 Kubernetes 환경에 **데이터세이커**가 설치되어 있지 않다면 **데이터세이커** 설치를 먼저 진행하여 주시기 바랍니다. [데이터세이커 설치하기](../../README.md)

# base agent 설치하기
## 1. base agent 설정 값 등록
```shell
cat << EOF >> ~/datasaker/config.yaml

baseAgents:
  enabled: true
EOF
```
### base agent 설정 값
base agent의 설정 값의 의미와 default값은 다음과 같습니다. 사용자마다 에이전트 설정에 대해 다른 요구사항이 있습니다. 따라서 에이전트 설정을 사용자 설정에 맞게 조정해야 합니다. 최적의 결과를 위해 에이전트 설정을 조정하세요.
"~/datasaker/config.yaml"에서 해당 값을 추가하거나 수정하세요.
```yaml
baseAgents:
  enabled: false          # base agent를 활성화를 설정합니다.
  enableMaster: true      # 기본적으로 마스터 노드의 정보를 수집합니다.
  tolerations: []         # 워커 노드에 taint가 설정되어 있을 경우 taint를 추가합니다.
  nodeAgent:              # 노드의 정보를 수집하는 agent입니다.
    imgPolicy: 'Always'   # agent의 Image Policy를 설정합니다. [Always, IfNotPresent, Never]
    imgVersion: 'latest'  # agent의 Image Version를 설정합니다.
    logLevel: 'INFO'      # agent의 log level을 설정합니다. [debug > info > warn > error > panic > fatal]
    resources:            # agent의 resource를 설정합니다. 너무 작게할 경우 정상동작을 못할 수 있습니다.
      requests:
        cpu: 100m
        memory: 512Mi
      limits:
        cpu: 1000m
        memory: 1000Mi
  containerAgent:         # 컨테이너의 정보를 수집하는 agent입니다.
    imgPolicy: 'Always'   # agent의 Image Policy를 설정합니다. [Always, IfNotPresent, Never]
    imgVersion: 'latest'  # agent의 Image Version를 설정합니다.
    logLevel: 'INFO'      # agent의 log level을 설정합니다. [debug > info > warn > error > panic > fatal]
    resources:            # agent의 resource를 설정합니다. 너무 작게할 경우 정상동작을 못할 수 있습니다.
      requests:
        cpu: 100m
        memory: 512Mi
      limits:
        cpu: 1000m
        memory: 1000Mi
```

## 2. base agent 동작
```shell
helm upgrade datasaker ~/datasaker/agent-helm -n datasaker \
  -f ~/datasaker/config.yaml
```

