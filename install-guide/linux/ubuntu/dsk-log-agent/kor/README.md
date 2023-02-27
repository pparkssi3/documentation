# Ubuntu 환경에 DataSaker Log agent 설치하기
`Log agent`는 다양한 환경에서 시스템 또는 애플리케이션에서 생성되는 로그 데이터를 거의 실시간으로 수집하고, 처리 및 전송하는 에이전트입니다.
`Log agent`를 통해 여러 대의 서버에서 생성되는 로그 데이터를 중앙 집중적으로 관리하고 분석하기 위해 사용할 수 있습니다.
그에 따라 사용자는 시스템 또는 애플리케이션에서 발생하는 문제를 빠르게 감지하고 대응할 수 있습니다.
또한, 로그 데이터를 분석하여 보안, 성능, 비즈니스 분석 등 다양한 목적으로 활용할 수 있습니다.
고객의 요구사항에 맞게 에이전트 설정을 조정하여 최적의 결과를 제공해 드립니다.

## DataSaker 설정파일을 생성하셨나요?
현재 Ubuntu 환경에서는 `DataSaker` 에이전트를 설치하기 위해서는 기본 설정 파일이 생성되어 있어야 합니다. 만약 기본 설정 파일을 생성하지 않았다면 생성하여 주시기 바랍니다. [DataSaker 설정하기](../../README.md)

# Log agent 설치하기

Coming Soon...

[//]: # ()
[//]: # (## 1. agent-config 설정)

[//]: # (`/etc/datasaker/dsk-log-agent/agent-config.yml` 경로에 agent-config 파일을 다음과 같이 생성합니다. &#40;*&#41;가 붙은 곳은 반듯이 입력해야 됩니다.)

[//]: # (```yaml)

[//]: # (agent:)

[//]: # (  metadata:)

[//]: # (    agent_name:         # 에이전트 이름)

[//]: # (    cluster_id:         # 클러스터 정보)

[//]: # (  environment:          # 로그 수집 환경 [kubernetes | docker | etc] &#40;기본 설정값: etc&#41;)

[//]: # (  collect:)

[//]: # (    - paths: []         # &#40;*&#41; 로그 수집 경로)

[//]: # (      exclude_paths: [] # 로그 수집 경로 중 제외시키고자 하는 로그 경로)

[//]: # (      keywords: []      # 로그 수집 키워드 &#40;키워드가 포함된 로그만 수집&#41;)

[//]: # (      tag:              # 사용자 설정 태그)

[//]: # (      service:)

[//]: # (        name:           # 서비스 이름  &#40;기본 설정값: default&#41;)

[//]: # (        category:       # 서비스 분류  [app, database, syslog, etc] &#40;기본 설정값: etc&#41;)

[//]: # (        type:           # 서비스 소스 타입 [postgres, mysql, java] &#40;기본 설정값: etc&#41;)

[//]: # (        address:        # 사용자 설정 - 데이터베이스 host 및 port 정보 &#40;type이 database 인 경우 작성&#41;)

[//]: # (```)

[//]: # ()
[//]: # (## 2. 패키지 설치)

[//]: # (`DataSaker`의 `Log agent`를 설치하기 위해서는 sudo 권한이 필요합니다.)

[//]: # (<!-- )

[//]: # (example API Key : VAR_GLOBAL_APIKEY=1234567890abcdef1234567890abcdef)

[//]: # ( -->)

[//]: # (```bash)

[//]: # (DSK_GLOBAL_APIKEY=${VAR_GLOBAL_APIKEY})

[//]: # ()
[//]: # (curl -fsSL -o installer.sh https://dsk-agent-s3.s3.ap-northeast-2.amazonaws.com/dsk-agent-s3/public/install.sh)

[//]: # (chmod 700 installer.sh)

[//]: # (sudo ./installer.sh dsk-log-agent)

[//]: # ()
[//]: # (sudo DSK_GLOBAL_APIKEY=${DSK_GLOBAL_APIKEY} bash -c '/usr/bin/dsk-log-agent init "'${DSK_GLOBAL_APIKEY}'" && sudo /usr/bin/dsk-log-agent start')

[//]: # (```)

[//]: # ()
[//]: # (## 3. 패키지 실행)

[//]: # (```bash)

[//]: # ($ sudo dsk-log-agent start)

[//]: # (```)

[//]: # (> 주의사항)

[//]: # (> 1. 로그 수집을 위해 패키지 실행 전 에이전트 설정 파일을 구성해야 한다&#40;`global-config.yml`, `config.conf`&#41;.)

[//]: # (> 2. 로그 에에전트는 기본적으로 fluentd를 사용하여 로그를 수집합니다. 이미 사용 중인 fluentd가 있는 경우 `DSK_FLUENTD_CMD` 환경변수를 설정하여 변경할 수 있습니다. 환경변수를 사용하여 실행할 경우 다음과 같이 실행하십시오. &#40;`sudo -E dsk-log-agent start`&#41;)

[//]: # ()
[//]: # (## 4. 패키지 실행 상태 확인)

[//]: # (```bash)

[//]: # ($ sudo dsk-log-agent status)

[//]: # (Agent is running)

[//]: # (```)

[//]: # ()
[//]: # (---)

[//]: # (# Log agent 제거하기)

[//]: # (## 1. 패키지 중단)

[//]: # (```bash)

[//]: # (sudo dsk-log-agent stop)

[//]: # (```)

[//]: # ()
[//]: # (## 2. 패키지 제거)

[//]: # (```bash)

[//]: # (sudo apt-get remove dsk-log-agent)

[//]: # (```)