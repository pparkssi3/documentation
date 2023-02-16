# Ubuntu 환경에 트레이스 에이전트 설치 하기

## 1. 패키지 설치

> **sudo** 권한이 필요합니다.

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

## 3. 패키지 중단

```bash
sudo dsk-trace-agent stop
```

## 4. 패키지 제거

```bash
sudo apt-get remove dsk-trace-agent
```
