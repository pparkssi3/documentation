# Ubuntu 환경에 DataSaker Log agent 설치하기
`Log agent`는 서버에서 생성되는 로그를 실시간으로 수집합니다. 
이를 통해 고객은 서버의 상태를 실시간으로 모니터링할 수 있으며, 이를 기반으로 서버의 문제를 예방하고 대처할 수 있습니다. 
또한, 로그 데이터를 분석하여 보안, 성능, 비즈니스 분석 등 다양한 목적으로 활용할 수 있습니다. 
고객의 요구사항에 맞게 에이전트 설정을 조정하여 최적의 결과를 제공해 드립니다.

# DataSaker 설정파일을 생성하셨나요?
현재 Ubuntu 환경에서는 `DataSaker` 에이전트를 설치하기 위해서는 기본 설정 파일이 생성되어 있어야 합니다. 만약 기본 설정 파일을 생성하지 않았다면 생성하여 주시기 바랍니다. [DataSaker 설정하기](https://github.com/datasaker/documentation/tree/main/install-guide/linux/ubuntu)

# Log agent 설치하기
## 1. agent-config 설정
`/etc/datasaker/dsk-log-agent/agent-config.yml` 경로에 agent-config 파일을 다음과 같이 생성합니다. (*)가 붙은 곳은 반듯이 입력해야 됩니다.
```yaml
agent:
  metadata:
    agent_name:         # 에이전트 이름
    cluster_id:         # 클러스터 정보
  environment:          # 로그 수집 환경 [kubernetes, docker, etc]
  collect:
    - paths: []         # (*)로그 수집 경로
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

## 2. 패키지 설치
`DataSaker`의 `Log agent`를 설치하기 위해서는 sudo 권한이 필요합니다.
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

## 3. 패키지 실행
```bash
$ sudo dsk-log-agent start
```
> 주의사항
> 1. 로그 수집을 위해 패키지 실행 전 에이전트 설정 파일을 구성해야 한다(`global-config.yml`, `config.conf`).
> 2. 로그 에에전트 기본 포트가 이미 설정되어 있는 경우 `FLUENTD_RELOAD_PORT`, `FLUENTD_HEALTHCHECK_PORT` 환경변수를 설정하여 변경할 수 있다.

## 4. 패키지 실행 상태 확인
```bash
$ sudo dsk-log-agent status
fluentd is running
Agent is running
```

---
# Log agent 제거하기
## 1. 패키지 중단
```bash
sudo dsk-log-agent stop
```

## 2. 패키지 제거
```bash
sudo apt-get remove dsk-log-agent
```
