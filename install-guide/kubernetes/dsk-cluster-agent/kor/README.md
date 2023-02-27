# 쿠버네티스 환경에서 DataSaker Cluster agent 설치하기
`Cluster agent`는 쿠버네티스 클러스터의 상태를 실시간으로 수집합니다.
이를 통해 클러스터의 노드 및 파드의 상태, 리소스 사용량, 이벤트 등 다양한 정보를 수집할 수 있습니다.
또한, 쿠버네티스 API 서버와 연동하여 클러스터 노드 및 리소스의 생성 이벤트를 실시간으로 감지하고, 문제 발생 시 적절한 대응을 할 수 있습니다.
수집된 데이터를 분석하여 클러스터의 성능 개선 및 안정성 강화에 기여할 수 있습니다. 고객의 요구사항에 맞게 에이전트 설정을 조정하여 최적의 결과를 제공해 드립니다.

# DataSaker를 설치하셨나요?
현재 Kubernetes 환경에 `DataSaker`가 설치되어 있지 않다면 `DataSaker` 설치를 먼저 진행하여 주시기 바랍니다. [DataSaker 설치하기](https://github.com/datasaker/documentation/tree/main/install-guide/kubernetes)

# Cluster agent 설치하기
## 1. Cluster agent 설정 값 등록
```shell
cat << EOF >> ~/datasaker/config.yaml

clusterAgents:
  enabled: true
EOF
```

### Cluster agent 설정 값
Cluster agent의 설정 값의 의미와 default값은 다음과 같습니다. 사용자마다 에이전트 설정에 대해 다른 요구사항이 있습니다. 따라서 에이전트 설정을 사용자 설정에 맞게 조정해야 합니다. 최적의 결과를 위해 에이전트 설정을 조정하세요.
"~/datasaker/config.yaml"에서 해당 값을 추가하거나 수정하세요.
```yaml
clusterAgents:
  enabled: false              # Cluster agent를 활성화, 비활설화를 설정합니다.
  enableMaster: true          # 기본적으로 마스터 노드의 정보를 수집합니다.
  tolerations: []             # 워커 노드에 taint가 설정되어 있을 경우 taint를 추가합니다.
  kubeStateAgent:             # kubernetes의 상태 정보를 수집하는 agent입니다.
    imgPolicy: 'Always'       # agent의 Image Policy를 설정합니다. [Always, IfNotPresent, Never]
    imgVersion: 'latest'      # agent의 Image Version를 설정합니다.
    logLevel: 'INFO'          # agent의 log level을 설정합니다. [debug > info > warn > error > panic > fatal]
    resources:                # agent의 resource를 설정합니다. 너무 작게할 경우 정상동작을 못할 수 있습니다.
      requests:
        cpu: 100m
        memory: 512Mi
      limits:
        cpu: 1000m
        memory: 1000Mi
  k8sAgent:                   # kubernetes의 resource manifest 정보를 수집하는 agent입니다.
    imgPolicy: 'Always'       # agent의 Image Policy를 설정합니다. [Always, IfNotPresent, Never]
    imgVersion: 'latest'      # agent의 Image Version를 설정합니다.
    logLevel: 'INFO'          # agent의 log level을 설정합니다. (debug > info > warn > error > panic > fatal)
    resources:                # agent의 resource를 설정합니다. 너무 작게할 경우 정상동작을 못할 수 있습니다.
      requests:
        cpu: 100m
        memory: 512Mi
      limits:
        cpu: 1000m
        memory: 1000Mi
```

## 2. Cluster agent 동작
```shell
helm upgrade datasaker ~/datasaker/agent-helm -n datasaker \
  -f ~/datasaker/config.yaml
```

