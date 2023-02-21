# Ubuntu 환경에 DataSaker의 Base agent 설치하기
`Base agent`는 agent가 배포된 노드의 상태 값 및 자원 사용량에 대한 정보를 수집합니다.

## DataSaker 설정파일을 생성하셨나요?
현재 Ubuntu 환경에서는 `DataSaker` 에이전트를 설치하기 위해서는 기본 설정 파일이 생성되어 있어야 합니다. 만약 기본 설정 파일을 생성하지 않았다면 생성하여 주시기 바랍니다. [DataSaker 설정하기](../../README.md)

# Base agent 설치하기
## 1. 패키지 설치
`DataSaker`의 `Base agent`를 설치하기 위해서는 sudo 권한이 필요합니다.
<!-- 
example API Key : VAR_GLOBAL_APIKEY=1234567890abcdef1234567890abcdef
 -->
```bash
DSK_GLOBAL_APIKEY=${VAR_GLOBAL_APIKEY}

curl -fsSL -o installer.sh https://nextcloud.exem-oss.org/s/eKiRRnym7KRW98Y/download/dsk-node-agent-install.sh
chmod 700 installer.sh
sudo ./installer.sh dsk-node-agent

sudo DSK_GLOBAL_APIKEY=${DSK_GLOBAL_APIKEY} bash -c '/usr/bin/dsk-node-agent init "'${DSK_GLOBAL_APIKEY}'" && systemctl enable dsk-node-agent --now'
```

## 2. 패키지 실행 상태 확인
```bash
systemctl status dsk-node-agent
```
또는
```bash
service dsk-node-agent status
```

---
# Base agent 제거하기
## 1. 패키지 중단
```bash
systemctl stop dsk-node-agent
```

## 2. 패키지 제거
```bash
sudo apt remove dsk-node-agent
```
