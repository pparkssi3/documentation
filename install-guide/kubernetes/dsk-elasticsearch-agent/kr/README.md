# 쿠버네티스 환경에서 데이터세이커 엘라스틸서치 에이전트 helm으로 설치하기

## 기본 에이전트를 설치하셨나요?

**기본 에이전트**가 설치되어 있지 않으면, 먼저 **기본 에이전트**를 설치해야됩니다. (링크)

## ~/datasaker/agent-helm/values.yaml 수정

'list:' 뒤에 대괄호를 제거합니다.  
'-name' 부터 주석을 제거 하여 엘라스틱서치의 정보를 입력합니다.

```shell
  vim ~/datasaker/agent-helm/values.yaml
```

```yaml
elasticsearchAgent:
  tolerations: []
  list: #[] # <- 필요시 제거처리 하거나, 주석처리
    - name: 'es-1'
      imgPolicy: 'Always'
      imgVersion: 'develop'
      logLevel: 'DEBUG'
      extraArgs:
        - --es.uri=https://es_id:es_pw@elasticsearch_service.namespace.svc.cluster.local:9200
        - --es.all=true
        - --es.indices=true
        - --es.aliases=true
        - --es.shards=true
      extraEnv:
        - name: ENV_1
          value: ENV_1
      resources:
        requests:
          cpu: 100m
          memory: 512Mi
        limits:
          cpu: 1000m
          memory: 1000Mi
```

### 에이전트 배포 전략

에이전트 배포 설정은 '~/datasaker/agent-helm/values.yaml' 에 `toleration` 설정을 추가한다. toleration 에 대한 자세한 설명은 [쿠버네티스 공식 문서 링크](https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration/)를 참고한다.

## 헬름 차트 업그래이드

```shell
  helm upgrade datasaker ~/datasaker/agent-helm -n datasaker \
    -f ~/datasaker/config.yaml
```

## Args 인자 목록

**데이터세이커 엘라스틱서치 에이전트**의 설정을 추가 할 수 있습니다.

다음 문서를 참고해주세요. [관련 문서](../../../../settings/dsk-elasticsearch-agent/settings.md)
