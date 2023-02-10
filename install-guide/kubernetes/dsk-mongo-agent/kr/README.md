# 쿠버네티스 환경에서 몽고 에이전트 헬름으로 설치하기

## 기본 에이전트를 설치하셨나요?

**기본 에이전트**가 설치되어 있지 않으면, 먼저 **기본 에이전트**를 설치해야됩니다. (링크)

## ~/datasaker/agent-helm/values.yaml 수정

'list:' 뒤에 대괄호를 제거합니다.  
'-name' 부터 주석을 제거 하여 mongodb의 정보를 입력합니다.

```shell
  vim ~/datasaker/agent-helm/values.yaml
```

```yaml
mongoAgent:
  tolerations: []
  list: # [] <- 대괄호 제거 혹은 주석처리
    - name: 'mongo-1'
      targetAddr: '127.0.0.1:27017'
      user: 'user'
      pass: "password"
      imgPolicy: 'Always'
      imgVersion: 'latest'
      logLevel: 'DEBUG'
      extraArgs:
        - --mongo.argument1
        - --mongo.argument2
      resources:
        requests:
          cpu: 100m
          memory: 512Mi
        limits:
          cpu: 1000m
          memory: 1000Mi

    - name: 'mongo-2'
      targetAddr: '127.0.0.1:27017'
      user: 'user'
      pass: "password"
      imgPolicy: 'Always'
      imgVersion: 'latest'
      logLevel: 'DEBUG'
      extraArgs: []
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

## 헬름 업그래이드 수행

```shell
  helm upgrade datasaker ~/datasaker/agent-helm -n datasaker \
    -f ~/datasaker/config.yaml
```
