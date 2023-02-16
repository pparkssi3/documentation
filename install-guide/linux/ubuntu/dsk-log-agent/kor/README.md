# Ubuntu 환경에 로그 에이전트 설치 방법

## global-config 설정
`/etc/datasaker/global-config.yml`이 존재하지 않는 경우, 아래 작성된 `global-config.yml`을 작성합니다.

**global-config.yml**
```bash
echo "
global:
  api_key: ${VAR_GLOBAL_APIKEY}
  gates:
    metric_datagate:
      url: gate.kr.datasaker.io:31302
      remote_timeout: 5s
    manifest_datagate:
      url: gate.kr.datasaker.io:31301
      remote_timeout: 5s
    trace_datagate:
      url: gate.kr.datasaker.io:31300
      remote_timeout: 5s
    plan_datagate:
      url: gate.kr.datasaker.io:31303
      remote_timeout: 5s
    loggate:
      url: gate.kr.datasaker.io:31304
      remote_timeout: 5s
  agent_manager:
    url: api.kr.datasaker.io
    base_url: /dsk-agentmanager-api/agent
    send_interval: 1m
" > /etc/datasaker/global-config.yml
```

## agent-config 설정

```yaml
agent:
  metadata:
    agent_name:         # 에이전트 이름
    cluster_id:         # 클러스터 정보
  environment:          # 로그 수집 환경 [kubernetes, docker, etc]
  collect:
    - paths: []         # 로그 수집 경로
      exclude_paths: [] # 로그 수집 경로 중 제외시키고자 하는 로그 경로
      keywords: []      # 로그 수집 키워드 (키워드가 포함된 로그만 수집)
      tag:              # 사용자 설정 태그
      service:
        - name:         # 서비스 이름  
        - kind:         # 서비스 종류
      source:
        - type:         # 로그 소스 타입 [app, database, syslog, etc]
        - kind:         # 로그 소스 종류 [postgres, mysql, java, python, go]
        - address:      # 사용자 설정 - DB host 및 port 정보
```

## 1. 패키지 설치

> sudo 권한이 필요합니다.

<!-- 
example API Key : VAR_GLOBAL_APIKEY=1234567890abcdef1234567890abcdef
 -->
```bash
DSK_GLOBAL_APIKEY=${VAR_GLOBAL_APIKEY}

curl -fsSL -o installer.sh https://nextcloud.exem-oss.org/s/qPwDBm8QRLf7PGA/download/dsk-log-agent-install.sh
chmod 700 installer.sh
sudo ./installer.sh

sudo DSK_GLOBAL_APIKEY=${DSK_GLOBAL_APIKEY} bash -c '/usr/bin/dsk-log-agent init "'${DSK_GLOBAL_APIKEY}'" && sudo /usr/bin/dsk-log-agent start'
```

## 2. 패키지 실행

```bash
$ sudo dsk-log-agent start
```

> 주의사항
> 1. 로그 수집을 위해 패키지 실행 전 에이전트 설정 파일을 구성해야 한다(`global-config.yml`, `config.conf`).
> 2. 로그 에에전트 기본 포트가 이미 설정되어 있는 경우 `FLUENTD_RELOAD_PORT`, `FLUENTD_HEALTHCHECK_PORT` 환경변수를 설정하여 변경할 수 있다.

## 3. 패키지 실행 상태 확인

```bash
$ sudo dsk-log-agent status
fluentd is running
Agent is running
```

## 4. 패키지 중단

```bash
sudo dsk-log-agent stop
```

## 5. 패키지 제거

```bash
sudo apt-get remove dsk-log-agent
```
