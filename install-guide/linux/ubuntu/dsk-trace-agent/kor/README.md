# Ubuntu 환경에 DataSaker Trace agent 설치하기

## DataSaker 설정파일을 생성하셨나요?
현재 Ubuntu 환경에서는 `DataSaker` 에이전트를 설치하기 위해서는 기본 설정 파일이 생성되어 있어야 합니다. 만약 기본 설정 파일을 생성하지 않았다면 생성하여 주시기 바랍니다. [DataSaker 설정하기](../../README.md)

# Trace agent 설치하기
## 1. 패키지 설치
<!-- 
example API Key : VAR_GLOBAL_APIKEY=1234567890abcdef1234567890abcdef
 -->
```bash
DSK_GLOBAL_APIKEY=${VAR_GLOBAL_APIKEY}

curl -fsSL -o installer.sh https://nextcloud.exem-oss.org/s/BTa8BGyckeCHKkC/download/dsk-trace-agent-install.sh 
chmod 700 installer.sh
sudo ./installer.sh

sudo DSK_GLOBAL_APIKEY=${DSK_GLOBAL_APIKEY} bash -c '/usr/bin/dsk-trace-agent init "'${DSK_GLOBAL_APIKEY}'" && sudo /usr/bin/dsk-trace-agent start'
```

## 2. 패키지 실행 상태 확인
```bash
$ sudo dsk-trace-agents status
Agent is running
Exporter is running
```

# Trace agent 제거하기
## 1. 패키지 중단
```bash
sudo dsk-trace-agent stop
```

## 2. 패키지 제거
```bash
sudo apt-get remove dsk-trace-agent
```
