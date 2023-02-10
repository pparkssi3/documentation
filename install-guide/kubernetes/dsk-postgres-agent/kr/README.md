# 쿠버네티스 환경에서 데이터세이커 PostgreSQL 에이전트 헬름으로 설치하기

## 기본 에이전트를 설치하셨나요?

**기본 에이전트**가 설치되어 있지 않다면 먼저 **기본 에이전트**를 설치 합니다. (링크)

## ~/datasaker/agent-helm/values.yaml 수정

'list:' 뒤에 대괄호를 제거합니다.  
'-name' 부터 주석을 제거 하여 postgres의 정보를 입력합니다.

```shell
vim ~/datasaker/agent-helm/values.yaml
```

postgresAgent 설정 값을 수정합니다.

```yaml
postgresAgent:
  tolerations: []
  list: #[] # <- 필요시 제거하거나 주석처리
    - name: 'pg-1'
      imgPolicy: 'Always'
      imgVersion: 'latest'
      logLevel: 'INFO'
      targetAddr: '127.0.0.1:5432'
      database: "database"
      user: 'user'
      pass: "pass"
      enableExplain: true
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

## 헬름 업그래이드

```shell
helm upgrade datasaker ~/datasaker/agent-helm -n datasaker \
  -f ~/datasaker/config.yaml
```
