# 쿠버네티스 환경에서 DataSaker Elasticsearch agent 설치하기
Elasticsearch agent는 DataSaker에서 elasitcsearch 정보를 수집하는 agent입니다.

# DataSaker 선행 작업을 진행하였나요?
현재 Kubernetes 환경에 `DataSaker`의 선행 작업이 진행되지 않으셨다면 `DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. [DataSaker 선행 작업](${MANUAL_KUBERNETES_KR})

# Elasticsearch agent 설치하기
## 1. Elasticsearch agent 설정값 등록
```shell
cat << EOF >> ~/datasaker/config.yaml

elasticsearchAgent:
  list:
    - name: 'es-1'
      uri: 'https://es_id:es_pw@elasticsearch_service.namespace.svc.cluster.local:9200'
      tolerations: []
      imgPolicy: 'Always'
      imgVersion: 'latest'
      logLevel: 'INFO'
      listenPort: 19114
      exporterArgs:
        - --es.timeout=5s
      extraArgs: []
      extraEnvs:
        - name: ENV_1
          value: ENV_1
      resources:
        requests:
          cpu: 100m
          memory: 512Mi
        limits:
          cpu: 1000m
          memory: 1000Mi
EOF
```

## 2. Elasticsearch agent 설치
```shell
helm upgrade datasaker datasaker/agent-helm -n datasaker \
  -f ~/datasaker/config.yaml
```

# Elasticsearch agent 설정하기

## Elasticsearch agent 설정 값
Elasticsearch agent의 설정 값의 의미와 default값은 다음과 같습니다. 사용자마다 에이전트 설정에 대해 다른 요구사항이 있습니다. 따라서 에이전트 설정을 사용자 설정에 맞게 조정해야 합니다. 최적의 결과를 위해 에이전트 설정을 조정하세요.
"~/datasaker/config.yaml"에서 해당 값을 추가하거나 수정하세요.
```yaml
elasticsearchAgent:
  list:
    - name: '엘라스틱서치 이름'           # 엘라스틱서치를 구분할 수 있는 이름 (반드시 사용자가 명시해야 합니다.)
      uri: '엘라스틱서치 주소'            # 엘라스틱서치 주소 (반드시 사용자가 명시해야 합니다.)
      tolerations: []                   # 배포할 워커 노드에 taint가 설정되어 있을 경우 taint를 추가합니다.
      imgPolicy: 'Always'               # agent의 Image Policy를 설정합니다. [Always, IfNotPresent, Never]
      imgVersion: 'latest'              # agent의 Image 태그를 설정합니다.
      logLevel: 'INFO'                  # agent에서 남기는 log level을 설정합니다. [debug > info > warn > error > panic > fatal]
      listenPort: 19114                 # agent에서 사용되는 exporter port를 설정합니다.
      exporterArgs:
        - --es.timeout=5s
      extraArgs: []
      extraEnvs:
        - name: ENV_1
          value: ENV_1
      resources:                        # agent의 resource를 설정합니다. 너무 작게할 경우 정상동작을 못할 수 있습니다.
        requests:
          cpu: 100m
          memory: 512Mi
        limits:
          cpu: 1000m
          memory: 1000Mi
```

더 자세한 정보가 필요하시면 다음 문서를 참고해주세요. [관련 문서](../../../../../settings/dsk-elasticsearch-agent/settings.md)
