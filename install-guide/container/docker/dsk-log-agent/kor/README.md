# Comming soon

<!--
* Kuernetes 설치용 이미지밖에 없어 따로 작업하지 못함.

# 도커 환경에 로그 에이전트 설치하기

`Log agent`는 다양한 환경에서 시스템 또는 애플리케이션에서 생성되는 로그 데이터를 거의 실시간으로 수집하고, 처리 및 전송하는 에이전트입니다.
`Log agent`를 통해 여러 대의 서버에서 생성되는 로그 데이터를 중앙 집중적으로 관리하고 분석하기 위해 사용할 수 있습니다.
그에 따라 사용자는 시스템 또는 애플리케이션에서 발생하는 문제를 빠르게 감지하고 대응할 수 있습니다.
또한, 로그 데이터를 분석하여 보안, 성능, 비즈니스 분석 등 다양한 목적으로 활용할 수 있습니다.
고객의 요구사항에 맞게 에이전트 설정을 조정하여 최적의 결과를 제공해 드립니다.

# DataSaker 선행 작업을 진행하였나요?

현재 Docker 환경에 `DataSaker`의 선행 작업이 진행되지 않으셨다면 `DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. [DataSaker 선행 작업](${MANUAL_DOCKER_KR})

# Log agent 설치하기

## 1. Log agent 설정값 등록

아래 명령어를 참고하여 `log-config`를 설정합니다.

```shell
cd ~
mkdir .datasaker
cat << EOF >> ~/.datasaker/log-config.yml
agent:
  metadata:
    agent_name:         # 에이전트 이름
  collect:
    - paths: []         # (필수) 로그 수집 경로
      exclude_paths: [] # 로그 수집 경로 중 제외시키고자 하는 로그 경로
      keywords: []      # 로그 수집 키워드 (키워드가 포함된 로그만 수집)
      tag:              # 사용자 설정 태그
      service:
        name:           # 서비스 이름  (기본 설정값: default)
        category:       # 서비스 분류  [app, database, syslog, etc] (기본 설정값: etc)
        type:           # 서비스 소스 타입 [postgres, mysql, java] (기본 설정값: etc)
        address:        # 사용자 설정 - 데이터베이스 host 및 port 정보 (type이 database 인 경우 작성)
EOF
```

[주의]

- 반드시 하나 이상의 로그 수집 경로(path)를 입력하십시오. 로그 수집 경로를 작성하지 않을 경우, Log agent가 정상적으로 동작하지 않을 수 있습니다.
- 다음과 같이 특정 경로의 모든 로그를 수집하도록 설정할 경우 로그 에이전트에 많은 부하가 발생할 수 있습니다. (/var/log/*) 수집 로그 파일을 개별적으로 작성하는 것을 권장합니다.(/var/log/containers/sampleApp.log)
- 로그 파일 이외의 파일이 경로에 설정되지 않도록 설정하십시오. 로그 수집 경로의 파일을 지정하거나 그 이외의 파일을 제외시키십시오. 로그 수집 설정에 포맷을 반드시 작성해주십시오.(예시: .log)

## 2. Log agent 설치

1. 데이터세이커가 사용할 로컬 디렉터리를 생성합니다.

   ```shell
    sudo mkdir -p /var/datasaker
    sudo chown -R datasaker:datasaker /var/datasaker/ 
   ```

2. 도커 명령어를 서버에 입력합니다.

   ```shell
   docker run  -it --name dsk-log-agent \
    -v /var/datasaker/:/var/datasaker/ \
    -v /var/lib/docker/containers/:/etc/datasaker/dsk-log-agent/log/containers/ \
    -v /var/log:/var/log\
    -v ~/.datasaker/config.yml:/etc/datasaker/global-config.yml:ro\
    -v ~/.datasaker/log-config.yml:/etc/datasaker/dsk-log-agent/agent-config.yml\
    -e DSK_LOG_LEVEL=INFO\
    -e DSK_CLUSTER_ID=${VAR_CLUSTER_ID} \
    --restart=always\
    datasaker/dsk-log-agent 
   ```
-->